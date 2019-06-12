# Commands, Interfaces & Listeners

For today's lesson, the subject is three-fold: **commands**, **interfaces**, and **listeners**. The three of these are our introduction to messing with the game itself, and making real change. Every worthwhile Lua mod on the market has these in it! Anything else just concatenates strings or applies arithmetic or some nonsense.

#### Commands

A command, at its bearest-bones, is a *CA-defined Lua function* that does stuff that we can't see. They function (hah) identically to the functions we've already covered; some have parameters, some have return values, they all do stuff. Whenever we use commands, we're treating them like *function calls*; they're a statement. Commands are used to communicate between the Lua environment and the actual Total War Engine - the base C++ code that defines the majority of the game.

Through these commands, we can script a whole lot of stuff! From character spawns, to new effects applied to characters and factions and armies, to quest battles, to unlocking rites, and so on.

In big part, commands make or break what can be done with scripts. Commands are the driving force of most any script. At this point, though, you might be asking - "how the HECK do I find out what the commands are?" And that, my friend, is why you're reading this lesson. Or maybe you got lost when you were just trying to find funny videos about cats. Either way, next paragraph!

So, once more in the ["Tools & Resources"](./chapter_1.md) page on this site, there's a section within called "Scripting Resources". We'll look at, first, the "Scripting Doc (CM Functions)" file. Open it up!

Now, before we go on, one thing MUST be covered. **EVERY command in this document needs to have a prefix of `cm:`.** Those two letters stand for "campaign_manager", and it's the big object that holds most all information we could want within the campaign game-mode.

***NOTE:*** *The campaign manager is ONLY usable in the campaign game-mode!*

In this document, let's take a look at two example commands to breakdown.

```lua
    Command: 	transfer_region_to_faction	
    Description: 	Transfer a region ownership to a faction.	
    Usage: 		transfer_region_to_faction("region", "faction")
```

This is relatively straightforward. The `Command:` line tells you what to affix to `cm:`, so this would be `cm:transfer_region_to_faction`. Since it's a *function call*, which we learned about just last episode and you wouldn't dare forget so soon, we add the brackets to it: `cm:transfer_region_to_faction()`.

We can see in `Usage:` that it takes TWO parameters. They seem to be strings, one of which says "region", the other "faction". It's easy enough to tell that means "the region key in string", and "the faction key in string". So, overall, the command call would look like this: `cm:transfer_region_to_faction("wh2_main_vor_phoenix_gate", "wh2_main_hef_eataine")`.

```lua
    Command: 	create_force	
    Description: 	create a navy or army at a position.  Final parameter specified whether the command goes via the command queue	
    Usage: 		create_force("faction_key", "unit_list", "region_key", x_position, y_position, exclude_named_characters, "id", true)
```

The same applies here. `cm:create_force()`, with a bunch of parameters. However, there's one caveat here - using cm:create_force() with those parameters will crash-bang-boom the game! Let's back up and talk about why.

The commands in this document are commands to the *game interface*, something that you really should never, ever touch directly, for fear of crashing the game if you mess something up. These functions -`cm:create_force()` - are *wrappers*, over that base command, so we don't have to directly touch the game interface. Using the CA functions will quickly error-check our code, and prevent any such crashes (when wrappers for those commands exist!). But what do I mean? Let's open up our script dump and go to script/_lib/lib_campaign_manager.lua, and search for `create_force(`.

We'll see a line that looks similar to what I have already shown you - `function campaign_manager:create_force(faction_key, unit_list, region_key, x, y, exclude_named_characters, success_callback)`. This is the *wrapper*. Within this function, lower down at the bottom, we can see the call to the game interface - `self.game_interface:create_force(faction_key, unit_list, region_key, x, y, id, exclude_named_characters, true);`. This looks a lot closer to the definition CA has in their documentation, and I belive it was the definition they were trying to go with. Not sure though, it's still a *little* wrong!

In these situations, it pays to be careful. I advise you to use the CM Scripting Doc cautiously. It isn't fool-proof, and every change that CA has made over the years hasn't been recorded in there - there are commands listed that haven't been functional since Total War: Empire. This is when I point you back to the [setup lesson](./chapter_3_1_0.md), if you haven't done that, and tell you to go do it. There are three hyper important tricks in there that will help you understand what to do in a situation like this:

1) Check the lib files! Most functions in this document will be in `lib_campaign_manager.lua`. Bonus, as you could see with the above example, CA gives a quick explanation for what the function does and what the types are, and where they are found. This is always my first check. Do note that not every campaign_manager function is listed in that file; there are a bunch that are defined in the game-engine, have no wrapper in this lib file, and aren't documented.

2) Use the script dump! All you have to do is use "Find In Files ... " within Notepad++, type in the name of the command (ie. `create_force`), and wait for it to search all the files. It will come up with some hits, and you can read through CA scripts to see what they do and how it's used. This is really handy for any undocumented functions, or if you'd like to get ideas on how a command is used. I do this all the time!

3) Use Kailua! It might take some time to fully understand the `ca_types.lua` file that we've made for TW:WH2, but it should be pretty easy to *read* it if you want to know the parameters of a function. Open `ca_types.lua`, within VSCode (or whatever), search for the name of the command again (`create_force`), and it will show you some neat stuff. I'll go over what it looks like now, just so you know:

```lua
--# assume CM.create_force: method(
--#     faction_key: string,
--#     unitstring: string,
--#     region_key: string,
--#     xPos: number,
--#     yPos: number,
--#     exclude_named_chars: boolean,
--#     callback: (function(CA_CQI))?
--# )
```

Each of these lines is a *parameter*, and it's split into a name and a type. The name is just a description of what should go there - a faction key, a region key, etc., - and the type defines, of course, the type!

There's a third example I'd like to go into, which is return types via the CA commands. It works just like regular return values (seen from the [last lesson](./chapter_3_1_7.md)), but sometimes the return value is vague, and it's really hard to read using CA's scripting doc. Again, utilize the script dump and Kailua. In Kailua, it'll look a little different - let's grab an example of a command with a return value.

```lua
--# assume CM.get_saved_value: method(valueKey: string) --> WHATEVER
```

*NOTE: `WHATEVER` is a beautiful keyword in Kailua that lets you not define the type of a value, instead of saying "return a number" it says "return literally whatever"*

Some functions in the ca_types files have a `-->` to their end; on the right side of that arrow is the return value. Easy enough to read!

And that's my explanation of CA commands! Hope you had as much fun as I did.

#### Interfaces

Similar to commands, script interfaces are a way to access some game data with some Lua. We looked at them previously - in the second lesson, we used a `FACTION_SCRIPT_INTERFACE`, which we got by using the command `cm:get_faction("faction_key")`.

Script interfaces are objects that we can apply some functions *to* (functions applied to an object are known as methods, but we'll talk about that terminology later). For example, let's build up something small:

```lua
    local faction_name = "wh2_main_hef_eataine"
    local faction = cm:get_faction(faction_name)
    if faction:is_dead() == true then
        -- sadface
    end
```

In this example, `cm:get_faction()` has a *return value* of a `FACTION_SCRIPT_INTERFACE`, which we assign to the local variable `faction`. `FACTION_SCRIPT_INTERFACES` have a method called `is_dead()`, which lets us read if the faction is, well... dead. But how did I know to use that? *gasp*

Also similar to commands, script interfaces are documented with a slightly-questionable CA file, though this one is typically waaaay more reliable, if a bit inconsistent with some naming. We can, again, grab it from the [Tools & Resources](./chapter_1_0.md) page, this time under the title "Scripting Doc (Events & Interfaces). Our big focus for now is the latter half of that name - Interfaces - though we'll be getting into events right in the next section below!

Open your newest Scripting Doc, and search for `FACTION_SCRIPT_INTERFACE`. It should be towards the top of the document, right near the word "Interfaces" in big boldage. Like this: **Interfaces**. Cool, found it? Click it!

You should get a big, long list of blue links, followed by more clear definitions. A typical entry here would look similar to the follows (because the following *is* an entry):

```lua
Function: holds_entire_province
Description: Does this faction hold the entire specified province? Also may include vassals
Parameters: holds_entire_province(String province, bool include_vassals)
Return: bool
```

As before, like the commands above were done with a prefix of `cm:`, these interface functions are done with a prefix of that script interface. But it won't just be `FACTION_SCRIPT_INTERFACE:holds_entire_province`, that makes no sense! In our previous, Tyrion-based example, it would look like the following:

```lua
    local faction_name = "wh2_main_hef_eataine"
    local faction = cm:get_faction(faction_name)
    if faction:holds_entire_province("wh2_main_vor_phoenix_gate", true) then
        -- do stuff!
    end
```

The second line - description - is quite obvious. It's just a description. Read it, don't read it, I don't care.

Third line: Parameters! We talked about this recently, you remember it right? It'll be a type and a very brief stand-in name to explain what should go there. In this case, you can tell that it wants the key of the province, as a string, and a boolean that determines whether or not you'd like to include vassals in the check. Sometimes, they'll just put the stand-in name in quotation marks to show that it should be a string.

Return values are much the same. Sometimes there will be returns of other script interfaces, and that's how you can start getting some fun stuff. With a `FACTION_SCRIPT_INTERFACE`, you can use `character_list()` to get a `CHARACTER_LIST_SCRIPT_INTERFACE`, and then use `item_at()` to get a `CHARACTER_SCRIPT_INTERFACE`, and then use `get_forename()` to get a string. That'll look like:

```lua
    local faction_name = "wh2_main_hef_eataine"
    local faction = cm:get_faction(faction_name)
    local character_list = faction:character_list() -- assign the CHARACTER_LIST_SCRIPT_INTERFACE to the variable "character_list"
    local character = character_list:item_at(0) -- assign the CHARACTER_SCRIPT_INTERFACE, at index "0", to the variable "character"
    local forename = character:get_forename() -- assign the string for the character's forename to variable "forename"
```

I will note that you don't have to assign everything a local variable here. It can look, albeit much uglier and less safe, like this:

```lua
    local faction_name = "wh2_main_hef_eataine"
    local forename = cm:get_faction(faction_name):character_list():item_at(0):get_forename()
```

But I do not recommend doing something like this, until you're very comfortable with the language. I do it very, very, very rarely, only when it makes perfect sense to do it, and that's lie-talk for "when I'm tired and it makes perfect sense to my tired brain".

Script interfaces are stupidly, stupidly powerful, and using all these smartly allows you to make some serious change within the game, and to read a lot more details, from "how many units are in this army" to "what level is this character" to "how many armies were in the last battle?" But, above all, these interfaces are used *best* and *most powerfully* when used in conjunction with listeners and events.

What are listeners and events? Ah, well...

#### Listeners and Events

A listener is an important script command that, simply, waits for something to happen - or, listens for something to happen - in the game, and then triggers something else. Using a listener, we can listen for an event such as the creation of an army, the occupation of a city, or even a battle, and then we use the information from that situation, check for something specific, and run our code if the event matches our criteria. For example: we may be looking for Mannfred to occupy Altdorf. In this case we'd have to use a listener to "listen" for a city being occupied, but we don't want to listen for just any city being occupied. In our example, we have to listen for Altdorf being occupied, and for Mannfred doing the occupying. The listener will listen for every single time a city is occupied, and once those two conditions are met - *Mannfred* occupying *Altdorf*... **Clue**, anyone? - then our listener will trigger some code. In layman's code, our listener will *do a thing!*

Overall, it is a very valuable connection between Lua and game data, and any scripter should understand the in's and out's of a listener.

Let's break down the basics of a listener. Here is it, at its most basic form:

```lua
    core:add_listener(
        "listener_name",
        "event_name",
        boolean1,
        function(context)
        end,
        boolean2
    )
```

- "listener_name": Can be whatever you want it to be! (Keep in mind if you share the same listener_name as another listener, and that name is called with core:remove_listener("listener_name"), one of those listeners will be removed, and it'd be hard to tell which would be. Use unique names!)
- "event_name": This must be one within a set of Events that CA defines. You can view this [list here][events_and_interfaces]. We'll cover this list more in depth later on.
- boolean1: This is the "conditional" boolean. If it is set to true, the function to execute will execute; if it is set to false, the function will not execute.
- function: This is the "callback" function, which only runs if the conditional boolean returns as true. Can be whatever valid function you want! Note that it MUST have the format of `function(context) --[[ do stuff ]] end,`
- boolean2: This is the "persistence" boolean. If it is set to true, the listener will continue listening. If it is set to false, the listener will be removed from the game data.

### CA Events

Let's talk more in detail about the Events I mentioned.

As previously clarified, Events are predefined objects that are used exclusively in tandem with Listeners. A full list of Events, and all of their methods, and all of the interfaces, can be found [here][events_and_interfaces]. Let's go over how to use this document!

At the top of the document is a long list of Events, each clickable to divert to another part of the document. Each of these Events have specific usage. It should be noted here that documentation on some events is rather light, ie. when to use certain ones versus others, but a lot can be found out by looking at vanilla scripts or other mods.

I want to start a new function after the player takes over Altdorf. After searching the term "Occu", the event "GarrisonOccupiedEvent" is highlighted. This looks promising! After clicking, the available functions are "garrison_residence" and "character", which seem to be the region that was occupied, and the character that led the assault, in order.



### Conditional Coding

Remember "boolean1" from up above? If that is set to true/false, it's not really effective code. We can replace that true/false with a function that returns a true/false value, and that way we can actually check the event, see if a specific thing happened in that event, and then spit out a boolean to trigger the callback function.

```lua
	function(context)
		return context:[thing we want to check]
    end,
```

In essence, the above is the foundation for what all effective conditional functions in a listener should be. Context, in the context of listeners, is an object which contains all the data that occurred within the Event. But let's back up. Remember on the document, we found `GarrisonOccupiedEvent`? Let's go back to it and click it again. We see two options, as mentioned before - "garrison_residence" and "character". As our intention is to find a specific region being occupied - Altdorf - we need to use that former path.

At this point, our WIP code looks like the following:

```lua
	function(context)
		return context:garrison_residence()
    end,
```

For the conditional function, you always want to start with "return context:", and the next method can only be one of the methods that is accessible by that Event - which is where this document comes SO in handy. For other examples, click around the different Events listed at the top. Each "Function" underneath each Event after clicking it is a valid method for the context of that Event. The "RegionSelected" event only has access to a "region" function, which means the only valid method is to use "context:region()". "DilemmaChoiceMadeEvent", however, has access to "faction", "choice", "campaign_model", and "dilemma", so those four are all valid methods.

***NOTE: Yes, you have to add "()" to the end of the function's name.***

Back to our example - you'll see the garrison_residence has `GARRISON_RESIDENCE_SCRIPT_INTERFACE` beneath it. Looking in the "GARRISON_RESIDENCE_SCRIPT_INTERFACE", I don't see a "key" or "name". Matter of fact, the most likely path is "region()". That brings our code to...

```lua
	function(context)
		return context:garrison_residence():region()
    end,
```

After checking the `REGION_SCRIPT_INTERFACE`, we finally see the "name" method, which allows us to fully check our context. Our line of code is finally built to:

```lua
	function(context)
		return context:garrison_residence():region():name() == "wh_main_reikland_altdorf"
    end,
```

But that's not all! All this does is to check if the Altdorf region is being occupied, but it says nothing about Mannfred! We'll go back to the very beginning - find the GarrisonOccupiedEvent on the document, and return to its functions, which were garrison_residence() and character(). We want to build an "and" sentence for our conditional, so that it only triggers the function when Altdorf is occupied **AND** when it's occupied by Mannfred. Let's try to plot our trajectory a little. We should try to find a script_interface that allows us to reference an agent_subtype key, which is the cleanest and most mod-friendly way to search for Mannfred. Looking at `CHARACTER_SCRIPT_INTERFACE`, I see two functions - character_subtype() and character_subtype_key(). The latter is used to return a string - ie., it doesn't check for a specific subtype key, but it will tell us what the characters subtype key is. We need to *check* for a specific subtype key, "vmp_mannfred_von_carstein".

```lua
	function(context)
		return context:garrison_residence():region():name() == "wh_main_reikland_altdorf" and context:character():character_subtype("vmp_mannfred_von_carstein")
    end,
```

Note here that the first line of context, that ends in ":name()", needs to have an equal-to check because we don't want to return the *name* of the region, but we want to return a value of true or false if the name of the region is equal to the string we define - "wh_main_reikland_altdorf". We can read this as "return true if the region's name is Altdorf, and if the character is Mannfred; else, return false."

A strong conditional function is very important to writing good listeners, as listeners can be very performance-heavy if there are many triggering at once, and the more that are whittled away via a strong conditional, the better performance will be. Be mindful of your conditionals!

### Callback Functions

At this point in the walkthrough, we have about 60% of our listener completed! It's not bad, let's take a quick look at what we actually have written:

```lua
core:add_listener(
	"MannfredOccupiesAltdorf",
	"GarrisonOccupiedEvent",
	function(context)
		return context:garrison_residence():region():name() == "wh_main_reikland_altdorf" and context:character():character_subtype("vmp_mannfred_von_carstein")
	end,
    function(context)
        -- code to execute
    end,
    boolean2
)
```

I'd like to point out a couple quick notes. To start off - yes, your listener_name can be anything you want, as long as it's a string. You need a comma after *every argument* in the command, of which there are five that are detailed way at the top - listener_name, name, conditional boolean, callback, and persistence boolean. The code above is written the way it is because that's a common application of it, and allows it to be easily read, but realistically this can all be written on one line.

Alright, so we are on the function to execute block, known as the callback function. Here, we have much more freedom than the conditional function, as we can do whatever is needed with this block. For instance, we can have a callback function that triggers an event (like a dilemma or a message), or grants a unit/experience/ancillary, or unlocks something like a Rite or a Legendary Lord. For our instance, I want to just give Mannfred's army a new unit and a new effect bundle.

***NOTE: For our sake, we're going to assume the new unit and new effect bundle are already defined.***

Looking up in our [commands document][cm], I see a grant_unit_to_character to command. Checking vanilla usage, I see it takes two args - a char_lookup_str and a unit_key string. The unit_key string - which refers to the main_units_tables - is, in our situation, `tutorial_big_bat`. To find a char_lookup_str, we have to first find the character's cqi.

*For more information on char_lookup_str and cqi, go to [the CA types page][ca_types]*

Thankfully for us, we can go back to where we were before and use the context of the Event to find all the info from before. Looking at the `CHARACTER_SCRIPT_INTERFACE`, we see a direct link to cqi, with the method "command_queue_index()". For this, we define our local values as such:

```lua
	local char_cqi = context:character():command_queue_index()
    local char_lookup_str = cm:char_lookup_str(char_cqi)
```

With this, we find the CQI first of the character, and then apply that to find the character's lookup string, which is the type needed for grant_unit_to_character.

Before we apply that command though, we should find the arguments we need for apply_effect_bundle_to_force(), which is our command that will give an effect bundle to Mannfred's army. We first need our effect bundle; we'll use `tutorial_free_hair`. The second argument is a military force CQI, and our third argument is a number of turns. We'll make it five turns long.

To find the military force CQI, we use something similar as above. We will go from character to military force, and then find the CQI there. We can use something like the following:

```lua
    local mf_cqi = context:character():military_force():command_queue_index()
```

Alright, now we have all we need to build our callback function! Let's mush it all together real quickly.

```lua
function(context)

	local char_cqi = context:character():command_queue_index()
	local char_lookup_str = cm:char_lookup_str(char_cqi)
	local mf_cqi = context:character():military_force():command_queue_index()
	
	cm:grant_unit_to_character(char_lookup_str, "tutorial_big_bat")
	cm:apply_effect_bundle_to_force("tutorial_free_hair", mf_cqi, 5)

end,
```

And that's a quick walkthrough on how to assemble your callback function! Please note, you can have whatever you want here, and there's nothing saying you *must* restrict yourself to the context of the event. As an example, I can do the same thing without context, like so:

```lua
function(context)

	local mannfred_faction = cm:get_faction("wh_main_vmp_vampire_counts")
	local mannfred = mannfred_faction:faction_leader()
	
	local char_cqi = mannfred:cqi()	
	local char_lookup_str = cm:char_lookup_str(char_cqi)
	local mf_cqi = mannfred:military_force():command_queue_index()
	
	cm:grant_unit_to_character(char_lookup_str, "tutorial_big_bat")
	cm:apply_effect_bundle_to_force("tutorial_free_hair", mf_cqi, 5)

end,
```

This takes a little bit longer than using context, but it is a solution to applying commands to objects that aren't involved in the context. So if, for example, you wanted to give all Vampire factions - aside from Mannfred's own - an `effect_bundle` that increases their diplomatic relations with Mannfred after he conquers Altdorf, you can. The world is your oyster when it comes to listeners.

### Persistence

One last thing, and it's gonna be much quicker than the last two. The final boolean - persistence - can be set to true, or false. True means that the listener will fire more than once; false means that, once the entire code is run through, the listener is removed from the game state and won't be called again. This only gives two options - once, or infinite. There is a third option, by using core:remove_listener("listener_name"). At whichever point this code is called, the listener with the matching "listener_name" is removed from the game state, and won't be called again. A remove_listener statement like this can be used after enabling a counter that listens for the listener firing five times, or to cancel the listener if the turn is beyond 100, or much else.

Keep in mind that, to "fire", only the conditional needs to return true. If there's more if statements in your callback function, and they fail, but your persistence is set to false, the listener will remove itself.

```lua
core:add_listener(
    "my_listener", 
    "GarrisonOccupiedEvent", 
    true, 
    function(context) 
        if this_is_done then
            --stuff happens
        end;
    end, 
    false
)
```

So this listener will cancel after one use if "GarrisonOccupiedEvent" is triggered, regardless of whether or not "this is done" returns as true or false.

### Summary

Overall, there's a lot of power to understanding these big three, and it takes your Lua knowledge from "adding and multiplying numbers" to "affecting the game in a real, and personable way". Make sure this stuff is good and mastered, and if you ever find confusion, come back and reread this stuff.


[events_and_interfaces]: files/scripting_doc.html
[cm]: files/scripting.txt
[ca_types]: chapter_3_1_9.md