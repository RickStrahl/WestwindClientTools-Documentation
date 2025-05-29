### Visual FoxPro 9.0
The current version of the West Wind Internet and Client Tools is designed for use with Visual FoxPro 9.0. This version can be downloaded from:

<a href="https://west-wind.com/files/wwclient.zip" target="top">https://west-wind.com/files/wwclient.zip</a>

Although the client tools are designed for 9.0 a lot of functionality works also on FoxPro 8.0, but we make no guarantees. We no longer test for older versions of FoxPro. 

If you need to support for older versions you can download one of the older versions, but realize that these are unsupported.

### Visual FoxPro 8.0 and 9.0
If you require support for FoxPro 8.0 of FoxPro please download the 5.72 version of the Client Tools from here:

<a href="https://west-wind.com/files/wwclient_572.zip" target="top">https://west-wind.com/files/wwclient_572.zip</a>

Most features with the exception of the some of the .NET Integration features work with FoxPro 8.0.

### Visual FoxPro 6.0 and 7.0
If you require support for even  older versions of FoxPro please download the 4.68 version of the Client Tools from here:

<a href="https://west-wind.com/files/wwclient_468.zip" target="top">https://west-wind.com/files/wwclient_468.zip</a>

Note this version is very old, but it works on much older versions of FoxPro.

### Visual FoxPro Requirements and Settings:

**Assumes SET EXACT OFF**  
All West Wind Tools run based on the assumption that SET EXACT OFF is set in the FoxPro environment. Due to various performance concerns which otherwise would be required to check exact values and existing SET flags SET EXACT is assumed to be OFF. If you happen to run your code with SET EXACT ON, you may have to explicitly switch your SET EXACT settings before calling functions in this library.