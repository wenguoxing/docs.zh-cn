---
title: SqlStreamChars.SetLength(Int64) 方法 (System.Data.SqlTypes)
author: stevestein
ms.author: sstein
ms.date: 12/20/2018
ms.technology: dotnet-data
topic_type:
- apiref
api_name:
- System.Data.SqlTypes.SqlStreamChars.SetLength
api_location:
- System.Data.dll
api_type:
- Assembly
ms.openlocfilehash: 6bdf7bf5b998135652bc63d8d3e6c71a61475d20
ms.sourcegitcommit: 8699383914c24a0df033393f55db3369db728a7b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2019
ms.locfileid: "65634294"
---
# <a name="sqlstreamcharssetlengthint64-method"></a>SqlStreamChars.SetLength(Int64) 方法

当在派生类中重写时释放流使用的资源。 包含此方法的程序集具有与 SQLAccess.dll 友元关系。 它被用于 SQL server 上。 对于其他数据库，使用提供该数据库的宿主机制。

```csharp
public abstract void SetLength (long value);
```

## <a name="parameters"></a>参数

`value`\
所需的当前流的长度（以字节表示）。

## <a name="remarks"></a>备注

> [!WARNING]
> `SqlStreamChars.SetLength`方法是私有的不适合直接在代码中使用。
>
> Microsoft 不支持在生产应用程序在任何情况下使用此字段。

## <a name="requirements"></a>要求

**Namespace**：<xref:System.Data.SqlTypes>

**程序集：** System.Data （在 System.Data.dll 中)

**.NET framework 版本：** 自 2.0 之后可用。
