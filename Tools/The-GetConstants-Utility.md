To access the utility click on the Web Connection Menu once you've started Web Connection. If the menu doesn't exist `DO WCSTART` to get it loaded.  Choose Tools|COM Constant Grabber. You'll see a simple interface that lets you pick a type library and an output file to send the Header constants to.

![](IMAGES\MISC\GETCONSTANTS.GIF)

### What does it do
COM components have the capability to embed enumerated types that hide specific values behind descriptive names that are easier to remember, read in code and type. For example, in ADO there are large numbers of parameter options for how connections are defined with values like adOpenForwardOnly, adOpenKeySet etc. Microsoft Office also has a huge library of enumarated values and types that describe specific parameters to functions and properties.

Visual FoxPro does not support the early binding and Intellisense features that other tools like Visual Basic provide that can automatically show the enumerated values. Therefore in Visual FoxPro the closest thing to an enumerated value is a constant defined in a header file (.h). For example we can define:

<pre>*** Constant Group: CursorTypeEnum
#define adOpenUnspecified                                 -1
#define adOpenForwardOnly                                 0
#define adOpenKeyset                                      1
#define adOpenDynamic                                     2
#define adOpenStatic                                      3
</pre>

and then use these values in a VFP program or class by including the .H file in the PRG or VCX.

The GetConstants utility can be used to easily retrieve these COM constants and enumerated types from a COM typelibrary or COM DLL and dump them into ready to use #DEFINE constants in a .H file. The utility allows you to simply pick the DLL, TLB, OLB (office typelibs) or OCX files and dumps the contents into a header file of your choice.