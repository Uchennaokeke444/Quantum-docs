---
author: bradben
description: Learn about different ways to call operators and functions in the Q# programming language.
ms.author: brbenefield
ms.date: 02/01/2021
ms.service: azure-quantum
ms.subservice: qsharp-guide
ms.topic: reference
no-loc: ['Q#', '$$v']
title: Call statements in Q#
uid: microsoft.quantum.qsharp.callstatements
---

# Call Statements

Call statements are a very important part of any programming language. While operation and function calls can be used as an expression anywhere as long as the returned value is of a suitable type, they can also be used as statements if they return `Unit`. 
The usefulness of calling functions in this form primarily lays in debugging, whereas such operation calls are one of the most common constructs in any Q# program. At the same time, operations can only be called from within other operations and not from within functions (for context, see also [this section](xref:microsoft.quantum.qsharp.quantumdatatypes#qubits)). 

With callables being first-class values, 
call statements in a sense are the generic way of supporting patterns that aren't common enough to merit their own dedicated language construct or dedicated syntax has not (yet) been introduced for other reasons. Examples for library methods that serve exactly that purpose are `ApplyIf`, that invokes an operation conditional on a classical bit being set, `ApplyToEach`, that applies a given operation to each element in an array, and `ApplyWithInputTransformation` shown below to give to just a few. 

```qsharp
    operation ApplyWithInputTransformation<'TArg, 'TIn>(
        fn : 'TIn -> 'TArg,
        op : 'TArg => Unit,
        input : 'TIn
    ) : Unit {

        op(fn(input)); 
    }
```

`ApplyWithInputTransformation` takes a function `fn`, an operation `op`, and an `input` value as argument, applies the given function to the input, before invoking the given operation with the value returned from the function.

For the compiler to be able to auto-generate the specializations to support particular [functors](xref:microsoft.quantum.qsharp.functorapplication#functor-application) usually requires that the called operations support those functors as well. The exception are calls in outer blocks of [conjugations](xref:microsoft.quantum.qsharp.conjugations#conjugations), which always need to support the `Adjoint` functor but never need to support the `Controlled` functor, and self-adjoint operations, which support the `Adjoint` functor without imposing any additional requirements on the individual calls. 

### *Discussion*
>More sophisticated [generation directives](xref:microsoft.quantum.qsharp.specializationdeclarations#auto-generation-directives) may allow to further relax that requirement in the future. 



