This library is distributed as a Zip file which you can unzip and install into a folder of your choice.

> #### @icon-info-circle The Registered Version uses Password Encrypted Zip File
> The registered version is protected with a password and high encryption. You will need unzip software to unzip this file with a password as Windows cannot natively deal with the strong encryption. You can download the [free 7-Zip tool](https://www.7-zip.org/) to unzip the encrypted Zip File.

Once installed you'll want to make sure that you can access the files in the following folders either via the FoxPro path or by explicitly copying some or all of the classes into your application's folders.

* \wwclientInstall
* \wwclientInstall\Classes

Files you'll want to copy:

### Source Files
* `classes\*.prg` (Full Version)
* `wconnect.h`  (Full Version)
* `wconnect.app` (Shareware Version)

To distribute these files include and add them into your application's path, or reference them from your project. Alternately you can leave them in the install folder and reference them by using `SET PATH ... ADDITIVE` to add both the `Classes` and `root` folder of the Client Tools installation.

### Support Dlls
* `wwIpstuff.dll` (required and used for most features - Win32)
* `wwDotnetBridge.dll` (needed for all .NET support features - .NET)
* `Newtonsoft.json.dll` for JSON Parser and JsonServices (.NET)
* `Markdig.dll` for Markdown Parsing (.NET)
* `renci.ssh.net` for SFTP support (.NET)
* `zlib1.dll` required for wwHttp Gzip operations (Win32)
* `dzip.dll` and `dunzip.dll` for ZipFiles functionality (Win32)

The DLLs need to be accessible from your application via the FoxPro path, or reside in the root folder of your application. You only need to distribute those DLL that you use but you should **always** distribute `wwIpstuff.dll` as it's used by many features.

### Load Libraries
The wwClient library contains a number of components that can be used either individually or altogether. A number of components have dependencies on other components and each component has a loader you can use.

For example to use the `wwHttp` class requires dependencies on `wwApi` and `wwUtils` and they are loaded with:

```foxpro
do wwHttp  && loads all libs needed for wwHttp
```

Note: If you're using the **shareware version**, you can only load all libraries all at once by running:

```foxpro
do wwclient
```

This fires `wwclient.app` and loads all libraries into memory at once from within the `.app` file.

In the full version you can also choose to load all via `do wwclient.prg`, but for the full version it's recommended you load only what you need via `do library`. This ensures **you only add dependencies to your project that you actually use**.

### Dependency DLLs

Remember you may still need to copy DLLs. For example, `wwSmtp` relies on .NET and so uses the `wwdotnetbridge.dll`. The documentation for each class includes what dependencies are required to use it, including the required DLLs (if any) but it's safe to assume you'll need to distribute:

* `wwipstuff.dll`
* `wwDotnetBridge.dll`