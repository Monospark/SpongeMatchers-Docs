==================
Developing plugins
==================

Why use SpongeMatchers?
=======================

SpongeMatchers can make developing a plugin much easier, since it helps you solving a very common plugin task, parsing and using config values.
A good example to show off the capabilities of SpongeMatchers would be a protection plugin.
This plugin can deny all kinds of player actions, for example placing blocks.
The config file would look like this::

  # Deny placing tnt and birch planks:
  "denyPlace": "{'type': 'minecraft:tnt'} | {'type': 'minecraft:planks', 'traits': {'variant': 'birch'}}"
  
  # Deny breaking all blocks of some mod:
  "denyBreak": "{'type': r'modName:.+'}"
  
  # Deny interactions using a damaged diamond pickaxe or an empty hand:
  "denyInteract": "{'id': 'minecraft:diamond_pickaxe', 'durability': > 0} | absent"
  
  # Deny enchanting tools with the Unbreaking enchantment with a level bigger than 1:
  "denyEnchanting": "{'id': 'minecraft:unbreaking', 'level': >1}"
  
As you can see, matchers are very powerful and also easy to understand.   
For a full tutorial about matchers and a matcher reference, you can read the previous pages of the documentation.

Getting SpongeMatchers
======================

There are 2 ways of getting SpongeMatchers:

As a dependency
---------------

If you are using a build tool like Gradle, you can easily add SpongeMatchers as a dependency to your project.
You can do this by adding the following to your build.gradle::

  repositories {
      mavenCentral()
      maven {
          url "https://repo.monospark.org/content/groups/public/"
      }
  }

  dependencies {
      compile 'org.monospark.spongematchers:1.1'
  }

As a jar
--------

If you are not into build tools, you can also download SpongeMatchers manually.
For stable versions, you can visit the `releases page <https://github.com/Monospark/SpongeMatchers/releases>`_ on GitHub.
Development builds can be found on the `continous Integration server <https://ci.monospark.org/job/SpongeMatchers>`_.

----
  
Note that SpongeMatchers is also required at runtime, so you have to shade SpongeMatchers into your plugin jar.
You can achieve this by using the `Gradle shadow plugin <https://github.com/johnrengelman/shadow>`_.

Parsing matchers
================

Matchers are usually parsed from a string that has been read from a config file.
First, you have to convert the string into a *StringElement* object.

StringElements
--------------

A `StringElement <https://docs.monospark.org/spongematchers/javadocs/latest/org/monospark/spongematchers/parser/element/StringElement.html>`_ object represents a matcher at a specific position in the string.
For example, the matcher string *[1, 2, 3]* would be converted to a `ListElement <https://docs.monospark.org/spongematchers/javadocs/latest/org/monospark/spongematchers/parser/element/ListElement.html>`_ with 3 nested `BaseElements <https://docs.monospark.org/spongematchers/javadocs/latest/org/monospark/spongematchers/parser/element/BaseElement.html>`_.
Conversion is done by calling the method `StringElementParser.parseStringElement(string) <https://docs.monospark.org/spongematchers/javadocs/latest/org/monospark/spongematchers/parser/element/StringElementParser.html#parseStringElement-java.lang.String->`_, which can throw a `SpongeMatcherParseException <https://docs.monospark.org/spongematchers/javadocs/latest/org/monospark/spongematchers/parser/SpongeMatcherParseException.html>`_ in case the string is malformed.

MatcherTypes
------------

`MatcherTypes <https://docs.monospark.org/spongematchers/javadocs/latest/org/monospark/spongematchers/type/MatcherType.html>`_ can create certain types of matchers from a *StringElement*.
For example, `MatcherType.STRING <https://docs.monospark.org/spongematchers/javadocs/latest/org/monospark/spongematchers/type/MatcherType.html#STRING>`_ can create only string matchers from a *StringElement*.
You can also create nested MatcherTypes like lists and optionals by using the methods `MatcherType.list(type) <https://docs.monospark.org/spongematchers/javadocs/latest/org/monospark/spongematchers/type/MatcherType.html#list-org.monospark.spongematchers.type.MatcherType->`_ and `MatcherType.optional(type) <https://docs.monospark.org/spongematchers/javadocs/latest/org/monospark/spongematchers/type/MatcherType.html#optional-org.monospark.spongematchers.type.MatcherType->`_.
Furthermore, it is also possible to create custom map matcher types using the methods `MatcherType.fixedMap() <https://docs.monospark.org/spongematchers/javadocs/latest/org/monospark/spongematchers/type/MatcherType.html#fixedMap-->`_ and `MatcherType.variableMap(type) <https://docs.monospark.org/spongematchers/javadocs/latest/org/monospark/spongematchers/type/MatcherType.html#variableMap-org.monospark.spongematchers.type.MatcherType->`_, which return builders.

To convert a parsed *StringElement* to a `SpongeMatcher <https://docs.monospark.org/spongematchers/javadocs/latest/org/monospark/spongematchers/matcher/SpongeMatcher.html>`_, invoke the method `parseMatcher(element) <https://docs.monospark.org/spongematchers/javadocs/latest/org/monospark/spongematchers/type/MatcherType.html#parseMatcher-org.monospark.spongematchers.parser.element.StringElement->`_ on the *MatcherType* object that you want to use.

Matching objects
================

After you successfully created a *SpongeMatcher* object from a string as described previously you can use it to check if an object matches the *SpongeMatcher* by calling the method `matches(object) <https://docs.monospark.org/spongematchers/javadocs/latest/org/monospark/spongematchers/matcher/SpongeMatcher.html#matches-T->`_.
*SpongeMatcher* objects have a generic type, which indicates what kind of objects can be matched.

Example code
============

The following examples summarize the previously covered topics of parsing matchers and matching objects::

    StringElement element1 = StringElementParser.parseStringElement("'string'")
    SpongeMatcher<String> stringMatcher = MatcherType.STRING.parseMatcher(element1);
    boolean matchesString = stringMatcher.matches(someString);
  
    StringElement element2 = StringElementParser.parseStringElement("[true, true | false]")
    SpongeMatcher<List<Boolean>> booleanListMatcher = MatcherType.list(MatcherType.BOOLEAN).parseMatcher(element2);
    boolean matchesBooleanList = booleanListMatcher.matches(someBoolean);
  
    StringElement element3 = StringElementParser.parseStringElement("{'id': r'minecraft:.+'} | absent")
    SpongeMatcher<Optional<ItemStack>> optionalItemStackMatcher = MatcherType.optional(MatcherType.ITEM_STACK).parseMatcher(element3);
    boolean matchesOptionalItemStack = optionalItemStackMatcher.matches(someOptionalItemStack);
  
    //Defined map types know every possible key name and their corresponding value type.
    MatcherType<Map<String, Object>> fixedMapType = MatcherType.fixedMap()
            .addEntry("boolean", MatcherType.BOOLEAN)
            .addEntry("block type", MatcherType.BLOCK_TYPE)
            .build();
    StringElement element4 = StringElementParser.parseStringElement("{'boolean': true, 'block type': {'id': 'minecraft:planks'}}")
    SpongeMatcher<Map<String, Object>> fixedMapMatcher = fixedMapType.parseMatcher(element4);
    boolean matchesFixedMap = fixedMapMatcher.matches(someMap);
          
    //Variable map types only define what possible type a value in the map can have.
    //They have no information about possible key names.
    MatcherType<Map<String, Object>> variableMapType = MatcherType.variableMap(MatcherType.multi()
          .addType(MatcherType.BOOLEAN)
          .addType(MatcherType.INTEGER)
          .addType(MatcherType.FLOATING_POINT)
          .build());
    StringElement element5 = StringElementParser.parseStringElement("{'boolean1': true, 'boolean2': true | false, 'integer': 1, 'floating-point': >= 3f}")
    SpongeMatcher<Map<String, Object>> variableMapMatcher = variableMapType.parseMatcher(element5);
    boolean matchesVariableMap = variableMapMatcher.matches(someMap);