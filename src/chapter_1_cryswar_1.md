# Creating Custom Units

#### Preface & Tools

**__Preface__**

The basics of creating new units are fairly easy and fast, but alien for someone just starting modding. While there are already a few unit creation guides out there (including [Sebidee's][sebidee]), there are often issues with them being out of date or (imo) laid out confusingly, and I have a few other guides I'd like to do that build off of this, custom lords and skills and such, so I figured I'd start out the series with a simple unit creation guide.

**__Tools__**

You will only really NEED one tool - [Rusted Pack File Manager][rpfm], hereafter referred to as RPFM. It's basically a better and actively updated version of the classic Pack File Manager (PFM) with a lot of useful tools like dependency checkers and MyMod functionality that I mostly won't go heavily into here.

The one thing I will stress is to get the **Dependency Checker** going ASAP; `ctrl+P` to open Preferences, check "Enable dependency checker for DB tables", and (if you didn't do this on initial startup) make sure that your Total War Warhammer game is correctly linked in Packfile->Preferences as well as that going to Game Selected and ensuring it's set to Warhammer 2. Dependency manager will catch most simple errors for you.

I also suggest having two instances of RPFM open at once when modding, one opening the Data file to compare with vanilla tables and one for your mod. I usually keep a notepad file open as well with just the unit key so I can copy-paste it easily.

As one final note - you don't HAVE to follow the exact order I did this guide in. Many things can be done in any order, though you will often run into red dependencies temporarily if you do stuff in a weird order. I tried to lay everything out in the way I found most natural and easy to do, but if you find you REALLY like doing main_units before land_units... suit yaself.

#### Cheat Sheet

While I will go down the list and explain what to do with each, I'm going to share my shortform list of tables for unit creation; once you get comfortable with unit modding you'll generally only need this rather than the full guide.

**Basic Tables**
- land units
- main units
- ui_unit_bullet_point_overrides
- unit_description_short_texts
- unit_set_to_unit_junctions
- units_to_groupings_military_permissions

**Visual Stuff**
- variants
- unit_variants
- variantmeshdefinitions
- unit icon

**Utility Tables**
- units_custom_battle_permissions
- cdir_military_generator_unit_qualities
- culture_to_battle_animation
- building_units_allowed

**Localisation**
- land_units.loc
- unit_description_short_texts.loc

#### Initial Setup and Naming Conventions

First things first, we're going to create a new packfile (Ctrl-N). This gives us a blank pack with no tables or name. If this is your first mod, I would suggest saving it now, NOT in the data folder, with a simple name. There are a lot of different naming conventions out there; some people like prefacing their stuff with their username, some just name everything after the mod's name, whatever. Ideally you do want it to be unique so it doesn't run into incompatibility issues (don't name it mod.pack lmao).

Note that you can muck with mod order by adding characters to the start, it's common to see packs with ! or !!!!! at the start to ensure they load first. I would not recommend doing this unless you know exactly what you're doing, it's more of a tool for startpos mods and other major overhauls that really want to be loaded first; for most purposes something basic will do just fine.

While you may not find this works for you, I personally use the same key as much as possible throughout unit creation - if I name a land unit crys_skinwolves, you had damn well better believe the unit variant will be named crys_skinwolves, the main unit will be called that, etc etc. I just find it helps keep things simple, especially when you get into creating agents (lords/heroes) and start needing to attach campaign art and skill sets and all that jazz, and it's just REALLY nice to just have to copy-paste "chs_styrbjorn" instead of remembering 20 different things.

#### Adding Tables

There are two ways to go about adding tables to your mod, both perfectly valid.

- Right clicking on the packfile->add->add from packfile->navigate to Steam\steamapps\common\Total War WARHAMMER II\data->open data.pack->open the DB folder->doubleclick on the tables you want to add, then delete all entries in the table and rename it
- Right clicking on the packfile->create->create DB and fill it out

For simple DB editing purposes, the latter is infinitely faster, easier, and simpler, with the added benefit of letting you name the table and not having to clean it out. However, it is good to know the former because for some purposes, like variants, you will need to do it that way.

If you do add a db table the first way for whatever reason, an easy way to clean it out is to use Special Stuff->Warhammer 2->Optimize Packfile.

---
**land_units**

For now, let's just create a new table. The bottom box lets you filter results; type in "land_units", then open the dropdown list - there's only 4 tables there; "land_units" is the one you care about. Add it and name it whatever you like, the game doesn't care as long as it doesn't clash with vanilla table names or other mod tables. Now rightclick and select "add row", and... oh jesus lord what the hell are all these things?!

Fret not, you don't actually have to know all of them! This is where having a second window of RPFM open in the data version of the table is useful. Just copy the row from the closest vanilla unit to get us started. At the bottom right is a dropdown menu to change what to filter by; change to "key" and then type in the unit name you want to copy from, which may take a few tries depending on CA's weird naming conventions. In general, only type in 1 word, like "champions" instead of messing around trying to figure out how CA decided to name Marauder Champions with Great Weapons (wh_dlc08_nor_inf_marauder_champions_1 as it happens).

Some important notes:

- Key is incredibly important - it should be unique and not shared with any mod or vanilla unit. I recommend NOT using CA conventions, keep it simple; if you're making Chaos Fimir then chs_fimir is a decent start, though for compatibility you may want to add more text, say crys_chs_fimir. Whatever you use, save it now and keep it handy somewhere else, you'll be copy-pasting that key a lot in this guide.
- When in doubt, just use the entry from the closest vanilla unit. Good rule of thumb. You don't have to know what the heck dr2_hydra is, but if the vanilla war hydra uses it as an animation, your budget hydra probably should too.
- "Man Entity" references the battle_entities table, which controls unit speed, mass, resistance to knockback, etc. Many vanilla entities are shared between many units; if you want to mess around I'd suggest making custom entities.
- Primary Melee/Missile weapons are fairly self explanatory in what they do; making new melee weapons is almost trivially easy, new missile weapons take some extra tables for projectile stuff but isn't that hard either.
- New attribute Groups have to be defined in unit_attribute_groups and attached to actual attributes in unit_attributes_to_groups.
- Note that you have to either reference vanilla armor/shield values or define new ones in their respective tables - you can't just fill in wh2_main_leather_88 and expect it to work.
Damage mod \[whatever\] is the unit's resistance to that type of damage. Flame can be negative, the rest can't as far as I can tell.

---
**main_units**

Similar to how you added the land_units table, do the same for main_units and copy the closest vanilla unit over.

As a general rule of thumb, the Main Units table is more focused on out of battle stuff - unit costs, caps in campaign or multiplayer, additional building requirements, etc.

- Change the "unit" field to your personal key, ideally the same one you used for your land unit.
- Additional Building Requirement is used if you want the player to have a secondary building to make the unit, and is left blank otherwise. References the building_levels table if needed.
- Campaign Cap is -1 for most units, but can be limited as desired if you only want a faction to be able to make, say, 6 Steam Tanks total
- Land Unit is a direct reference to the land_units table entry you just filled out, make sure you have your unit's key there.
- Unique Index should be, well, unique. Not much else to say there. Just pick a number that isn't in vanilla.
- Ui Unit Group Land/Naval can be customized if you make a new one in UI Unit Groupings, but using vanilla ones is much easier starting off.

---
**ui_unit_bullet_point_overrides**

Despite having an utterly terrifying name, this is just the table that assigns those little green/red text lines on the unit card. Note that these DO NOT HAVE ANY MECHANICAL EFFECT, they're solely visual, but you still want to make them as accurate as possible since they're the #1 way you can communicate to users what your unit actually does. You can make custom ones as well but again, that gets into loc editing and if this is your first unit it's probably easiest just to copy and paste from the closest vanilla unit.

---
**unit_description_short_texts**

Super simple table, just put your unit's key (chs_fimir or whatever) here. This is the stuff that occasionally pops up in events when you recruit a unit, just flavor text.

---
**unit_set_to_unit_junctions**

Ah, unit sets, love of my life. Much maligned and little understood, unit sets are a huge part of how your unit interacts with campaign mechanics; they define what technology, lord skills, faction effects, etc. work on them. If you're just making, say, tier 4 Empire Halberdiers you can just copy the same unit set junctions from the empire version; new units may take a little more work and especially if you have custom skills targeting specifically them you may need to define some additional unit sets (explained below) as well. Kinda as usual, Unit Record references the main_units key. As usual, this is ezpz if you just use the same key for everything LOL.

Unit Set can be a bit confusing at times, but as a loose rule of thumb, there are some main types of groups to look out for;

- Oftentimes there's a faction_all set, ex. chs_all, that is used for some factionwide effects
- Some techs and skills may involve unique sets, like empire_infantry or chs_manticores. Try to match up against vanilla units as relevant.
- The format varies by faction, but most units will be in a set of 2-4 units with a name vaguely similar to wh2_dlc11_emp_spearmen_swordsmen_flagellants_halberdiers or something along those lines. That's for the basic red line skill that buffs those unit types. If you put your new unit in that set, they will benefit from those redline buffs.
- Similarly, there's usually a set with "vet" somewhere in the name, ex. wh2_dlc11_chs_vets_chariots_chaos_knights_dragon_ogres_warhounds_manticore. That is for the rank 7+ locked skills.

You can generally ignore caste, category, and class, and unless you WANT to exclude a unit from a set, leave "exclude" unchecked too.

As a general rule of thumb, most vanilla units are in 4-6 unit sets.

Note that if you make a custom unit set (using the **unit_sets** table), you can replicate the vanilla rank 7+ groups by checking "use unit EXP level range" and defining the inclusive range, say from 7-9 or 4-9 or whatever. Otherwise leave that unchecked.

---
**units_to_groupings_military_permissions**

This table is pretty simple; aside from starting to attach variants together (more on that soon) it also lets you scale your units up or down as desired. "Scale" lets you change the side of non-mounted entities as desired, while "Scale Variation" is how much randomization there is within a unit, often 0.05 in vanilla so not everyone is exactly the same height.

Note that the actual physical height of the unit, for purposes of being hit by artillery etc, is defined in battle_entities, NOT here, this is just visual.

Tech folder can be left blank in almost all situations - it's only needed if you're making a custom mount which is A. complex as hell and B. WAY out of the scope of this guide.

---
**unit_variants**

This table just links units to variants, cards, etc. I just use the same key for every column to keep things simple.

---
**variantmeshdefinitions**

This is where things get tricky. So the basic idea is that you are going to import a variantmeshdefinition from one of the "variants" files in the actual data folder, then rename it for your unit and customize what's in it. The complexity of this depends a lot on the unit and how many moving parts there are, but as some general commentary;

- Any given variantmeshdefinition is divided into multiple slots, usually stuff like "body" and "head" and "weapon 1". 
- If a given slot has multiple variant meshes inside it, the game will randomly pick one for each model of the unit. Five entries - five possible visuals for that slot. On lower graphics settings the game only cares about the first three, so keep that in mind.
- Weapons have a very specific set of hooks that must be done correctly. As a simple version;
- Dual wielders define their weapons in slots 2 and 5 individually
- Two-handed weapons go in slot 1
- Shields go in the shield slot, huge surprise
- Units with multiple weapon types (ex. Marauder Hunters, Darkshards, etc) vary WILDLY in where each weapon is and should generally be referenced from vanilla

Beyond that, different models have very different setups - for example a vanilla variantmeshdefinition may or may not have a randomized head slot or just the one. You can mess around and do some seriously cool stuff but be aware that a lot of stuff is built specifically for one model or animation and can be hard to muck with.

As usual, just copy-pasting from a vanilla unit's definition is a safe start.

I already mentioned this above, but to repeat, make sure to rename the file to fit whatever you set up in the variants db table so the game knows to link this definition to your unit.

---
**Unit Icon**

Unit icons are the visuals on the bottom bar of campaign map or in battle. You can find vanilla ones in data/ui/units/icons.

Depending on what your unit is, you can just steal an existing vanilla one wholesale (ex. if you're giving Fimir to the Warriors of Chaos), or edit it to look a bit different, or use an entirely new image. The only real rules here are that it should be 60x130 and NOT use any copyrighted material from other people that isn't warhammer related.

Some people are wizards with art and can customize an existing icon in photoshop/gimp to look amazing, others of us are dain bramaged and stuck taking ingame screenshots of the unit then doing some basic editing to focus on the unit and get them down to 60x130, then using "add->add file" to get that icon into the correct folder layout.

While creating custom lords is out of the scope of this guide and will be focused on later in the series, I will note that they actually store their icon in a different place, specifically ui/portraits/units.

***NOTE FROM VANDY:*** *You can also check out [Cataph's guide on unit cards][unit_card].*

---
**units_custom_battle_permissions**

This is a completely optional table but one that is very useful for testing units quickly. Overall very self explanatory, just define the faction(s) that can get this unit and link the unit key.

---
**cdir_military_generator_unit_qualities**

Another optional table, this one loosely defines how the AI views the unit's quality, which decides how likely it is to actually recruit it.

As a general rule of thumb, the lower the number the more likely the AI is to pick it. Don't stress it too much, just rip off the closest unit from vanilla and call it a day.

---
**culture_to_battle_animation**

This table is only necessary if you're using a cross culture animation - for example, a Wood Elf animation for a Norscan unit. Some factions share cultures with similar ones (ex. Norsca and Chaos don't need to worry about this, the Norscan Berserker animation will work fine on a custom Chosen unit) but if you're trying to give Bretonnians a War Hydra unit, you will need to use this.

The "Man Animation" column in land_units is the animation you'll want to put here.

---
**building_units_allowed**

This table allows your unit to be recruited in campaign, using the cost and recruit time values designated in main_units. Pretty self explanatory overall, just note that you need a unique string of numbers for the Key and that EACH level of a building needs a separate entry. For example, if you want to add your new custom Empire Swordsmen to all levels of the Chaos Warrior building (why????), you have the unit key, ex. "emp_my_awesome_swordsmen", and have three entries for "wh_main_horde_chaos_warriors_1" "wh_main_horde_chaos_warriors_2" "wh_main_horde_chaos_warriors_3" that each attach to your swordsmen.

If you don't do this and only define it at say, "wh_main_horde_chaos_warriors_1", then while the chaos warrior building is at rank 1 you'll be able to recruit your new unit there, but as soon as it upgrades to 2 or 3 you'll permanently lose access to recruiting your unit.

---
**land_units_to_unit_abilities**

Thankfully this is another simple and easy table - you just link your **land unit** (you DO remember your key, right?) to a unit ability - you can either compare across in the vanilla version of this table for a common one (ex. Frenzy) or check **unit_abilities** if you want to add another ability.

#### .loc Files

You could load up your mod now and see the unit ingame, assuming you didn't muck anything up, but you'll quickly notice there's a problem - the unit's name, description, and (if you made any) ui unit groupings etc. are all blank! That's because you need localization files, which can be found in the "local_en" file in the data folder. You add them the same way you do a normal table, importing from that file and purging the unclean entries, and same as with DB you'll want a second instance of RPFM open to compare.

---
**land_units.loc**

This may look ugly at first, but is very easy to setup thankfully!

So the first important thing to know is that your Key will ALWAYS start the same way: "land_units_onscreen_name_". Then you append your unit's key. So if your unit is "crys_naked_skinwolves", you end up with a key that looks like "land_units_onscreen_name_crys_naked_skinwolves". Kinda long and intimidating but whatever.

For the Text column, just put in the name you want to show up. "Unarmored Skinwolves" or whatever.

---
**unit_description_short_texts.loc**

You remember about 80,000 years ago when we added that one super small table about unit descriptions? This is where you write the actual description. 

Same as with the last .loc, you preface your unit key with "unit_description_short_texts_text_"

I don't think there's TECHNICALLY any length cap, I've seen some seriously gnarly long descriptions, but for practical reasons I'd suggest keeping it concise.

---
**Other Loc Files**

Unless you went crazy with customizing stuff you won't need any extra loc files for a generic unit, but if you're like me and can't stop with the verbal diarrhea, you may need to add more, like "ui_unit_groupings.loc" if you made a new UI group for your unit to add extra flavor text to them.

#### Troubleshooting

First thing you always do: after making sure that your preferences are set up correctly, **look for red boxes**! It's almost always the reason the game crashes or the unit doesn't work right, and even as an experienced modder you can still make dumbass mistakes and leave the "shield" column blank instead of "none". Speaking from experience on that one.

.loc files will almost never cause a crash (apparently there are a few edge cases with some tables) but in general, mucking up loc files will only lead to invisible text rather than crashes.

Variants USUALLY only make your unit invisible instead of crashing but it depends on what you mucked up there - be sure that you named the variantmeshdefinition correctly for example.

#### Balancing

This is on many levels a deeply subjective and heavily mod reliant subject so I won't tell you what you have to do here, but as a few loose rules of thumb;

- I highly recommend using vanilla reference points for most numbers - if your unit is just a set of Fimir with, idk, shields, then price them around other Fimir, maybe a bit higher if they have all better stats, or similar if they have tradeoffs.
- Similar to the above - tradeoffs are fun! If your unit has incredibly powerful offense, making them squishy or short ranged can make up for it. If you're making 80 speed Irondrakes riding boars, strip off most of their armor. Maybe your new custom Hellcannon hits harder but fires slower.
- AP damage is a weird subject that not even CA seems to know exactly what to do with, given that they've been slowly buffing a number of duelist characters to have a much more AP heavy profile. As a general rule of thumb, if the unit doesn't have a good reason to be AP heavy, don't make them too AP heavy by default; almost all weapons have a healthy balance of AP or not, 99 ap damage and 1 normal can be a fun gimmick but if all your units look like that and have no downsides, they're probably OP.
- Balance is friggin' hard, and you shouldn't feel bad if you aren't sure about it initially! I'm by no means an expert myself, just try testing stuff and see how it feels in practice. Some ideas work out great, others don't.

#### Closing Note

This is my first TWW2 guide and I am under no illusions about myself being a flawless master of my craft - I am sure there are parts of this guide that will be confusing for newcomers where I accidentally used unnecessary lingo or forgot to explain what the heck to do. Please, let me know! Nicely if possible, I would greatly appreciate that part, but I am eager to improve the guide and constructive criticism is greatly appreciated.

#### Credits and Links

I cannot in good faith fail to link [Sebidee's Complete Guide to Warhammer Unit Modding][sebidee], which is what got me into unit modding in the first place. It goes into a lot of detail on many things, and the dude is clearly VASTLY more skilled at the fancy pants art side of things than I am. I did not copy anything from it, but at the same time I feel that credit where credit is due.

I highly, highly, HIGHLY recommend the [C&C Modding Discord][c&c]; there are a lot of very knowledgeable modders there who have helped me out greatly over time, and the community as a whole is pretty great. Modding tech 1-4 channels are where you ask questions, they don't always get answered, especially if it's about extra complex or poorly known stuff like startpos modding and the few people who are super knowledgeable about that aren't around, but in general it's helped me a ton.

I linked it above, but [Rusted Pack File Manager][rpfm] is incredibly valuable to all modding I do and I cannot recommend it highly enough, especially over the aging PFM that older guides often recommend. Nothing against PFM, it has its uses, but in general RPFM is just better on almost every level.

[sebidee]: https://steamcommunity.com/linkfilter/?url=http://www.twcenter.net/forums/showthread.php?749888-Sebidee-s-Complete-Guide-to-Warhammer-Unit-Modding
[rpfm]: https://steamcommunity.com/linkfilter/?url=https://github.com/Frodo45127/rpfm/releases

[unit_card]: (./chapter_1_3.md)

[c&c]: https://steamcommunity.com/linkfilter/?url=https://discord.gg/vtRPChd