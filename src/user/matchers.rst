========
Matchers
========

Introduction
============

Matchers are used to check if an object meets some specific requirements.
It is possible to create matchers for almost any object type.
For example, the integer matcher *>2* matches all integers that are greater than 2.
Using simple matchers like integer matchers as building blocks, it is possible to create more advanced matchers, for example item stack matchers or entity matchers.
This user documentation will show you how to do that.

Creation
========

For every object type, like integers or entities, there is a specific matcher syntax.
You can find a full reference of all supported object types and their corresponding matcher syntax in the :doc:`Object types <types>` topic.
Furthermore, there is a useful set of extra features that allow for flexible matcher creation:

* *And* conjunctions
    :Syntax: ``<matcher1> & <matcher2>``
    :Usage: Matches, if both, *<matcher1>* and *<matcher2>*, match.
    :Example: ``>=1 & <=4`` is equal to the integer range *1..4*.

* *Or* conjunctions
    :Syntax: ``<matcher1> | <matcher2>``
    :Usage: Matches, if either *<matcher1>* or *<matcher2>* match.
    :Example: ``1 | 5`` matches *1* and *5*.

* Negations
    :Syntax: ``!<matcher>``
    :Usage: Negates the result of a matcher.
    :Example: ``!1`` matches every integer except *1*.

* Parentheses
    :Syntax: ``(<expression>)``
    :Usage: Parentheses allow you to group expressions and change the order of operations to your needs. Nested parentheses are also supported.
    :Example: ``(>=1 & <=2) | (>=5 & <=6)`` matches *1*, *2*, *5* and *6*.

* Wildcard matchers
    :Syntax: ``*``
    :Usage: Matches always.