The Client Tools package includes quite a bit of functionality and while it's not very large, it generally not a good idea to include all the functionality that you don't use in your application. For this reason we recommend that you **selectively include features** in your own application, by picking and choosing what to install.

There are two sets of install issues that you need to address:

* Binary Dependendencies (.NET or Win32 DLLs)
* FoxPro Dependencies

> All components in this help file list their dependencies - both FoxPro and binary. You can also load dependencies automatically by `DO <Component>.prg` as components automatically *(assuming they are accessible)* which loads their dependencies via `SET PROCEDURE` or other `DO <Component>.prg` commands.

## Binary Dependencies
The Client Tools includes a number of binary support files that are used by the various FoxPro libraries. The files need to be available **on the FoxPro path** when your application runs and need to be physically available on disk when your application runs.

In other words you have to distribute these files, and they need to be kept up to do date and in sync with the FoxPro files that use them.

**Core Components - always required**

* **wwIpStuff.dll**  - a Win32 helper library used for many support functions   
This is the base library that provides a number of core helpers and is used by most tools in this library.

* **wwDotnetBridge.dll** - .NET library that provides Interop for FoxPro  
In addition to several components that use .NET (wwSmtp, wwJsonSerializer, wwFtpClient/wwSFtpClient etc.), many of the support functions in wwUtils and wwApi also rely on `wwDotnetBridge`.

**Feature Specific .NET Components** 
* **Newtonsoft.Json.dll** - JSON Parsing used for JSON Serialization and REST Services
* **FluentFtp.dll** - Used for `wwFtpClient` and FTP/FTPS functionality
* **Renci.SshNet.dll** - Used for `wwSFtpClient` and SFTP functionality
* **Markdig.dll** - Used for markdown parsing in the `MarkdownParser` class
* **dzip32/dunzip32/zlib1.dll** - Zip functions other than `ZipFolder()`\\`UnzipFolder()`

Make sure that you copy these dependencies with your application **when you deploy it**. 

> It's recommended that you create a small FoxPro program that copies the latest versions of the files you need into your project folder for distribution (or a build folder if you do a proper install).

## FoxPro Dependencies
FoxPro depdendencies are referenced exclusively from PRG files and can be compiled into your application, so you only need to make sure they are accessible during development.

There are a couple of ways you can include features:

* Reference the Library from the Install Folder
* Copy files and dependencies into your project

### Reference Libraries from the Install Folder
This is the recommended way to handle dependencies in your own projects. Rather than copying files you need into your project folder you make sure your FoxPro path includes a reference to the Client Tools install folders. You need to add two folders (depending on the install location)

```ps
SET PATH TO "c:\wwclient\classes" ADDITIVE
SET PATH TO "c:\wwclient" ADDITIVE
```

> A variation of this is to create a project specific copy of the Client Tools alongside your application. That way you can use a fixed path with a version that remains fixed if needed or can be updated wholesale from an Client Tools update. You can also check that version into source control 

You can set the path in your `config.fpw` or use an environment or application startup script to set the paths. I like to have a `_setenv.prg` file that includes the above.

Once you do this you can now access the libraries directly without any pathing:

```foxpro
*** load with dependencies
DO wwHttp

*** or explicitly
SET PROCEDURE TO wwHttp ADDITIVE
```

Your project will then automatically pull in the PRG files from the path referenced locations automatically.

> Note if you are worried about versioning, you can install the Client Tools into version specific folders `(ie. wwClient_801`) and reference each specific folder that matches the desired version. 
>
> Note that you may have to update references in your project if you version in this way as files are not adjusted once they have been added to a project.

### Don't Copy FoxPro Files into your Project
Alternately, you can add the files that you need directly to your project. This ensures you get a specific snapshot version of files, but on the downside it's more complicated to keep files updated as newer versions of the Client Tools are installed.

As a compromise you can copy the entire `wwClient` folder into your project and add a path to the `classes` and root folder to find components. This ties a version specific version that holds all dependencies. 


### Summary
One way or another the binary dependencies have to be explicitly managed - preferrably with a small copy/install build script - to ensure the correct version of the files are installed with your projects. FoxPro dependencies get compiled into your project regardless of how you reference them, but you need to choose between local copies of the files and manually managing the file versions, or directly referencing the West Wind Client library from its install folder.