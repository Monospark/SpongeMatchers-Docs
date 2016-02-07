==============
Core mechanics
==============

Base Types
==========

.. _boolean:

Booleans
--------

A boolean can either be *true* or *false*.
As a result, the two possible values a boolean matcher can have are ``true`` or ``false``.

.. _integer:

Integers
--------

All Integers use 64-bit internally.
The following matcher constructs are supported:

``<value>``
  Matches, if the integer is equal to *<value>*.

``> <value>``
  Matches, if the integer is bigger than *<value>*.
  
``>= <value>``
  Matches, if the integer is bigger than or equal to *<value>*.
  
``< <value>``
  Matches, if the integer is less than *<value>*.
  
``<= <value>``
  Matches, if the integer is less than or equal to *<value>*.
  
.. _floating-point:  
  
Floating-point numbers
----------------------

All floating-point numbers use 64-bit internally.
Floating-point number literals must have an **f** or **F** suffix to distinguish them from normal integers.
Leading zeros can also be omitted.
This means that *1f*, *0.5f* and *.3f* are all valid floating-point number literals.
Moreover, the following matcher constructs are supported:

``<value>``
  Matches, if the floating-point number is equal to *<value>*.

``> <value>``
  Matches, if the floating-point number is bigger than *<value>*.
  
``>= <value>``
  Matches, if the floating-point number is bigger than or equal to *<value>*.
  
``< <value>``
  Matches, if the floating-point number is less than *<value>*.
  
``<= <value>``
  Matches, if the floating-point number is less than or equal to *<value>*.
  
.. _string:
  
Strings
-------

There are two different ways to match a string:

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

Advanced types
==============

.. _list:

Lists
-----

List contain elements of the same type in a specific order, for example a list of strings.
There are four different ways to match a list:

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

Empty list matchers
^^^^^^^^^^^^^^^^^^^

:Syntax: ``none``
:Usage: Matches, if the list contains no elements.
:Example: ``none`` matches *[]* but not *[1]*.

.. _map:

Maps
----

Maps associate keys with values. 
You can match maps using the following matcher construct:

:Syntax: ``{'<key1>': <matcher1>, '<key2>': <matcher2>, ...}``
:Usage: Matches, if the matcher of every key matches the associated value for that key in the map. You can use *empty* to match non-existent values.
:Example: ``{'entry1': false, 'entry2:' >=2 | empty}`` matches the map *{'entry1':false}*, *{'entry1':false, 'entry2:' 5}* and also *{'entry1':false, 'entry2:' 5, 'entry3': 'string'}*.


Logical operations
==================

By using logical operations, you are able to further customize your matchers and create every matcher combination you want.

*And* conjunctions
------------------

:Syntax: ``<matcher1> & <matcher2>``
:Usage: Matches, if both, *<matcher1>* and *<matcher2>*, match.
:Example: ``>=1 & <=4`` is equal to the integer range *1..4*.

*Or* conjunctions
-----------------

:Syntax: ``<matcher1> | <matcher2>``
:Usage: Matches, if either *<matcher1>* or *<matcher2>* match.
:Example: ``1 | 5`` matches *1* and *5*.

Negations
---------

:Syntax: ``!<matcher>``
:Usage: Negates the result of a matcher.
:Example: ``!1`` matches every integer except *1*.

Parentheses
-----------

:Syntax: ``(<expression>)``
:Usage: Parentheses allow you to group expressions and change the order of operations to your needs.
:Example: ``(>=1 & <=2) | (>=5 & <=6)`` matches *1*, *2*, *5* and *6*.

Wildcard matchers
-----------------
  
:Syntax: ``*``
:Usage: Matches always.
:Example: ``*`` matches every type.
