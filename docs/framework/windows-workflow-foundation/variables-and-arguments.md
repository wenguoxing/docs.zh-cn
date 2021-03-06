---
title: 变量和自变量
ms.date: 03/30/2017
ms.assetid: d03dbe34-5b2e-4f21-8b57-693ee49611b8
ms.openlocfilehash: 29ce5222435b68ed13cbc967e58e72a937625e8e
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "61669480"
---
# <a name="variables-and-arguments"></a>变量和自变量
在 Windows Workflow Foundation (WF) 中，变量表示的数据存储区和自变量表示数据的流执行和跳出执行活动。 活动拥有一组自变量，这些自变量构成活动的签名。 此外，活动可以维护一个变量列表，在工作流设计期间，开发人员可在该列表中添加或移除变量。 使用可返回值的表达式可以绑定参数。  
  
## <a name="variables"></a>变量  
 变量是数据的存储位置。 变量被声明为工作流定义的一部分。 变量在运行时获取值，并将这些值存储为工作流实例状态的一部分。 变量定义指定了变量的类型，如果需要，还可指定变量的名称。 以下代码演示如何声明变量，使用 <xref:System.Activities.Statements.Assign%601> 活动为变量赋值，然后使用 <xref:System.Activities.Statements.WriteLine> 活动将其值显示在控制台上。  
  
```csharp  
// Define a variable named "str" of type string.  
Variable<string> var = new Variable<string>  
{  
    Name = "str"  
};  
  
// Declare the variable within a Sequence, assign  
// a value to it, and then display it.  
Activity wf = new Sequence()  
{  
    Variables = { var },  
    Activities =  
    {  
        new Assign<string>  
        {  
            To = var,  
            Value = "Hello World."  
        },  
        new WriteLine  
        {  
            Text = var  
        }  
    }  
};  
  
WorkflowInvoker.Invoke(wf);  
```  
  
 可根据需要将默认值表达式指定为变量声明的一部分。 变量还可具有修饰符。 例如，如果某个变量为只读变量，则可应用只读 <xref:System.Activities.VariableModifiers> 修饰符。 在以下示例中，创建具有指定默认值的只读变量。  
  
```csharp  
// Define a read-only variable with a default value.  
Variable<string> var = new Variable<string>  
{  
    Default = "Hello World.",  
    Modifiers = VariableModifiers.ReadOnly  
};  
```  
  
## <a name="variable-scoping"></a>变量的作用域  
 变量在运行时的生存期与声明该变量的活动的生存期相同。 活动完成后，其变量将被清除，并且无法再引用。  
  
## <a name="arguments"></a>自变量  
 活动作者使用参数来定义数据流入流出活动的方式。 每个自变量都有特定的方向：<xref:System.Activities.ArgumentDirection.In>、<xref:System.Activities.ArgumentDirection.Out>、或 <xref:System.Activities.ArgumentDirection.InOut>。  
  
 工作流运行时对数据流入流出活动的时间有以下保证：  
  
1. 活动开始执行时，将计算其所有输入和输入/输出参数的值。 例如，不管何时调用 <xref:System.Activities.Argument.Get%2A>，返回值都为调用 `Execute` 之前运行时所计算的值。  
  
2. 调用 <xref:System.Activities.InOutArgument%601.Set%2A> 时，运行时将立即设置值。  
  
3. 可根据需要指定自变量的 <xref:System.Activities.Argument.EvaluationOrder%2A>。 <xref:System.Activities.Argument.EvaluationOrder%2A> 是指定自变量计算顺序的从零开始的值。 默认情况下，自变量的计算顺序未指定且等于 <xref:System.Activities.Argument.UnspecifiedEvaluationOrder> 值。 将 <xref:System.Activities.Argument.EvaluationOrder%2A> 设置为一个大于或等于零的值，以便为此自变量指定一个计算顺序。 Windows Workflow Foundation 的计算结果的参数和指定的计算顺序按升序排列。 注意：未指定计算顺序的参数将先于指定计算顺序的参数计算。  
  
 活动作者可使用强类型机制来公开该活动的自变量。 实现方法是声明 <xref:System.Activities.InArgument%601>、<xref:System.Activities.OutArgument%601> 和 <xref:System.Activities.InOutArgument%601> 类型的属性。 这允许活动作者建立有关流入流出活动的数据的特定协定。  
  
### <a name="defining-the-arguments-on-an-activity"></a>定义活动的自变量  
 可通过指定 <xref:System.Activities.InArgument%601>、<xref:System.Activities.OutArgument%601> 和 <xref:System.Activities.InOutArgument%601> 类型的属性来定义活动的自变量。 以下代码演示如何为 `Prompt` 活动定义参数，该活动接收一个字符串以显示给用户，并返回包含用户响应的字符串。  
  
```csharp  
public class Prompt : Activity  
{  
    public InArgument<string> Text { get; set; }  
    public OutArgument<string> Response { get; set; }  
    // Rest of activity definition omitted.  
}  
```  
  
> [!NOTE]
>  返回单个值的活动可从 <xref:System.Activities.Activity%601>、<xref:System.Activities.NativeActivity%601> 或 <xref:System.Activities.CodeActivity%601> 派生。 这些活动拥有定义完善的名为 <xref:System.Activities.OutArgument%601> 的 <xref:System.Activities.Activity%601.Result%2A>，它包含活动的返回值。  
  
### <a name="using-variables-and-arguments-in-workflows"></a>在工作流中使用变量和自变量  
 以下示例演示如何在工作流中使用变量和自变量。 该工作流是一个声明三个变量：`var1`、`var2` 和 `var3` 的序列。 该工作流中的第一个活动是 `Assign` 活动，它将变量 `var1` 的值赋给变量 `var2`。 接下来是 `WriteLine` 活动，该活动打印 `var2` 变量的值。 之后是另一个 `Assign` 活动，该活动将变量 `var2` 的值赋给变量 `var3`。 最后是另一个 `WriteLine` 活动，该活动打印 `var3` 变量的值。 第一个 `Assign` 活动使用 `InArgument<string>` 和 `OutArgument<string>` 对象，显式表示对活动自变量的绑定。 将 `InArgument<string>` 用于 <xref:System.Activities.Statements.Assign.Value%2A> 是因为值通过其 <xref:System.Activities.Statements.Assign%601> 自变量流入 <xref:System.Activities.Statements.Assign.Value%2A> 活动，而将 `OutArgument<string>` 用于 <xref:System.Activities.Statements.Assign.To%2A> 是因为值从 <xref:System.Activities.Statements.Assign.To%2A> 自变量流出而赋给变量。 第二个 `Assign` 活动完成相同的操作，它采用更加紧凑的语法，通过使用隐式强制转换而达到相同目的。 `WriteLine` 活动也使用紧凑语法。  
  
```csharp  
// Declare three variables; the first one is given an initial value.  
Variable<string> var1 = new Variable<string>()  
{  
    Default = "one"  
};  
Variable<string> var2 = new Variable<string>();  
Variable<string> var3 = new Variable<string>();  
  
// Define the workflow  
Activity wf = new Sequence  
{  
    Variables = { var1, var2, var3 },  
    Activities =   
    {  
        new Assign<string>()  
        {  
            Value = new InArgument<string>(var1),  
            To = new OutArgument<string>(var2)  
        },  
        new WriteLine() { Text = var2 },  
        new Assign<string>()  
        {  
            Value = var2,  
            To = var3  
        },  
        new WriteLine() { Text = var3 }  
    }  
};  
  
WorkflowInvoker.Invoke(wf);  
```  
  
### <a name="using-variables-and-arguments-in-code-based-activities"></a>在基于代码的活动中使用变量和参数  
 先前的示例演示如何在工作流和声明性活动中使用自变量和变量。 自变量和变量也用在基于代码的活动中。 从概念上来说，二者的使用非常相似。 变量表示活动中的数据存储区，而自变量表示流入或流出活动的数据，二者由工作流作者绑定到工作流中表示数据流至或流出位置的其他变量或自变量。 若要获取或设置活动中的变量或参数的值，必须使用表示该活动当前执行环境的活动上下文。 活动上下文由工作流运行时传入活动的 <xref:System.Activities.CodeActivity%601.Execute%2A> 方法中。 在本示例中，为自定义 `Add` 活动定义了两个 <xref:System.Activities.ArgumentDirection.In> 参数。 为了访问这些自变量的值，使用了 <xref:System.Activities.Argument.Get%2A> 方法，并使用了由工作流运行时传入的上下文。  
  
```csharp  
public sealed class Add : CodeActivity<int>  
{  
    [RequiredArgument]  
    public InArgument<int> Operand1 { get; set; }  
  
    [RequiredArgument]  
    public InArgument<int> Operand2 { get; set; }  
  
    protected override int Execute(CodeActivityContext context)  
    {  
        return Operand1.Get(context) + Operand2.Get(context);  
    }  
}  
```  
  
 有关使用参数、 变量和代码中的表达式的详细信息，请参阅[编写工作流、 活动和表达式使用命令性代码](authoring-workflows-activities-and-expressions-using-imperative-code.md)和[所需自变量和重载组](required-arguments-and-overload-groups.md).
