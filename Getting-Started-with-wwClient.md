<img src="images\logo.png" style="width: 128px;margin: 0 20px 5px 0; float: left;">

This library is designed as a **modular set of classes** that can be used individually or in combination. It's a developer product made of individual classes and support DLLs that you can integrate into distributable applications of your own. Each of the components may have a set of dependencies that are described in the relevant help topics.

To find out about the many features available your best resources is the documentation that you are viewing now and peruse class reference. The class names should give you a good idea of purpose and features that are available.

> #### @icon-warning Important: Shareware Version Differences
> The shareware version **does not ship with source code** and so how you load libraries works differently than in most examples. With the Shareware version you have to **pre-load all components via wwClient.app**:
> ```foxpro
> *** Must load all libraries for shareware version
> DO wwClient
>
> *** Use your components
> loHttp = CREATEOBJECT("wwHttp")
> ```
> More shareware version info: [Using the West Wind Client Tools with the Shareware Version](VFPS://Topic/_51F1BSWLK)

### Quick Start
Here are a few quick links for getting started with popular library components:

* [Internet Components Quick Start](VFPS://Topic/_S9001ZXI9)
* [Using wwDotnetBridge](VFPS://Topic/_24N1CFW3A)
* [Using Business Objects](VFPS://Topic/_0H009JS4A)
* [Using wwJsonSerializer](VFPS://Topic/_1WU18OWBA)

### Using the Components
Most components in this library are provided as PRG files, with only 3 class libraries provided as VCXs. Additionally some components require external DLLs for their functionality. **Code and external dependencies are noted in the help topic for each class at the end of the topic**. 

To use these components in your own application you can simply copy the dependencies into your own project folder. **Make sure you include all dependencies**. Alternately you can just reference the `wwClient` configuration path and let your project pull in what it needs from there. If you get errors that certain components cannot be found you're missing a reference.


### Loading PRG Libraries and Dependencies
All PRG based classes are self-loading, meaning that you can `DO <ClassPrgFile>.prg` to load the class and all of its dependencies. For example to load the `wwhttp` class you can do it like this:

```foxpro
DO wwhttp && load library and dependencies
...
loHttp = CREATEOBJECT("wwhttp")
```

This loads wwHttp, wwUtils and wwApi PRG files and adds them to the Procedure stack.

> #### @icon-info-circle Include `wconnect.h`
> Note that many of the PRG and VCX based components also require `wconnect.h` to be accessible in order to compile. This file contains many constants that are used throughout the various components. **Make sure `wconnect.h` can be found on the FoxPro path when compiling**.

Each of the PRG based classes includes a header section that does `SET PROCEDURE TO` itself and all of its dependencies. You can use `DO <ClassPrgFile>.prg` either **on application startup** (recommended) to do this once in one place which is most efficient, or **before every use** explicitly which adds a little overhead. Note that either approaches is considerably faster than `NEWOBJECT()`. It'll work but will be slower and won't resolve unloaded dependencies so we recommend you `DO <classfile>.prg` to get libraries loaded.

### Shareware Version has no PRG/VCX Files
Note that the shareware version doesn't ship source files and instead ships a `wconnect.app` file that you can run that loads all libraries at once by running `DO WCONNECT.APP`. You still need to make sure that the various DLLS can be found though so you still need to copy the files, or include them in your FoxPro Path.

```foxpro
*** Loads all libraries
DO wwClient  && wwclient.app

loHttp = CREATEOBJECT("wwHttp")
lcHtml = loHttp.HttpGet("https://microsoft.com")
```

For more detailed info on using the shareware version, please see:  

* [Using West Wind Client Tools with the Shareware Version](VFPS://Topic/_51F1BSWLK).


##### VCX based Components
The VCX based class libraries have to be loaded explicitly as well as any dependencies they may have. For example to load wwBusiness you'd do:

```foxpro
SET PROCEDURE TO wwUtils ADDITIVE
SET CLASSLIB to wwSql ADDITIVE

loSql = CREATEOBJECT("wwSql")
```

##### Required DLLs
Many components like wwDotnetBridge, wwSmtp, wwHttp, wwJsonSerializer have external dependencies on DLLs. If you're adding these components to your applications make sure you also add these DLLs to your project's distribution. DLLs must be discoverable in the current or FoxPro path. 

> #### @icon-warning Blocked DLLs from Zip Downloads
> If you're using any components that use DLL components like wwipstuff.dll, wwdotnetbridge.dll etc., it's possible that these files will be restricted due to Windows security policy. Any zip file downloaded from the Internet and unzipped automatically blocks DLL files. To fix you need to **unblock** the DLL files. 
>
>To do this on Windows 7-10, right click the file and click on the **Unblock** checkbox or (on Windows 7 and 8) the **Unblock** button.
>
> To avoid this problem in distributed applications make sure you use an installer that runs under Administrator rights to install DLL files.

* **wwIpStuff.dll** - Required for many components and all .NET based components exposed
* **wwDotnetBridge.dll** - Required for any component that interfaces with .NET in some way like **wwSmtp**, **MarkdownParser**, **wwJsonSerializer** and many more
* **Newtonsoft.Json.dll** - .NET JSON Parser used for parsing JSON to objects and values
* **RenciSoft.ssh.dll** - .NET component used for FTPS support
* **Markdig.dll** - .NET Markdown Parser used in **MarkdownParser**
* **dzip,dunzip** - Win32 DLLs used for various Zip functions

At minimum we recommend that your application deploys **wwipstuff.dll** and **wwdotnetbridge.dll** since these are used for many of the components. The other dlls are usage specific and should be included on an as needed basis.

### What's New
You can find out what's new in each new version released here: 

* [What's new in West Wind Internet & Client Tools](VFPS://Topic/What%27s%20new)

This topic links to all that is new as well as breaking changes from previous updates.

### The Business Object Wizard
The wwBusiness object comes with a new Business Object Wizard that lets you interactively build a new business object. To acces the Wizard:

```foxpro
DO wwClientConsole
```

which opens the Console, from which you can launch the **Business Object Wizard**.