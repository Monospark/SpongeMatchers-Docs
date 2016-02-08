============
Object types
============

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

Sponge types
============

Sponge types are types that represent some object in Sponge, like an item stack or an entity.
Since these types can be very diverse and complex, it would be hard to create an unique matcher syntax for every type.
As a result, the Sponge types are converted to a :ref:`Map <map>`, which can then be matched using map matchers.

Item stacks
-----------

Map structure
^^^^^^^^^^^^^

========== ============================ ===========
Key        Type                         Description
========== ============================ ===========
type       :ref:`String <string>`       The item type. For example *minecraft:apple*.
durability :ref:`Integer <integer>`     The durability of the stack. This value is used as a damage indicator for tools and to distinguish item variants, for example coal and charcoal.
quantity   :ref:`Integer <integer>`     The amount of items in the stack.
properties :ref:`Map <map>` of          The properties of the block type.
           properties (See table below)
data       :ref:`Data <data>`           The additional data of the stack, for example enchantments, nametags and custom tags. For a full reference, you can visit the `<http://minecraft.gamepedia.com/Tutorials/Command_NBT_Tags#Items>`_. This entry is not present if the item stack has no additional data.
========== ============================ ===========

Available properties
^^^^^^^^^^^^^^^^^^^^

========================= ====================================== ===========
Name                      Type                                   Description
========================= ====================================== ===========
ArmorTypeProperty         :ref:`String <string>`
BurningFuelProperty       :ref:`Integer <integer>`               Defines the amount of fuel ticks an item will grant to a furnace.
DamageAbsorptionProperty  :ref:`Integer <integer>`               Defines the percentage of damage the item will absorb when equipped.
EfficiencyProperty        :ref:`Floating point <floating-point>` Defines the efficiency qualifier of an item when mining harvestable blocks.
EquipmentProperty         :ref:`String <string>`                 Defines the equipment type that the item can be used in an equipment inventory.
FoodRestorationProperty   :ref:`Integer <integer>`               Defines how much the level will be restored when the item is consumed.
SaturationProperty        :ref:`Floating point <floating-point>` Defines how much the item replenishes saturation upon use or consumption.
ToolTypeProperty          :ref:`String <string>`                
UseLimitProperty          :ref:`Integer <integer>`               Defines a limit on the number of uses on an item.
========================= ====================================== ===========

Matcher examples
^^^^^^^^^^^^^^^^

* ``{'type': 'minecraft:diamond_pickaxe', 'data': {'ench': matchAny: {'id': 'minecraft:efficiency'}}`` matches a diamond pickaxe that has an *Efficiency* enchantment of any level.
* ``{'type': 'minecraft:coal', 'durability': 1, 'quantity': >1}`` matches a stack of charcoal that contains more than one item.

Item enchantments
-----------------

Map structure
^^^^^^^^^^^^^

========== ======================== ===========
Parameter  Type                     Description
========== ======================== ===========
id         :ref:`String <string>`   The enchantment id. For example *minecraft:efficiency*.
level      :ref:`Integer <integer>` The level of the enchantment.
========== ======================== ===========

Matcher examples
^^^^^^^^^^^^^^^^

* ``{'id': 'minecraft:unbreaking'}`` matches any *Unbreaking* enchantment.
* ``{'id': 'minecraft:fortune' | 'minecraft:efficiency', 'level': >1}`` matches any *Fortune* or *Efficiency* enchantment with a level bigger than *1*.

.. _blocktype:

Block types
-----------

Map structure
^^^^^^^^^^^^^

========== ============================== ===========
Parameter  Type                           Description
========== ============================== ===========
id         :ref:`String <string>`         The block type id. For example *minecraft:stone*.
properties :ref:`Map <map>` of properties The properties of the block type.
           (See table below)
========== ============================== ===========

Available properties
^^^^^^^^^^^^^^^^^^^^

========================= ====================================== ===========
Name                      Type                                   Description
========================= ====================================== ===========
BlastResistanceProperty   :ref:`Floating point <floating-point>`
FlammableProperty         :ref:`Boolean <boolean>`               Defines whether a block is able to catch fire.
GravityAffectedProperty   :ref:`Boolean <boolean>`               Defines whether a block type is affected by "gravity" such that if a block is placed and there is no block supporting it, the block will fall.
GroundLuminanceProperty   :ref:`Floating point <floating-point>` Defines the amount of light on a block location.
HardnessProperty          :ref:`Floating point <floating-point>`
HeldItemProperty          :ref:`Boolean <boolean>`
IndirectlyPoweredProperty :ref:`Boolean <boolean>`
LightEmissionProperty     :ref:`Integer <integer>`               Defines the amount of light emitted from the owning block.
MatterProperty            :ref:`String <string>`
PassableProperty          :ref:`Boolean <boolean>`
PoweredProperty           :ref:`Boolean <boolean>`
ReplaceableProperty       :ref:`Boolean <boolean>`               Defines if a block type can be replaced by other block types when other blocks are being placed.
SkyLuminanceProperty      :ref:`Floating point <floating-point>` Defines the amount of light emitted from the owning block.
SolidCubeProperty         :ref:`Boolean <boolean>`               Defines whether a block type is a "solid cube" and therefore can be considered to place a torch on its side.
StatisticsTrackedProperty :ref:`Boolean <boolean>`
TemperatureProperty       :ref:`Floating point <floating-point>`
UnbreakableProperty       :ref:`Boolean <boolean>`
========================= ====================================== ===========

Matcher examples
^^^^^^^^^^^^^^^^

* ``{'id': 'minecraft:stone'}`` matches the block type *minecraft:stone*.
* ``{'id': r'minecraft:.+', 'properties': {'GravityAffectedProperty': true}}`` matches all vanilla blocks which are affected by gravity, for example *minecraft:sand*.

Block states
------------

Map structure
^^^^^^^^^^^^^

========== ================================= ===========
Parameter  Type                              Description
========== ================================= ===========
type       :ref:`Block type <blocktype>`     The block type.
traits     :ref:`Block traits <blocktraits>` The traits of the block.
data       :ref:`Data <data>`                The additional data of the block, for example tile entity data. For a full reference, you can visit the `Minecraft wiki <http://minecraft.gamepedia.com/Tutorials/Command_NBT_Tags#Blocks>`_. This entry is not present if the block state has no additional data.
========== ================================= ===========

Matcher examples
^^^^^^^^^^^^^^^^

* ``{'type': {'id': 'minecraft:planks'}, 'traits': {'variant': 'birch'}}`` matches only birch planks.
* ``{'type': {'id': 'minecraft:chest'}, 'data': {'CustomName': 'MyChest'}}`` matches chests that have been renamed to *MyChest*.


Undefined types
---------------

Undefined types do not have predefined key names and predefined value types.
Instead, every possible key is allowed and the type of a value is restricted to a list of types for every undefined type. 

.. _data:

Data
----

Data types correspond to the `NBT tags <http://minecraft.gamepedia.com/Tutorials/Command_NBT_Tags?cookieSetup=true>`_ in Minecraft.
NBT tags are used to store the data of everything in Minecraft, for example the data of an item stack or an entity.

Possible value types
^^^^^^^^^^^^^^^^^^^^

* :ref:`Booleans <boolean>`
* :ref:`Integers <integer>`
* :ref:`Floating points <floating-point>`
* :ref:`Strings <string>`
* :ref:`Data <data>`
* :ref:`Lists <list>` of every type listed above

Examples
^^^^^^^^

* ``{'ench': [{'id': 'minecraft:fortune', 'lvl': 2}, {'id': 'minecraft:fortune', 'lvl': 1}]}`` matches the additional data of an item stack if the stack has the two specified enchantments.

.. _properties: