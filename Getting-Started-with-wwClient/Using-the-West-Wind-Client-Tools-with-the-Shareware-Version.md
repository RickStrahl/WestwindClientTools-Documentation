The shareware version of the West Wind Client tools ships with a pre-compiled version of the classes in a FoxPro `.app` file - `wclient.app`. This `.app` library contains all the library files and running the the app loads all included libraries into memory at once.

> #### @icon-warning Samples and Load Differences
> Note: The shareware version is loaded differently than the full version, so you will need to adjust the examples in this help file to pre-load all libraries.

### Loading Libraries
We recommend you use `DO wwClient.app` to load all libraries up front when your application starts, or if your checking out the samples before you run any of the samples:

```foxpro
*** Loads all libraries
DO wwClient  && wwclient.app

loHttp = CREATEOBJECT("wwHttp")
lcHtml = loHttp.Get("https://microsoft.com")

loBridge = CREATEOBJECT("wwDotnetBridge")
? loBridge.GetDotnetVersion()
```

Alternately you can also load any PRG libraries individually using the following syntax:

```foxpro
set procedure to wwHttp in wconnect.app
loHttp = CREATEOBJECT("wwHttp")
lcHtml = loHttp.HttpGet("https://microsoft.com")
```

Once libraries have been loaded you can use the same syntax shown for other examples (usually `do wwhttp` or `do wwdotnetbridge` which load all related dependencies) or you can create instances of classes as needed.

Note that most examples show code like this:

```foxpro
DO wwHttp   && Load Libraries

*** Instantiate
loHtml = CREATEOBJECT("wwHttp")
```

**But using the shareware version you'll need to load the library like this:**

```foxpro
DO wwClient.app   && Load all Libraries

*** Instantiate
loHtml = CREATEOBJECT("wwHttp")
```

> **Note:** Loader PRG commands in the examples like `DO wwHttp` will work **once you've run wwClient.app** to load all libraries, but will fail if you don't.

To minimize the differences it's recommended that you call `DO wwClient.app` at the start of your application. Once you register, you can switch that code to load the specific libraries that you need (ie. `DO wwHttp`). We always recommend pre-loading libraries on application startup regardless of loader mode.

### Switching to the Registered Version
If you decide to register your copy of the Client tools, the code to load libraries will be slightly different. You need to explicitly load each library on its own using `SET PROCEDURE TO`, `DO <LibraryName>.prg` or using `NEWOBJECT()` with a class library provided.

Most PRG libraries in Web Connection come with a dependency loader which lets you execute the main PRG file which automatically loads all related dependencies.

For example:

```foxpro
DO wwHTTP
```

loads the libraries needed for HTTP which includes a dependency on `wwUtils` and `wwApi` which are both loaded by the above command. See individual documentation for each class whether this is supported 

The registered version of the client tools provides all classes as source code so you can selectively include them in your application.