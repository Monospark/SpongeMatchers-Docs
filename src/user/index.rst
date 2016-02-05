==================
User documentation
==================

If you are using a Sponge plugin that uses SpongeMatchers and you want to make use of its powerful features, this is the right place for you.

Base Matchers
=============

Base matchers are the basic building blocks of every complex matcher.

Boolean matchers
----------------

Boolean matchers can match a boolean, which can either be true or false. So the two possible values a boolean matcher can have are ``true`` or ``false``.

Integer matchers
----------------

Integer matchers can match a 64-bit integer. The following matcher constructs are all supported:

*<value>*
  Matches, if the integer is equal to *<value>*.
  
*<min>* - *<max>*
  Matches, if the integer is bigger or equal than *<min>* and less or equal than *<max>*.
  Note that *<min>* must be less than *<max>*.
  
> *<value>*
  Matches, if the integer is bigger than *<value>*.
  
>= *<value>*
  Matches, if the integer is bigger than or equal to *<value>*.
  
< *<value>*
  Matches, if the integer is less than *<value>*.
  
<= *<value>*
  Matches, if the integer is less than or equal to *<value>*.
  
Floating-point number matchers
------------------------------

Floating-point number matchers can match a 64-bit floating-point number.
Floating-point numbers must have an **f** or **F** at the end to distinguish them from normal integers.
Leading zeros can also be omitted.
This means that *1f*, *0.5f* and *.3f* are all valid floating-point number literals.
Moreover, the following matcher constructs are all supported:

*<value>*
  Matches, if the floating-point number is equal to *<value>*.
  
*<min>* - *<max>*
  Matches, if the floating-point number is bigger or equal than *<min>* and less or equal than *<max>*.
  
> *<value>*
  Matches, if the floating-point number is bigger than *<value>*.
  
>= *<value>*
  Matches, if the floating-point number is bigger than or equal to *<value>*.
  
< *<value>*
  Matches, if the floating-point number is less than *<value>*.
  
<= *<value>*
  Matches, if the floating-point number is less than or equal to *<value>*.
  
String matchers
---------------

String matchers can match strings. There are two different types of string matchers:

Literal string matchers
  Matches, if the string is equal to the specified matcher string. 
  Literal string matchers must be surrounded by single quotes.
  For example, the string matcher *'string'* would match *string*.
  
Regular expression matchers
  Matches, if the specified regular expression completely matches the string.
  Regular expression matchers also must surrounded by single quotes.
  To distinguish them from literal string matchers, they have to be prefixed with an **r** or **R**.
  For example, the string matcher *r'(string)+'* would match *string* and also *stringstring*.
  
  
