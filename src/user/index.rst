==================
User documentation
==================

If you are using a Sponge plugin that uses SpongeMatchers and you want to make use of its powerful features, this is the right place for you.

Base Matchers
=============

Base matchers are the basic building blocks of every advanced matcher.

Boolean matchers
----------------

Boolean matchers can match a boolean, which can either be true or false. So the two possible values a boolean matcher can have are ``true`` or ``false``.

Integer matchers
----------------

Integer matchers can match a 64-bit integer. The following matcher constructs are all supported:

``<value>``
  Matches, if the integer is equal to *<value>*.
  
``<min> - <max>``
  Matches, if the integer is bigger or equal than *<min>* and less or equal than *<max>*.
  Note that *<min>* must be less than *<max>*.
  
``> <value>``
  Matches, if the integer is bigger than *<value>*.
  
``>= <value>``
  Matches, if the integer is bigger than or equal to *<value>*.
  
``< <value>``
  Matches, if the integer is less than *<value>*.
  
``<= <value>``
  Matches, if the integer is less than or equal to *<value>*.
  
Floating-point number matchers
------------------------------

Floating-point number matchers can match a 64-bit floating-point number.
Floating-point numbers must have an **f** or **F** at the end to distinguish them from normal integers.
Leading zeros can also be omitted.
This means that *1f*, *0.5f* and *.3f* are all valid floating-point number literals.
Moreover, the following matcher constructs are all supported:

``<value>``
  Matches, if the floating-point number is equal to *<value>*.
  
``<min> - <max>``
  Matches, if the floating-point number is bigger or equal than *<min>* and less or equal than *<max>*.
  Note that *<min>* must be less than *<max>*.
  
``> <value>``
  Matches, if the floating-point number is bigger than *<value>*.
  
``>= <value>``
  Matches, if the floating-point number is bigger than or equal to *<value>*.
  
``< <value>``
  Matches, if the floating-point number is less than *<value>*.
  
``<= <value>``
  Matches, if the floating-point number is less than or equal to *<value>*.
  
String matchers
---------------

String matchers can match strings. There are two different types of string matchers:

Literal string matchers
^^^^^^^^^^^^^^^^^^^^^^^

:Syntax: ``'<string>'``
:Usage: Matches, if the string is equal to *<string>*.
:Example: ``'test'`` matches the string *test*.

Regular expression matchers
^^^^^^^^^^^^^^^^^^^^^^^^^^^

:Syntax: ``r'<regex>'``
:Usage: Matches, if the specified regular expression completely matches the string.
:Example: ``r'(string)+'`` would match *string* and also *stringstring*
  
Logical operations
==================

By using logical operations, you are able to create every matcher combination you want.

*And* conjunction
-----------------

:Syntax: ``<matcher1> & <matcher2>``
:Usage: Matches, if both, *<matcher1>* and *<matcher2>*, match.
:Example: ``>=1 & <=3`` is equal to the integer matcher *1-3*.

*Or* conjunction
----------------

:Syntax: ``<matcher1> | <matcher2>``
:Usage: Matches, if either *<matcher1>* or *<matcher2>* match.
:Example: ``1 | 5`` matches *1* and *5*.

Negation
--------

:Syntax: ``!<matcher>``
:Usage: Negates the result of a matcher.
:Example: ``!1`` matches every integer except *1*.

Parentheses
-----------

:Syntax: ``(<expression>)``
:Usage: Parentheses allow you to group expressions and change the order of operations to your needs.
:Example: ``(>=1 & <=2) | (>=5 & <=6)`` matches *1*, *2*, *5* and *6*.
  
Advanced Matchers
=================

Optional matchers
-----------------

Optional matcher are needed when the existence of a value, which should be matched, can not be guaranteed.

List matchers
-------------

List matchers can match a list that contains elements of the same for type, for example a list of strings.
There are four different types of list matchers:

Literal list matchers
^^^^^^^^^^^^^^^^^^^^^

:Syntax: ``[<matcher1>, <matcher2>, ...]``
:Usage: Matches, if every matcher matches the list element at the same index.
:Example: ``[1-3, 4-6]`` matches the integer list *[2, 5]* but not *[2, 5, 1]*.

Any element matchers
^^^^^^^^^^^^^^^^^^^^

:Syntax: ``matchAny: <matcher>``
:Usage: Matches, if the specified matcher matches at least one element of the list.
:Example: ``matchAny: 1-3`` matches the integer list *[4, 5, 2]* but not *[4, 5]*.

Every element matchers
^^^^^^^^^^^^^^^^^^^^^^

:Syntax: ``matchAll: <matcher>``
:Usage: Matches, if the specified matcher matches every element of the list.
:Example: ``matchAll: 1-3`` matches the integer list *[1, 2, 3]* but not *[1, 4]*.

Empty list matcher
^^^^^^^^^^^^^^^^^^

:Syntax: ``none``
:Usage: Matches, if the list contains no elements.
:Example: ``none`` matches *[]* but not *[1]*.

Map matchers
------------

Map matchers can match maps, which associate a name with a value.
Furthermore, map matchers can make use of optional matchers, since it's not guaranteed that a map contains a value for a specific key.

:Syntax: ``{'<entry1>': <matcher1>, '<entry2>': <matcher2>, ...}``
:Usage: Matches, if the matcher of every key matches the value for that key in the map.
:Example: ``{'entry1':false, 'entry2:' >=2 | empty}`` matches the map *{'entry1':false}* and also the map *{'entry1':false, 'entry2:' 5}*.
