The shareware version of the West Wind Client tools ships with a pre-compiled version of the classes in a FoxPro `.app` file - `wclient.app`. This `.app` library contains all the library files and running the the app loads all included libraries into memory at once.

> #### @icon-warning Samples and Load Differences
> Note: The shareware version is loaded differently than the full version, so you will need to adjust the examples in this help file to pre-load all libraries. 
>
> Whereever you see `SET PROCEDURE TO <wwClientLibrary>.prg ADDITIVE` you will need to use `DO wwClient` instead, although **you only need to do that once**. `wwClient.app` loads **all** of the wwClient libraries at once and adds them to the Procedure Stack.

### Loading Libraries
Use `DO wwClient.app` to load all wwClient libraries up front when your application starts, or if you're checking out the samples before you run any of the samples:

```foxpro
*** Loads all libraries
DO wwClient  && wwclient.app

loHttp = CREATEOBJECT("wwHttp")
lcHtml = loHttp.Get("https://microsoft.com")

loBridge = CREATEOBJECT("wwDotnetBridge")
? loBridge.GetDotnetVersion()
```

Once libraries have been loaded you can use the same syntax shown for other examples (usually `do wwhttp` or `do wwdotnetbridge` which load all related dependencies) or you can create instances of classes as needed.

The Client Tools libraries use self-loaders - that are a small stub at the top of the class PRG - that load the required library dependencies for easier usage.

Most examples show code like this:

```foxpro
DO wwHttp   && Load Libraries

*** Instantiate
loHtml = CREATEOBJECT("wwHttp")
```

**But using the shareware version you'll need to load the library like this:**

```foxpro
DO wwClient   && Load all Libraries

* This now works, but is not required
*** DO wwHttp

*** Instantiate
loHtml = CREATEOBJECT("wwHttp")
```

> **Note:** Loader PRG commands in the examples like `DO wwHttp` will work **once you've run wwClient.app** to load all libraries, but will fail if you don't. 


To minimize the differences it's recommended that you call `DO wwClient` at the start of your application. 

Once you register, you can switch that code to load the specific libraries that you need (ie. `DO wwHttp`) even though that's not really required since all libraries are loaded.

### Switching to the Registered Version
If you decide to register your copy of the Client tools, the code to load libraries will be slightly different. You need to explicitly load each library on its own using `SET PROCEDURE TO`, `DO <LibraryName>.prg` or using `NEWOBJECT()` with a class library provided.

This is the reason we use the `DO <library>` self-loading syntax to ensure that dependencies are automatically included. 

Most PRG libraries in Web Connection come with this self dependency loader which lets you execute the main PRG file which automatically loads all related dependencies.

For example:

```foxpro
DO wwHTTP
```

loads the libraries needed for HTTP which includes a dependency on `wwUtils` and `wwApi` which are both loaded by the above command. See individual documentation for each class whether this is supported.

If you want to plan ahead in the shareware version you can use this syntax as needed.

The registered version of the client tools provides all classes as source code so you can selectively include them in your application.

### Suggestion: Create an Application Library Loader Program
Just like we use the best practice of providing a library self-loader, it's also a good practice to have an application library self-loader which allows you to load libraries all at once on startup of the application, or interactively for local testing.

We recommend the following:

* Create your own Application specific loader for your libraries
* Something like `DO LOAD_<MyApp>`
* In there use `DO <library>` or `SET PROCEDURE TO <lib> ADDITIVE` OR `SET CLASSLIB <lib> ADDITIVE`

The advantage of this approach is many fold:

1. All your libraries are loaded in one place
2. You can easily load all libraries from the Command Window  
*<small>(which makes it easy to test components)</small>*
3. You can easily see dependencies
4. You don't need to reload libraries or use the slower  `NEWOBJECT()`

A application loader program looks like this:

```foxpro
* LOAD_MyApp.prg

DO wwClient  && if using shareware version

*** Declare 3rd party or internal librarys
DO wwHttp
DO wwDotnetBridge

*** Declare app libraries (also using self-loader)
DO busCustomer
DO busInvoice

*** If main programs contain callable components
SET PROCEDURE TO MainApp ADDITIVE
SET PROCEDURE TO MainApp_Utils ADDITIVE
```