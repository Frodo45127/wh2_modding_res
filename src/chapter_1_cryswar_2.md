# Creating a Custom Legendary Lord

#### Setup

This guide assumes that you have created the actual unit for your custom lord - if you don't know how to do that, my [previous guide][previous_guide] covers the basics of unit creation. Same process, just copy from a vanilla lord as much as possible instead of a normal unit. And remember that you need their card in portraits instead of units!

We will be using [Rusted Pack File Manager][rpfm] (RPFM) again for this guide. If you don't have it, get it, it's free and easy to learn.

One section of the guide touches on the possibility of using the Assembly Kit, specifically DAVE. I recommend against it unless absolutely necessary, but it is a decent search engine; if you mouse over Library, go to Tools, and install the TWW2 Assembly Kit, then open it, DAVE is the DB editor.

---
**agent_subtypes**

All lords and heroes are 'agents' as far as the game is concerned, and we'll start here since most of the future tables will reference this one.

Fortunately this is fairly self explanatory and very similar to our previous work; your "Key" is extremely important and should be saved since you'll be referencing it a lot.

**Auto Generate** should be checked ONLY if you are making a generic hero/lord, and left off if you are making a unique lord or hero.

**Is Caster** is self explanatory - do they use magic?

**Associated Unit Override** links to the main_unit key, not land_unit, though I would use the same key for both when possible personally.

**Audio Voiceover Actor Group** is the set of lines they'll use on the campaign map, just pick the closest you can get.

The rest can just be copied from a similar vanilla lord without issue.

---
**agent_uniforms**

This is a lot like the unit_variants table, it just links various things together. I like to use the same name for all of them but you can do whatever you want as long as you stick to your scheme and more importantly remember it.

---
**campaign_character_art_sets**

This table sets up some of the basics of your agent on the map - their size, whatever faction or culture/subculture they're linked to, what type of agent they are, etc.

Since we're making a custom Lord, their agent type will be general; obviously for say, Exalted Heroes, you would use a different agent type.

DON'T toggle "is custom", stuff gets weird.

ALWAYS toggle "is male" to true! I don't know why but the game gets super weird if you call them female, it can break scripts and lords very badly. Learned that one from Mixu. Even vanilla doesn't understand how this field works, most female lords, legendary or otherwise (ex. Hellebron, Supreme Sorceresses, Isabella von Carstein) are marked as male here.

Campaign Map Scale is self explanatory - how big your guy is on the campaign map - but be warned that you may have to fiddle with it, some base models are friggin' huge and have to be scaled down to .5 or less to function well, others are fine at 1.0. If you're making monstrous lords, like Arachnaroks or something, you will very likely have to fiddle with this a lot, human lords usually work fine at 1.0.

---
**campaign_character_arts**

Almost all of this table can just be copy-pasted from a similar vanilla lord without needing much explanation or understanding.

Ensure that your numerical ID is unique and that the **art set ID** and **uniform** link to the correct ones for your lord - especially if you copy pasted stuff.

Land Animation must exist already and this can be a problem, most monsters, monstrous infantry, and basically anything that isn't a generic human or a mount don't have campaign map animations, or may only have limited ones, ex. giants have one since there's a giant boss, but it has no walking animation, just idle. It's up to you if you're willing to let your giant lord moonwalk across the map (sorry, Prometheus) or want to limit yourself to only lords who have existing animation types.

Your land animation can usually be just ripped off of a vanilla lord by comparing vanilla agents in RPFM, but you can view ALL existing campaign map animations in the table **campaign_anim_set_enums** which I believe can only be opened in the Assembly Kit.

---
**character_skill_node_sets**

A full overview of how skills work is outside the scope of this guide, but we do need to touch on this table regardless. This table is how we attach a set of skill nodes to an agent type; thankfully it's pretty simple and if you've gotten this far it shouldn't be too hard to figure out that your agent subtype goes in the agent subtype field etc.

Leave the campaign/faction/subculture/army/navy fields empty, they're not used.

This will leave you with a blank skill tree - I go into MUCH more detail on how to create and customize skills in [this guide][next_guide] but if you want to get a ghetto skeleton up, copying a similar lord, I would go to **character_skill_nodes** in the vanilla data file, search character_skill_node_set_key for a similar lord; ex. if you're making a new Chaos custom Legendary Lord, search for "chs" to see all Chaos lords, find **wh_main_skill_node_set_chs_lord** which is the generic Chaos Lord, then change the search parameters to that. That will only show you his nodes (and unfortunately the Lord of Change too...) but still, that tells you all the basic nodes you need.

How you go from there is up to you, but what I personally do is copy ALL of that lord's generic rows over to [Notepad++][npp], then go to Search -> Replace..., and find "wh_main_skill_node_chs_lord" and replace it with say "my_lord". That turns **wh_main_skill_node_chs_lord_battle_01** into **my_lord_battle_01** which is a lot easier to keep track of lol, at least for me. **chs_gaius_battle_01** is even easier to remember... assuming your lord is a Chaos lord named Gaius anyways.

After you're done, just copy that entire set of rows back into RPFM, specifically your mod's character_skill_nodes table, and make sure that the **character_skill_node_set_key** matches whatever you put in **character_skill_node_sets**. Again, I like to keep it simple; agent subtype chs_gaius, node set named chs_gaius, node set key chs_gaius.

---
**faction_agent_permitted_subtypes**

Another easy table, this one just tells the game that it's cool for a faction to have your lord.

---
**Portraits Part 1 - Portholes**

Portholes are the round character avatars you see at the bottom left when you select them, in diplomacy, etc. Note that they don't actually have to be round images, the game just clips a round section of it so they can be square or whatever.

Portholes are placed in ui/portraits/portholes, but actually getting them ingame is a bit trickier; as far as I know it's not possible with pure vanilla tools - so I will refer you to the best guide I've found to do it! This guy [wrote up a great guide AND most importantly provides a super handy tool to do it with][marth_thing], and I'd rather give him the attention than waste your time pretending I have a solution haha.

---
**Portraits Part 2 - Unit Cards**

I mentioned in the unit creation guide that if you were making a lord, you put the unit card in **ui/portraits/units** instead of ui/units/icons. This section is just there to remind you of that.

I personally find that taking a screenshot of the lord and then mucking around a bit in GIMP gives the best, most fitting cards since that's what the game does for the most part with vanilla lords - but you can use any image of appropriate size.

---
**names**

The naming system is a bit whack, but I'll try to explain it simply.

First you define a surname or forename here (the numerical **ID**, must be unique).

Next you assign it to a **names_group** which tells the game "oh this is a Chaos-ey name"

Finally you tell the game what **type** of name it is - Forename is 0, Family Name is 1.

Gender doesn't actually matter I think, I usually just leave it 0, but can be male (0), female (1), or no gender (3).

Similarly, Frequency is left at 0 for any legendary lord/hero/etc and 1 if you want it to be added to the autogen pool.

Keep in mind that if you have a multi part name, like Sarthorael the Everwatcher or Sigvald the Disappointment, you will need to define two name IDs here - one for the first part (Sarthorael) and the other for the second (The Everwatcher). And don't screw up their Type!

We'll actually define the name later via localization.

---
**agent_subtypes.loc**

This localization file is most commonly used for the "Legendary Lord" subtitle in vanilla, using **agent_subtypes_onscreen_name_override_** followed by the agent_subtype key on the left, and just "Legendary Lord" in the **text** field

You can also override description with **agent_subtypes_description_text_override_** followed by the key, which seems to be used for generic heroes and such.

---
**names.loc**

Remember when we defined some arbitrary unique IDs in the "names" DB table earlier? Now we can FINALLY attach them to an actual damn name - use `names_name_` followed by that number for the key, ex. **names_name_1234567**, and whatever name you like in the Text field.

#### Actually Spawning Your Lord

There are a few ways to go about this depending on what kind of lord you went with.

- Generic lords will spawn ingame without any further effort if you set them to "auto generate" in agent_subtypes.
- Legendary lords can be added via script fairly simply, more on that soon.
- You CAN startpos the lord in DAVE, but unless you're making a new faction and want a custom lord this destroys compatibility for no good reason.

---
**Spawning Part 1 - Scropting**

No that's not a typo in the title, Vandy calls it scropting and I find it hilarious.

Anyways, there are many ways to actually script a lord spawn but for the sake of simplicity (and multiplayer compatibility) I'll just go with the simplest option possible here: [linking to an amazing basic scripting guide][tsd]!

That explains the basic folder organization and Notepad++ usage better than I could and I'd rather not plagiarize their work, so let's move on to what we actually want to do - making the script add our lord.

```lua
    function modname_script()
        if cm:is_new_game() then
            cm:spawn_character_to_pool("faction_key", "names_name_XXXXXXXX", "names_name_YYYYYYYY", "", "", 18, true, "general", "agent_subtype", true, "")
        end;
    end
```

What does this do? It adds the lord to your factions lord recruit pool at the start of a new campaign. Actually spawning characters can be a little funky in MP and I don't want to get into some of the complexities of finding a valid spawn location, I'd rather keep it VERY simple here and I don't know lua well enough to teach y'all every possible way to do it. So let's look over how to fill the skeleton above out.

- `modname_script()` - make sure this is the exact same as what you named the file! In other words, if you named the file "storm_script.lua", this should be "storm_script()".
- `faction_key` is the, uh, faction key, stolen from the **factions** table. For example, if you're adding this lord to Clan Rictus (Tretch's faction), fill in **wh2_dlc09_skv_clan_rictus**
- `names_name_XXXXXXXX` is whatever the character's forename is, based on what you defined in the names table earlier.
- `names_name_YYYYYYYY` is the character's surname. If you don't need a surname, leave this completely blank, so it looks like "",
- The first "true" is for gender; as discussed earlier, the game gets super derp about women for some reason so just leave this true (male).
- `General` can be changed to say, champion if you're spawning a Champion or Runesmith or something, but since this guide is for lords, we're just gonna assume you leave it as **general**.
- `Agent_subtype` should be easy to fill in, we've been copy-pasting the damn key all throughout this guide!
- The next "true" is a boolean as to whether or not the character is immortal. Unless you specifically want your legendary lord to perma-splat when they die, leave this true like with normal vanilla Legendary Lords.

I'll post a few versions of what the final result might look like, using examples from my mod.

```lua
    function storm_script()
        if cm:is_new_game() then
            cm:spawn_character_to_pool("talons_of_tzeentch", "names_name_19880600", "", "", "", 18, true, "general", "chs_prometheus", true, "");
        end;
    end
```

```lua
        function storm_script()
        if cm:is_new_game() then
            cm:spawn_character_to_pool("blades_of_khorne", "names_name_515043905", "names_name_769371438", "", "", 18, true, "general", "chs_styrbjorn", true, "");
        end;
    end
```

Note that if you're adding multiple lords, you can just have multiple of those cm lines, the game doesn't mind, ex.

```lua
    function storm_script()
        if cm:is_new_game() then
            cm:spawn_character_to_pool("blades_of_khorne", "names_name_19880603", "names_name_19880604", "", "", 18, true, "general", "chs_atropos", true, "");
            cm:spawn_character_to_pool("talons_of_tzeentch", "names_name_19880601", "", "", "", 18, true, "general", "chs_saenathra", true, "");
        end;
    end
```

---
**Spawning Part 2 - Asspossessed**

I want to reiterate what I mentioned above - DO NOT DO THIS unless you have to, specifically, if you are making a new playable faction with a custom lord. Startpos.esf mods cannot function alongside each other, only one will work - so making a startpos mod means your mod will never be compatible with say, Crynsos's Faction Unlocker, or any mod that changes how many slots major cities have, etc.

A full guide for making a new faction in AssKit is outside of the scope of this guide and will be explained in more detail when I post the Faction Creation guide, but as an oversimplified note; the notable parts of the lord that NEED to be done in Assembly Kit are: land_units, main_units, agent_subtypes, character_skill_node_sets, in that order. You can use dummy entries for all of them, it's possible to edit them in RPFM (as we did earlier in the guide), but the game will not let you finish a start_pos_character without the first three defined and for some dumbass reason, the skill node set HAS to be startposed for the faction leader or they can never access their skills/items ingame.

You'll also need to fill out a number of other tables (ex. start_pos_characters, start_pos_generals, mercenaries, etc). but again, that's outside the scope of the guide.

#### The Next Step

Custom skills, ancillaries, effects, etc. are an extremely widespread topic overall so they are not covered in this guide, however I do have a guide for them! If you [follow this link][next_guide] I explain them all.

#### Credits and Links

I could not possibly have come as far as I have as a modder without the excellent people at the [C&C Modding Discord][c&c]. I cannot recommend it highly enough; while you won't ALWAYS get an answer, I've found it to be by far the best place to get help and interact with other modders. Plus you get to bug Vandy for scripting help!

thesniperdevil has a series of Steam guides about scripting, touching on various subjects such as listeners, but [this specific guide][tsd] was my intro to basic scripting and he explains script folder design better than I could so I wanted to link and credit it again here.

Special thanks to Vandy and Mixu in particular for the times they've helped me break through roadblocks in modding in the past! Mixu was the reason I could figure out the game's weird hatred for female lords and get Saenathra working properly as a result, Vandy is just like the #1 guy in the C&C discord in terms of how crazy much he helps people all the time.



[previous_guide]: (./chapter_1_cryswar_1.md)
[next_guide]: (./chapter_1_cryswar_3.md)

[rpfm]: https://steamcommunity.com/linkfilter/?url=https://github.com/Frodo45127/rpfm/releases
[npp]: https://steamcommunity.com/linkfilter/?url=https://notepad-plus-plus.org/

[marth_thing]: https://steamcommunity.com/sharedfiles/filedetails/?id=1194811468
[tsd]: https://steamcommunity.com/sharedfiles/filedetails/?id=1312347007

[c&c]: https://steamcommunity.com/linkfilter/?url=https://discord.gg/xs6DMbn