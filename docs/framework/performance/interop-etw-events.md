---
title: 互操作 ETW 事件
ms.date: 03/30/2017
helpviewer_keywords:
- interop events [.NET Framework]
- ETW, interop events (CLR)
ms.assetid: eb6eac2e-45f4-4923-a32c-38f203da66df
author: mairaw
ms.author: mairaw
ms.openlocfilehash: 38db390b8fad9cd36dacf33f9647b0272eddc4a0
ms.sourcegitcommit: 2701302a99cafbe0d86d53d540eb0fa7e9b46b36
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2019
ms.locfileid: "64616410"
---
# <a name="interop-etw-events"></a>互操作 ETW 事件
<a name="top"></a> 互操作事件捕获有关 Microsoft 中间语言 (MSIL) 存根生成和缓存的信息。  
  
 此类别包含以下事件：  
  
- [ILStubGenerated 事件](#ilstubgenerated_event)  
  
- [ILStubCacheHit 事件](#ilstubcachehit_event)  
  
<a name="ilstubgenerated_event"></a>   
## <a name="ilstubgenerated-event"></a>ILStubGenerated 事件  
 下表显示了关键字和级别。 （有关详细信息，请参阅 [CLR ETW Keywords and Levels](../../../docs/framework/performance/clr-etw-keywords-and-levels.md)。）  
  
|引发事件的关键字|级别|  
|-----------------------------------|-----------|  
|`InteropKeyword` (0x2000)|信息性 (4)|  
  
 下表显示了事件信息。  
  
|Event|事件 ID|在发生以下情况时引发|  
|-----------|--------------|-----------------|  
|`ILStubGenerated`|88|已生成 MSIL 存根。|  
  
 下表显示了事件数据。  
  
|字段名|数据类型|描述|  
|----------------|---------------|-----------------|  
|ModuleID|win:UInt16|模块标识符。|  
|StubMethodID|win:UInt64|存根方法标识符。|  
|StubFlags|win:UInt64|存根标志：<br /><br /> 0x1 - 反向互操作。<br /><br /> 0x2 - COM 互操作。<br /><br /> 0x4 - 由 NGen.exe 生成的存根。<br /><br /> 0x8 - 委托。<br /><br /> 0x10 - 可变参数。<br /><br /> 0x20 - 非托管被调用方。|  
|ManagedInteropMethodToken|win:UInt32|托管互操作方法的标记。|  
|ManagedInteropMethodNameSpace|win:UnicodeString|托管互操作方法的命名空间。|  
|ManagedInteropMethodName|win:UnicodeString|托管互操作方法的名称。|  
|ManagedInteropMethodName|win:UnicodeString|托管互操作方法的签名。|  
|NativeMethodSignature|win:UnicodeString|本机方法签名。|  
|StubMethodSignature|win:UnicodeString|存根方法签名。|  
|StubMethodILCode|win:UnicodeString|存根方法的 MSIL 代码。|  
|ClrInstanceID|win:UInt16|CLR 或 CoreCLR 的实例的唯一 ID。|  
  
 [返回页首](#top)  
  
<a name="ilstubcachehit_event"></a>   
## <a name="ilstubcachehit-event"></a>ILStubCacheHit 事件  
 下表显示了关键字和级别。  
  
|引发事件的关键字|级别|  
|-----------------------------------|-----------|  
|`InteropKeyword` (0x2000)|信息性 (4)|  
  
 下表显示了事件信息。  
  
|Event|事件 ID|在发生以下情况时引发|  
|-----------|--------------|-----------------|  
|`ILStubCacheHit`|89|已访问 MSIL 缓存。|  
  
 下表显示了事件数据。  
  
|字段名|数据类型|描述|  
|----------------|---------------|-----------------|  
|ModuleID|win:UInt16|模块标识符。|  
|StubMethodID|win:UInt64|存根方法标识符。|  
|ManagedInteropMethodToken|win:UInt32|托管互操作方法的标记。|  
|ManagedInteropMethodNameSpace|win:UnicodeString|托管互操作方法的命名空间。|  
|ManagedInteropMethodName|win:UnicodeString|托管互操作方法的名称。|  
|ManagedInteropMethodName|win:UnicodeString|托管互操作方法的签名。|  
|ClrInstanceID|win:UInt16|CLR 或 CoreCLR 的实例的唯一 ID。|  
  
 [返回页首](#top)  
  
## <a name="see-also"></a>请参阅

- [CLR ETW 事件](../../../docs/framework/performance/clr-etw-events.md)
