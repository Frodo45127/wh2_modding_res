# Custom Skills, Items, and Effects

#### Setup

As with basically every guide I write, we will be using [Rusted Pack File Manager][rpfm] as much as possible. Since this guide builds off of unit creation and lord creation, I will not be explaining the usage of RPFM in detail and will assume you understand the basics of adding tables, creating packfiles, etc.

Note that unlike the last two guides, you DON'T need to, and shouldn't necessarily fill out every table mentioned here for everything. Skills, items, and effects all link together regularly but in general you use different skills to grant items than you do to grant effects, and ancillaries can grant effects instead of skills, so it's up to you to scope exactly which tables you need - I just tell you what they do and how.

---
### Skills Overview

First, let's discuss the basics of how skills actually work

Every lord and hero has a **character skill node set**, which lets you connect skill nodes to the character.

**Character Skills** are defined as their own 'thing', then attached to **nodes** which are the fancy buttons you see ingame.

Skills can be attached to either **effects** (stats, abilities, etc) or **ancillaries** (items, mounts, banners, etc).

**Character Skill Nodes** can be organized in a variety of ways, such as which row they are in, which other nodes are required and how many of them, organized in sets, made mutually exclusive, and other things.

Essentially, the process of adding a custom skill to a lord involves making the skill, telling the game what the skill DOES, adding the skill to the lord's node set, and then telling the game where the skill should be in the set and what requirements or special mechanics it has, if any. It's not THAT complicated once you wrap your head around it, I promise, but can be a little obtuse starting off.

---
**character_skills**

This is the first and perhaps most important table since huge chunks of it are fundamental to how it functions at all.

**Key** is the same as it always is - unique and important - whatever you call it, you'll be copy pasting this a lot later so make sure to save it.

**Image Path** is crucially important, this is what will show up ingame. Compare to vanilla to decide which to use, in general if the skill buffs in battle stats then the name will have "battle_" in it, campaign focused stuff will be "campaign_", self-buffs usually have "character_" in the name, etc. If you're making a new lord trait, look for "innate" in the vanilla "key" section, or "trait" in the image path section.

**Localized Name** and **Description** are irrelevant, you'll fill them out properly in loc later.

**Unlocked at Rank** doesn't actually matter, another table actually defines the unlock level.

**Is Background Skill** is incredibly important - the starting traits lords and heroes start off with are all actually just background skills. Don't turn this on for 'normal' skills though.

The rest of the table is irrelevant unless it's a background skill (background weighting 5 in that case). Influence cost is a high elf thing.

---
**character_skill_level_details**

This table exists pretty much entirely to limit what level(s) the skill is available at. Most of the fields are rarely or never used.

**Level** is the level of the skill, not character level. Usually just 1 (once you unlock a skill, all levels of it can be bought immediately) but some skills may have different character level requirements for each level of the skill; if so, you will need to define multiple rows for skill levels 1, 2, etc.

**Unlocked at Rank** references the character level requirement. Notice that it starts at 0 not 1; rank 9 is actually level 10 ingame for example. Otherwise very simple.

---
**character_skill_nodes**

This table is the alpha and the omega of skill trees - and needs some explanation before I start.

Visually speaking, the window people see ingame is defined in terms of up to 7 **Indents** and any number of **Tiers**. Indents are numbered from 0 to 6 going from top to bottom, while Tiers go from 0 to infinity from left to right.

Only indents 0 through 5 are actually visible in the window, 6 is an invisible indent used for background skills, dummies that grant spells, etc. The window will expand rightward to fit in extra tiers, though I'm sure there's a cap. Now, let's go through this...

**Key** is actually not used as much as keys usually are; this mostly just exists for node link and organization purposes. But it's still important! Try to name them clearly, it makes node links a LOT less obnoxious to do.

**Character Skill Key** is where you link the actual Character Skill. This is also where you link a lord or hero's innate 'trait' (actually a background skill as we discussed earlier).

**Character Skill Node Set Key** attaches this node to the character's node set - I told you we'd need it eventually a guide ago!

**Indent** was discussed in detail earlier. All skills with the same Indent will be in the same row. Remember that Indent 6 is not visible ingame!

**Tier** __MUST BE UNIQUE IN THAT INDENT__ - if two skills both have Indent 2 and Tier 2, one will not appear ingame! You can have the same Tier in different Indents, though; one skill at Indent 0 Tier 0 will coexist fine with a skill at Indent 1 Tier 0.

**Points on Creation** is mostly used for lord/hero innate background skills and for dummy skills that give access to spells. 'Normal' skills start with 0 points invested.

**Required Num Parents** is where you define how many points the player needs to put into skill(s) to satisfy the conditions laid out in character_skill_node_links. 0 for most skills but be careful how you fill this one out.

---
**character_skill_node_links**

You know how in vanilla, you usually have to waste 1 skill point in the leadership skill to unlock the rest of the basic red line abilities, then ~6 points in them to unlock Rally, then that unlocks the rank 7 buffs? That's all done here.

**Child** and **Parent** keys link two skills together; if you have four rows that say skills 2, 3, 4, and 5 all require rank 1, the game will realize "ok, the player needs to put points in skill 1 before they can get any of the others." How MANY points you need is defined elsewhere, this just tells the game there's a link.

**Link Type** is the... type... of link... yeah. There are two that you'll be using;

- **REQUIRED** means that the child key requires the parent key.
- **SUBSET_REQUIRED** means that any parent keys in this set count for unlocking the child.

To use our above example, that leadership skill (Skill 1) is a Required Parent for all 6 of the initial red line skills - you can't get any of them without knowing it. Meanwhile, those 6 skills are all SUBSET_REQUIRED Parents of the child Rally skill (Skill 8), while the leadership skill isn't - the game only cares that you know the leadership skill to unlock skills 2-7, and then it turns around and only cares that you put points into skills 2-7 to unlock skill 8, Rally.

If my explanation didn't help, just go look at the vanilla, it'll make sense pretty fast.

---
**character_skill_level_to_effects_junctions**

This table links skills to **effects**. What are effects you may ask? Boy howdy are YOU going to be happy dealing with my verbal diarrhea about effects later in the guide! Seriously though, the short version is that **effects** tell the game WHAT to do and how; this table on the other hand tells you how much to do with the effect. I'll go through it in parts.

**Character Skill Key** is - well, the key I told you to save earlier.

**Effect Key** I will go into a GREAT amount of detail on later, but there are a ton of useful vanilla effects that you can filter by and use for many simple purposes. If your new skill is just supposed to add +500% hp to your lord, **wh_main_effect_character_stat_mod_unit_health** is a vanilla effect that will do that no questions asked. Want melee attack instead? **wh_main_effect_character_stat_mod_melee_attack**. Want to add melee attack to Norscan warhounds? **wh_dlc08_effect_skill_melee_attack_increase_warhounds**. We'll get into scope very soon, don't worry...

**Level** is the skill level; you need to define each effect at each skill level you want it available at. For example, if you have a 2 level skill, and it gives melee attack to the character, you'll have two different rows, each with the same skill name and effect, but one will be level 1 and the other will be level 2.

**Effect Scope** is quite frankly obnoxiously complex and you will probably mess it up at least once while modding if you try ANYTHING fancy. My #1 piece of advice is to blatantly rip off the closest vanilla thing - you don't have to UNDERSTAND what faction_to_faction_own_provincewide is, but if a vanilla skill uses it in a certain way... yea. I'll list some of the most commonly useful scopes here.

- **character_to_character_own** is used for self-buffs and unlocking abilities/attributes for the lord/hero learning this skill alone.
- **general_to_force_own** is used for buffs that apply to the entire army or segments of the army based on the effect - if the effect is attached to a unit set, only those units will get the effect's.... effects, otherwise your entire army will.
- **character_to_force_own_unseen** is mostly used for Winds of Magic buffs
- **general_to_force_enemy_regionwide** applies to all enemies in the region, used mostly for reducing enemy WoM or morale.

There are MANY others but most are used in other contexts, those are the big 4 for character skills.

**Value** is very straightforward - what is the value of the effect? If you're boosting Melee Attack, 5 means +5 melee attack. If your effect is a % boost, 5 here means 5%. Mostly self explanatory, I'll go A LOT more into detail on effect shenanigans later.

~~

One final note - for most skills you will want to add an extra row using the effect **wh_main_effect_agent_action_success_chance_enemy_skill**. Most vanilla skills have this effect attached; it is why higher level lords/heroes are typically more resistant to enemy agent actions. There is a similar but opposite effect called **wh_main_effect_agent_action_success_chance_skill** which only needs to be used for agent skills that increases action success chance. Both effects use the scope **character_to_character_own** and typically have the same value (albeit negative) as the skill level, ex. a skill with two levels will have -1 at level 1 and -2 at level 2.

You do not NEED to do this for a functional skill, however I suggest getting into the habit of doing so. It's a good habit to get into and I feel it shows that you care enough about your mod to not halfass skills.

---
**character_skill_level_to_ancillaries_junctions**

Thankfully we're back to another easy table. I'll touch a lot more on ancillaries later, but to briefly summarize; items, mounts, and banners are all ancillaries.

---
**character_skills_to_quest_ancillaries**

Similar to last table this one is just a basic link.

DO NOT check the "enable quest for prefix" field unless you're actually doing a quest battle. Way way way way waaaaaaaaay easier to just give the item for learning the skill.

---
**character_skills.loc**

Two different prefixes, name and description as usual.

character_skills_localised_name_
character_skills_localised_description_

---
### Ancillaries

**Ancillaries** are a catchall term for the various item types that you can get - mounts, items, banners, etc. We'll go over how to make a skill grant an ancillary below; for the sake of simplicity I will focus on items but the same general approach applies to all.

---
**ancillaries_tables**

As usual, your key needs to be unique and copied to notepad or w/e since you'll be referencing it a lot. Most of the fields here are deprecated from older games and easily solved by copy-pasting from a vaguely similar item, but some important columns...

**Type** is what kind of ancillary you're making; wh_main_anc_arcane_item, wh_main_anc_weapon, wh_main_anc_talisman, wh_main_anc_enchanted_item, wh_main_anc_armour are the item types, but they can also be various kinds of followers, mounts, etc.

**Transferrable** means tradable - leave empty if it's a unique item/mount/whatever unless you WANT your lord to be able to give it away!

**Category** should match with type; if it's a **wh_main_anc_weapon**, its category should be **weapon**

**Uniqueness Score** is 200 for legendary lord items as a general rule of thumb, and decreasing rarity from there.

---
**ancillary_info**

This just links the ancillary to its description.

---
**ancillary_included_subcultures**

Basically, what subculture does this ancillary go to? Check the vanilla ones to see.

---
**ancillaries_included_agent_subtypes**

If you only want a specific character or character type to be able to equip this, say, ONLY King Louen can equip the new mount you're making for him, this is your table.

---
**ancillary_to_effects**

This is basically the exact same deal as character_skills_to_effects, just without the levels. Scope and Value work the same way.

---
**ancillaries.loc**

Ancillaries have two loc fields needed, as always they go in different rows: 
- ancillaries_onscreen_name_ is the prefix for its name
- ancillaries_colour_text_ is the prefix for description

---
### Effects

Effects are among the most powerful tools in TWW2 modding - think of them as messengers that tell the game this skill should boost those stats or give that item. They have so many uses that actually elucidating all of them is fairly difficult, but since they are core to most skill modding, I'll list most of the commonly used ones.

To oversimplify it, you create the Effect in the effects table, link it to a bonus value in any of a number of other tables, then actually apply it with one of the X_to_effects_junctions tables; ancillary, character skill, building, etc.

---
**effects_tables**

This table just registers your effect to the game - it doesn't DO anything yet, but this does decide what the icon is and how high priority it is in a list of effects.

The **Effect** column is basically just Key in every other table - name it what you want, and save it in notepad or something 'cuz it's gonna come up a lot.

**Icon** and **Icon Negative** are very simple to copy from vanilla; if you want this effect to give extra Melee Attack to Hexwraiths, filter the vanilla table Icon column by "melee" and oh hey, "melee.png", we gucci. Generally speaking you just use the same icon for both positive and negative.

**Priority** tells the game where the effect goes in a list of effects - priority 1 goes at the top, 600+ is gonna be way down at the bottom. Most vanilla effects range from 50-100 and yours probably should too, but effects that grant abilities tend to be in the 600s for whatever reason. It doesn't REALLY matter, just an aesthetic thing.

*Category* tells the game if this effect will be used for battle related stuff (ex. boosting melee attack) or campaign stuff (ex. upkeep).

**Is Positive Value Good** can usually just be left checked, HOWEVER, you want it unchecked for good but negative numbers - upkeep reduction, most spell upgrades (WoM cost, cooldown, miscast reduction, etc), that kind of thing. Otherwise your -30% upkeep effect will show up red in the UI.

---
**effect_bonus_value_ids_unit_sets**

This is probably the overall most used effect table - it's used for direct stat boosts from effects. The layout is fairly simple thankfully!

**Bonus Value ID** is crucial - this is how the game actually knows what the heck the effect DOES. Definitely look up values in vanilla, but most are pretty easy to figure out. **mod_land_movement_battle** gives movespeed in battle - big surprise there. **ammo_mod** increases your ammo, who would have guessed. Note that weapon damage is split into several possibilities, the main 3 are: 

- melee_damage_ap_mod_mult
- melee_damage_mod_mult
- melee_damage_ap_mod_add
- melee_damage_mod_add

Mult just means it's a multiplier, so when you define the # value of the skill later, 30 actually means 30%. Add, surprise, means it's added - so a 30 there means you ADD 30 damage.

**Effect** is just the effect you named in the last section.

**Unit Set** is related to, surprise, unit sets! I explained them in a fair bit of detail in my unit creation guide, but to summarize, you define them in unit_sets, add units to them in unit_set_to_unit_junctions, and then this bonus value will apply to all units in that set once you turn the effect on (usually via a skill). Vanilla already has a bunch of unit sets defined; If you want your skill to buff ALL chaos units, for example, you can just use the existing chs_all.

---
**effect_bonus_value_unit_ability_junctions**

This is a very simple table - you attach an effect to an ability, and tell the game how to interpret it. Does your effect enable a skill? Reduce the Winds of Magic cost of one? Make it recharge faster?

Making a custom ability is outside the scope of this guide but whether you have a vanilla or custom ability, the unit_abilities table is where you're drawing from here.

The **effect_bonus_value_unit_attribute_junctions** table works identically to this one if you want to add, say, Stalk to your lord instead (though there's at least one vanilla effect that already does that!)

---
**effect_bonus_value_unit_set_unit_ability_junctions**

This is... similar to the last table but more complex; as you may notice on the right, you're relying on the **unit_set_unit_ability_junctions** table to tell the game to apply this ability to an entire set of units; the first vanilla example to come to mind is the Tomb Kings banner that gives Frenzy to all Ushabti.

Thankfully if you look at that table, it's actually not that complex, you just attach the unit set to the unit ability, give it a key, and use that key back in this section's titular table.

The Attribute version of this table works the same way and links to **unit_set_unit_attribute_junctions** similarly.

---
**effect_bonus_value_agent_junction**

This table and **effect_bonus_value_agent_subtype_junctions** are used to make agents available or add to their caps. Not much to explain here honestly, it's very straightforward.

---
**effect_bonus_value_military_force_ability_junctions**

You know how some factions can give themselves abilities on the sidebar on the right? Black Ark bombardments, Lizardmen spawning those clever girls or the holy stella, Skaven spawning clanrats, all that stuff? We do that here. Pretty simple for the most part.

Note that you'll need a secondary table - **army_special_abilities** - to actually attach the ability to its so called "army special ability", which also lets you check the Enables Siege Assault box if you're doing something like Vaul's Hammer.

Note that unless your faction already has this ability in vanilla, you will most likely need to import the UI for it to look good. You can make your own if your a gfx wizard I guess, or just import it from **data_2**, ui>skins, extract both hexagon_ability_frame and hexagon_ability_holder, then create a folder following that path + the faction key in your mod replicating how it looks in vanilla, ex. ui>skins>wh_main_chs_chaos if you're going to be giving army abilities to Chaos like I did.

---
**effect_bonus_value_special_ability_phase_record_junctions**

I haven't explained phases here yet, that's more of a unit ability thing, but to briefly summarize them, **phases** are the buffs/debuffs that abilities apply for a certain length of time. Contact phases in particular are debuffs like poison, sundered armor, Discouraged, etc.

This table lets you give them to whatever the scope of the effect is, as far as I'm aware you can't attach it to a unit set, it's either defined as only to the character or to the entire army with no in between. We'll talk about effect scope more when we get to character_skills_to_effects later.

---
**effect_bonus_value_faction_junctions**

This table lets you affect diplomacy with specific factions, with their keys taken from the factions_tables as needed.

---
**effect_bonus_value_subculture_junctions**

This table lets you affect diplomacy, ex. if you have a skill that gives +10 diplomatic relations to vampire counts. Unlike the previous table, this works for entire subcultures rather than specific factions - good if you want to torpedo relations with ALL dwarfs for example.

---
**effect_bonus_value_battle_context_junctions**

There's a couple of very similar tables that all work basically the same way so I'll just focus on this one - these tables are for effects that only kick in under certain contexts, like one that gives bonus attack while at sea, or physical resistance when fighting any Chaos culture, or sieging an enemy walled settlement. They generally work the same as the basic versions I detailed earlier in the guide.

Note that these context tables DO NOT support unit sets - you can't do "regeneration to dwarf warriors while underground", only "regen to all units in army while underground."

---
**effects.loc**

As a pleasant surprise, you only have one prefix needed for effect localization files - effects_description_

On the flip side, the actual text part is a lot more complicated since any good effect will reflect the number you give it in character_skill_level_to_effects or ancillaries_to_effects. How? Well... let's go over a few common tools.

- **%+n** displays the number as itself, no fancy stuff. Good if the effect is a flat boost.
- **%+n%** displays the number followed by a %, used if the effect is a % boost.
- **{{tr:rank7}}** gives you that fancy little chevron dealio that the vet rank 7 skills use

Surprisingly those are the only three you'll really need for much, if an effect grants an ability or attribute you just type it out, ex. "**Passive ability "Earthshaker Rounds" for Ereshkigal-Namatar.**"

### Example - Buffing Specific Units

So in this series of guides I've talked a lot about all the things you can do, but now let's try a practical example - we want to give Karl Franz a custom skill that grants 10 armor and 5 Melee Defense to Halberdiers.

As an important caveat, you can do this is any order - I will just suggest the order that I would do it in to minimize how many red boxes you see along the way.

**Effects**

First of all, we make the effects and ensure there's a functional unit set. The effects are easy; create one for armor and one for melee defense, ripping off similar vanilla effects.

We then skip on over to **unit_set_to_unit_junctions** to look up vanilla unit sets; search Unit Record by Halber - oh hey we can already see there's an "emp_halberds" set, copy that and search by unit set... ah hell, that includes the halberd wielding demigryph knights. So do you want to buff them too? If so, we can just use this set. Otherwise, if you ONLY want Halberdiers to get the buff, we make a custom set in unit_sets, ex. **emp_halberdiers** and then go back to junctions and ONLY add the unit record **wh_main_emp_inf_halberdiers** to our new unit set.

BUT WAIT, THERE'S MORE! Now we need to attach the effects to actual stats - which, as we discussed earlier in the guide, is done in **effect_bonus_value_ids_unit_sets** for this particular example. As usual you'll shamelessly plagiarize similar vanilla effects to find the **Bonus Value IDs** you're looking for; searching effects for "armour" (darn Britbongs) will reveal that the necessary ID for armor is **armour_mod** and doing the same with melee stats reveals **melee_defence_mod** for MD. So we create two new rows in our mod's version of the table, and attach the effects we made two paragraphs ago to the bonus value IDs we just found, and then put the relevant unit set on the right - either emp_halberds or emp_halberdiers depending on your decision above.

**Skills**

Well, now we have the effects, but no way to apply them, and even if we did they wouldn't have any text. So let's take care of the first bit - applying them.

As we discussed earlier in the guide, first we make the skill in **character_skills**, and add an entry in **character_skill_level_details** for the level requirement. We then move to **character_skill_level_to_effects_junctions** where we add three rows, each level 1 - two for the effects we made in the last section, with scope **general_to_force_own**, and in the third row we use the vanilla effect **wh_main_effect_agent_action_success_chance_enemy_skill** (scope: character_to_character_own), set to -1, which means that learning that skill makes it slightly harder for enemy agents to cuck us - vanilla skills do this and it's a good habit to get into. Then we put the actual values of the effects on the far right (again, 10 for the armor one, 5 for the MD one).

If your skill is gonna have two levels, repeat these steps with three new rows each set to level 2 and higher values for the effects. Maybe you want 20 armor and 10 MD when maxed. Whatever. You do you fam.

Unfortunately the skill isn't actually ATTACHED to anyone, so now we have to go to **character_skill_nodes**. First off we need to figure out what the heck Karl Franz's character skill node set even is, so filter by that and look for some variation of his name; thankfully his, **wh_main_skill_node_set_emp_karl_franz** includes both halves of his name - some lords don't! Balthasar Gelt, for example, is **wh_main_skill_node_set_emp_balthasar** and searching by "gelt" will bring up nothing.

Now that we know what Franz's skill node set is, we can look over it and see how it's laid out. Army buffing unique skills are usually in indent 1, and we see that indent 1 tier 0-5 are all taken by vanilla skills. So you can either go for Indent 1 Tier 6, or if you're concerned another mod will overwrite it, you can go for tier 128 or some BS number. If you're making generic skills, for the love of god, use a high number for compatibility, but lord unique skills are generally fine at low numbers. Anyways. For the sake of this guide, we'll just use Indent 6.

So here, we're gonna use the key of the character_skills entry we made earlier for the character_skill key, whatever we want for this table's key (I'd use **wh_dlc08_skill_emp_lord_unique_karl_6** personally), make sure it's attached to Franz's character skill node set, and doublecheck to make sure it looks similar to his existing skills. If you want to test the skill, you can put points into it at creation so you can go ingame and check, but be sure to remove them when you release the mod.

**Locs**

Ok, the skill works now, but if you test it you'll see it doesn't have jack for text. Woops. Let's fix that. You're gonna want to create/import a couple loc files, nothing too much. Rename them when you create/import them so they don't overwrite vanilla ones.

character_skills.loc
effects.loc

character_skills is easy to fill out; just append the skill name after the last underscore for each of the two rows you need

character_skills_localised_name_
character_skills_localised_description_

So say your skill is named `karl_franz_halberdiers`, your keys will look like

character_skills_localised_name_karl_franz_halberdiers
character_skills_localised_description_karl_franz_halberdiers

Fill out the flavor text however you want, but try to spellcheck it for everyone's sake.

Effects are pretty easy; you only need one row for each of the two effects we made, in this format

effects_description_

So say the first effect is `emp_halberdier_armor`, we're looking at

effects_description_emp_halberdier_armor

As we discussed above, you'll be looking to fill out the text field with something like:

**Armor: %+n for Halberdiers**

You can fluff it up a bit if you want, but something along those lines. Repeat for the MD effect, and you should be good to go!

---
### The Next Step

Yes, I'm shilling another guide, deal with it. Seriously though, abilities and phases are definitely linked heavily to effects, but are out of the scope of this already pretty huge guide so I wanted to split them up. Hence, [we have one more guide][next_guide] covering those things! If you want to make custom(ish) spells, tinker with existing ones, or just give your dude a buff that grants him 50 million MA forever, check it out! ...Please don't do that last one tho.






[rpfm]: https://github.com/Frodo45127/rpfm/releases

[next_guide]: (./chapter_1_cryswar_4.md)