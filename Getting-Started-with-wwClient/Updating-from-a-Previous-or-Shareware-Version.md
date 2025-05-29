Once you've registered the product you will get a new copy of the library files sent to you via a registration link. The registered version ships with all source code PRG files.

To update to the latest version you need to:

* Install the new files from the registered package
* Install over the old ones or delete the old files first
* Recompile all of the library code
* Delete `wwClient.app` if upgrading from the Shareware version

### Delete wwclient.app <small>(if upgrading from Shareware)</small>
In the shareware version `wwclient.app` holds all the Client Tools library files in pre-compiled and shareware mode format. In the registered version **this file is not used** and therefore it needs to be deleted. It's important to delete it because you may still run `DO wwClient` to load all libraries, but that will now trigger `wwClient.prg` to load the libraries.

To delete the file:

```foxpro
ERASE FILE wwclient.app
```

### Recompiling your Classes
After installing a new version be sure to recompile all classes and PRG files manually by using something like the following from the VFP IDE command line:

```foxpro
ERASE FILE classes\*.fxp
COMPILE classes\*.prg
```

to ensure all files are up to date when you run the code. If you've added the libraries to a project make sure that you **also recompile your project explicitly** with the `COMPILE EXE <yourApp> from <yourApp> RECOMPILE` (or in the Project dialog with the Recompile Checkbox set) to force updating of all files.

### Changing Library Loading from the Shareware Version
The shareware version uses `DO wwClient.app` to load libraries - this file is not shipped with the registered version of the Client tools and we recommend you change your code **to directly load the libraries that you want to use individually**. 

> **We recommend** you load all the libraries you actually use in an application **at the start of the application** in one place using `DO <wwClientFeature>` or `SET PROCEDURE TO <wwClientClass> ADDITIVE`, since it's so much easier to reason about versions and where and how libraries are loaded.

But... it's obviously not a requirement and you can load on demand, or use `NEWOBJECT()`. Keep in mind that in most cases `NEWOBJECT()` is **considerably slower** than `CREATEOBJECT()` on a pre-loaded library especially on many repeated instantiations.

When explicitly loading individual libraries, use code like the following by running the class PRG files directly to load dependencies:

```foxpro
DO wwHttp
DO wwSql
DO wwDotnetBridge
```

If you prefer to be more explicit you can also explicitly load libraries instead, but if you do, you're responsible for loading all required dependencies. For example, to load `wwHttp` with all of its dependencies you need to use are:
 
```foxpro
SET PROCEDURE TO wwHTTP ADDITIVE
SET PROCEDURE TO wwUtils ADDITIVE
SET PROCEDURE TO wwAPI ADDITIVE
```

as opposed to the simpler:

```foxpro
DO wwHttp
```

which does the same thing. You can always check the `.prg` file for the dependencies required.

> #### @icon-info-circle wwClient.prg: Load all Libraries
> We also ship `wwclient.prg` so if you don't want to change anything from the shareware `DO wwClient.app` you can just replace with  `DO wwClient.prg` instead, and effectively get the same behavior as the .app file from the source files. But be aware it'll pull in **all** libraries into your project when you compile, which I wouldn't recommend. It's better to only include what you actually need.

  
### Update DLL Libraries
A number of the Client Tools libraries have dependencies on various `dll` files, and these files **need to be kept in sync with the FoxPro PRG classes in each released version of the Client Tools**. 

> Although we don't frequently change the dependency relationships between DLL and PRG classes, they do change occasionally. You can find the explicit changes in the Change log in the **Breaking Changes** section of each release.   
>   
> However, the safer, recommended approach is to keep the DLLs and PRG files in sync to ensure they are a matched set.

#### Always update
These following two DLLs are specific to the tool so the version should match the `Major.Minor` Client Tools release version. You should update these with each update to the Client Tools as they are used extensively for many support features.

* wwipstuff.dll
* wwDotnetBridge.dll

#### Update based on Usage
These libraries are third party libraries. They are occasionally updated and should match the version of the files used in the Client Tools release.

* Newtonsoft.Json.dll  (any JSON features)
* Renci.SshNet.dll (wwsftp)
* Markdig.dll (MarkdownParser)

#### These Never Change
These are old libraries that will never be updated, so they don't need to be updated.

* dzip.dll
* dunzip.dll
* wwimage.dll