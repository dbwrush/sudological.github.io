---
layout: post
title: Advancement Command has dropped.
description: My first commissioned plugin!
---
New Minecraft plugin! 
This plugin allows you to run console commands when a group of advancements are made.

Advancements can be grouped into 'sets', which are defined using the config.yml file.

When all the advancements in a set are made, the commands for that set will run.

The PlaceholderAPI plugin, found <a href="https://www.spigotmc.org/resources/placeholderapi.6245/">here,</a>
is required since many commands will require a player's name or some other contextual information.

An example config is below, and this is generated automatically if the plugin runs without any config.
Note that advancements should include the namespace.

```
# Configuration file for AdvancementCommand plugin

#setting to true will cause the plugin to print extra information which may be useful for debugging.
#debug mode will spam your console! Turn it off when you're done.
debug: false

# Define sets of advancements and commands
sets:
  #set names can be anything! They will be printed to console in debug mode!
  set1:
    #namespaced paths for every advancement needed for this set
    #other namespaces are possible if you have them
    Advancements:
      - "minecraft:story/craft_planks"
      - "minecraft:story/upgrade_tools"
    #commands that will be run if the set is met.
    #commands should not include the beginning /
    #be sure to install any PlaceholderAPI expansions that are required.
    #in debug mode, warnings will be displayed about possibly missing expansions.
    Commands:
      - "give %player_name% minecraft:diamond_pickaxe"
      - "tellraw %player_name% {\"text\":\"Congratulations on completing the set!\"}"
  set2:
    Advancements:
      - "minecraft:story/smelt_iron"
      - "minecraft:story/enter_the_nether"
    Commands:
      - "give %player_name% minecraft:diamond_sword"
      - "tellraw %player_name% {\"text\":\"You have completed the second set!\"}"

```

<br>
You can download the plugin <a href="https://www.spigotmc.org/resources/advancementcommand.109739/">here.</a>
