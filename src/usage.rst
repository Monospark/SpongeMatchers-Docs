=====
Usage
=====

Why use SpongeMatchers?
=======================

SpongeMatchers can make developing a plugin much easier, since it helps you solving a very common plugin task, parsing and using config values.
A good example to show off the capabilities of SpongeMatchers would be a protection plugin.
This plugin can deny all kinds of player actions, for example placing blocks.
The config file would look like this::

  # Deny placing tnt and birch planks:
  denyPlace: "{'type': 'minecraft:tnt'} | {type: 'minecraft:planks', traits: {'variant': 'birch'}}"
  
   # Deny breaking all blocks of some mod:
  denyBreak: "{'type': r'modName:.+'}"
  
   # Deny interactions using a damaged diamond pickaxe or an empty hand:
  denyInteract: "{'id': 'minecraft:diamond_pickaxe', 'durability': > 0} | absent"
  
   #Deny enchanting tools with the Unbreaking enchantment with a level bigger than 1:
  denyEnchanting: "{'id': minecraft:unbreaking, 'level': >1}"
  
As you can see, matchers are very powerful and also easy to understand.   
For a full tutorial about matchers and a matcher reference, you can read the :doc:`user documentation </user/index>`.

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
          url "http://repo.monospark.org"
      }
  }

  dependencies {
      compile 'org.monospark.spongematchers:<version>'
  }
  
Note that SpongeMatchers is also required at runtime, so you have to shade SpongeMatchers into your plugin jar.
You can achieve this by using the `Gradle shadow plugin <https://github.com/johnrengelman/shadow>`_.

As a jar
--------

If you are not into build tools, you can also download SpongeMatchers manually.
For stable versions, you can visit the `Releases page <https://github.com/Monospark/SpongeMatchers/releases>`_ on GitHub.
Development builds can be found on the `Continous Integration server <https://ci.monospark.org/job/SpongeMatchers>`_.
Note that SpongeMatchers is also required at runtime, so you have to include SpongeMatchers when exporting your plugin.

Matcher parsing
===============

Matchers are usually parsed from a string that has been read from a config file.
First, you have to convert the string into a *StringElement* object.

StringElements
--------------

A *StringElement* object represents a matcher at a specific position in the string.
For example, the matcher string *[1, 2, 3]* would be converted to a *ListElement* with 3 nested *BaseElements*.
Conversion is done by calling the method *StringElementParser.parseStringElement(string)*, which can throw a *SpongeMatcherParseException* in case the string is malformed.

MatcherTypes
------------

*MatcherTypes* can create certain types of matchers from a *StringElement*.
For example, *MatcherType.STRING* can create only string matchers from a StringElement.
You can also create nested MatcherTypes like lists and optionals by using the methods *MatcherType.list(type)* and *MatcherType.optional(type)*.
Furthermore, it is also possible to create custom map matcher types using the methods *MatcherType.definedMap()* and *MatcherType.undefinedMap()*, which return builders.

Examples
--------

::

  MatcherType<String> stringType = MatcherType.STRING;
  StringElement element1 = StringElementParser.parseStringElement("'string'")
  SpongeMatcher<String> stringMatcher = stringType.parseMatcher(element1);
  boolean matchesString = stringMatcher.matches(someString);
  
  MatcherType<List<Boolean>> booleanListType = MatcherType.list(MatcherType.BOOLEAN);
  StringElement element2 = StringElementParser.parseStringElement("[true, true | false]")
  SpongeMatcher<List<Boolean>> booleanListMatcher = booleanListType.parseMatcher(element2);
  boolean matchesBooleanList = booleanListMatcher.matches(someBoolean);
  
  MatcherType<Optional<ItemStack>> optionalItemStackType = MatcherType.optional(MatcherType.ITEM_STACK);
  StringElement element3 = StringElementParser.parseStringElement("{'id': r'minecraft:.+'} | empty")
  SpongeMatcher<Optional<ItemStack>> optionalItemStackMatcher = optionalItemStackType.parseMatcher(element3);
  boolean matchesOptionalItemStack = optionalItemStackMatcher.matches(someOptionalItemStack);
  
  //Defined map types know every possible key name and their corresponding value type.
  MatcherType<Map<String, Object>> definedMapType = MatcherType.definedMap()
          .addEntry("boolean", MatcherType.BOOLEAN)
          .addEntry("block type", MatcherType.BLOCK_TYPE)
          .build();
  StringElement element4 = StringElementParser.parseStringElement("{'boolean': true, 'block type': {'id': 'minecraft:planks'}}")
  SpongeMatcher<Map<String, Object>> definedMapMatcher = definedMapType.parseMatcher(element4);
  boolean matchesDefinedMap = definedMapMatcher.matches(someMap);
          
  //Undefined map types only define what possible type a value in the map can have.
  //They have no information about possible key names.
  MatcherType<Map<String, Object>> undefinedMapType = MatcherType.undefinedMap()
          .addType(MatcherType.BOOLEAN)
          .addType(MatcherType.INTEGER)
          .addType(MatcherType.FLOATING_POINT)
          .build();  
  StringElement element5 = StringElementParser.parseStringElement("{'boolean1': true, 'boolean2': true | false, 'integer': 1, 'floating-point': >= 3f}")
  SpongeMatcher<Map<String, Object>> undefinedMapMatcher = undefinedMapType.parseMatcher(element5);
  boolean matchesUndefinedMap = undefinedMapMatcher.matches(someMap);