---
title: 如何：捕捉非 CLS 异常
ms.date: 07/20/2015
helpviewer_keywords:
- exceptions [C#], non-CLS
ms.assetid: db4630b3-5240-471a-b3a7-c7ff6ab31e8d
ms.openlocfilehash: 27b36d85b2ece957c8ef3fce70a6fd794bb3d4e2
ms.sourcegitcommit: 2701302a99cafbe0d86d53d540eb0fa7e9b46b36
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2019
ms.locfileid: "64595671"
---
# <a name="how-to-catch-a-non-cls-exception"></a>如何：捕捉非 CLS 异常
包括 C++/CLI 在内的某些 .NET 语言允许对象引发并非派生自 <xref:System.Exception> 的异常。 这类异常被称为非 CLS 异常或非异常。 无法在 C# 中引发非 CLS 异常，但有两种方式可以捕获它们：  
  
- 在 `catch (RuntimeWrappedException e)` 块内捕获。
  
     默认情况下，Visual C# 程序集将非 CLS 异常作为包装的异常捕获。 如需访问原始异常（可通过 <xref:System.Runtime.CompilerServices.RuntimeWrappedException.WrappedException%2A?displayProperty=nameWithType> 属性访问），请使用此方法。 本主题的后续过程将解释如何通过此方式捕获异常。  
  
- 在位于所有其他 `catch` 块之后的常规 catch 块（未指定异常类型的 catch 块）之中。
  
     如果为了响应非 CLS 异常需要执行某些操作（如写入日志文件），且无需访问异常信息时，请使用此方法。 默认情况下，公共语言运行时包装所有异常。 要禁用此行为，请将此程序集级别属性添加到代码中，通常位于 AssemblyInfo.cs 文件：`[assembly: RuntimeCompatibilityAttribute(WrapNonExceptionThrows = false)]`。  
  
### <a name="to-catch-a-non-cls-exception"></a>要捕捉非 CLS 异常  
  
在 `catch(RuntimeWrappedException e)` 块中通过 <xref:System.Runtime.CompilerServices.RuntimeWrappedException.WrappedException%2A?displayProperty=nameWithType> 属性访问原始异常。  
  
## <a name="example"></a>示例  
 下面示例显示如何捕捉以 C++/CLI 编写的类库所引发的非 CLS 异常。 请注意，在此示例中，C# 客户端代码预先已知被引发的异常类型是 <xref:System.String?displayProperty=nameWithType>。 可将 <xref:System.Runtime.CompilerServices.RuntimeWrappedException.WrappedException%2A?displayProperty=nameWithType> 属性转换回其原始类型，前提是可从代码访问该类型。  
  
```csharp
// Class library written in C++/CLI.
var myClass = new ThrowNonCLS.Class1();

try
{
    // throws gcnew System::String(  
    // "I do not derive from System.Exception!");  
    myClass.TestThrow();
}
catch (RuntimeWrappedException e)
{
    String s = e.WrappedException as String;
    if (s != null)
    {
        Console.WriteLine(s);
    }
}
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.Runtime.CompilerServices.RuntimeWrappedException>
- [异常和异常处理](../../../csharp/programming-guide/exceptions/index.md)
