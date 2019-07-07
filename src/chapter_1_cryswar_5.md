# Creating a New Faction
## By Cryswar

#### Tools & Preface

First of all, I would like to be brutally blunt here: startpos modding sucks. This isn't like normal modding where you can easily reverse engineer other mods to figure out how they work, and most errors have a clear or clear-ish cause. If anything is wrong, Dave and Bob may or may not let you know at all, compiling may or may not fail without any useful reason, and even if it does succeed the game may crash during loading for no apparent reason. I'm not trying to discourage you from trying it - it's not that HARD - but it can be time consuming to get the hang of and even for startpos experts it can be a pain.

------

With that all said, let's discuss what faction creation is. It's typically called startpos modding because it uses the startpos.esf file, which dictates the initial setup of a new campaign. Many things can be changed mid-campaign, like building costs or recruitable units, but startpos is essentially baked into the campaign from the start - it's how the game knows what settlements exist and where, where to initially put lords, what units they have, who is playable, etc.

Startpos editing cannot be done in RPFM as usual; we are instead forced to use the Assembly Kit, usually called Dave. If you hover over your Library button near the top of steam, then select Tools you'll probably see a huge list of crap no one cares about. You will also, however, see something called Total War Warhammer 2 Assembly Kit, which you will need to install.

When you start it up, it'll ask which editor you want to use; BOB, Dave, or Terry. Dave is database editing, Bob is compiling, Terry is map editing. We will be using both Bob and Dave in this walkthrough and ignoring Terry.

You will also need [RPFM][rpfm] as in... basically every guide I write. While almost all of faction creation can be done in Dave, some is a lot easier and faster in RPFM, and you'll need to use RPFM to edit tables and entries in a way that allows your mod to be compatible with literally anything bloody else.

#### Warnings 2: Electric Boogaloo

Ok ok I know I just tried to scare y'all off of doing this but I wanted to add one more thing here: even making a very basic vanilla faction requires knowledge of a lot of different stuff, like X to effects junction tables and potentially making custom lords, units, etc. I have guides for each thing but can't (and won't) explain each in detail here.

Making COMPLETELY custom factions like a Vampire Counts horde faction will require far, far more tables than are listed in this guide - potentially up to needing to make an entirely new military group for something like a custom hobgoblin khanate faction. I DO NOT recommend it to start off with.

#### Getting Started: The Basics of Dave

First off, we're going to start Dave. Dave is kind of a jerk, and loud too, but we need him, so pretend to like him just long enough to get this done.

If this is the first time you're using Dave, the first thing you'll want to do is to look at the top bar, hit Connection, and then Connect. Most likely it'll tell you you're already connected to the database, and that's fine, we're just making sure.

After that, View->Table Launcher will get us into the data editing side of things. Experiment a bit with the search bar. It's extremely intelligent, you can type in any word in the table's name and it'll display all tables with that word in it. You rarely need to type a full table name, just maybe 2-3 words (including the underscores!) and pick the one you need out of the list.

If you're familiar with RPFM you may notice some tables here aren't visible in RPFM; Dave is a great search engine for certain things RPFM can't handle, like existing animations and animation to skeleton links for example. You may also notice there are a number of tables with start_pos in the name; we don't need most of them but will definitely need a fair few!

We're gonna start with the `factions` table in the next section anyways, so open that now, but before we start filling it out I want to discuss a few things.

Note the **filters** line above the rows - hit that little + button to open a filter. Filters are very powerful and you will use them A LOT. If you check the "not" box then it filters out anything you enter in that box, otherwise it'll search for cells with that word/words. Make sure to filter column based on what you're doing, some tables have an **ID** column that it defaults to that is basically useless so you may want to switch to the **Key** column in that scenario, or maybe you want to search by **subculture** and copy in `wh_main_sc_brt_bretonnia` to see what all the bretonnian factions are. Maybe you should add a second filter of **Lyonesse** with the "NOT" checkbox marked so it gets rid of Lyonesse but displays all other Bret factions. Some tables make this very useful, others don't need it much/at all.

#### Part 1: Assembly Kit

**factions**

This table is the start of everything - it will get called a million times by a million other tables, so we're starting with the basics.

Now, for your first lesson in startpos modding: plagiarism is okay! Kidding aside, what I found helped me learn was to use 1-3 filters to get the displayed list down to only a single faction of the same race and gameplay style I wanted to make my faction - for example, since I learned startpos modding making custom Chaos factions, I used one filter for **wh_main_chs_chaos**, which limits it down to 6 rows, then added a second filter of NOT **chaos_**. In other words, it shows all factions with a key containing wh_main_chs_chaos, but not any of the factions that have anything after that stuff, like wh_main_chs_chaos_qb1, wh_main_chs_chaos_rebels, etc.

As long as you copy most of the fields from a similar vanilla faction, this table is EXTREMELY easy, and you can edit it in RPFM later to change most of it on the fly - so don't stress TOO much about picking the right flag or color scheme or whatever. This guide DOES NOT cover making 100% custom factions using unique military configs or military groups etc, it's not impossible but trying to cover that many tables in one guide would give me a heart attack and I don't love y'all THAT much.

That all said, make sure you remember the **faction key**; Dave won't make you retype the full thing (and you SHOULDN'T do it in most tables anyways) but you will want to make it easy to remember since you'll want to type 1 word from it a lot. I'd suggest keeping it simple, memorable, and unique. Something like `chs_talons_of_tzeentch` or `emp_cult_of_shallya` and then in future tables where you have to call it, just start typing `talons` or `shallya` to very quickly narrow down the search.

====

**frontend_faction_leaders**

While all frontend tables are editable in RPFM, you do unfortunately need to faff about with this one in Dave since it's attached to some startpos stuff. Fortunately it's a pretty simple table, same as last time just copy everything but the key from the closest existing faction. Most of these won't even show up ingame and will need to be done manually in RPFM anyways, especially the locs.

Note that if you want your faction to be playable in both Mortal Empires AND Vortex, you will need two distinct entries here with different keys! You can use whatever naming scheme you want but I recommend doing something simple, like **sarthorael_ruler** and **sarthorael_ruler_vortex**.

====

**effect_bundles**

Another random table, I know. We're not gonna worry too much about it now, but you do need to fill out a key here for the lord's faction trait (ex. **lzd_gorrok_trait**) to fill in a table later.

====

**Custom Lords (optional)**

No that's not a table name. Anyways, the rest of this entry can be ignored if you are using a vanilla legendary or generic lord as the faction leader.

That said, if you want to use a custom lord as your new faction's leader, you will need to have dummy entries with the correct keys in several tables - as far as I can tell NONE of these are optional to startpos for a custom lord, they HAVE to be mentioned here. You don't have to fill them out well, just the key - you can edit the rest in RPFM later. And I suggest doing so because that way you don't have to make custom weapons, armor/shield types, etc. in asskit which just makes Bob exporting take even longer.

Here are the tables you will need, in order that they need to be done. Keep in mind that you will need to fill these out for any custom lords/heroes you want to startpos! You DON'T have to worry about this if you're going to add a new lord to the recruit pool via script for example.

* land_units
* main_units
* agent_subtypes
* character_skill_node_set
* names

Everything else about a custom lord can be done in RPFM without adverse effects, however these tables are required - without character skill node set for example, a custom faction leader will never be able to access his skills ingame. Why? I don't know. But that's how it works, learned that the hard way trying to get the asskit list of tables as short as possible.

====

**start_pos_factions**

Yay, finally, an actual startpos table!

Everything here but the faction name can and should be copy pasted from the nearest vanilla faction, half the fields are deprecated or useless anyways. 

However, note that for ME+Vortex factions, you will need two rows here, one for each campaign. They can (and should) use the same faction name, just have one of them use **main_warhammer** for the **campaign** cell and the other with **wh2_main_great_vortex** campaign.

====

**start_pos_characters**

If you're using a custom lord, NOW you see why I had you fill out like 6 different tables a bit earlier.

At any rate, most of this table is simple and self explanatory. You can't copy-paste nearly as much of it though, but it's not too hard to figure out that "oh hey they want a faction name HMMMM IF ONLY I HAD MADE A FACTION A FEW SECONDS AGO". Remember that for a faction playable in both ME/VO you will need two rows, each linking to the start pos faction you made in the last section.

`startx` and `starty` are extremely important - unless you're starting the lord in a settlement with another table, these two fields define where your lord starts in the world. Unfortunately the X/Y coordinate system is upside down and freaking backwards so the only way I know of to really do it is either rip off a vanilla lord and slightly offset (ex. a custom chaos faction starting 1 north and 1 east of the normal WoC start) OR, far better option, visit the C&C modding discord I link in every guide, go to **modding_resources**, download the maps created by Marthenil/posted by Vandy, and then follow the instructions there. You can just open them in normal MS paint, it works, I promise lmao.

Also be sure that the agent_subtype and override_general_unit point to your desired lord, if you made a custom lord point to them. Not hard, just DO NOT screw it up.

====

**start_pos_starting_general_options**

NOW you know why I had you fill out effect bundle and frontend faction leaders earlier!

This table is very simple and straightforward despite the obscene verbal diarrhea of the first few sections - all you really have to do is just start typing the faction name or character name and autocomplete in the general/replaces general cells, then fill in the rest with effect bundle key and frontend faction leader key.

Note that if you are making a faction with multiple lord options, you will need to worry more about precedence and WHO they replace. For example, look at the Empire - three entries here; one General for each playable character, but they all replace karl franz. Franzy boy is precedence 0 with the others trailing him as 1 and 2. You'll need different political leaders and effect bundles for them, too.

As usual, if you're making a ME+VO faction, you'll need two rows per lord - one for ME and one for Vortex.

I personally take the route of only having 1 playable lord for a custom faction and using scripts to add extra lords to the faction. Less work overall and I can do the entirety of the secondary lords in RPFM. You do you.

====

**start_pos_land_units**

Nice and simple table - this is the generic set of units that you start with, regardless of which starting lord you pick. They do not vary based on which lord you pick, we'll touch on how to do that next section.

In vanilla this table typically has from 3 to 10 entries per faction per campaign type, one for each unit that everyone starts out with. You can fill this out however you want, just try to keep some degree of balance in mind, and also costs - starting with 4000 upkeep worth of expensive stuff is rough.

As usual, for ME+VO factions you need to fill this out with two different sets of units for the Mortal Empires and Vortex starts of a faction. Be sure you link the correct start_pos_characters IDs!

====

**start_pos_starting_general_option_additional_units**

THIS table is what decides that Franz gets to start with Reiksguard whereas Gelt gets a Mortar and Greatswords, and Volkmar gets Flagellants. Usually 2-3, though it doesn't HAVE to be. It also decides which units display on the frontend when you select a lord.

If your faction only has a single startpos'd lord, you can just toss 2-3 flavorful units for them in here. Don't stress it too much. And if you have custom units you want to start with, don't stress that either! Starting units are by far the easiest stuff to script so you can completely ignore them in asskit, or make a skeleton entry for them here just to include them here then fill them out properly in RPFM.

====

**start_pos_regions (Settled Factions Only)**

Unlike most tables, you will probably not add any new rows here, just edit existing ones.

This table decides who owns which settlements, its slot cap, and what kind of rebels spawn here. Remember that there are entries for both ME and vortex versions of some settlements - so be sure you're editing the right ones! If you're making a ME+VO faction, remember that some settlements may be in very different places on different maps or only exist on one but not the other.

I personally find it very helpful to google campaign map images to help keep the overall world in my head.

====

**start_pos_settlements (Settled Factions Only)**

A fairly straightforward table, it just decides where settlements are and what buildings they start with. If you're significantly changing who owns what (ex. giving a former Norscan territory to a custom Empire faction) then you may have to look around a bit to see what buildings replace which.

blabla ME/VO as usual.

====

**start_pos_characters_to_settlements (Optional)**

I personally like using startx/starty coordinates in start_pos_characters to decide where a lord starts, but if you don't wanna mess with that you can use this table instead. Set the startx/starty coords in start_pos_characters to 0, 0 and attach whatever characters and settlements you like here. If you changed settlement ownership, make sure the previous owner's lord doesn't start here! You may have to edit vanilla entries here to fix that.

blabla ME/VO honestly that's getting old to say every time

====

**start_pos_horde_details (If Horde)**

Obviously this is only relevant for horde factions, but it lets you set stuff like initial population surplus, primary building, and any secondary buildings they start with. Overall very simple, the dropdown list and smart searching makes this pretty easy to do.

As usual, for ME+VO factions you'll need to fill this out for both campaigns.

====

**faction_to_mercenary_set_junctions**

This table defines RoR availability, and MUST be asspossessed for your faction to have access to units! You cannot add new RoRs to the menu in play, it has to be done here, or attached to buildings.

====

**start_pos_diplomacy**

This table isn't strictly necessary, but it's a fun one - it decides starting relations between various factions. You can start at war, with a NAP, with a military alliance, whatever. As of this writing it is the only way other than vassalization for a horde faction to have a military access pact as well.

While you can tick multiple boxes in a given row (ex. Military Alliance+Military Access), you will need additional rows for each different pair of factions, and as usual you will need to do all of it for both ME and Vortex campaigns if your faction is meant to be played in both.

====

**start_pos_character_ancillaries (Optional)**

While most factions don't bother with this one, some definitely do. Boris's starting cloak, Alarielle's starting Horn of Isha and Stave of Avelorn, and Grimgor's Da Immortulz banner are all granted here. You can also give generic ancillaries like the Mark of Khorne away.

Anecdotally speaking I have found that it doesn't like you giving multiple of the same kind of accessory - you get ONE mark of khorne but not two. Different types work fine, though.

====

**Victory Conditions (Optional)**

You can absolutely make a faction and play it without them, but if you want to have victory conditions, you need to work for it.

`Steam\steamapps\common\Total War WARHAMMER II\assembly_kit\raw_data\EmpireDesignData\campaigns` is the rough location you're looking for, then in each of the **main_warhammer** and **wh2_main_great_vortex** folders are separate text files called `victory_objectives.txt`. I would recommend Notepad++ to see the formatting better.

Actually filling it out is pretty much just a question of copying vanilla entries and substituting your desired keys.

====

**Bob the Builder**

Yeah I went there, whatchu gonna do about it?

Anyways, you can skip this section for now and do most of the rest of the guide in Dave too if you want, but at this point we've filled out the bare minimum of tables that MUST be startpos'd in order to function, so I personally prefer to export now (Export->Export Changes to Binary) and then swap to RPFM for the rest. If you want Dave to be your wedding man then just come back to this section later.

At any rate, now we select export in Dave, wait for it to finish, and then close. It'll remind you to save your work because DAVE is goofy, you can ignore that and just close out, and open up Bo- I'm kidding, first of all we're going to follow good practice and disable ALL mods in the vanilla launcher. All of them, but especially startpos mods; this can mess up your compiling if you don't.

Also make sure the game is not running.

Okay, NOW we can open Bob. When it finally starts up 20 years after you click on it, it's going to be a pretty incomprehensible menu full of nonsense I'm not even going to try to explain. All you have to do is look for Campaigns under Working Data (the middle section), and then instead of opening Campaigns, click on the empty white box next to it. A little menu will pop up. Depending on if your mod is only for ME or Vortex or for both of them, "Process Startpos" for one or both campaigns, and if you filled out victory conditions then fill out one or both of those boxes as well. Then check the Pack / Create Pack File box in the section just below.

After that, you can hit "Start" in the very bottom right of the window, and wait anywhere from a couple to potentially 20+ minutes for the game to start up (and repeat if you're doing ME+vortex) and compile the entire startpos with all of your changes. If it fails, you messed something up... probably. There are no useful error messages, and no one is gonna be able to immediately tell you what you did wrong. But CA also breaks asskit every so often, usually after major patches. So there's very little advice I can offer here, just wear your big girl panties and look through your tables to see if any of them calls a Vortex character for a ME faction or something.

When it finishes, the mod will be in something along the lines of `Steam\steamapps\common\Total War WARHAMMER II\assembly_kit\retail\data`. All of the tables will include all vanilla entries and be named data, so you'll want to sterilize them as with any mod - run RPFM's Special Stuff->Warhammer 2 ->Optimize Packfile utility to remove vanilla table entries and rename tables to something else for compatibility.

As a note - you CANNOT rename startpos.esf or it will stop working. Full stop.

#### Part 2: RPFM

Unfortunately, your faction is NOWHERE near playable right now, and even if they were, you wouldn't be able to select them in the front end faction select screen, and ingame they'd have no siege vehicles, take attrition from basically everything, can't recruit any heroes... etc etc.

Also, if you used placeholders for your custom lord leader, they are like 1/10 of an actual character right now, with no art. And that's... bad.

So from here I'm going to assume you have the .pack file created, cleaned out, and both pack and tables are renamed and moved somewhere else so you can work on it.

====

**battle_siege_vehicle_permissions**

This is a very simple table - it just decides what kind of battering rams and siege towers your faction can create. Just copy from a similar vanilla faction unless you want to do some weird mix.

====

**campaign_map_attrition_faction_immunities**

Most factions are by default immune to `non_vampire_territory` attrition and unless yours is a Vampire Counts faction, yours probably should too - otherwise you take attrition basically everywhere. Similar for other factions with special kinds of corruption, ex. wood elves are immune to `athel_loren` attrition.

Beyond that varies WILDLY by faction. For example, Norscan, Chaos, and some event factions (ex. Blood Voyages) are all immune to `nurgle_plague` and `disease`, which are respectively the Norscan worldwide plague and Skaven rite plague I believe, but basically no one else is - whereas 'normal' attritions like `Snow` and `Desert` are often given to SOME members of a race but not others.

In the end, fill it out however you want, I would generally suggest roughly following vanilla examples though.

====

**campaign_mp_coop_groups_to_factions (Vortex only)**

You know how in Vortex coop, everyone has to be the same race? This table is why. If you make a new High Elves faction in the Vortex map, toss 'em into this table and people can coop it.

Note that you CAN create new groups! Using the table **campaign_mp_coop_groups** just define a new entry and then junction the factions as desired. Using that, you can add say a Chaos group and add a couple of playable Chaos factions in Vortex, which forces everyone to play Chaos.

You could also data core this table to completely remove all Vortex race restrictions, but that's not really the point of this guide lol.

====

**campaign_stances_factions_junctions**

This is where you can edit what stances a faction has access to - for example, Alith Anar's Tunneling stance availability comes from here. Blabla rip off a vanilla faction unless you want something special. Just be aware that you may not be able to put together every stance you want, anecdotally speaking it seems like it only allows you to have a single variation of a given stance at once.

====

**climbing_ladders_meshes_animations**

Without this table, your guys will climb invisible ladders. Doesn't break anything, just looks bad.

====

**faction_agent_permitted_subtypes**

Another fairly simple table, this just lets you recruit agents of each type. Can mostly copy from vanilla; if you're making a custom Chaos faction you don't need to invent entirely new agents, just reuse Exalted Heroes and Chaos Sorcerers.

That said, keep in mind that any lord you want to be able to recruit must be represented here too! For example, vanilla Chaos includes the agent subtypes for all their legendary lords (and Sarthorael), the Empire has access to Volkmar, Karl, and Gelt, etc. If you have any custom lords or heroes, give them entries here too.

====

**faction_banners**

This just changes the colors of your banner. Not too hard just basic RGB stuff.

====

**faction_uniform_colours**

SURPRISE! MORE COLORS!

====

**frontend_factions**

This is primarily for where your faction is in the faction select order.

====

**political_parties**

Just more setup. Should be fairly simple, but remember to make a row for each of the Mortal Empires and Vortex versions of your faction.

====

**faction_political_parties_junctions**

Bruh this is probably the most obvious table in the entire guide, I'm pretty sure you can figure this out by now.

Nah seriously though, this is asking for faction not frontend faction, but doesn't matter if you named 'em both the same.

====

**faction_to_faction_groups_junctions**

just copy vanilla lol

====

**political_parties_frontend_leaders_junctions**

Honestly I'm running out of things to say for these super short and easy tables. Just link political parties to frontend leaders. As usual, if you have ME/VO versions, one row for each.

====

**frontend_faction_groups_to_factions**

This table covers a few things; if it's an Order or Destruction faction, major or otherwise (don't think that matters after CA removed major faction AI autoresolve bonus), and sort order on the frontend faction select.

*Vandy note: major/order don't matter at all, all of this data affects only the frontend and has no affect on the campaign. It was probably gonna be some different grand UI in the frontend that split factions between 'Order' and 'Destruction', and major factions and tiny ones*

====

**frontend_faction_effect_groups**

This table is where you create different effect groups for Lord and Faction bonuses, for both ME and Vortex if needed.

Note that these ARE NOT THE BONUSES YOU GET INGAME - just what the game says you'll get in faction select!

====

**frontend_faction_effect_junctions**

Works the same as any X to effect junction table; attach effects, give scope and value. Remember that these are solely visual and solely for faction select, so don't spend too much time worrying about getting the right scope here. It just needs to look OK.

====

**effect_bundles_to_effects_junctions**

We created an effect bundle way back near the start of the guide and attached it, and now we can finally use it! This table decides what your factionwide buffs actually are, all that frontend crap was just for show. So scope is VERY important here - everything has to ACTUALLY work not just look good.

You can use custom effects here just fine, and that's why I recommend doing this in RPFM rather than asskit.

Note that all effects here need to use faction_to_x scope, which is very rarely used elsewhere, so be sure to check similar vanilla entries for usage tips if you're confused, and be sure to test ingame later!

====

**units_to_exclusive_faction_permissions**

You can ban or allow specific RoRs from a given faction in this table. Pretty simple, 99% of the time you can just copy from vanilla and allow all.

====

**units_custom_battle_permissions (Optional)**

If you're making a campaign faction you may very well not give a damn what they look like in custom battles, but for completeness sake you MAY want to spend some time filling this out for every unit the faction has access to.

====

**Custom Lords (Optional)**

Well it says optional but if you're using a custom lord as faction leader it's really not lol. You have to make your guys NOT placeholders. I... sincerely hope you already know what you're doing if you are, but if not, have no fear! I already wrote guides on how to make a custom LL from scratch; the basics of creating the unit are here and filling out all the agent related stuff and lord art/portholes/etc are here

#### Localisation

Believe it or not, there actually aren't too many necessary .loc files for all that, unless you made a full custom lord of course but those are covered in the relevant guides. I'll just list the universally needed ones alphabetically over the next few sections.

If you aren't familiar with loc files, the simple version is that they are where you actually define the text that shows up ingame - DB files generally don't actually matter in that regard.

====

**effect_bundles.loc**

This one is nice and simple; you just need `effect_bundles_localised_title_` and `effect_bundles_localised_description_` filled out. Add a little lore, give a fancy name, have fun!

If you aren't familiar with how loc files work.... well we're pretty late to be touching on that I guess lol, but the basic idea is that you use the generic parlance at the start (ex. `effect_bundles_localised_description_`) and then append your key to it, so if your key is say, **karl_trait**, the final version would look like `effect_bundles_localised_description_karl_trait`. Might take a little getting used to but it's not too hard once you get the hang of it.

====

**factions.loc**

* **factions_screen_name_** - Faction name. Self explanatory.
* **factions_screen_adjective_** - usually just the race, like Chaos or Skaven
* **factions_defend_desc_** - something like You are defending against the Blades of Khorne!
* **factions_attack_desc_** - something like You are attacking the Talons of Tzeentch!
* **factions_screen_name_when_rebels_** - something like Faction Name Rebels.

====

**frontend_factions.loc**

This is where you set up most of the text on the left of character select in singleplayer. Not too complex, honestly most of it can just be copy-pasted from a vanilla faction and then edited a bit if desired for yours.

frontend_factions_localised_info_
frontend_factions_localised_mechanics_
frontend_factions_localised_playstyle_

====

**names.loc**

I go over this in more detail the lord guide, but tl;dr if you are making a custom lord with custom names, the loc is where you attach actual words to the ID you defined earlier back in assembly kit.

#### Polishing

Ok, assuming you filled everything out right, your faction is 100% playable right now. Probably. That doesn't necessarily mean it's perfect though! See if the faction works, and look around to see what's busted. Missing text? Frontend units not showing up? No access to RoRs? Lord taking attrition in normal territory? Some will require you to go back to DAVE and edit, then recompile in BOB.

If you have to recompile, you only need to get the startpos.esf(s) from the remade packfile - so long as a few things (ex. faction ID) are the same. So you can just open your existing packfile, delete the startpos.esf in it, and add from the newly compiled pack.

Note that in multiplayer games, you automatically get a preview of start positions on the dynamic map. For singleplayer lord selection, unfortunately, you don't. However! You can do it with photoshop. Cataph has a handy little template in modding_resources in the C&C modding discord I link below.

#### Troubleshooting

Unfortunately, pretty much anything involving startpos modding is a pain in the butt to troubleshoot. You will rarely get anything useful from Bob and while Dave isn't TOO bad about completely wrong entries, it also doesn't tell you about every possible clash. So a lot of troubleshooting is just gonna be basic and boring, looking over tables trying to find one magic bullet entry. Still, I'll share solutions to a few issues I've run into over time.

__BOB doesn't compile__

* Make sure game, Kaedrin's mod manager, vanilla launcher, etc are not running when you start compiling.
* Make sure all mods are disabled in mod manager, especially other startpos mods!
* Go back in and look over all your tables. Honestly, 99% of the time, it's this.

__Game crashes__

* This is generally a DB issue, though WHAT DB is... fun to figure out. If in RPFM, make sure dependency checker is on and look for red entries. Also check to make sure that if a table is asking for a land unit, you didn't enter a different main unit key or something.
* If doing a normal faction, check to be sure there's no clashing entries in asskit - that you don't share ownership of a settlement with another faction, or that they don't have a lord that starts in that settlement.

__I have no idea what is going wrong__

* Welcome to my life! Seriously though, go through the DBs... again.
* Download a mod that creates a single faction and look at their DB tables in RPFM. You can't see their startpos entries, but can at least check and see how they filled out, say, frontend factions.

#### Conclusions and Acknowledgements

Making a new faction can be a lot of fun. It can also be a journey to the pits of hell and endless troubleshooting. Or both! At once! If you got it working though, congratulations - not many people can.

While I mostly figured out how2startpos on my own, I would be absolutely remiss in not linking the [C&C modding discord][cnc] for many times I got help along the way! Special thanks to **Vandy** for the many, many things he's helped me out with along the way as I learned to mod.

I'd also like to thank **Crynsos** (of faction unlocker fame) for helping me figure out that RoRs and character skill node sets had to be startposed, it was driving me friggin' crazy.

**Mixu** is also a complete beast; I'd especially like to shill his new-ish faction unlocker which makes all his dozens of custom lords playable. He also helped me with some kinks in lord creation a while back, tho that's not super relevant to this specific guide.

[rpfm]: https://github.com/Frodo45127/rpfm/releases/
[cnc]: https://discord.gg/xs6DMbn