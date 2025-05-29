<img src="images\logo.png" style="width: 256px;margin: 0 8px 5px; float: left;">

West Wind Client Tools is a rich suite of utility classes that provide Internet functionality including **SMTP and POP3 Email**, **HTTP Web**, **FTP** and low level **TCP/IP access**, as well as many useful application level classes. 

Utility classes include providing easy access to .NET objects, object based data access to SQL Server and Web Data, a light-weight distributed and Web centric Business Object framework, simple encryption helpers, JSON and XML Serialization, a JSON Service client for calling REST Services,  PDF document generation from VFP reports, configuration management classes, and a very rich library of utility functions useful in everyday Visual FoxPro development.

This tool provides most the client and every day development functionality of the West Wind Web Connection product line, without all the server features and is an upgrade to the functionality of the West Wind Internet Protocols suite, which has been discontinued.

West Wind Client Tools is designed and tested for **Visual FoxPro 9.0**. Most features should also work with VFP 8.0 but we no longer test or support VFP 8.0 officially.

### Internet Access Features:

**SMTP and POP3 Email support**  
SMTP Send Mail capability both in blocking synchronous and asynchronous modes. Support for file attachments, CCs and BCCs, special content types (HTML or XML messages for example) and more. All you need is a mail server to send message through (your ISP's or your own) and off you go. POP3 support provides a separate class to handle mail retrieval with an easy to use interface to retrieving, parsing and managing messages in a POP3 mailbox.

**HTTP - Access Web content from VFP**  
Grab any content from the Web using the HTTP protocol. Download files or access HTML pages, submit and drive Web based forms, send XML from client to server and back or build custom distributed front ends (ie. like a SOAP client). HTTP is the key protocol for distributed applications and wwIPStuff includes the wwHTTP class that includes full support for all of the important features of the HTTP protocol including authentication, proxy support, timeouts, SSL/HTTPS encryption, cookies and more. Supports both high level and low level interfaces. The high level interface takes two lines of code to retrieve a Web page into a string or file.

**Advanced HTTP support**  
In addition, tools required for efficient HTTP operation are also included such as fast URLEncoding/URLDecoding using fast C code routines, UTF-8 encoding/decoding, binary packaging for DBF files for transfer over HTTP, as well as inclusion of the powerful wwXML class which can be used to convert VFP cursors and objects into XML.

**FTP - Transfer files**  
New updated wwFTP class allows for file downloads and uploads. Low and high level methods for simple transfers and control over the connection are supported.

**Low Level TCP/IP Socket Access**  
Create low level TCP/IP socket connections and create socket servers with with the wwSocket class to access Internet or TCP/IP services. This class is very easy to use and includes high-level methods that session and transaction based TCP/IP access very easy.

**Dial up Networking**  
Basic support for dialing and disconnecting a RAS Dial-up Networking Connections.

**IP Address validation**  
Support for domain lookup from IP address and IP Address from domain name.

**Fast C based Conversion Routines**  
UrlEncoding and UrlDecoding, UTF-8 Conversion, Base 64 encoding.

### Application Features
**wwBusiness Class**  
The wwBusiness class is a single level business object class that implements both the business and data tier in a single object to provide ease of use and maximum performance. The base object provides core data access functionality (Load, Save, Query, New etc.) through its base class methods. Query based data access is also supported through native Fox syntax or using the class methods which can route to the appropriate backend. Fox or SQL Server are natively supported. SQL Server is supported with SQL Passthrough commands. Remote Web data is also supported against any FoxPro based Web backend (Web Connection, ASP+COM, FoxISAPI etc.) Web data can run against a SQL backend (with some limitations a Fox backend can be used as well).

**wwSQL Class**  
The wwSQL class is a light-weight wraper around SQLPassthrough in Visual FoxPro and provides handle abstraction and a simplified interface to executing SQL commands and managing connections. The class provides error handling and error recovery for connection issues as well as a number of high level methods that deal with updating data and passing data between Fox and SQL backends.


**wwDotNetBridge Class**  
Provides the ability to call any .NET component without requiring the called components to be registered through COM interop. This allows access to many types not directly loadable through COM Interop. The class also provides many helper methods for instantiating static .NET types, enums and access arrays and collections that otherwise would not be accessible. Note: Still uses COM Interop, but doesn't require instantiation through COM interop.


**wwJsonSerializer and wwJsonServiceClient Classes**  
The serializes and deserializes JSON to and from FoxPro objects and values. The serializer provides many options that allow working around FoxPro's limitations in dealing with effective serialization. The wwJsonServiceClient class makes it easy to call a REST service and automatically have a parameter passed serialized to JSON and sent to the service, and automatically have result values deserialized back into FoxPro accessible objects/values.

**wwXML Class**  
wwXML is a powerful data conversion class that lets you convert Fox cursors and object to and from XML. This class is more flexible than VFP 7's native commands and provides a number of additional features including direct support for .NET datasets and of course the ability to persist and restore objects.

**wwHTTPSQL/wwHTTPSQLServer Web SQL Class**  
The wwHTTPSQL class provides a Web based remote SQL Engine to VFP client applications. The wwHTTPSQL -> wwHTTPSQLServer pair allows an application to query data from a Web server and return this data in standard VFP cursor format. Data travels over the wire as XML and is transparently converted. The server can be implemented in any VFP code capable Web development tool (Web Connection, ASP, FoxISAPI, AFP etc) and allows remote execution against VFP or any ODBC datasource on the Web server. Ever wanted to run Visual FoxPro as a remote SQL Engine? This is one way you can do it. Very easy to install and access with only a few lines of code.

**wwPDF Class**  
Easily create PDF documents from FoxPro reports using any of the supported drivers using either Adobe Acrobat, AmyUni or ActivePDF.

**MarkdownParser Class**  
This class and the corresponding `Markdown()` function easily let you parse Markdown text to HTML. Markdown is a very easy to use and very popular format that can represent HTML as easy to type plain text.

**wwEncryption Class**  
Provides basic services for two-way encryption and hashing using various algorithms to allow you to effectively store secure information in your applications.

**wwImaging Functions** 
The wwAPI class also includes a set of imaging functions that can be used to create thumbnails, resize and crop images and get info about images using GDI+.

**Utility Classes**  
In addition the Client Tools include a few smaller classes:
**wwConfig**  -  A very useful configuration utility that can persist data from an object into a configuration file (XML, INI or the registry) and can be used in any application as a single access configuration manager object.
**wwXMLState** - This class can be used to manage dynamic properties inside of a string or memo field. It's XML based and stores multiple values in a single string. Useful for extending database schemas without changing the structure and to store data more efficiently when data is sparse and would waste space as a field.
wwWebGraphs - This class provides a very easy way to generate various types of charts (bar, line, point, pie etc) from VFP cursors or arrays. Uses the Office Web Components to create flashy graphs with very little code and without having to deal with the OWC object model.
**wwEval** - A 'safe execution' class that can be used to run user specified or unknown code. Sort of a poor man's Try/Catch implementation that allows you to run code that potentially could fail safely and receive error information. Execute single commands, expressions and even entire code blocks. Also includes MergeText() functionality to parse script like pages using ASP style syntax.
**wwUtils Library** - This extensive function library contains a variety of functions to perform general purpose tasks from text formatting, to HTML formatting, to debugging, COM configuration, viewing out and even several GUI pop up tools.
**wwAPI Utilities library** A number of methods to access common API routines like Registry access, INI access, Computername checks, Clock information etc.
**wwCache Class** - Class that lets you store string data in a table for easy reuse without re-generating. Provides basic level caching on a VFP instance basis.
**wwScripting** - a Client side ASP style scripting engine that allows you to expand expressions and code from within a script. Great for generating dynamic HTML with embedded FoxPro data for usage in desktop client applications that use the Web Browser control to display local content. Supports both scripts and templates. (VFP8 and later required).
**wwLocaleInfo Class** - Class that provides locale specific information from delimiters used for numbers and dates, to name of locales etc.


**Useful Tools**  
Client Tools also includes a number of tools that are useful in the course of development of applications:
* **Structure Exporter**  
Takes VFP table structures and exports it to text in various ways: CREATE TABLE statements, object declarations, field list, Help Builder field list and several Web Connection template types more.

* **Text Wrapper**  
Useful tool to take a block of text and create string based code writes out this text via code.