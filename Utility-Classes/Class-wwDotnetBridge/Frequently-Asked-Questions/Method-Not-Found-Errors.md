﻿Method not found errors originate inside of .NET code when either your method name isn't correct (duh!) or when the parameters passed from FoxPro do not EXACTLY match the signature of the .NET method. This is more common than you might think because FoxPro often marshals .NET parameters - especially numeric values - in funky ways.

To troubleshoot this problem go into Reflector and examine the exact method signature of the method you are trying to call.

![](/images/wwDotNetBridge/Reflector_MethodSignature.png)

and examine each of the parameters that you are trying to pass to pass.

### Special Parameter Types - use ComValue
One of the most common method mapping errors is caused by the mismatch between FoxPro and .NET types. 

To work around this issue there's a [ComValue class](vfps://Topic/Class%20ComValue) that provides the ability coerce FoxPro values into specific .NET types. ComValue allows you to assign a FoxPro value and store that value as a specific type inside of .NET. The value never leaves .NET. You can then substitute this ComValue for the real parameter in any call to the indirect method calls to InvokeMethod() and SetProperty().

### Common Problem Parameter Types

**Numbers**  
One of the most common problem types are numbers. FoxPro essentially has a single number type which is a double. When passed to .NET that number is marshalled by COM as best as possible which works automatically. When calling methods that have overloads however .NET may get confused and may not be able to match the numeric type. In this case use any of the following methods the ComValue object:

* SetInt16()
* SetInt64()
* SetLong()
* SetSingle()
* SetDouble()
* SetByte()

Long and Int64 types in .NET are very common, but FoxPro doesn't support them so you'll want to *always* use the SetLong method. 

Here's an example:

```foxpro
loStoreId = loBridge.CreateComValue()
loStoreId.SetLong(9999)

*** MUST always use indirect invokation
loProxy.InvokeMethod(loService,"Authenticate",loStoreId,lcUserName, lcPassword)
```

**Enum Values**  
Another common types that are passed to methods are enumerated values. Although wwDotnetBridge includes the GetEnumValue() method that allows you retrieve an enum value as a number, this does not work for method invokation because wwDotnetBridge needs to map the method's types exactly.

Instead you should use the SetEnum() property of the ComValue object to assign an enum value directly:

```foxpro
loOwnerApp = loBridge.CreateComValue()
loOwnerApp.SetEnum("TransactionProcessing.WSOwnerApplication.Web_Service")

*** MUST always use indirect invokation
loResponse = loProxy.InvokeMethod(loService,"GetItems",loOwnerApp)
```