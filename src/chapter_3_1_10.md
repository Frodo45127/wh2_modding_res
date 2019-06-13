# Tables and Loops

Alright, here we go - tables, and loops. We've covered tables in a good depth so far, but this should get you to full comfort. We'll also talk about loops, and iterating through tables, and using maps. It'll be cool. Let's start with arrays and looping.

#### Arrays and Looping

Arrays, for those who don't recall, are tables with incremental indexes starting at `1`. Lua automatically assigns values to an index, not the Lua-author. An example would be:

```lua
    local example_array = {5, 4, 3, 2, 1}

    out(example_array[1]) → 5
    out(example_array[5]) → 1

```

Arrays are always numbered incrementally. It's easy enough to assign new values to existing arrays - and, after all, that's 90% of their usefulness.

```lua
    local example_array = {5, 4, 3, 2, 1}

    out(example_array[1]) → 5
    out(example_array[5]) → 1

    out(example_array[6]) → nil -- doesn't exist!

    example_array[6] = 20
    out(example_array[6]) → 20
```

Say you don't know how many indexes there currently are in an array; say you've edited it a bunch of times, and there's no easy way to read how many indexes there are (which is the norm). You can add on to the end of an array using the following:

```lua
    local example_array = {5, 4, 3, 2, 1}

    out(example_array[1]) → 5
    out(example_array[5]) → 1

    out(example_array[6]) → nil -- doesn't exist!

    -- some stuff that randomizes the array, woo hoo

    example_array[#example_array + 1] = 20 -- #example_array would be the current number of indexes; +1 is incrementing it, assigning '20' to an unused index
    out(example_array[#example_array]) = 20 -- now #example_array is one higher than the previous statement, due to the assignment of '20'!
```

Remember that the # operator returns the *number of indexes in that array*, and that it *should not be used with maps*. 

With that remembered, let's look at a loop!

```lua
    local example_array = {5, 4, 3, 2, 1}

    for i = 1, #example_array do
        out(example_array[i]) → 5, 4, 3, 2, 1
    end
```

And that's really it! The keyword `for` is hyper important, and allows us to loop over various indexes in an array. `for` defines a local variable - in this case, `i`, though it can be WHATEVER - to a minimum value, and a maximum value. In this instance, the **minimum** value of `i` is *1*, while the **maximum** value of `i` is *5*. And then, it runs the `for` chunk ***once for each value of `i`***. That means, it runs `out(example_array[i])` once with i == 1, then once with i == 2, with i == 3, i == 4, and i == 5.

Loops for arrays like this are super useful, as I've hinted at. But how does this matter for Total War, you might be asking? Well, look no further! List interfaces!

Remember the script interface chapter, we saw something called a `CHARACTER_LIST_SCRIPT_INTERFACE`? List interfaces are common, and they're a cool type of array that we can kinda use just like the above one, but differently. Since it's an *object*, not just an *array*, we have to do a couple of different things. I'll show you, then we'll talk, kay?

```lua
    local faction_name = cm:get_local_faction()
    local faction = cm:get_faction(faction_name)

    local character_list = faction:character_list()
    for i = 0, character_list:num_items() - 1 do
        local character = character_list:item_at(i)
        -- check stuff about the character
    end
```

There are THREE important distinctions to make here. 

First off - the *minimum* is `0`, NOT `1`, and the maximum has to be the *number of items in that list minus one*. That's because these lists are defined in C++, where indexes start at 0, unlike Lua where indexes start at 1.

Secondly - the maximum can't be defined as `#character_list`, because it's not an array! `character_list:num_items()` returns the total number of items in that array, and there's a `num_items()` method for every list interface.

Thirdly - you can't use `character_list[i]` to read the index. IT'S NOT AN ARRAY! `character_list:item_at(i)` grabs the item at the index in the array; there's an `item_at()` method for every list interface.

Arrays are really common in the CA script interfaces, and having a healthy grasp of them will allow you to do much more with your scripts.

#### Maps and Looping

While maps aren't used in CA script interfaces, like arrays kinda are, they're still really vital and can allow some super ease of scripting.

Reminder for what maps are: they're tables *with user-defined indexes*.

```lua

    local example_map = {
        ["first"] = 1,
        ["second"] = 5,
        ["third"] = 10
    }

    out(example_map["first"]) → 1
```

Looping in maps is pretty similar to arrays, though you have to define *two* local variables, instead of just one.

```lua

    local example_map = {
        ["first"] = 1,
        ["second"] = 5,
        ["third"] = 10
    }

    for key, value in pairs(example_map) do
        out(value) → 1, 5, 10
    end
```

As before, we define the local variables with `for`. Those local variables are `key` and `value` - though, just like `i` in the above example, those variables can be named whatever you'd like. Using `key` and `value` is simply convention, same with `i`.

Using the keyword `in` is syntax here, it's necessary between the two variables, and the pairs() function call. The function `pairs` basically defines that you want to iterate through the table (provided as a parameter to `pairs`). It doesn't require understanding - just know that, to iterate through a map, use the above.

Now, where's the usefulness? There's a LOAD. 

For one, a map loop is *quicker* than an array loop, and easier to write using Excel

```lua
    -- you
    local lame_units = {"wh_dlc02_vmp_cav_blood_knights_0", "wh_dlc04_vmp_veh_corpse_cart_0", "wh_dlc04_vmp_veh_corpse_cart_1", "wh_dlc04_vmp_veh_corpse_cart_2", "wh_main_vmp_inf_crypt_ghouls", "wh_main_vmp_mon_crypt_horrors", "wh_main_vmp_mon_vargheists", "wh_main_vmp_mon_varghulf", "wh_main_vmp_mon_varghulf"}

    for i = 1, #lame_units do
        cm:add_event_restricted_unit_record_for_faction(lame_units[i], "wh2_dlc11_vmp_the_barrow_legion")
    end

    -- the guy she told you not to worry about
    local kill_units = {
        ["wh_dlc02_vmp_cav_blood_knights_0"] = true,
        ["wh_dlc04_vmp_veh_corpse_cart_0"] = true,
        ["wh_dlc04_vmp_veh_corpse_cart_1"] = true,
        ["wh_dlc04_vmp_veh_corpse_cart_2"] = true,
        ["wh_main_vmp_inf_crypt_ghouls"] = true,
        ["wh_main_vmp_mon_crypt_horrors"] = true,
        ["wh_main_vmp_mon_vargheists"] = true,
        ["wh_main_vmp_mon_varghulf"] = true,
        ["wh_main_vmp_veh_black_coach"] = true
    }

    for unit, _ in pairs(kill_units) do
        cm:add_event_restricted_unit_record_for_faction(unit, "wh2_dlc11_vmp_the_barrow_legion")
    end
```

For another, it allows you to iterate through a *lot* of data at once, with real ease. (the following is a further example from the Return of the Lichemaster mod!)

```lua

    local subtypes = {
        ["AK_hobo_nameless"] = {
            ["forename"] = "names_name_666777891",
            ["family_name"] = "names_name_666777892",
            ["clan_name"] = "",
            ["other_name"] = "", 
            ["age"] = 50, 
            ["is_male"] = true, 
            ["agent_type"] = "general",
            ["agent_subtype"] = "AK_hobo_nameless",
            ["is_immortal"] = true, 
            ["art_set_id"] = "AK_hobo_nameless"
        },
        ["AK_hobo_draesca"] = {
            ["forename"] = "names_name_666777893",
            ["family_name"] = "names_name_666777894",
            ["clan_name"] = "",
            ["other_name"] = "", 
            ["age"] = 50, 
            ["is_male"] = true, 
            ["agent_type"] = "general",
            ["agent_subtype"] = "AK_hobo_draesca",
            ["is_immortal"] = true, 
            ["art_set_id"] = "AK_hobo_draesca",
            ["ancillary1"] = "AK_hobo_draesca_helmet"
        },
        ["AK_hobo_priestess"] = {
            ["forename"] = "names_name_666777895",
            ["family_name"] = "names_name_666777896",
            ["clan_name"] = "",
            ["other_name"] = "", 
            ["age"] = 50, 
            ["is_male"] = true, 
            ["agent_type"] = "general",
            ["agent_subtype"] = "AK_hobo_priestess",
            ["is_immortal"] = true, 
            ["art_set_id"] = "AK_hobo_priestess",
            ["ancillary1"] = "AK_hobo_priestess_trickster",
            ["ancillary2"] = "AK_hobo_priestess_charms"
        }
    }

    for key, subtype_data in pairs(subtypes) do
        cm:spawn_character_to_pool(
            "wh2_dlc11_vmp_the_barrow_legion",
            subtype_data.forename,
            subtype_data.family_name,
            subtype_data.clan_name,
            subtype_data.other_name,
            subtype_data.age,
            subtype_data.is_male,
            subtype_data.agent_type,
            subtype_data.agent_subtype,
            subtype_data.is_immortal,
            subtype_data.art_set_id
        )
    end

```

Yep! You can have tables within tables, maps within maps, loops within loops.

And yes, you can access an index using `table.index_key`, if that index is a *string*. You obviously can't do that with a number. That syntax is really common - using `table["index_key"]` to define an index's value, and then accessing that using `table.index_key`, though you can use them interchangeably; they're literally the exact same thing, no functional difference.

#### Other Loops

There are a couple other ways to loop, that don't involve tables.

The most common other one - especially in the Total War context - is a `while` loop. For instance, we can see the following example in the wh_main_chs_chaos_start.lua file:

```lua
	local x = desired_spawn_point[1];
	local y = desired_spawn_point[2];
	local valid = false
	
	while not valid do
		if is_valid_spawn_point(x, y) then
			valid = true;
		else
			x = x + 1;
			y = y + 1;
		end;
	end;
```

In this instance, the `while` chunk is run once entirely through, continuously, until "not valid" returns *false* - aka, until `valid` is `true`.

Don't be frivolous with your use of `while` loops - they should last no more than, like, 0.05s. Using them causes the rest of the Lua environment to come to a screeching halt, as basically nothing else can happen during a while loop.

Similarly, you can use a `repeat` loop, which is the same functionally - runs until a condition is met - except a `repeat` loop will always run AT LEAST once, whereas a `while` loop won't run at all if the condition isn't met. Let's spy another CA example, this time in wh2_dlc11_treasure_maps.lua:

```lua
	repeat
		random_number = cm:random_number(#level);
		ancillary = level[random_number];
    until(context:faction():ancillary_exists(ancillary) == false)
```

Which is loud-person talk for "go through random numbers until we find an ancillary in this array which doesn't exist in this faction".

Again, repeat is pretty performance-heavy, as it holds up basically everything else. Don't do big repeat loops, please. Please. PLEASE.

Or, do. I don't really care.

#### CA Loops and Timing

There are some CA-defined methods to do a repeated function, or to delay a function, which do *not* hold up the entire environment like the above two loops. These callbacks are super helpful for repeated constant operation; for instance, I used a repeat_callback recently to print out the camera coordinates every half a second, so I could work on a custom cutscene. There was absolutely no performance drop, because Lua is beautiful.

The two types are *callback* and, spoiler alert, *repeat_callback*. The former simply delays a function by a time; the latter delays a function by that time, and repeats that same delay and function over and over again.

```lua
    cm:callback(function()
        -- run this chunk ONCE after five seconds
    end,
    5)

    cm:repeat_callback(function()
        -- run this chunk every five seconds
    end,
    5)
```

For the campaign manager, these two functions are wrapped and the final parameter is in *seconds*. However, for battle and frontend, these would be milliseconds.

```lua

    -- BATTLE
    bm:callback(function()
        -- run this chunk ONCE after five seconds
    end,
    5000)

    bm:repeat_callback(function()
        -- run this chunk every five seconds
    end,
    5000)

    -- FRONTEND
    local timer_obj = get_tm()
    timer_obj:callback(function()
        -- run this chunk ONCE after five seconds
    end,
    5000)

    timer_obj:repeat_callback(function()
        -- run this chunk every five seconds
    end,
    5000)
```

There are lots of uses for these; namely, waiting for some UI to load, putting a timer on a quest battle for deployments, disabling/reenabling event feeds of a certain type, cutscene timing, and much else!