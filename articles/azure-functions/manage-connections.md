---
title: 如何管理 Azure Functions 中的连接
description: 了解如何通过使用静态连接客户端来避免 Azure Functions 中的性能问题。
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
ms.date: 11/02/2018
ms.author: glenga
ms.openlocfilehash: eb5c302c807f85f24f53fa1ba32ef4cd7b52274a
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2018
ms.locfileid: "51036455"
---
# <a name="how-to-manage-connections-in-azure-functions"></a>如何管理 Azure Functions 中的连接

函数应用中的函数共享资源，这些共享资源包含各种连接 &mdash; HTTP 连接、数据库连接以及到 Azure 服务（如存储）的连接。 当许多函数同时运行时，可能会耗尽可用的连接。 本文介绍如何编写函数，以避免其使用的连接数超过实际所需。

## <a name="connections-limit"></a>连接限制

可用连接数量受到限制，部分原因是函数应用在 [Azure 应用服务沙盒](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)中运行。 沙盒对代码施加的限制之一便是[连接数量上限，目前为 300 个](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox#numerical-sandbox-limits)。 达到此限制时，函数运行时会创建一个包含以下消息的日志：`Host thresholds exceeded: Connections`。

[缩放控制器添加函数应用实例](functions-scale.md#how-the-consumption-plan-works)以处理更多请求时，超出限制的可能性会增加。 每个函数应用实例可同时运行许多函数，所有这些函数使用的连接都会计入 300 个限制中。

## <a name="use-static-clients"></a>使用静态客户端

为了避免占用的连接数超过实际所需，可重复使用客户端实例，而不是在每次调用函数时创建新的实例。 如果使用单个静态客户端，[HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)、[DocumentClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient
) 和 Azure 存储客户端等 .NET 客户端可以管理连接。

在 Azure Functions 应用程序中使用特定于服务的客户端时，请遵循以下准则：

- **不要**在每次调用函数时创建新的客户端。
- **应**创建一个可在每次调用函数时使用的静态客户端。
- 如果不同的函数使用相同的服务，请**考虑**在共享帮助程序类中创建单个静态客户端。

## <a name="client-code-examples"></a>客户端代码示例

本部分演示了用于通过函数代码创建和使用客户端的最佳做法。

### <a name="httpclient-example-c"></a>HttpClient 示例 (C#)

下面是用于创建静态 [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx) 的 C# 函数代码示例：

```cs
// Create a single, static HttpClient
private static HttpClient httpClient = new HttpClient();

public static async Task Run(string input)
{
    var response = await httpClient.GetAsync("http://example.com");
    // Rest of function
}
```

关于 .NET [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx) 的一个常见问题是“我应该释放我的客户端吗？” 一般情况下，在使用对象实现了 `IDisposable` 之后，你会释放这些对象。 但是，你不会释放静态客户端，因为在函数结束后还需要使用它。 你需要将静态客户端一直保留到应用程序生存期结束。

### <a name="http-agent-examples-nodejs"></a>HTTP 代理示例 (Node.js)

因为它提供了更好的连接管理选项，所以你应当使用本机 [`http.agent`](https://nodejs.org/dist/latest-v6.x/docs/api/http.html#http_class_http_agent) 类而不应当使用非本机方法，例如 `node-fetch` 模块。 连接参数是使用 `http.agent` 类上的选项配置的。 有关 HTTP 代理可用的详细选项，请参阅[new Agent(\[options\])](https://nodejs.org/dist/latest-v6.x/docs/api/http.html#http_new_agent_options)。

`http.request()` 使用的全局 `http.globalAgent` 将这些值全部设置为其各自的默认值。 在 Functions 中配置连接限制的建议方法是全局设置一个最大数量。 以下示例为函数应用设置最大 socket 数量：

```js
http.globalAgent.maxSockets = 200;
```

 以下示例创建一个新的 HTTP 请求，并采用仅用于该请求的自定义 HTTP 代理。

```js
var http = require('http');
var httpAgent = new http.Agent();
httpAgent.maxSockets = 200;
options.agent = httpAgent;
http.request(options, onResponseCallback);
```

### <a name="documentclient-code-example-c"></a>DocumentClient 代码示例 (C#)

[DocumentClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient
) 连接到 Azure Cosmos DB 实例。 Azure Cosmos DB 文档建议[在应用程序生存期内使用单一实例 Azure Cosmos DB 客户端](https://docs.microsoft.com/azure/cosmos-db/performance-tips#sdk-usage)。 以下示例展示了在函数中执行该操作的一种模式：

```cs
#r "Microsoft.Azure.Documents.Client"
using Microsoft.Azure.Documents.Client;

private static Lazy<DocumentClient> lazyClient = new Lazy<DocumentClient>(InitializeDocumentClient);
private static DocumentClient documentClient => lazyClient.Value;

private static DocumentClient InitializeDocumentClient()
{
    // Perform any initialization here
    var uri = new Uri("example");
    var authKey = "authKey";
    
    return new DocumentClient(uri, authKey);
}

public static async Task Run(string input)
{
    Uri collectionUri = UriFactory.CreateDocumentCollectionUri("database", "collection");
    object document = new { Data = "example" };
    await documentClient.UpsertDocumentAsync(collectionUri, document);
    
    // Rest of function
}
```

## <a name="sqlclient-connections"></a>SqlClient 连接

函数代码可使用 SQL Server 的 .NET Framework 数据提供程序 ([SqlClient](https://msdn.microsoft.com/library/system.data.sqlclient(v=vs.110).aspx)) 连接到 SQL 关系数据库。 这也是依赖于 ADO.NET 的数据框架（例如实体框架）的基础提供程序。 与 [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx) 和 [DocumentClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient
) 连接不同，ADO.NET 默认实现连接池。 但是，由于连接仍可能耗尽，因此应优化数据库连接。 有关详细信息，请参阅 [SQL Server 连接池 (ADO.NET)](https://docs.microsoft.com/dotnet/framework/data/adonet/sql-server-connection-pooling)。

> [!TIP]
> 某些数据框架（例如[实体框架](https://msdn.microsoft.com/library/aa937723(v=vs.113).aspx)）通常从配置文件的 ConnectionStrings 部分获取连接字符串。 在这种情况下，必须将 SQL 数据库连接字符串显式添加到函数应用设置的连接字符串集合以及本地项目中的 [local.settings.json 文件](functions-run-local.md#local-settings-file)中。 如果要在函数代码中创建 [SqlConnection](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection(v=vs.110).aspx)，则应将连接字符串值与其他连接一起存储在应用程序设置中。

## <a name="next-steps"></a>后续步骤

有关为何建议使用静态客户端的详细信息，请参阅[不当实例化反模式](https://docs.microsoft.com/azure/architecture/antipatterns/improper-instantiation/)。

有关更多 Azure Functions 性能提示，请参阅[优化 Azure Functions 的性能和可靠性](functions-best-practices.md)。
