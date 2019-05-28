# Creating and Editing Abilities

### Preface

As with every one of my guides, we will be using [RPFM][rpfm] as much as possible. Since abilities are a relatively complex subject and I've explained the basics a few times already in previous guides, I will assume you already know how the basics of importing and editing tables works.

### Overview

Fist of all, let's define what an **ability** isn't; it is NOT a skill, character skills are a very different subject. I will not refer to abilities as skills in this guide and I recommend you learn not to do so as well.

So what is an ability? Spells, augments, many passives, vortexes, summons (including army abilities like Tomb King's Ushabti summon), etc. Despite some similarities and functional overlap, they are NOT predefined attributes like Stalk, Vanguard, etc.

As a general rule of thumb, abilities have a very similar initial setup - you setup mostly frontend stuff in unit_abilities, linking them to the unit_special_abilities entry which covers most of the simple numbers (recharge, initial cooldown, range, etc) and details of the actual ability, and link to the relevant vortex, projectile, phase, etc. as needed depending on spell type.

---
**unit_abilities**

This table covers mostly frontend UI stuff for your ability, but it's important to get right, so let's go through each field!

- **Key** must be unique and should be easy to remember, same as keys... usually are. As always, copy it to notepad or something for easy access.
- **Supersedes Ability** is very rarely used in vanilla, I think just for the Black Ark ability progression. You can ignore it for the vast majority of abilities.
- **Requires Effect Enabling** is used if you're going to have a skill, tech, etc. make the ability available; if you're just going to attach the ability to a unit permanently via say land_units_to_unit_abilities you can leave this blank though.
- **Icon Name** is simple but annoying; either include your own skill icon or reference one of the vanilla ones from ui/battle ui/ability icons in the vanilla data file. You may want to open it in a separate instance of RPFM to peruse them at your leisure.
- **Overpower Option** is commonly used for spells that have a more powerful version that does more damage but costs more/can miscast. This links to a separate special ability that just looks similar!
- **Type** is just a UI thing, basically tells the game what kind of ability this is, rip it off from a similar vanilla ability.
- **Uniqueness** is another UI deal that decides if the spell's background is blue, purple, whatever. It's just an aesthetic thing. As usual, rip off vanilla unless you have a specific goal.
- **Source Type** varies based on what you're making the spell for; an army ability has source type army while a spell has source type spell. Fairly simple, just rip off vanilla as usual LOL.

---
**unit_special_abilities**

This is a good contender for the most complex table you'll find, but if you take it bit by bit it's not that bad. That said, while I say this a lot, USE VANILLA ABILITIES FOR REFERENCE! It's almost always easier to literally just copy-paste the entire row from the closest vanilla ability and then change keys and some details rather than trying to do it all from scratch. With that said...

- **Key** references the key you just made in unit_abilities.
- **Active Time** is how long your character directly interacts with the spell; special ability phases, vortexes, projectiles, etc. all have separate parameters. As a general rule of thumb this is the same as the duration and staves off recharge until the spell is done.
- **Recharge Time** does exactly what it says on the tin, in seconds. Not much else to explain here.
- **Num Uses** is a little weird; -1 means infinite uses (or a passive that isn't 'used' at all per se), everything else lets you use the ability that many times. So 3 means 3 uses, separated by the recharge time (or recharge+active time if relevant).
- **Effect Range** is self explanatory for the most part - 40 range means 40 range - but similar to the last one, -1 means mapwide. 0 means self-only.
- **Num Effected Friendly/Enemy units** can usually be left 0 but if you want to mess around with them they follow similar same rules as before.
- **Initial Recharge** starts the spell on that amount of cooldown. Most abilities start at 0, but army abilities (Black Ark calldowns, Ushabti summons, etc) bug out BADLY if you have them at 0 so you should at least have a short initial cooldown (ICD) on them.
- **Activated Projectile** is how you actually call the projectile for spells like Fireball and Shem's Burning Gaze, and is left completely blank if your spell doesn't create projectiles.
- **Target Friends/Enemies/Ground** are simple - what can you target with the spell? If the spell doesn't have a target per se and just goes off when you hit the button, you can leave these all unchecked.
- **Target Intercept Range** is how close to the target you have to be to successfully cast the spell. 
- **Assume Specific Behavior** is VERY rarely used but that's how stuff like Bretonnia's Lance Formation works; this tells the unit "ok go into wedge formation" or whatever.
- **Wind Up Time** is how long it takes your caster to jerk off before actually casting the damn ability.
- **Passive** is... passive? idk what else to say
- **Unique ID** should be unique, no surprise there.
- **Bombardment**, similar to the Projectile field, links the spell to a bombardment type - think Piercing Bolts of Burning, Wraithstorm, Doombolt, that kind of thing. When the big boom comes from above instead of the caster.
- **Spawned Unit** is... self explanatory, link it to a land_unit key. 
- **Mana Cost** is the cost of the ability in Winds of Magic, most non-spells have a 0 here.
- **Min Range** is how far away from the caster you have to start the spell.
- **Targeting/Passive/Active AOEs** are all best plagiarized from a similar vanilla spell.
- **Vortex** links the spell to a relevant entry in battle_vortexes - think stuff like Flame Storm, Banishment, etc. Note that most Wind type spells are actually vortexes with special parameters for movement!
- **Miscast Chance** doesn't need a ton of explanation if you've ever overcasted an ability. Note that most base level spells and almost all abilities have 0 miscast chance, but you can do whatever you want for flavor. 0.0 to 1.0 is 0 to 100% chance of miscasting.
- **Miscast Explosion** links your miscast to whatever actually happens. Usually just a vanilla explosion like wh_main_fire_miscast, but you can make your own. One unusual usage is to set miscast to 100%, then make a custom explosion that does minimal damage but imbues an effect to the character.
- **AI Usage** tells the AI how to use the ability.
- **Additional Melee/Ranged CP** tells the game that your character is that much stronger, as far as I know mostly/only used for autoresolves.
- **Spawn Type** either creates the spawned unit at the location you click, or at an offset from that location.

Most of the rest is either self explanatory or useless 99-100% of the time so I won't go into detail on it.

---
**battle_vortexs**

Weird spelling CA's, not mine.

Anyways, despite the name, all breath and wave spells are also vortexes. In general, vortexes just kinda... do their own thing, once you create them they follow the rules set in this table and don't really give a god damn about whatever else you set up in the ability's other tables.

- **Vortex key** - ok look honestly I've explained keys so many times now that there's not much point in doing it again. Keep it unique, write it down, this is old news.
- **Change Max Angle** is what decides what kind of zany directional shenanigans your vortex can pull off. 0 means it never changes directions at all, 360 means it can go any direction each move tick.
- **Contact Effect** applies a phase of your choice to anyone hit by this vortex; commonly used for vanilla effects like poison, discouraged, etc. but you can make your own. Read the phases section for more info.
- **Damage/Damage AP** do exactly what you expect them to. For balance purposes, I'd suggest copying from vanilla and then making minor changes as needed.
- **Duration** is just how long the vortex survives. Some like Flame Storm have abnormally long durations.
- **Expansion Speed** isn't used much, at least not meaningfully. Set to positive for explosions and expanding breath attacks. In my experience negative values don't actually make the vortex shrink.
- **Goal Radius** is how much the vortex will expand to. Seems to be hardcapped by the actual animation in most cases.
- **Infinite Height** seems to pretty much just be a fancy way to say "does this hit fliers?".
- **Move Change Frequency** is how often, in seconds I believe, the vortex will try to pull movement shenanigans.
- **Movement Speed** does exactly what it says on the tin - how fast does your vortex move? 0 is great for immobile vortexes, ex. Pit of Shades, or point blank AOEs like targeted explosions that expand outwards.
- **Ignition Amount** (set to 1 for ignite) and Is Magical just change the vortexes damage types.
- **Composite Scene** is basically a fancy way of saying "what vanilla vortex are you plagiarizing today?"
- **Detonation Force** is similar to mass in charges - more force means more throwing people around.
- **Height Off Ground** is only ever used in vanilla for vortexes that ONLY hit air units - Tempest and Storm of the Night. Normally you leave this at 0.
- **Num Vortexes** is my favorite field in this table for how stupid it is. If you set this higher than 1, the game spawns additional vortexes from the same spot as the original vortex, following all the same rules. If you have Change Max Angle set to anything other than 0 and Movement is on, they can go in random directions and do weird stuff. If two vortexes are on the same location, they both do damage to anything unfortunate enough in its path. Use this sparingly, it's crazy OP in most uses, but it's super fun to play with. Mazdamundi's Ruination of Cities uses this column.
- **Affects Allies** can be turned off if you don't want the vortex to do friendly fire. Most vanilla abilities have it on, but a few, like Settra's Wrath of Ptra, don't.

---
**projectiles**

Projectiles are extremely versatile, used for everything from missile weapons (archers, crossbowmen, handgunners, etc) to really big missile weapons (artillery) to many spells (Fireball, Shem's Burning Gaze, etc). Somewhat similar to vortexes, once the projectile is fired it stop really caring about your ability and just does its own thing based on this table.

Note that many vanilla units do not have any animation for casting projectiles and will glitch the game severely if you try to force them to - projectile spells will often cast (no animation) and instantly refund all cooldown/WoM to the user anyways. So far as I can tell there is no way around this, just use vortexes, bombardments, or augments/debuffs instead for those units.

Also note that if you give a projectile ability to a group of units, each of them will try to cast it whenever you use the ability. I don't think there is any way around this aside from maybe toning the spell down in damage enough that all of their combined projectiles are roughly equivalent to what you had planned originally? A 200 count unit of zombies all casting fireball might fry some computers, though, so... yea. Maybe steer clear.

As one final note - projectiles are complicated. Like, really complicated. I HIGHLY recommend just copying the closest vanilla row and then editing a few things as needed.

- **Category** tells the game what kind of projectile this is; arrow, grenade, fireball, etc. Use the vanilla categories.
- **Shot Type** is similar and generally lines up with Category; a Musket category projectile is almost always gonna be a small_arm_default shot type. Ushabti ROR is one of the few exceptions, 
an arrow that explodes into an artillery canister
- **Explosion Type** links to the projectiles_explosions table and tells the game what the explosion is like. To use the last section's example again, the Ushabti ROR projectile spawns the wh2_dlc09_tmb_ushabti_bolt_ror projectile explosion when it, uh, explodes.
- **Spin Type** is usually none, but projectiles with really funky looks, like Vindictive Glare, Gaze of Nagash, Shem's Burning Gaze, etc. definitely have a spin.
- **Projectile Number** is one way of creating multiple projectiles; again, the Gaze type spells usually spawn 5-10 individual projectiles, as do shotgun type weapons like gunnery wight blunderbusses or gunnery mob handcannons.
- **Effective Range** is the range stat ingame - this is the maximum range, we'll get more into detail on range later.
- **Muzzle Velocity** is how fast the projectile starts out moving; air resistance and enemy bodies can slow it down after firing though.
- **Marksmanship Bonus** is one of the levers for accuracy, the higher this is the more accurate.
- **Spread** is pretty self explanatory, but note that while any number 0 or above works, larger projectiles like fireballs may need relatively large spreads (2+) to actually visibly move apart.
- **Calibration Distance** is the other side of Effective Range - your calibration distance is the max range at which you get your full accuracy, usually ~60-80% of range - you get full accuracy up to calibration distance and then reduced accuracy up to your max range.
- **Calibration Area** is another field for accuracy - basically how much 'wobble' there is in aiming, the smaller it is the smaller the area that the projectile will land in.
- **Bonus vs. Infantry/Cav/Large** is pretty simple, works the same way as melee weapons - note that Bonus vs. Cav is deprecated and doesn't do jack in Warhammer 2, everything is either Infantry or Large.
- **Overhead Stat Effect** is mostly used for artillery; stuff like the Hellcannon debuff for any units under the projectile.
- **Contact Stat Effect** is for projectiles that actually hit stuff. Often used for stuff like Shieldbreaker.
- **Burst Size** is pretty rarely used in vanilla; best known for its use with Irondrakes/Warpflame Throwers, the ROR necrofex Colossus, and some high level towers.Add in its Delay in the next column and you can have several rapid-successive shots.
- **Ignition Amount** defines if they are Flaming attacks - 1 is on fire, 0 is not.
- **Expiry Range** is the max max range for a projectile; at this range it just goes splat. Should definitely be higher than Effective Range/Calibration Distance haha. -1 means it doesn't expire until it hits something.
- **Shots Per Volley** is another way to do multishot, used more commonly than Burst Size. Dragon/Hydra breaths use this, hence why you see many projectiles in very rapid succession; organ guns and helblaster volley guns do as well. Interestingly the ROR Necrofex has both burst size AND shots per volley but I think it's the only unit that does.

---
**projectile_bombardments**

After the massive clusterfraggle that is projectiles, this table is a lot simpler. Most of it is fairly self explanatory, especially if you copy from a vanilla one AS YOU SHOULD, but a few notes;


- **Arrival Window** is the duration of the bombardment - your number of projectiles will spawn in this window. If you have "Randomize Launch" toggled at the end of this table, they'll spawn randomly throughout this window, otherwise it seems to just divide arrival window by projectile number.
- **Projectile Type** links you to the projectile you want to use - covered projectiles in the last section but stuff like damage, homing, speed, etc. are all changed there, not here.

---
**special_ability_to_invalid_target_flags**

This table isn't used by all abilities, but is used to tell the game that an ability can't be used against enemies on a wall, or only works on commanders, or can't be used if you're climbing a ladder. Fairly situational but useful at times.

---
**special_ability_to_invalid_usage_flags**

This is the opposite of the previous table; that one limits what you can target, this one limits when you even have the option of targeting at all. **climbing** is a common flag that means you can't cast the ability when climbing a ladder for example, but you can also have abilities deactivate automatically when morale drops too low (ex. frenzy) or health drops too low (ex. martial expertise)

---
**special_ability_to_recharge_contexts**

Rarely used, this table lets you limit when the countdown to ability recharge is even allowed to happen. Usually used for an **engaged_in_melee** check for stuff like WAAAAGH! or **out_of_melee** if the character has to avoid melee instead. You don't need this for 99% of abilities but it's good to know it exists.

---
**special_ability_phases**

Yaaaay another obnoxiously large table! This table is optional and only used for abilities that have buffs/debuffs baked in, NOT for vortexes that inflict poison or fireballs that sunder armor. But if your ability is a buff/debuff, you will need a phase.

- **ID** has to be unique, yada yada you guys know the drill by now.
- **Duration** is very important! Unlike Active Time in special abilities, this directly decides how long the phase lasts; -1 for forever (for passives or permanent buffs/debuffs) or 60 for 60 seconds, etc.
- **Effect Type** is either positive or negative. If it's a debuff go negative, obviously.
- **Requested Stance** is used, like, twice in vanilla and seems to be for skills that auto revive the user, I'm not sure much more than that honestly.
- **Unbreakable** is the first example of an attribute that an ability can imbue. Note that this only lasts as long as the phase's duration, though you can make a permanent ability just for that, but then you'd be a lot better off just giving them the unbreakable attribute for 1/10 the work, so...
- **Can't Move** - derp I wonder what this does???
- **Freeze Fatigue** means you don't lose fatigue for the duration of the effect, don't think you can gain it either. It's never used in vanilla so may be deprecated. The attribute of Perfect Vigor does this anyways without having to muck around with abilities.
- **Fatigue Change Ratio** is a little complicated. Negative is good here, positive is bad. Used to give a burst of positive/negative to fatigue.
- **Inspiration Aura Range Mod** is only really used if you want a temporary AOE boost, otherwise doing it via effects is usually better.
- **Ability Recharge Change** adds or subtracts that many seconds from ability cooldowns.
- **HP Change Frequency** is only used for heal/damage over time abilities and is in seconds; 0.5 means every 0.5 seconds, the ability will try to do whatever you filled out the heal/damage fields with.
- **Heal Amount** and **Heal Amount Duplicate** define how much the ability heals for each time it ticks based on the frequency field. Ex. your ability heals for 20 health every 4 seconds.
- **Damage Chance** is the chance that the ability will actually do damage each time the Frequency pops. Spirit Leech is 60% for example.
- **Damage Amount** is how much damage the ability actually does. This number is NOT magical damage, it's 'just' damage, so it bypasses magical/physical resistance (but not ward save).
- **Max Damaged Entities** lets you tune how many people it tries to do this to. Some AOE spells, like Flck of Doom, have a hardcap on entities effected per tick; damaging passive abilities like Mist of the Lady usually have a much lower hardcap (ex. 4) to keep them from just casually doing X damage to every individual - entity in rnage and multiplying their power several hundred fold. Obviously, if it isn't a damaging phase, you don't need a cap (0).
- **Resurrect** is a toggle that lets you overheal models back to life. Set to false for most heals, Earth Blood for example doesn't revive people, but true for - Invocation of Nehek.
- **Mana Regen Mod** and **Mana Depletion Mod** ADD directly to the WoM per second and max WoM respectively. Think Arcane Conduit and Greater Arcane Conduit.
- **Imbue Magical** is self explanatory. Imbue Ignition is a little weirder, set it to 10 to enable flaming attacks.
- **Imbue Contact** lets the unit this phase is active on inflict another phase on enemies when attacking them. Think Poison, Discouraged, Blinded, etc. Since they're just phases, you can define your own to say, create a custom poison that lowers stats and does damage over time.
- **Phase Display** and **Phase Audio** should just be plagiarized from a similar vanilla ability, self explanatory and simple.
- **Recharge Time** is the cooldown for a phase - VERY rarely used in vanilla, usually -1 is fine, but if you want to limit a passive like "heal 2000 hp when at low health" to a 120 second cooldown, this is where you do it.
- **Affects Allies/Enemies** is generally only relevant if you have an AOE heal/debuff/whatever that you DON'T want to touch the opposite of whoever you cast it on.
- **Replenish Ammo** is fairly new, and allows you to heal a burst of ammo as soon as the phase starts. If you want an ability to slowly regen ammo (say, 10% every X seconds) you'll need to chain multiple phases together. A vanilla example is the Gunnery Wight's ability.

---
**special_ability_phase_stat_effects**

This table lets you affect a unit's stats while under a phase, either for better or for worse. This is the only easy way to affect some stats in battle; **accuracy** for example can't be easily done by character skills and effects alone, similar to explosion size. The way it works is pretty simple; you pick your desired stat from the dropdown, give it a value, and then select whether to **add** the value (can be negative) or **mult** (above 1 is good, between 0 and 1 is bad).

Note that similar to effects, you can affect non-AP and AP damage separately. If you want to do both you'll generally need 2 rows.


---
**special_ability_phase_attribute_effects**

This is a fun little table that adds attributes for the duration of the phase only. You can give people temporary Perfect Vigor or make them cause terror. If the phase is permanent then this will always apply, but usually for permanent attributes you want to just give them via an actual effect so they show up outside of battle too.

---
**special_ability_to_special_ability_phase_junctions**

This is a fairly simple table that a unit_special_abilities key to the special_ability_phases key we just made. Note that you can link multiple phases to a single ability - depending on the Order on the left here. So you can have say, 3 phases, each of which lasts for 3 seconds and replenishes 10% ammo, if you want to have to replenish ammo in small chunks over 9 seconds instead of all at once, or if you want an ability to give a huge buff to stats for 90 seconds and then a significant debuff for 30 seconds.

If your ability only has 1 phase, as most do, then just put 0 for the order here and don't worry about this table further.

---
**special_ability_groups_to_unit_abilities_junctions**

This table is almost exclusively used to assign new spells to a Lore. Great if you're adding new spells. You can also define your own special ability groups to create new lores (ex. Lore of Tzeentch) but in my experience it's buggy as hell.

---
### Actually Getting Your Ability In-game

There are a number of ways to do this which I will cover below. As a general rule of thumb, you use an **effect** to attach the ability to a unit, but army abilities and other mechanics get a little weirder.

---
**land_units_to_unit_abilities**

Completely belying what I just said, this is the simplest and easiest way to attach an ability to a unit - but very inflexible in turn.

Doing things this way permanently attaches the ability to every single unit of that land_unit key from turn 1; in vanilla this is typically used for built-in abilities like Frenzy, Rage, Martial Mastery, Regeneration, etc.

---
**Character Skills**

I touched on this in the lord guide but essentially...

- Create an effect in the **effects** table.
- If you want to give the effect to a lord for learning the skill, link the effect to the unit_ability in **effect_bonus_value_unit_ability_junctions**. Then attach the effect to the skill in **character_skills_to_effects_junctions**.
- If you want to give the ability to a set of units (say, only Empire Swordsmen, or to Giants and Trolls), you set up the **unit_set** and **unit_set_to_unit_junctions** tables, link the set and ability in unit_set_unit_ability_junctions, link the unit set ability we just created to the effect in **effect_bonus_value_unit_set_unit_ability_junctions**, and then attach the effect to the skill as above.

---
**Army Abilities**

You know how Tomb Kings can summon Ushabti, Dark Elf Black Arks can call down magic-esque abilities, and Vampire Coast can calldown artillery barrages? Those are all army abilities, and they're surprisingly easy to do yourself! At least the simpler ones, heavy UI scripted stuff is hard to replicate.

**army_special_abilities** to actually attach the ability to its so called "army special ability", which also lets you check the Enables Siege Assault box if you're doing something like Vaul's Hammer. Then use **effect_bonus_value_military_force_ability_junctions** to attach it to the effect.

Most commonly army abilities are linked to buildings (ex. vamp coast barrages), which means **building_effects_junctions**. As with all building stuff, remember that you have to enable the ability for every tier of the building you want to have access to it! By the same token, if you want the second tier of the building to have access to a better version of the ability but NOT the weaker one, attach the weaker ability only to tier 1 of the building, and the better ability only to tier 2 of the building.

Note that unless your faction already has this ability in vanilla, you will most likely need to import the UI for it to look good. You can make your own if your a gfx wizard I guess, or just import it from data_2, ui>skins, extract both **hexagon_ability_frame** and **hexagon_ability_holder**, then create a folder following that path + the faction key in your mod replicating how it looks in vanilla, ex. ui>skins>wh_main_chs_chaos if you're going to be giving army abilities to Chaos like I did.

---
**Ancillaries**

You know how the Fay Enchantress can call down a ghetto Comet of Casadora from her item? That's an ancillary-granted ability. You know how that one Tomb Kings lord gives a banner that gives Frenzy to all Ushabti units? That's also an ancillary granted ability!

Actually very simple to do ingame - you make the effect as usual and attach it to the ability as desired (can add the ability to the lord only (Chalice of Potions) or if desired give it to a unit set (Lahmizash's Ushabti Frenzy), then use the **ancillary_to_effects** table to make it kick in whenever the ancillary is equipped.

### Credits and Links

I could not possibly have come as far as I have as a modder without the excellent people at the [C&C Modding Discord][c&c]. I cannot recommend it highly enough; while you won't ALWAYS get an answer, I've found it to be by far the best place to get help and interact with other modders. Plus you get to bug Vandy for scripting help!


[rpfm]: https://github.com/Frodo45127/rpfm/releases
[c&c]: https://discord.gg/xs6DMbn