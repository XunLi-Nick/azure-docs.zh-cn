---
title: Azure 容器映像的产品/服务设置 | Microsoft Docs
description: 配置 Azure 容器的产品/服务设置。
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 11/01/2018
ms.author: pbutlerm
ms.openlocfilehash: 9a669ebad674f938d11c8e372e6f586ca3c67f03
ms.sourcegitcommit: 1fc949dab883453ac960e02d882e613806fabe6f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2018
ms.locfileid: "50980123"
---
# <a name="container-offer-settings-tab"></a>容器的“产品/服务设置”选项卡

“容器”>“新建产品/服务”页打开时将聚焦在“产品/服务设置”选项卡。 

![套餐标识](./media/containers-offer-settings.png)

## <a name="offer-identity-settings"></a>“产品/服务标识”设置

在“产品/服务标识”下，必须为下表中描述的字段提供信息。 名称旁边附有星号 (*) 的字段表示必填字段。 

|  **字段**       |     **说明**                                                          |
|  ---------       |     ---------------                                                          |
| **产品/服务 ID**       | 产品/服务的唯一标识符（在发布者配置文件内）。 此标识符将显示在产品 URL 和见解报表中。 它的最大长度为 50 个字符，并且可以使用小写字母数字字符和短划线 (-)。 （标识符不能以短划线结尾。）**注意：** 产品/服务推出后，无法更改此字段。 <br> 例如，如果 Contoso 发布了产品/服务 ID 为 **sample-container** 的产品/服务，则会为该产品/服务分配 Azure 市场 URL `https://azuremarketplace.microsoft.com/marketplace/apps/contoso.sample-container?tab=Overview`。 |
| **发布者 ID**     | 你的组织在 Azure 市场中的唯一标识符。 你的所有产品/服务都应当与你的发布者 ID 相关联。 保存产品/服务后，无法更改此值。 |
| **名称**          | 产品/服务的显示名称。 此名称显示在 Azure 市场和云合作伙伴门户中。 该名称不能超过 50 个字符。 建议为你的产品使用可识别的品牌名称。 不要包括你的组织的名称，除非那是你的产品的营销方式。 如果你在其他网站和出版物中营销此产品/服务，请确保名称在所有出版物中相同。 |
|  |  |

选择“保存”以保存你的产品/服务设置。

## <a name="next-steps"></a>后续步骤

使用 [SKU](./cpp-skus-tab.md) 选项卡配置你的产品/服务的 SKU。
