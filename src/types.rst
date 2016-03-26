============
Object types
============

Base Types
==========

Base types are the basic building blocks of every more advanced type.

.. _booleans:

Booleans
--------

A boolean can either be *true* or *false*.
As a result, the two possible values a boolean matcher can have are ``true`` or ``false``.

.. _integers:

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
  
.. _floating-points:  
  
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
  
.. _strings:
  
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

.. _lists:

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

:Syntax: ``[]``
:Usage: Matches, if the list contains no elements.

.. _optionals:

Optionals
---------

Optionals wrap around other types and indicate that the value may not be present.
There are three ways to match optional types:

``absent``
  Matches, if the value is not present.

``existent``
  Matches, if the value is present.

``<matcher>``
  Matches, if the value is present and <matcher> matches the value.

.. _maps:

Maps
----

Maps associate keys with values. 
You can match maps using the following matcher construct:

:Syntax: ``{'<key1>': <matcher1>, '<key2>': <matcher2>, ...}``
:Usage: Matches, if the matcher of every key matches the associated value for that key in the map.
:Example: ``{'entry1': false, 'entry2:' >=2 | absent}`` matches the map *{'entry1':false}*, *{'entry1':false, 'entry2:' 5}* and also *{'entry1':false, 'entry2:' 5, 'entry3': 'string'}*.

Sponge types
============

Sponge types are types that represent some object in Sponge, like an item stack or an entity.
Since these types can be very diverse and complex, it would be hard to create an unique matcher syntax for every type.
As a result, the Sponge types are converted to a :ref:`Map <maps>`, which can then be matched using map matchers.

.. _dimensions:

Dimensions
----------

A dimension represents the type of a world, like the *overworld* and the *nether* dimension.

Map structure
^^^^^^^^^^^^^

================ ========================= ===========
Key              Value type                Description
================ ========================= ===========
name             :ref:`String <strings>`   The dimension name.
respawnAllowed   :ref:`Boolean <booleans>` Defines whether players can respawn within the dimension after death.
waterEvaporating :ref:`Boolean <booleans>` Defines whether water evaporates in the dimension.
sky              :ref:`Boolean <booleans>` Defines whether there is a sky in the dimension (lack of bedrock).
height           :ref:`Integer <integers>` The highest naturally generated y-coordinate in the dimension. Usually 128 (no sky) or 256 (sky).
buildHeight      :ref:`Integer <integers>` The maximum y-coordinate a non-air cuboid can exist at in the dimension. Usually 256.
================ ========================= ===========

Matcher examples
^^^^^^^^^^^^^^^^

* ``{'name': 'nether'}`` matches the *nether* dimension.
* ``{'respawnAllowed': 'false', 'sky': true}`` matches any dimension that has a sky and denies player respawns.

.. _worlds:

Worlds
------

Map structure
^^^^^^^^^^^^^

========= ============================================= ===========
Key       Value type                                    Description
========= ============================================= ===========
name      :ref:`String <strings>`                       The world name.
dimension :ref:`Dimension <dimensions>`                 The dimension.
seed      :ref:`Integer <integers>`                     The world seed.
gameRules :ref:`Map <maps>` of :ref:`Strings <strings>` The game rules that are active in the world. A full list of game rules can be found at the `gamerule command reference <http://minecraft.gamepedia.com/Command#gamerule>`_.
========= ============================================= ===========

Matcher examples
^^^^^^^^^^^^^^^^

* ``{'name': 'myworld', 'dimension': {'name': 'overworld'}}`` matches overworld worlds called *myworld*.
* ``{'dimension': {'name': 'nether'}, 'gameRules': {'doMobLoot': false}}`` matches any nether world in which mobs drops are disabled.

.. _block-locations:

Block locations
---------------

A block location represents a position in a world at which a block can exist.

Map structure
^^^^^^^^^^^^^

===== ========================= ===========
Key   Value type                Description
===== ========================= ===========
x     :ref:`Integer <integers>` The x-coordinate.
y     :ref:`Integer <integers>` The y-coordinate.
z     :ref:`Integer <integers>` The z-coordinate.
world :ref:`World <worlds>`     The world.
===== ========================= ===========

Matcher examples
^^^^^^^^^^^^^^^^

* ``{'x': >100}`` matches any block location that has an x-coordinate bigger than *100*.
* ``{'x': 1, 'y': 2, 'z': 3, 'world': {'name': 'myworld'}}`` matches the block location *(1,2,3)* in the world *myworld*.

.. _exact-locations:

Exact locations
---------------

Map structure
^^^^^^^^^^^^^

===== ======================================= ===========
Key   Value type                              Description
===== ======================================= ===========
x     :ref:`Floating point <floating-points>` The x-coordinate.
y     :ref:`Floating point <floating-points>` The y-coordinate.
z     :ref:`Floating point <floating-points>` The z-coordinate.
world :ref:`World <worlds>`                   The world.
===== ======================================= ===========

Matcher examples
^^^^^^^^^^^^^^^^

* ``{'y': <20f}`` matches any block location that has an x-coordinate smaller than *20.0*.
* ``{'x': 10f, 'y': -2f, 'z': 3f, 'world': {'name': 'myworld'}}`` matches the block location *(10.0,-2.0,33.0)* in the world *myworld*.

.. _data:

Data
----

Data types correspond to the `NBT tags <http://minecraft.gamepedia.com/Tutorials/Command_NBT_Tags?cookieSetup=true>`_ in Minecraft.
NBT tags are used to store the data of everything in Minecraft, for example the data of an item stack or an entity.

Possible value types
^^^^^^^^^^^^^^^^^^^^

* :ref:`Booleans <booleans>`
* :ref:`Integers <integers>`
* :ref:`Floating points <floating-points>`
* :ref:`Strings <strings>`
* :ref:`Data <data>`
* :ref:`Lists <lists>` of every type listed above

Examples
^^^^^^^^

* ``{'ench': [{'id': 'minecraft:fortune', 'lvl': 2}, {'id': 'minecraft:fortune', 'lvl': 1}]}`` matches the additional data of an item stack if the stack has the two specified enchantments.

.. _item-stacks:

Item stacks
-----------

Map structure
^^^^^^^^^^^^^

========== ================================================ ===========
Key        Value type                                       Description
========== ================================================ ===========
type       :ref:`String <strings>`                          The item type. For example *minecraft:apple*.
durability :ref:`Integer <integers>`                        The durability of the stack. This value is used as a damage indicator for tools and to distinguish item variants, for example coal and charcoal.
quantity   :ref:`Integer <integers>`                        The amount of items in the stack.
properties :ref:`Map <maps>` of :ref:`Strings <strings>`    The properties of the block type. For a list of all possible properties, see the table below.
data       :ref:`Data <data>` (:ref:`Optional <optionals>`) The additional data of the stack, for example enchantments, nametags and custom tags. For a full reference, you can visit the `<http://minecraft.gamepedia.com/Tutorials/Command_NBT_Tags#Items>`_.
                                                            This value is not present if the item stack has no additional data.
========== ================================================ ===========

Available properties
^^^^^^^^^^^^^^^^^^^^

All property values are :ref:`Optionals <optionals>`.
If a property does not apply for an item stack, it is not present.

================= ======================================= ===========
Name              Value type                              Description
================= ======================================= ===========
armorType         :ref:`String <strings>`
burningFuel       :ref:`Integer <integers>`               Defines the amount of fuel ticks an item will grant to a furnace.
damageAbsorption  :ref:`Integer <integers>`               Defines the percentage of damage the item will absorb when equipped.
efficiency        :ref:`Floating point <floating-points>` Defines the efficiency qualifier of an item when mining harvestable blocks.
equipment         :ref:`String <strings>`                 Defines the equipment type that the item can be used in an equipment inventory.
foodRestoration   :ref:`Integer <integers>`               Defines how much the level will be restored when the item is consumed.
saturation        :ref:`Floating point <floating-points>` Defines how much the item replenishes saturation upon use or consumption.
toolType          :ref:`String <strings>`                
useLimit          :ref:`Integer <integers>`               Defines a limit on the number of uses on an item.
================= ======================================= ===========

Matcher examples
^^^^^^^^^^^^^^^^

* ``{'type': 'minecraft:diamond_pickaxe', 'data': {'ench': matchAny: {'id': 'minecraft:efficiency'}}`` matches a diamond pickaxe that has an *Efficiency* enchantment of any level.
* ``{'type': 'minecraft:coal', 'durability': 1, 'quantity': >1}`` matches a stack of charcoal that contains more than one item.

Item enchantments
-----------------

Map structure
^^^^^^^^^^^^^

========== ========================= ============================================================================================================================================
Key        Value type                Description
========== ========================= ============================================================================================================================================
id         :ref:`String <strings>`   The enchantment id. For example *minecraft:efficiency*.
level      :ref:`Integer <integers>` The level of the enchantment.
========== ========================= ============================================================================================================================================

Matcher examples
^^^^^^^^^^^^^^^^

* ``{'id': 'minecraft:unbreaking'}`` matches any *Unbreaking* enchantment.
* ``{'id': 'minecraft:fortune' | 'minecraft:efficiency', 'level': >1}`` matches any *Fortune* or *Efficiency* enchantment with a level bigger than *1*.

.. _block-types:

Block types
-----------

Map structure
^^^^^^^^^^^^^

========== ============================================= ===========
Key        Value type                                    Description
========== ============================================= ===========
id         :ref:`String <strings>`                       The block type id. For example *minecraft:stone*.
properties :ref:`Map <maps>` of :ref:`Strings <strings>` The properties of the block type. For a list of all possible properties, see the table below.
========== ============================================= ===========

Available properties
^^^^^^^^^^^^^^^^^^^^

All property values are :ref:`Optionals <optionals>`.
If a property does not apply for a block type, it is not present.

================= ======================================= =======================================================================================================================================================================================
Name              Value type                              Description
================= ======================================= =======================================================================================================================================================================================
blastResistance   :ref:`Floating point <floating-points>`
flammable         :ref:`Boolean <booleans>`               Defines whether a block is able to catch fire.
gravityAffected   :ref:`Boolean <booleans>`               Defines whether a block type is affected by "gravity" such that if a block is placed and there is no block supporting it, the block will fall.
groundLuminance   :ref:`Floating point <floating-points>` Defines the amount of light on a block location.
hardness          :ref:`Floating point <floating-points>`
heldItem          :ref:`Boolean <booleans>`
indirectlyPowered :ref:`Boolean <booleans>`
lightEmission     :ref:`Integer <integers>`               Defines the amount of light emitted from the owning block.
matter            :ref:`String <strings>`
passable          :ref:`Boolean <booleans>`
powered           :ref:`Boolean <booleans>`
replaceable       :ref:`Boolean <booleans>`               Defines if a block type can be replaced by other block types when other blocks are being placed.
skyLuminance      :ref:`Floating point <floating-points>` Defines the amount of light emitted from the owning block.
solidCube         :ref:`Boolean <booleans>`               Defines whether a block type is a "solid cube" and therefore can be considered to place a torch on its side.
statisticsTracked :ref:`Boolean <booleans>`
temperature       :ref:`Floating point <floating-points>`
unbreakable       :ref:`Boolean <booleans>`
================= ======================================= =======================================================================================================================================================================================

Matcher examples
^^^^^^^^^^^^^^^^

* ``{'id': 'minecraft:stone'}`` matches the block type *minecraft:stone*.
* ``{'id': r'minecraft:.+', 'properties': {'gravityAffected': true}}`` matches all vanilla blocks which are affected by gravity, for example *minecraft:sand*.

.. _block-states:

Block states
------------

Map structure
^^^^^^^^^^^^^

========== ============================================= ===========
Key        Value type                                    Description
========== ============================================= ===========
type       :ref:`Block type <block-types>`                The block type.
traits     :ref:`Map <maps>` of :ref:`Strings <strings>` The traits of the block. All block traits are :ref:`Optionals <optionals>`. If a block trait does not apply for an item stack, it is not present.
data       :ref:`Data <data>`                            The additional data of the block, for example tile entity data. For a full reference, you can visit the `Minecraft wiki <http://minecraft.gamepedia.com/Tutorials/Command_NBT_Tags#Blocks>`_. This entry is not present if the block state has no additional data.
========== ============================================= ===========

Matcher examples
^^^^^^^^^^^^^^^^

* ``{'type': {'id': 'minecraft:planks'}, 'traits': {'variant': 'birch'}}`` matches only birch planks.
* ``{'type': {'id': 'minecraft:chest'}, 'data': {'CustomName': 'MyChest'}}`` matches chests that have been renamed to *MyChest*.

.. _blocks:

Blocks
------

Map structure
^^^^^^^^^^^^^

========== ======================================= ===========
Key        Value type                              Description
========== ======================================= ===========
state      :ref:`Block state <block-states>`       The block state.
location   :ref:`Block location <block-locations>` The block location.
========== ======================================= ===========

Matcher examples
^^^^^^^^^^^^^^^^

* ``{'state': {'type': {'id': 'minecraft:planks'}, 'traits': {'variant': 'birch'}}, 'location': {'y': >62}}`` matches birch planks above the sea level.
* ``{'state': {'type': {'id': r'minecraft:.*'}}, 'location': {'x': 1, 'y': 1, 'z': 1}}`` matches the block at *(1,1,1)* if it is a vanilla block.

.. _entities:

Entities
--------

Entities encompass all dynamic, moving objects throughout the Minecraft world.

Map structure
^^^^^^^^^^^^^

=========== ====================================================== ===========
Key         Value type                                             Description
=========== ====================================================== ===========
type        :ref:`String <strings>`                                The entity type. A list of all entity types can be found at on the `entity ID list <http://minecraft-ids.grahamedgecombe.com/entities>`_.
location    :ref:`Exact location <exact-locations>`                The current location.
rotX        :ref:`Floating point <floating-points>`                The rotation around the x-axis.
rotY        :ref:`Floating point <floating-points>`                The rotation around the y-axis.
rotZ        :ref:`Floating point <floating-points>`                The rotation around the z-axis.
vehicle     :ref:`Entity <entities>` (:ref:`Optional <optionals>`) The entity that the current entity is riding. This value is not present if the entity is not riding any vehicle.
passenger   :ref:`Entity <entities>` (:ref:`Optional <optionals>`) The entity passenger that rides the entity. This value is not present if there is no entity riding on the entity any vehicle.
baseVehicle :ref:`Entity <entities>`                               The entity vehicle that is the base of what ever stack the current entity is a part of. This can be the current entity, if it is not riding any vehicle. 
=========== ====================================================== ===========

Matcher examples
^^^^^^^^^^^^^^^^

* ``{'type': 'Creeper' | 'Skeleton' | 'Zombie' | 'Spider' | 'Enderman'}`` matches all normal-spawning and hostile mobs.
* ``{'type': 'Player', 'vehicle': {'type': 'Minecart' | 'Boat'}}`` matches players that are currently in a minecart or a boat.
* ``{'type': 'Spider', 'location': {'y': >62}'passenger': {'type': 'Skeleton'}}`` matches spiders that have a skeleton riding on them and currently located above the sea level.

.. _living-entities:

Living entities
---------------

A living entity is a normal entity with additional health values.
The living entity type inherits all properties from the :ref:`entity type<entities>`.

Additional map structure
^^^^^^^^^^^^^^^^^^^^^^^^

=========== ======================================= ===========
Key         Value type                              Description
=========== ======================================= ===========
health      :ref:`Floating point <floating-points>` The current health amount.
maxHealth   :ref:`Floating point <floating-points>` The current maximum health amount. 
=========== ======================================= ===========

Matcher examples
^^^^^^^^^^^^^^^^

* ``{'type': 'Zombie', 'health': >10f}`` matches zombies that have more than 5 hearts of health.
* ``{'maxHealth': '20f'}`` matches all living entities that can have 10 hearts of health.

Players
-------

The player type inherits all properties from the :ref:`living entity type<living-entities>`.

Additional map structure
^^^^^^^^^^^^^^^^^^^^^^^^

=========== ================================================================================= ===========
Key         Value type                                                                        Description
=========== ================================================================================= ===========
name        :ref:`String <strings>`                                                           The player name.
uuid        :ref:`String <strings>`                                                           The player UUID.
permissions :ref:`Map <maps>` of :ref:`Booleans <booleans>`                                   All current applicable permissions. If the value for a specific permission is true, then the permission is explicitly granted. If it is false, then the permission is explicitly denied. If the map does not contain a specific permission, then it is not set.
gamemode    :ref:`String <strings>`                                                           The current gamemode. Can be *survival*, *creative*, *adventure* or *spectator*.
helmet      :ref:`Item stack <item-stacks>` (:ref:`Optional <optionals>`)                     The currently worn helmet. If there is none, this value entry is not present.
chestplate  :ref:`Item stack <item-stacks>` (:ref:`Optional <optionals>`)                     The currently worn chestplate. If there is none, this value entry is not present.
leggings    :ref:`Item stack <item-stacks>` (:ref:`Optional <optionals>`)                     The currently worn leggings. If there is none, this value entry is not present.
boots       :ref:`Item stack <item-stacks>` (:ref:`Optional <optionals>`)                     The currently worn boots. If there is none, this value entry is not present.
itemInHand  :ref:`Item stack <item-stacks>` (:ref:`Optional <optionals>`)                     The item stack that the player currently has in his hand. If there is none, this value entry is not present.
=========== ================================================================================= ===========

Matcher examples
^^^^^^^^^^^^^^^^

* ``{'type': 'Notch'}`` matches the player *Notch*.
* ``{'permissions': {'somePlugin.admin': true} | {'somePlugin.somePermission': 'true'}}`` matches players that have either an admin permission or a specific permission for a plugin.
* ``{'helmet': {'type': 'minecraft:diamond_helmet'}, 'chestplate': {'type': 'minecraft:diamond_chestplate'}, 'leggings': {'type': 'minecraft:diamond_leggings'}, 'boots': {'type': 'minecraft:diamond_boots'}}`` matches players that have full diamond armor equipped.
* ``{'gamemode': 'survival', 'itemInHand': {'properties': {'burningFuel': existent}}}`` matches players that have a burnable item in their hand.