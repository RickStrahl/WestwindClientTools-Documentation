### Version 8.1
<small>October 1st, 2024</small>

* **[ComValue.SetValueFromCreateGenericInstance() with no Parameters](VFPS://Topic/_5C51DGO92)**  
Simplified this method so that the parameter list can be passed as `.NULL.` rather than requiring at least an empty parameter list.

* **[wwDotnetBridge.GetLoadedAssemblies()](VFPS://Topic/_70218VS3G)**  
New method that returns all assemblies that are loaded into the current process as a collection that includes name, location and fully qualified .NET name.

* **[wwDotnetBridge.ComArrayToCollection()](VFPS://Topic/_70218V2EX)**  
A helper method that returns a FoxPro collection from a ComArray instance. 

### Version 8.0
<small>May 30th, 2024 &bull; [Release Notes](https://west-wind.com/wconnect/weblog/ShowEntry.blog?id=9177) &bull; [Breaking Changes](#breaking-changes-in-8.0)</small>

* **[FTPS Support via the wwFtpClient class](VFPS://Topic/_6WP0MRZ80)**  
Added a replacement for the `wwFtp` class, that supports FTP and FTPS. The new class uses a .NET Library - Fluent FTP - to provide FTP functionality. The new API has been simplified to just a few simple functions with clearer naming than the legacy wwFtp class.

* **[Refactored wwSftpClient Class](VFPS://Topic/_6WR0ZM6JD)**  
Added a replacement for the `wwSftp` class, that supports SFTP (FTP over SSH). This library has the same interface   and can be used interchangeably with the `wwFtpClient` class.

* **[New wwZipArchive Class](VFPS://Topic/_6WW0Y5KQQ)**  
Added a new wwZipArchive class that provides better control over zip functionality using modern, native .NET functionality that removes the old dependency on the ancient DynaZip libraries. The new class provides the ability to zip and unzip folders, files, wildcards, append and get entry listings and retrieve individual files selectively.


* **[wwDotnetBridge.GetPropertyRaw()](VFPS://Topic/_6VO0PU6U2) and [ComArray.ItemRaw()](VFPS://Topic/_6VO0P9YVG)**  
Overridden methods that allow for retrieval of property values in raw format that bypass the usual FoxPro fixups that ensure type safe values are returned to FoxPro. Useful in scenarios where the values are sometimes in ComValue or ComArray that can be accessed directly, or in scenarios where types have dual meaning (ie. char with raw number vs. string fixup or Guid with raw binary vs. string fixup).

* **[ComArray.GetInstanceTypeName()](VFPS://Topic/_6VO0OEM1H) and [ComArray.GetItemTypename()](VFPS://Topic/_6VO0OF6WL) helpers**  
Added a couple of helpers to the ComArray class to provide type information about the Array instance and it's client types for debugging or testing purposes. This can be useful to determine whether the `.Instance` member can be accessed directly via FoxPro code (many .NET collections cannot and require intermediary operations provided by ComArray or wwDotnetBridge).


* **[wwDotnetBridge.GetPropertyRaw()](VFPS://Topic/_6VO0PU6U2) and [ComArray.ItemRaw()](VFPS://Topic/_6VO0P9YVG)**  
Overridden methods that allow for retrieval of property values in raw format that bypass the usual FoxPro fixups that ensure type safe values are returned to FoxPro. Useful in scenarios where the values are sometimes in ComValue or ComArray that can be accessed directly, or in scenarios where types have dual meaning (ie. char with raw number vs. string fixup or Guid with raw binary vs. string fixup).

* **[ComArray.GetInstanceTypeName()](VFPS://Topic/_6VO0OEM1H) and [ComArray.GetItemTypename()](VFPS://Topic/_6VO0OF6WL) helpers**  
Added a couple of helpers to the ComArray class to provide type information about the Array instance and it's client types for debugging or testing purposes. This can be useful to determine whether the `.Instance` member can be accessed directly via FoxPro code (many .NET collections cannot and require intermediary operations provided by ComArray or wwDotnetBridge).

* **[wwDotnetBridge: Improved support for Task Exception Handling](VFPS://Topic/_5PJ0XL2YP)**  
When making calls to .NET `async` or `Task` methods, wwDotnetBridge now does a better job of handling exceptions and returning the result in the `OnError()` callback. More errors are handled and error messages should be more consistent with the actual error (rather than a generic error and an innerException).

* **Updated to latest .NET `Newtonsoft.Json` Library for Json Serialization**  
We've updated the .NET JSON parsing library to ensure continued compatibility with the latest .NET versions widely used. It's been a while since the last update, and `v13.0.3` has been in circulation now for quite a while and this gets us up to date with the latest performance and parsing improvements of Newtonsoft's JSON library. 
**This is a potentially breaking change: Make sure to update the `Newtonsoft.json.dll` and `wwDotnetBridge.dll` in your projects.**

* **wwJsonServiceClient CallService Url Fix up**  
You can now use a site relative URL when calling `CallService()` rather than requiring a fully qualified URL. You can use Urls like `/authenticate` which makes it easier to switch between different host sites. If the URL does not start with `http://` or `http://`, the `cServiceBaseUrl` is prepended to the URL to create a full URL. This is useful if you switch between different sites for example when running against dev, staging and production servers. 

* **wwJsonServiceClient: Optionally capture Request and Response Data** 
You can now optionally capture all request and response data via the `lSaveRequestData` flag. If set any POSTed JSON data will be capture in `cRequestData` and any result data is capture in `cResponseData` both of which are useful for debugging.

* **wwJsonServiceClient is abstracted into its own PRG File**  
wwJsonServiceClient now has migrated out of the `wwJsonSerializer.prg` file to its own `wwJsonServiceClient.prg` file. This is a minor breaking change - you'll need to make sure `DO wwJsonServiceClient` is called explicitly now to ensure the library is loaded along with all dependencies.

* **Fix: Stop wwDotnetBridge from Attempting to Serialize Non-FoxPro Objects**  
wwDotnetBridge now checks for COM objects and components and won't serialize those objects rather than attempting and failing. FoxPro's `AMEMBERS()` can't reliably retrieve properties from COM objects so rather than getting an incomplete object, or worse a crash due to non-properties being access, we now serialize `null` for COM objects.

#### Breaking Changes in 8.0{style='color: firebrick'}

* **wwFtp::OnFtpBufferUpdate() and wwSFtp::OnBufferUpdate()**  
The signatures for the FTP events handler for the **wwFtp** and **wwSFtp** classes have changed using a unified interface that now works the same both for the old and new FTP classes. If you are using these event handlers with wwFtp or wwSFtp make sure that update to the new signatures.

* **Make sure to update  DLLs**  
All DLL files have updated versions and potentially breaking changes if not synced to the current versions of their matching PRG files:
    * wwipstuff.dll
    * wwDotnetBridge.dll
    * Newtonsoft.Json.dll  
    * FluentFtp.dll   (for wwFtpClient)
    * Renci.SshNet.dll  (for wwSFtpClient)


### Version 7.33

<small>June 12th, 2023</small>

* **[wwDotnetBridge: Improved support for Task Exception Handling](VFPS://Topic/_5PJ0XL2YP)** 
When making calls to .NET `async` or `Task` methods, wwDotnetBridge now does a better job of handling exceptions and returning the result in the `OnError()` callback. More errors are handled and error messages should be more consistent with the actual error (rather than a generic error and an innerException).

* **Updated to latest .NET `Newtonsoft.Json` Library for Json Serialization**  
We've updated the .NET JSON parsing library to ensure continued compatibility with the latest .NET versions widely used. It's been a while since the last update, and `v13.0.3` has been in circulation now for quite a while and this gets us up to date with the latest performance and parsing improvements of Newtonsoft's JSON library. 
**This is a potentially breaking change: Make sure to update the `Newtonsoft.json.dll` and `wwDotnetBridge.dll` in your projects.**


* **wwJsonSerializer no longer uses PropertyExclusionList on EMPTY Object**  
When serializing `EMPTY` objects, or by association cursors and collections which internally use `EMPTY` objects, the `PropertyExclusionList` is not applied to properties. The list is meant to keep FoxPro default properties from polluting the output JSON, but EMPTY objects do not have any base properties, so the list is not necessary. This allows for creating properties with reserved FoxPro property names like `Comment`, `Name`, `Classname` etc.

* **Fix: wwJsonSerializer::AssumeUtcDates Output still converting to Local**  
Fixed issue that when this flag was set, it would not convert the inbound date from local to UTC but use the current date as UTC, but it would still convert the date back to local when deserializing. This change now leaves the deserialized date in the original UTC time, but returns it as a local FoxPro time (ie. the date is not adjusted for timezone) which was the original assumption of this flag. This was broken when we switched from FoxPro based parsing to .NET parsing using JSON.NET. **This is a potentially breaking change if you used this obscure flag in your code**.

* **Fix: Stop wwDotnetBridge from Attempting to Serialize Non-FoxPro Objects**  
wwDotnetBridge now checks for COM objects and components and won't serialize those objects rather than attempting and failing. FoxPro's `AMEMBERS()` can't reliably retrieve properties from COM objects so rather than getting an incomplete object, or worse a crash due to non-properties being access, we now serialize `null` for COM objects.

#### Breaking Changes in 7.33{style='color: firebrick'}

* **Make sure to update `Newtonsoft.Json.dll` and `wwDotnetBridge.dll` in your Projects**  
The two dlls are linked to each other and so they always need to stay in sync in order for JSON serialization to work consistently. Needed if you use JSON deserialization using `wwJsonSerializer`, or you're building API Web projects.

### Version 7.31

<small>January 14th, 2023</small>

* **[wwDotnetBridge COM Array Support for Lists, Dictionaries, Collections](VFPS://Topic/Class%20ComArray)**  
Added support for direct access to all sorts of collections directly in `ComArray` instances which are returned for collection results from `InvokeMethod()`, and `GetProperty()` or when creating lists. You can now access `List<T>` and `Dictionary<TKey, TValue>` collections directly with ComArray, via `Item()`, `AddItem()`, `RemoveItem()` etc. All methods have been updated to work with `IList`, `IDictionary`, `ICollection`, `Array` and `IEnumerable` with support for numeric indexes or key or value lookups.   
**Note:** This slightly changes behavior of working against collections and lists as now ComArray is returned. However,  you can still access `ComArray.Instance` to get the raw instance, and `ComArray` is now auto converted to the `Instance` when passing as the base instance for intrinsic commands.

* **Auto-Fixup of ComArray for Instance Parameters in all Intrinsic Methods**  
You can now pass ComArray instances directly as the first `loInstance` parameter for most intrinsic invocation methods like `InvokeMethod()`, `GetProperty()`, `SetProperty()` etc. Thanks to [Christof Wollenhaupt](https://github.com/cwollenhaupt) for this improvement and bug fix around a regression error related to the new list and dictionary support. [#27](https://github.com/RickStrahl/wwDotnetBridge/issues/27)

* **ComArray.Add() method to complement AddItem()**  
There's now a `ComArray.Add()` that performs the same task as AddItem but provides better native compatibility with .NET collection syntax (ie. `List.Add(loItem)`). Note that this method only supports a single parameter used for Lists and non-dictionary collections. For dictionaries with keys use `AddDictionaryItem()` to an item with a key.

* **Improved Support for .NET Core in wwDotnetBridgeCore**  
Improved runtime discovery logic for wwDotnetCoreBridge. Fixed `GetDotnetVersion()` to show .NET Core version.

* **wwDotnetBridge: [GetwwDotnetCoreBridge()](VFPS://Topic/_6G50UTL8E) and [InitializewwDotnetCoreBridge()](VFPS://Topic/_4X60SEGBF)**  
Added these helper functions to load the .NET Core Runtime of wwDotnetBridge and caching the runtime in the same way that the Full Framework versions of these functions do.

### Version 7.29

<small>October 14th, 2022</small>

* **[wwJsonSerializer::MapPropertyName()](VFPS://Topic/_67X0KWDFD)**  
Method that allows you to map an individual property name to a new name. This allows you to map names to values that standard FoxPro property serialization would not normally allow for such as property names with spaces or special characters.  Similar to `PropertyNameOverrides` in behavior but with more control.

* **Added [wwJsonSerializer::DefaultEmptyDate Property](VFPS://Topic/_6920VLWCN)**  
Added `DefaultEmptyDate` property to wwJsonSerializer to allow you set a predefined date value for empty dates since JSON doesn't have the notion of the *empty date*. The default date uses JavaScript's default zero date which `1970-1-1` which was the previous value set by default and couldn't be overridden.

* **Fix: wwJsonService UTF-8 Encoding/Decoding**   
Fixed inconsistencies in UTF-8 encoding by the service client. Now data sent is encoded and data received is decoded. Optional parameters allow disabling this auto en/decoding.

### Version 7.27

<small>April 19th, 2022</small>

* **[Automatically clear wwSql Named Parameters before new command executes](VFPS://Topic/_1EY05DC86)**  
Automatically clear SQL parameters set on the last command, before executing the next command, unless `lNoParameterReset` is explicitly set to avoid this. This removes the need to explicitly clear parameters when running multiple commands on a single instance/connection. This was previously implemented but lacked the `lNoParameterReset` flag to allow to keep parameters for multiple commands for unusual use cases where the same command is run many times in a row. Note parameters are cleared **before the next command runs** not when the original command has completed, so you can review parameters and also receive out parameters.

* **[wwJsonSerializer::MapPropertyName()](VFPS://Topic/_67X0KWDFD)**  
Method that allows you to map an individual property name to a new name. This allows you to map names to values that standard FoxPro property serialization would not normally allow for such as property names with spaces or special characters.  Similar to `PropertyNameOverrides` in behavior but with more control.

* **Automatically clear wwSql Named Parameters before new command executes**  
Automatically clear SQL parameters set on the last command, before executing the next command, unless `lNoParameterReset` is explicitly set to avoid this. This removes the need to explicitly clear parameters when running multiple commands on a single instance/connection. This was previously implemented but lacked the `lNoParameterReset` flag to allow to keep parameters for multiple commands for unusual use cases where the same command is run many times in a row. Note parameters are cleared **before the next command runs** not when the original command has completed, so you can review parameters and also receive out parameters.

* **Improved wwSmtp Error Reporting (.NET mode)**  
wwSmtp now reports error information using the base exception rather than the top level exception which provides more info for connection level errors such as socket connect failures or invalid connection information/domain names.

* **[Improved access to HTTP and Serializer functionality in wwJsonServiceClient](VFPS://Topic/_4JF1F19ZR)**  
Refactored the `wwJsonServiceClient` to always create the the `.oHttp` and `.oSerializer` properties on startup. These objects allow customization of the HTTP request and JSON Serializer respectively. A new `AddHeader()` method is a shortcut method to make HTTP Header access more discoverable.

* **[wwEncryption::ComputeHash()](VFPS://Topic/_4C10W1PS4) BinHexMode parameter**  
Added a parameter that lets you specify the output format as **binHex** optionally which is a double character representation of each byte in the result hash. The default format is **base64** and the `llUseBinHex` flag optionally overrides to let you use **binHex**. If the parameter is not specificied the global `SetBinHexMode()` setting is used instead.

* **Refactored wwEncryption::[EncryptString()](VFPS://Topic/_4C10W1PRR)/[DecryptString()](VFPS://Topic/_4C10W1PRX)**  
These methods now let you choose the crypto provider (TripleDES and AES), the cipher mode, IV (for various ciphers) and optional key hashing modes. You can also set the binHex mode optionally.

* **wwUtils: ShowJson() Function**  
Added a new `ShowJson()` function in wwUtils that shows the content of the string in the configured JSON editor (typically VS Code). Also update the ShowHtml, ShowXml methods to output UTF-8 files that are then rendered from disk to avoid garbage characters otherwise shown.

#### Breaking Changes in 7.27 {style='color: firebrick'}

* **wwJsonServiceClient fixes UTF-8 decoding by default**  
There was a bug that caused `wwJsonServiceClient` to not decode UTF-8 content automatically, which would result in extended characters in deserialized values and objects to be misformatted. In 7.27 content now is UTF-8 decoded by default and you can disable the auto-decoding via the `lNoUtfDecoding=.T.` to keep the old behavior.

* **NewtonSoft.Json.dll needs to be Updated**  
We've updated the .NET JSON serializer to the latest version `13.0.1` in Web Connection and if you're using the REST API Process classes or using the JSON Serializer in any way make sure to update this DLL in your application folder or `DeserializeJson()` might fail.


### Version 7.23 

<small>August 16th, 2021</small>

* **[wwEncryption::ComputeHash()](VFPS://Topic/_4C10W1PS4) BinHexMode parameter**  
Added a parameter that lets you specify the output format as **binHex** optionally which is a double character representation of each byte in the result hash. The default format is **base64** and the `llUseBinHex` flag optionally overrides to let you use **binHex**. If the parameter is not specificied the global `SetBinHexMode()` setting is used instead.

* **Refactored wwEncryption::[EncryptString()](VFPS://Topic/_4C10W1PRR)/[DecryptString()](VFPS://Topic/_4C10W1PRX)**  
These methods now let you choose the crypto provider (TripleDES and AES), the cipher mode, IV (for various ciphers) and optional key hashing modes. You can also set the binHex mode optionally.

* **Add support for AES Encryption in wwEncyrption Class**  
You can now use AES encryption optionally instead of the default TripleDES encryption previously supported for the `EncryptString()` and `DecryptString()` methods. You can now also create encrypted content that **does not** encode the provided encryptionKey with MD5 (as is common) and instead uses the raw encryption key to allow matching encrypted content generated by tools other than this method.

* **[wwJsonSerializer::DeserializeCursor()](VFPS://Topic/_3N51EFR1D)**  
New helper function that wraps `Deserialize()` and `CollectionToCursor()` which deserializes into a cursor/table that's already open to provide the data structure to import to.

* **Local User Credentials for SMTP Emails**  
Added support for a special `LOCALACCOUNT` username value that can be used to specify to use the logged on user's Windows credentials for SMTP server access. Note this is unusual and typically only works for internal servers running Exchange or other Managed Mail platforms using Windows Domain Accounts for access.

* **New wwHttp::AddPostFile() Method**  
Added an explicit method that lets you add File Upload POST data for multi-part form based uploads. This is just a specialized version of the `AddPostKey()` method provided to make it explicit when uploading a file as part of form POST operation and to simplify the documentation.

* **Fix: Large Number JSON Result Values**  
[Fixed issue](https://support.west-wind.com/Thread5W1099FY1.wwt#5W20W5Y0Q) with large number rounding errors that would break JSON serialization. Caused by numeric overflows in FoxPro's value to string conversions, fixed by explicitly rounding down numbers to the `SET DECIMAL` setting.

### Version 7.15
<small>December 1st, 2020 &bull; [Breaking Changes](#v7015breakingchanges)</small>

* **wwDotnetBridge support for non-Property Indexers in GetPropertyEx()**  
You can now access Indexed properties (`[0]` or `["name"]`) without requiring a property as part of the property name. So you can now do: `loBridge.GetValueEx(loDataRow,'["name"]')` for example where `loDataRow` is an array or collection.

* **Fix: Large Number JSON Result Values**  
[Fixed issue](https://support.west-wind.com/Thread5W1099FY1.wwt#5W20W5Y0Q) with large number rounding errors that would break JSON serialization. Caused by numeric overflows in FoxPro's value to string conversions, fixed by explicitly rounding down numbers to the `SET DECIMAL` setting.

* **Fix: JSON Date Formatting Rounding Issue**  
Fixed a bug where dates from cursors would not show the exact second/millisecond format with a float rounding error. Reworked the Json conversion function to adjust date explicitly to UTC format and then using `TTOC(ldDate,3)` instead.


#### @icon-warning Breaking Changes for v7.15
<a name="v7015breakingchanges"></a>

* **Update wwDotnetBridge.dll**  
There have been changes in `wwDotnetBridge.dll` and the `wwDotnetBridge` class that require that the versions are synced up. If you update your code make sure you also update `wwDotnetBridge.dll`.

### Version 7.14
<small>June 8th, 2020 &bull; [Breaking Changes](#v7014breakingchanges)</small>

* **[New wwDotnetBridge.InvokeTaskMethodAsync() to call Async .NET Methods](VFPS://Topic/wwDotNetBridge.InvokeTaskMethodAsync)**  
Added a new method to make it easier to call `async` .NET methods that return `Task` or `Task<T>` results. This method takes a callback handler object reference that is called on completion or failure of the method call. Similar to `InvokeMethodAsync()` which calls non-async methods asynchronously but natively intercepts the `Task` result and fires events on completion.

* **wwDotnetBridge Instances can now by ComValue instances**  
You can now pass `ComValue` instances to `InvokeMethod()`, `SetProperty()` and `GetProperty()` so that you can invoke functionality on objects that are otherwise not accessible in FoxPro. When you pass `ComValue` for the `loInstance` parameter, wwDotnetBridge automatically uses the `Value` property.

* **wwDotnetBridge Constructor Support for Structs**  
Fix ability to instantiate .NET `struct` types which now are returned as `ComValue` instances.

* **[wwDotnetBridge::ToString()](VFPS://Topic/wwDotNetBridge%3A%3AToString)**  
Added a helper method to reliably return the result from .NET's internal `.ToString()` method. `ToString()` tends to be overloaded and so is not directly accessible on most via COM interop. This helper ensures it'll work on all .NET types.

* **Fix: wwDotnetBridge Guid Results and Properties**  
Changed behavior of wwDotnetBridge methods that return Guid values or accss Guid Properties. Guids are value types and fixed up by wwDotnetBridge, but previously used the obsolete `ComGuid` type. This has been changed to use the standard `ComValue` type and the `GetGuid()` method. So a method that returns a Guid returns a `ComValue`, as does a Guid property accessed with `GetProperty()`.

* **[wwUtils `ExecuteCommandLine()`](VFPS://Topic/wwUtils%3A%3AExecuteCommandLine)**  
Added a new command line execution command that makes it easier to execute external executables using ShellExecute. Simplified syntax to take a single command line as string. Supports OS path resolution also works with Application Protocols.

* **[wwUtils IsDotnetCore() Helper](VFPS://Topic/wwUtils%3A%3AIsDotnetCore)**  
Support function that checks if .NET Core is installed. Returns the highest version number.

* **`SftpFile::IsDirectory` Property**  
Added a new `IsDirectory` property to the SftpFile object returned in the `sftp::FtpListFiles()` method to simplify figuring out whether an entry is a file or directory.

* **Fix: wwJsonSerializer JSON Date Formatting Rounding Issue**  
Fixed a bug where dates from cursors would not show the exact second/millisecond format with a float rounding error. Reworked the Json conversion function to adjust date explicitly to UTC format with additional .NET fixup to normalize the date value.

#### @icon-warning Breaking Changes for v7.14
<a name="v7014breakingchanges"></a>

* **Update wwDotnetBridge.dll**  
There have been changes in `wwDotnetBridge.dll` and the `wwDotnetBridge` class that require that the versions are synced up. If you update your code make sure you also update `wwDotnetBridge.dll`.


### Version 7.07
<small>July 1st, 2019</small>

* **Fix JSON Handling for Large Strings and JSON Buffers**  
JSON size is limited by the FoxPro 16 meg string limit, but due to the inefficiencies of JSON generation presizing a buffer the JSON string parser uses an overly pessimistic pre-sized buffer to hold JSON text that is passed to internal APIs. Changed buffer management so that the buffer now gets close to the 16 meg string limit. If the buffer is greater than 16 megs you will still get an error.

* **[wwUtils FixComErrorMessage()](VFPS://Topic/_5GM0TMZD8)**  
Simple helper function that strips the Ole Dispatch error prefix from the front of a COM error message leaving just the actual error message text.

* **wwHttp now supports String Downloads > 16mb**  
You can now download data larger than 16mb into strings. FoxPro has a 16mb string limit but with careful usage you can access strings much larger than 16mb as long as the strings are not updated. wwHttp has always supported downloading larger files directly to file and this is just another option that works by default.

* **wwHttp large File Download Speed Improvement**  
Changed the wwHttp buffer sizing behavior to dynamically size the buffer depending on the download size. Larger buffers drastically improve download performance of large HTTP payloads easily improving 5 fold for megabyte sized downloads.

* **Add Option to not Allow Html in Markdown Parser and Function**  
The `Markdown()` function and `MarkdownParser` class now have options to disallow HTML markup inside of any Markdown parsed. This can be useful to cut down on possible XSS attack vectors that go beyond the XSS prevention already built into the Markdown parsing process.

* **wwSFTP Client now fails if renci.ssh.dll is missing**  
wwSFTP now immediately fails if the SSH library used for the SFTP connection cannot be found on startup. Previously this resulted `Undefined COM Error` on sending - you now get a specific warning that points at the missing assembly.

* **wwApi ZipFolder() and UnzipFolder()**  
.NET based folder zip functions that work of source and target folders on disk. Basic zipping and unzipping features only but handles special files and long file names better than the old `ZipFiles()` and `UnzipFiles()` functions.

* **Add support for Binary Data to wwXml::XmlToObject/ObjectToXml**  
wwXML can now serialize and deserialize binary data (of type `BLOB` or `Q`) to and from XML using base64 serialization.

* **[wwSql.nTimeout Property](VFPS://Topic/wwsql%3A%3AnTimeout)**  
Allows you to set the SQL Connection timeout globally before a connection is made.

* **wwApi ZipFolder() and UnzipFolder()**  
.NET based folder zip functions that work of source and target folders on disk. Basic zipping and unzipping features only but handles special files and long file names better than the old `ZipFiles()` and `UnzipFiles()` functions.

* **Add support for Binary Data to wwXml::XmlToObject/ObjectToXml**  
wwXML can now serialize and deserialize binary data (of type `BLOB` or `Q`) to and from XML using base64 serialization.

* **[wwSql.nTimeout Property](VFPS://Topic/wwsql%3A%3AnTimeout)**  
Allows you to set the SQL Connection timeout globally before a connection is made.

### Version 7.0
<small>@icon-clock-o February 9th, 2019</small>

* **[New wwDynamic Class](VFPS://Topic/_5CK0PF06K)**  
Added a new dynamic object class that allows for creating or extending objects with dynamic properties that are created as you reference them. Ideal in scenarios where you need to return 'tuple' like object results from methods or where you dynamically need to build up object structures in user code.

* **wwSql and wwXml Classes now are PRG classes**  
We've switched wwSql and wwXml to run as PRG classes in order to simplify distribution and compilation. You will need to update your project references to the PRG files and any declarations from `SET CLASSLIB TO` to `SET PROCEDURE TO`. You should just be able to do **Replace in Files** in your Favorite editor or explicitly use [GoFish](https://github.com/mattslay/GoFish) to find and fix up any references to **wwSql** or **wwXml**.

* **wwBusinessObject replaces wwBusiness as a PRG class**  
We've replaced `wwBusiness.vcx` with a PRG based version called `wwBusinessObject.prg`. Unlike the wwSql and wwXml classes, we've explicitly renamed the PRG and class to `wwBusinessObject`. The old version will still be available in the `OldClasses` folder for those that need visual classes but any new development will be made only on `wwBusinessObject`. To replace search for any references to `SET CLASSLIB TO wwBusiness` and replace with `SET PROCEDURE TO wwBusinessObject`. Find any class references to `AS wwBusiness` and replace with `AS wwBusinessObject`. If you are using a VCX based subclasses of `wwBusiness` you have to continue using old wwBusiness.vcx **unless you explicitly migrate the visual class to a code class** - you can do that easily with the VFP Class Browser export feature. I suspect the same is true for most applications. 

* **Old VCX Files are still shipped in `classes\oldclasses`**  
Although we don't recommend that you use the old VCX classes that have been migrated to PRGs, you can still use them instead of the new PRG based classes. To do so just change your declaration from `SET PROCEDURE TO` to `SET CLASSLIB TO`. Note that the  `wwBusiness` class name has changed to `wwBusinessObject`.

* **[wwHttp::AddPostKey()](VFPS://Topic/wwHTTP%3A%3AAddPostkey) tcExtraHeaders Parameter**  
Added a `tcExtraHeaders` parameter to `AddPostKey()` that allows specify extra HTTP headers for multi-part form values posted.

* **wwHttp .Get(), .Post(), .Put(), .Delete() Methods**  
Added wrapper methods for specific Http Verb operations for clarity. `HttpGet()` was always a misnomer for non `GET` operations and these functions are meant purely for code clarity. They have the same parameters as the `HttpGet()` method.

* **wwHttp.GetCertificates()**  
You can now get a list of TLS certificates installed on the local machine to potentially pass to requests.


* **[wwHttp::lDecodeUtf8](VFPS://Topic/wwHTTP%3A%3AlDecodeUtf8) to automatically decode UTF-8 Content**  
Added a new wwHttp::lDecodeUtf8 property that is `.t.` by default to decode UTF-8 content based on the `Content-Type` header.

* **<a href="Class wwCollection" target="top">wwCollection Enhancements</a>**   
Added `Find()` method to `wwCollection` to allow for finding elements by their value. You can now search any type of element including objects. New `Set()` property allows setting a value if it exists or add a new one if it doesn't. A new `RequiresUniqueItems` property allows collections that can't have duplicate values. The `Item()` method fixes a few scenarios where non-matched items would fail and now return null.

* **wwUtils [GetRelativePath()](VFPS://Topic/wwUtils%3A%3AGetRelativePath) Function**  
Added a new function that returns a relative path. Unlike `SYS(2014)` this function returns the path in proper case. 

* **wwUtils [SaveFileDialog()](VFPS://Topic/wwUtils%3A%3ASaveFileDialog) and [OpenFileDialog()](VFPS://Topic/wwUtils%3A%3AOpenFileDialog) Functions**  
Save and Open file dialogs that unlike `PUTFILE()` and `GETFILE()` return the filename and path in proper case so files can be saved and retrieved by their actual on disk filenames.

* **[wwUtils JsonBool()](VFPS://Topic/_5E50P8RWV) Function to embed JavaScript Boolean Values**   
Rounds out routines to write out Json values as string in addition to `JsonString()` and `JsonDate()`. 

* **All Web Connection DLLs and EXEs are now digitally signed**  
Web Connection is now built with a proper build process that builds the entire product in an automated fashion. Previously some parts were using this automated process but it has now been fully automated. As part of that process all West Wind DLLs and EXEs and Setup files are signed with a digital signature to allow verifying authenticity. This will also make it less 'scary' to run the installer for the first time if Windows SmartScreen is is enabled.

* **wwUserSecurity Password Encryption Improvements**  
wwUserSecurity will now automatically encrypt a password that's not already encrypted if `cPasswordEncryptionKey` is set to encrypt passwords. You can now add a non-encrypted password into the file, and next time the user is authenticated the password will be encrypted and written into the database in encrypted form.

* **wwUtils.SanitizeHtml() to sanitize HTML strings**
Added a new SanitizeHtml() function to wwUtils to strip scriptable code out of HTML text. Useful for cleaning up user captured HTML or Markdown text to prevent XSS attacks from user input. Requires .NET.

* **[wwPdfPrinterDriver PDF Printer for wwPDF](VFPS://Topic/_5AS0URRCN)**  
Added a generic PDF driver that can be used with any printer driver that outputs to PDF including the built-in **Microsoft Print to PDF** driver in Windows 10 and Windows Server 2016 and later. This driver handles report printing, capturing and waiting for the output file from the print spooler and dealing with unattended mode issues when running in COM modes.


### Version 6.22
<small>@icon-clock-o September 24th, 2018</small>

* **wwDotnetBridge - Automatically unblock DLL**  
Added logic to automatically unblock `wwDotnetBridge.dll`. This should help with many common loader error issues.

* **Improved Error Message for wwDotnetBridge Load Errors**  
wwDotnetBridge has a few rules required in order to run it - specifically related to Internet downloaded-loaded DLLs and loading from a network. Updated the loader error message with common causes and link to documentation.

* **wwUtils.SanitizeHtml() to sanitize HTML strings**
Added a new SanitizeHtml() function to wwUtils to strip scriptable code out of HTML text. Useful for cleaning up user captured HTML or Markdown text to prevent XSS attacks from user input. Requires .NET.

* **[wwDotnetBridge support for Event Handling](VFPS://Topic/_57Q019PW0)**  
wwDotnetBridge now supports subscribing to and handling of events via the new `loBridge.SubscribeToEvents()` method. You provide a source .NET object that fires events and a FoxPro object that implements the corresponding event methods that you want to implement. Thanks to Edward Brey who built out this feature in the OSS version of wwDotnetBridge!

* **wwDotnetBridge Fixup for List<T> return values**  
Added additional support for `List<T>` return values to `InvokeMethod()` and `GetProperty()` results, which returns these values as a ComArray now.

* **wwDotnetBridge.dll and wwIpstuff.dll are now signed**   
Both wwDotnetBridge and wwipstuff are now signed with a code certificate. To make sure you have authentic versions you should check for the digital signature. Signed assemblies also have fewer security issues in terms of loading from network or downloaded sources.

* **Support >16meg uploads with wwHttp**  
You can now upload files larger than 16mb with wwhttp by using the new `lUseLargePostBuffer` flag and setting it to `.T.`. Internally this class uses  a file stream buffer to hold the POST buffer.

* **[Updated wwFileStream Class to create >16mb Strings](VFPS://Topic/_56G0MO1OA)**  
The wwFileStream class provides a file based stream that allows creation of large strings that exceed 16mb and can be loaded into FoxPro strings as a whole. FoxPro supports >16mb strings in limited fashion (basically >16mb string can't be mutated), and this class allows you to create those strings.

* **wwJsonSerializer::Serialize() llFormat Option**  
The `Serialize()` method now has an `llFormat` parameter that allows specifying that the content should be pretty formatted. This is in addition to the `FormattedOutput` flag on the serializer itself.

* **Fix: File2Var() UTF-8 Detection**  
Fixed bug in File2Var() that would cause problems with UTF-8 BOM detection in non-Western alphabets.

* **Fix: wwEncryption with Upper ASCII Characters**  
Fixed issue with wwEncryption not properly two way decrypting upper ASCII characters as the Code Page mapping was off. Switched to UTF-8 character sets when encoding and decoding which works on all ANSI codepages as long as the same code page is used to encode and decode.

* **JsonDate() function in wwUtils**  
Added a `JsonDate()` function that creates a JSON formatted date string that is properly UTC adjusted. Function lives in wwUtils but requires wwDotnetBridge/.NET to work.

* **Fix: wwJsonSerializer UTC Formatting**  
Fixed problem with the wwJsonSerializer where UTC dates weren't properly showing time zones other then current time zone.

* **Fix: wwJsonSerializer Deserialization Property Names**  
FoxPro supports only alpha-numeric plus `_` for property names and this filter ensures that deserialized properties are cleaned up to remove all but those characters via a new `PropertyNameCharacterFilter` property.

<div class="updatenotice" style="margin-left: 0;margin-right">

#### @icon-warning Breaking Changes for v6.22
<a name="v615breakingchanges"></a>

* **wwJsonSerializer::Serialize() now relies on .NET**  
The wwJsonSerializer now uses .NET functions to serialize date values with proper UTC conversions that previously weren't handled. `wwJsonSerializer::DeserializeJson()` already depended on .NET so not a big change.

</div>

### Version 6.17
<small>@icon-clock-o October 20th, 2017</small>

* **Better Error information for wwDotnetBridge Load Errors**  
wwDotnetBridge will now report slightly better error information on load failures, when failing to load the runtime or initial wwdotnetbridge instance.

* **JsonSerialize() and JsonDeserialize() Helper Function**  
Added helper functions that let you use the JSON serializer without having to create a new object instance of wwJsonSerializer for each serialization process operation. The helpers simply wrap the `Serialize()` and `DeserializeJson()` methods for easier usage.

* **[wwDotnetBridge InitializeDotnetVersion()](VFPS://Topic/wwDotNetBridge%3A%3AInitializeDotnetVersion)**  
An explicit function to initialize the .NET Runtime for the entire application. Although this function doesn't really do anything but create an instance of wwDotnetBridge with a specific version, the function makes it clear that this initializes the .NET version for the entire application.

* **Updated GetUniqueId() to be more Random**  
Changed the algorithm of GetUniqueId() using Guids that are redistributed over printable characters. The result is truly random Ids that are more unique with much smaller character counts - as little as 8 characters can provide safe ids for low impact applications with a max of 16 for near 100% fidelity.

* **Add content type to wwHttp::AddPostKey() when uploading files**  
You can now specify an optional content type when uploading files to a server using `.nHttpPostMode=2`. The content type is appended to the file data sent to the server.

* **Fix: HMACSHA Hashing in wwEncryption**   
Fixed a salt encoding bug in the `ComputHash()` function when calling the HMAC specific methods that require a salt value. Previously the salt value were added to the original string and then encoded. Now the salt value is only applied to the HMAC algorithm processor.

* **Fix: wwSFTP Large File Upload**  
Fixed an issue due to a buffer size issue with the underlying SSH library used in wwSFTP. Fixed by ignoring the `nBufferSize` parameter. Make sure to update `renci.ssh.net` dependency dll which has the fix.

<div class="updatenotice" style="margin-left: 0;margin-right">

#### @icon-warning Breaking Changes for v6.10
<a name="v610breakingchanges"></a>

* **wwDotnetBridge recompiled to .NET 4.0**   
wwDotnetBridge now runs as a .NET 4.0 assembly and **requires .NET 4.0 runtime**. We are also shipping wwdotnetbridge_NET20.dll that can be still used with .NET 2.0. The 2.0 assembly will not work for JSON Deserialization or SFTP requests. If trying to run under XP make sure to use the NET20 dll by renaming.

</div>

### Version 6.10
<small>@icon-clock-o February 2nd, 2017  
<a href="http://west-wind.com/wconnect/weblog/showentry.blog?id=925" target="top">Release Blog Post</a></small>

* **< %: expr % > for Templates and Scripts**  
Scripts and templates can now use automatic HTML encoding by using `< %: expr % >` instead of `< %= EncodeHtml(expr) % >` (spacing added for rendering). This makes it much easier to create XSS safe string output without more verbose syntax. 

* **<a href="https://marketplace.visualstudio.com/items?itemname=rickstrahl.westwindwebconnectionvisualstudioadd-in" target="top">Visual Studio 2017 Addin Support and Visual Studio Gallery</a>**   
The Web Connection Visual Studio Addin now supports Visual Studio 2017 and is also published to the Visual Studio Extension Gallery. This means you can install and update the addin directly from Visual Studio and once installed, the extension can automatically update itself.


* **[New wwSFTP Class for SSH based FTP](VFPS://Topic/Class%20wwSFTP)**  
There's a new wwSFTP class that provides secure FTP (SFTP only, not FTPS) to connect to SSH based SFTP servers. The class inherits from wwFTP so can be interchangeably with wwFtp based FTP code.

* **[wwUserSecurity Password Encryption via cPasswordEncryptionKey](VFPS://Topic/_4U605502P)**  
You can now encrypt your UserSercurity table's passwords by setting the `cPasswordEncryptionKey` when you create your wwUserSecurity instance. When set passwords are automatically hashed before getting saved to the database.

* **[wwEncryption::ComputeHash()](VFPS://Topic/wwEncryption%3A%3AComputeHash) adds HMAC Hash Algorithms**  
wwEncryption::ComputeHash() adds HMACSHA1, HMACSHA256, HMACSHA384 and HMACSHA512 algorithms. HMAC algorithms use complex salting cycles to add complexity and delay to generated hashes using an industry standard and repeatable algorithm.

* **[wwRequest::GetLogicalPath() can return Proxied Urls](VFPS://Topic/_S850QHU25)**  
GetLogicalPath() now gets a parameter that when true will return the original URL specified before a proxy redirected the URL to your application. Useful when using extensionless URLs redirected via Rewrite rules in IIS.

* **Switch to Markdig Markdown Parser**   
Switch markdown parser to use the <a href="https://github.com/lunet-io/markdig" target="top">MarkDig .NET Markdown parser</a> which natively supports Github flavored markdown and many Markdown extensions, including support for auto-links, extra emphasis (strikeout, sub/superscript etc.)

* **wwUtils::GetUniqueId()**  
Allows generation of unique IDs based off a full or partial GUID value. Allows specification of a size that determines the size of the generated string between 15 and 32 characters where 32 is a full GUID.

* **wwUtils::SplitString()**  
String utility like ALINES() to return a collection of strings that allows you to specify a length to break lines at. Uses ALINES[] to capture lines and splits lines longer than specified via MEMLINES(). Useful for any code parsing that needs to ensure string literals don't exceed 255 characters in length.

* **wwDotnetBridge now supports TLS 1.2**   
wwDotnetBridge now sets the default HTTPS protocols settings to allow for SSL3, TLS1, TLS11 and TLS12. The default previously only allowed for SSL3 and TLS1. Overrides `ServicePointManager.SecurityProtocol` on initial load of the .NET runtime. This affects any .NET API called that uses HTTP including Web service calls.

* **wwScripting::CompileAspScript**  
Explicit function that can be used to compile a single ASP Script page using the same mechanism used to run the page. 

* **WCSCompile for pre-compiling Scripts on the Server**   Fixed the pre-compilation link on the Admin page that allows recompilation of Scripts (wcs style FoxPro script pages) on the the entire site. This feature now works recursively and can reliably compile scripts assuming all referenced dependencies used in scripts are loaded by the application before compilation occurs.

* **wwJsonServiceClient::CreatewwHttp()**  
You can now create a new instance of wwHttp to be used in the wwJsonSerializer implementation when making service calls. You can create a new instance of wwHttp that is pre-configured with Authentication, headers etc. and pass it to this method before a service call.

<div class="updatenotice" style="margin-left: 0;margin-right">
<a name="v610breakingchanges"></a>

#### @icon-warning Breaking Changes for v6.10
<a name="v610breakingchanges"></a>

* **Add Markdig.dll to your Distribution for Markdown Parsing**  
The new Markdig Markdown parser requires a new DLL to be used. If you're using the `MarkdownParser` class or the `Markdown()` function, make sure to update to this DLL. You can remove `CommonMark.dll`.

</div>


### Version 6.10
<small>@icon-clock-o March 22nd, 2017  &bull; [Breaking Changes](#v610breakingchanges)</small>

* **[New wwSFTP Class for SSH based FTP](VFPS://Topic/Class%20wwSFTP)**  
There's a new wwSFTP class that provides secure FTP (SFTP only, not FTPS) to connect to SSH based SFTP servers. The class inherits from wwFTP so can be interchangeably with wwFtp based FTP code.

* **wwJsonServiceClient::CreatewwHttp()**  
You can now create a new instance of wwHttp to be used in the wwJsonSerializer implementation when making service calls. You can create a new instance of wwHttp that is pre-configured with Authentication, headers etc. and pass it to this method before a service call.

* **[wwEncryption::ComputeHash()](VFPS://Topic/wwEncryption%3A%3AComputeHash) adds HMAC Hash Algorithms**  
wwEncryption::ComputeHash() adds HMACSHA1, HMACSHA256, HMACSHA384 and HMACSHA512 algorithms. HMAC algorithms use complex salting cycles to add complexity and delay to generated hashes using an industry standard and repeatable algorithm.

* **Switch to Markdig Markdown Parser**   
Switch markdown parser to use the <a href="https://github.com/lunet-io/markdig" target="top">MarkDig .NET Markdown parser</a> which natively supports Github flavored markdown and many Markdown extensions, including support for auto-links, extra emphasis (strikeout, sub/superscript etc.)

* **wwUtils::GetUniqueId()**  
Allows generation of unique IDs based off a full or partial GUID value. Allows specification of a size that determines the size of the generated string between 15 and 32 characters where 32 is a full GUID.

* **wwUtils::SplitString()**  
String utility like ALINES() to return a collection of strings that allows you to specify a length to break lines at. Uses ALINES[] to capture lines and splits lines longer than specified via MEMLINES(). Useful for any code parsing that needs to ensure string literals don't exceed 255 characters in length.

* **wwDotnetBridge now supports TLS 1.2**   
wwDotnetBridge now sets the default HTTPS protocols settings to allow for SSL3, TLS1, TLS11 and TLS12. The default previously only allowed for SSL3 and TLS1. Overrides `ServicePointManager.SecurityProtocol` on initial load of the .NET runtime. This affects any .NET API called that uses HTTP including Web service calls.

* **< %: expr % > for Templates and Scripts**  
Scripts and templates can now use automatic HTML encoding by using `< %: expr % >` instead of `< %= EncodeHtml(expr) % >` (spacing added for rendering). This makes it much easier to create XSS safe string output without more verbose syntax. 

* **wwScripting::CompileAspScript**  
Explicit function that can be used to compile a single ASP Script page using the same mechanism used to run the page. 

* **wwUtils GetWords() Function**  
Added a new function that parses a string of multi-word text into a collection of words.

* **[wwDotnetBridge::InvokeMethodAsync()](VFPS://Topic/wwDotNetBridge%3A%3AInvokeMethodAsync) and [wwDotnetBridge::InvokeStaticMethodAysnc()](VFPS://Topic/wwDotNetBridge%3A%3AInvokeStaticMethodAsync)**  
New method that allows invoking .NET methods on an instance asynchronously. You pass in callback object that receives `OnCompleted()` and `OnError()` callbacks to get notified of completion of the async operation.

* **[wwUtils::HumanizedDate() Function](VFPS://Topic/wwUtils%3A%3AHumanizedDate)**  
Function that returns a human readable string date or time (English only) such as *just now*, *10 minutes ago*, *3 hours ago*, *yesterday*. Anything over a year is turned into a short date: *May 1, 2015*.

* **[wwUtils::FormatValue](VFPS://Topic/wwUtils%3A%3AFormatValue) and [wwUtils::FormatString](VFPS://Topic/wwUtils%3A%3AFormatString)**  
Two methods that use .NET string formatting to allow you to use sophisticated string formatting in FoxPro. Easily create nicely formatted date strings, MIME or ISO date formats, non-padded number strings formatted to decimal places with automatic localized formatting.


<div class="updatenotice" style="margin-left: 0;margin-right">
<a name="v610breakingchanges"></a>

#### @icon-warning Breaking Changes for v6.10
<a name="v610breakingchanges"></a>

* **Add Markdig.dll to your Distribution for Markdown Parsing**  
The new Markdig Markdown parser requires a new DLL to be used. If you're using the `MarkdownParser` class or the `Markdown()` function, make sure to update to this DLL. You can remove `CommonMark.dll`.
</div>


### Version 6.0
<small>@icon-clock-o May 24th, 2016  &bull; [Breaking Changes](#v6breakingchanges)</small>

* **[New wwEncryption Class](vfps://Topic/Class%20wwEncryption)**  
Added a new class that provides two-way TripleDES encryption with a passphrase and and one-way hashes for MD5, SHA256, SHA384 and SHA512. Also includes Checksum generators for files and binary data. Based on wwDotnetBridge (requires .NET).

* **[New Markdown Parser](VFPS://Topic/Class%20MarkdownParser)**  
Added new `MarkdownParser` and `MarkdownParserExtended` classes that allow parsing of Markdown text to HTML. Markdown is a popular text editing format for developer Web sites and CMS systems that simplifies text input via the simple markup formatting of the <a href="http://daringfireball.net/projects/markdown/syntax" target="top">Markdown Syntax</a>.

* **[wwDotnetBridge::InvokeMethodAsync()](VFPS://Topic/wwDotNetBridge%3A%3AInvokeMethodAsync) and [wwDotnetBridge::InvokeStaticMethodAysnc()](VFPS://Topic/wwDotNetBridge%3A%3AInvokeStaticMethodAsync)**  
New methods that allows invoking any .NET methods on an instance asynchronously. You pass in callback object that receives `OnCompleted()` and `OnError()` callbacks to get notified of completion of the async operation.

* **[ComValue::SetValueFromSystemConvert](VFPS://Topic/ComValue.SetValueFromSystemConvert)**  
Method that allows wwDotnetBridge to set a value from the .NET `System.Convert` static class in a simpler way. Allows access to all `System.Convert` methods.

* **[ComValue::SetUInt64 and SetUInt32 Methods](VFPS://Topic/ComValue.SetUInt64)**  
Added additional type conversions for UInt64 and UInt32 values.

* **[wwDotnetBridge::GetIndexedProperty()](VFPS://Topic/wwDotNetBridge%3A%3AGetIndexedProperty)**  
New method adds the ability to retrieve an indexed value from an IList based object like arrays and generic lists. 

* **[wwUtils::FormatValue](VFPS://Topic/wwUtils%3A%3AFormatValue) and [wwUtils::FormatString](VFPS://Topic/wwUtils%3A%3AFormatString)**  
Two methods that use .NET string formatting to allow you to use sophisticated string formatting in FoxPro. Easily create nicely formatted date strings, MIME or ISO date formats, non-padded number strings formatted to decimal places with automatic localized formatting.

* **[wwUtils HumanizedDate() Function](VFPS://Topic/wwUtils%3A%3AHumanizedDate)**  
Function that returns a human readable string date or time (English only) such as *just now*, *10 minutes ago*, *3 hours ago*, *yesterday*. Anything over a year is turned into a short date: *May 1, 2015*.

* **[wwUtils CollectionToCursor and CusorToCollection Functions](vfps://Topic/_45Y0X32PH)**  
Added CollectionToCursor to reverse CursorToCollection SCATTER NAME style objects back into a table after updates. Useful for REST services where inbound data might need updating. Allows for an optional search expression. Cursor must exist and be open to import to.

* **[wwUtils.FlattenSql](VFPS://Topic/_4GL18I04C)**  
Flattens a multi-line SQL into a single line that FoxPro can execute at runtime using Macro expressions. Useful to allow you to write SQL with TEXT/ENDTEXT without semicolons. Used internally in wwBusiness::Execute and wwBusiness::Query.

* **[wwUtils.IsNumber](VFPS://Topic/wwUtils%3A%3AIsNumber)**
Returns .T. if the provided value is a number, or a string that contains only digists and decimal/seperators (. and ,). Useful in many data binding scenarios to determine validity of number input.

* **[New wwJsonServiceClient Wrapper Class for Calling API Services](VFPS://Topic/Class%20wwJsonServiceClient)**  
This class provides an abstract `CallService()` method that automatically handles JSON serialization of parameters, making the HTTP call, and deserializing the JSON result back into a FoxPro class. The method makes the HTTP calls and handles all errors.

* **[wwJsonSerializer::FormattedOutput Flag](VFPS://Topic/_S8X02FE4U)**  
Added a flag to automatically format all output from Serialize() to be pretty formatted. This is a post-processing operation, so it adds some additional overhead, but can provide a nicer development experience.

* **Add `cursor_legacy:cursorName` to wwJsonSerializer table serialization**  
You can now serialize cursors in the 'old' default format which included `Rows` and `Count` properties for the actual data array and record count. Provided for backwards compatibility.

<div class="updatenotice" style="margin-left: 0;margin-right">

<a name="v6breakingchanges"></a>

#### @icon-warning Breaking Changes in v6.0
* **wwDotnetBridge default .NET Version changed to .NET 4**  
Changed the default version that wwDotnetBridge uses if no version is specified to Version 4.0 (which includes all 4.x versions including 4.5.x and 4.6.x). If you want to use .NET 2.0 explicitly pass `CREATEOBJECT("wwDotnetBridge","V2")`. We recommend you always specify your version **explicitly** anyway and remember only the first load determines the runtime loaded. We switched to v4 because it is now the most used version of .NET and pre-installed on Windows 7 and newer.


* **[wwJsonSerializer::Serialize "cursor:TCursor" now returns a raw Array](VFPS://Topic/wwJsonSerializer%3A%3ASerialize)**  
wwJsonSerializer now serializes cursors that are created with `cursor:CursorName` syntax to a straight array. Previously this generated an object with a `Count` and `Rows` properties while `cursor_rawarray:CursorName` generated a raw array. The `cursor:CursorName` syntax now has the same behavior as the `cursor_rawarray:CursorName` syntax, which still works, but it is deprecated in favor of the shorter and more logic syntax.

</div>


### Version 5.72
<small>May 10th, 2015</small>

* **[wwJsonSerializer Large Object Performance Improvements](vfps://Topic/Class%20wwJsonSerializer)**  
Drastically improved performance for large objects by consolidating various object and array routines and removing result values from internal worker methods. For large objects performance can be up to **4 times faster** than previously.

* **[wwJsonSerializer::AssumeUtcDates](vfps://Topic/wwJsonSerializer%3A%3AAssumeUtcDates)**  
Flag that allows you to override the UTC generation of dates being serialized. If .T. dates are not UTC adjusted and assumed to already be in UTC format. Useful if you store dates in UTC format in your application.

* **[wwJsonSerializer Key Collection Serialization](vfps://Topic/Class%20wwJsonSerializer)**  
wwJsonSerializer now serializes Key/Value collections as an object map in JSON where each key is translated to a JSON property on the object. This can greatly simplify object creation for JSON results by simply creating a collection and adding properties and values rather than creating EMPTY objects and using ADDPROPERTY().

* **[wwJsonSerializer::FormatJson()](vfps://Topic/wwJsonSerializer%3A%3AFormatJson)**  
Added a new FormatJson() method to wwJsonSerializer that takes any valid JSON string as input and creates indented and more easily readable JSON output. Use for debugging, or error display or any time you prefer to have more easily readable JSON content after you've serialized objects to JSON.

* **[wwJsonSerializer::Property()](vfps://Topic/wwJsonSerializer%3A%3AProperty) **  
By default wwJsonSerializer only serializes lower case property names which can be a problem for interacting with services that expect proper cased property names. Added a Property() method to wwJsonSerializer that works in the same way as FoxPro's AddProperty() but also adds the property name to the PropertyNameOverrides property. This allows you to create properties on the object specified that are automatically serialized with the proper casing you used in the name applied.

*** [ComValue.SetValueFromInvokeStaticMethod](vfps://Topic/_4BN0T7FDN)**  
You can now set a ComValue from the result of a static method invocation. Useful if the result type of a static method returns a result that is not accessible in FoxPro - like a struct or generic object. You can then use GetProperty/GetPropertyEx to access values of that object.

* **[Add support for ByRef Parameters with wwDotnetBridge::InvokeMethod/InvokeStaticMethod](vfps://Topic/_24O04PA5V)**  
You can now call InvokeMethod/InvokeStaticMethod and use ComValue parameters pass and recover byref parameters. Previously ByRef parameters only worked on direct COM invokation - now you can use the indirect methods as long as you use ComValue objects for the ByRef parameters.

* **[wwUtils::ForceTableRefresh](vfps://Topic/_46E0PBNWN)**  
Forces a cursor or table to refresh itself from disk reliably in high volume scenarios. Flushes buffers and forces the latest data to be re-read from disk. Used internally by Session object to ensure latest data is always read from disk.


### Version 5.70
<small>December 20th, 2014</small>

* **[New wwUtils CollectionToCursor and CusorToCollection Functions](vfps://Topic/_45Y0X32PH)**  
Added CollectionToCursor to reverse CursorToCollection SCATTER NAME style objects back into a table after updates. Useful for REST services where inbound data might need updating. Allows for an optional search expression. Cursor must exist and be open to import to.

* **[Improvements to wwDotnetBridge](vfps://Topic/Class%20wwDotNetBridge)**  
Added better support for binary data, singles, and enum types passed as parameters to methods. Fixed error reporting to be more consistent across method calls. Parameter/value fixup now properly works for all indirect method and property access.

* **[wwUtils::ForceTableRefresh](vfps://Topic/_46E0PBNWN)**  
Function to force VFP table read buffers to refresh, to ensure data read is not read from VFP's data cache. Internally this function moves the record pointer explicitly using GOTO RECNO().

* **[wwSmtp::ClearAttachments()](vfps://Topic/wwSmtp%3A%3AClearAttachments)**  
Add ClearAttachments method that clears out attachments added with AddAttachment(). This allows you to control attachment counts when adding during multiple SendMessage() calls on the same SMTP connection.



### Version 5.68
<small>Mar 15th, 2014</small>

* **Address concerns for running on XP**  
Change compiler target using Visual Studio 2013 runtimes for wwIpstuff. Recent Microsoft compiler changes have caused some failures on XP with certain C++ runtime functions missing. The VS2013 runtimes fix this and wwIPstuff embeds all the required dependencies for no runtime support requirements on XP through 8.1. Affects: wwIpStuff.dll, wwImaging.dll

* **[Add support for wwDotnetBridge char parameters and results](vfps://Topic/wwDotNetBridge%3A%3AConvertToDotNetValue)**  
.NET char parameters can now be set with ComValue::SetChar() by passing ineither a string value or number. You can also use loBridge.ConvertToDotnetValue(val,"char")to create the ComValue structure. char results are converted to string when returned from .NET method calls.

* **[wwJsonService now uses DeserializeJson](vfps://Topic/Class%20wwJsonService)**  
Updated the Json Service interface to use DeserializeJson which uses the .NET JSON serializer for much more reliable JSON inputs to the service. Much improved performance for large objects.

* **Fix NULL Value Issue wwDotnetBridge**  
Fixed bug with NULL values passed to wwDotnetBridge calls. COM Interop changes Fox NULLs to DbNulls which failed. Indirect methods now translate DbNull values to raw .NET nulls when passed. You can still pass DbNull with ComValue if needed.

* **[Allow for 24 parameters in wwDotnetBridge::InvokeMethod](vfps://Topic/wwDotNetBridge%3A%3AInvokeMethod)**  
Due to many, many support requests with Invokemethod requirements for an enormous amount of parameters, we bumped the supported parameter count to 24 parameters which is the maximum VFP allows (26 - 2 for object and method). For more parameters yet you can still use the InvokeMethod_ParameterArray() method. This is crazy but there you have it.

* **[wwJsonSerializer::Property() to help with mixed case JSON Properties](vfps://Topic/wwJsonSerializer%3A%3AProperty)**  
A helper that provides ADDPROPERTY() functionality and also adds any property names added to the PropertyNameOverrides list to automatically force proper case for properties exported into JSON. Unfortunately due to Visual FoxPro's reflecting limitations this override is necessary in order to match object structures to actual applications. This method reduces the explicit assignment in PropertyNameOverrides when dynamically creating classes for serialization.

* **[wwAPI::MapNetworkDrive](vfps://Topic/wwAPI%3A%3AMapNetworkDrive)**  
Added MapNetworkDrive method that allows mapping a network drive programmatically. This updated version supports specifying of username and password.

* **Many small bug fixes**  
The bulk of this release are small bug fixes and tweaks for this small maintenance release.

I want to type