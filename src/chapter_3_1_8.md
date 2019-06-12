# Calling Scripts

At this point, we need to discuss *when* to call your scripts.

#### Foreword

So, what do I mean when I use the term "initialisation"? I don't know!

Just kidding. So, when you write your script, you have a bunch of defined functions, but when you *call* them is vital. You can't just call them willy-nilly, it has to be intentional.

When I write my scripts, I usually always have a main initialisation function, which calls various other functions, adds the listeners I want to enable, checks if the campaign is a new game and runs first-time-only-time stuff, things like that. When initialising my script, I have to be mindful on *when* it's called, because calling stuff too soon can make nothing happen - or, worse, crash the game. Calling stuff too late, and I might be too late!

#### Initialisation

There are a few good times to call functions, when initialising. I'll cover three, specifically, and one that isn't necessarily for initialising:

**FIRST TICK**

First Tick is likely the most useful of the bunch, and the most common of the bunch. Practically every campaign mod I've done uses this, and that's not changing any time soon. First Tick is shorthand for "FirstTickAfterWorldCreated" - which means, "0.1s after the game is ready to be played, everything is setup, and good to go." You can trigger functions on First Tick using one of two methods, both of which I show below:

```lua
    cm:add_first_tick_callback(
        function()
            --[[ do stuff ]]
        end
    )

    -- OR --

    core:add_listener(
        "first_tick_example",
        "FirstTickAfterWorldCreated",
        true, -- no conditional needed, only happens once!
        function(context)
            --[[ do stuff ]]
        end,
        true -- doesn't matter
    )
```

If you're following along these tutorials properly, you will be writing all your functions in a local scope. So, you need to have this FirstTick call at the very bottom. I'll give a quick example of how that looks:

```lua

    local function initialisation()
        local faction_name = cm:get_local_faction() -- get the local player's faction name
        local faction = cm:get_faction(faction_name) -- get the local player's faction script interface

        local faction_leader = faction:faction_leader()
        out(faction_leader:command_queue_index())
    end

    cm:add_first_tick_callback(
        function()
            initialisation()
        end
    )

```

Doing it this way means you *don't need to name your file and constructor function the same anymore*. I avoid doing it, personally, because I feel it's easier to follow along with my own scripts this way. But, do note - if you have your file and constructor function the same name, it works JUST like this, the function is called on the First Tick.

***You HAVE TO USE First Tick whenever you're messing with the campaign manager, internal game script interfaces, campaign objects, anything like that.***

**UI CREATED**

Another event we could listen for is UICreated. This is the point at which... well... do I have to say it? This is a really useful event *mostly* for basic UI edits. It's VERY useful for the frontend. This is the point at which the UI exists and can be messed with, and I believe it triggers for the frontend after the cutscene is over. It triggers *before* FirstTick for the campaign.

```lua
    core:add_listener(
        "example_ui_created",
        "UICreated",
        true, -- only happens once!
        function(context)
            -- [[ do stuff ]]
        end,
        true
    )

    -- OR --

    core:add_ui_created_callback(
        function()
        --[[ do stuff ]]
        end
    )
```

It is recommended (by CA, in their lib_core.lua file) to use the latter, core: method, rather than listen for the event.

**LOADING SCREEN DISMISSED**

You can call a function right on the dismissal of the loading screen, which works for campaign or battle (it might work for frontend in Three Kingdoms, needs testing). This is mostly useful for cutscenes, either in campaign or battle. You can call it with the following:

```lua
    core:progress_on_loading_screen_dismissed(
        function()
            --[[ do stuff ]]
        end
    )
```

**FACTION TURN START**

A lot of times, you'll want to do a check of something every single turn, usually on every player-turn. This isn't really a good way to initialise your script, most of the time, but it's a great tool when checking, say, if a player has a certain amount of money.

```lua
    core:add_listener(
        "example_faction_turn_start",
        "FactionTurnStart",
        function(context)
            return context:faction():is_human() -- if the faction is human, do the stuff
        end,
        function(context)
            -- [[ do stuff ]]
        end,
        true -- don't destroy this listener
    )
```

**IS NEW GAME**

For campaign, there's a very useful command - `cm:is_new_game()` - which allows you to check if, well... 

Using this command allows you to initialise some stuff that you'd only want run on game creation. Some examples would be spawning new lords for turn one, triggering a mission, or changing region ownership. 

#### When Scripts Die

It's also very important to note, at this point, that *your script is run once every time that game-mode is entered, from top to bottom*, and that *nothing is automatically saved between game sessions*. What do I mean? Well, we can look at it with the following example (assuming it's a full file in script/campaign/mod):

```lua
    local counter = 1

    local function init()
        core:add_listener(
            "counting",
            "FactionTurnStart",
            function(context)
                return context:faction():is_human()
            end,
            function(context)
                counter = counter + 1 -- increment the number
                out(counter)
            end,
            true
        )
    end
```

If we run this, and take, say, five turns, the last thing to output would be `6` - 1 + 5. However, if we save, exit the game, and load back in, we'll notice that on turn 7, the output would be `2`. How could this be?

Well, when you open the save - on turn 6 - the ENTIRE script is called again. That means the `counter` variable is returned to its value of `1`. After that turn ends and turn 7 begins, the value is incremented once, to get to `2`.

In this example, there's no need to have such a counter - since you can get the current turn number using `cm:model():turn_number()`. However, for others you might want a variable to be *persistent* between saves - something like, how many times a faction has fought another, which can't be easily found somewhere else.

To do that, you have to save and load values. There are two types of values we can save - either a *saved value*, or a *named value*. The difference is very small, but let's go over it:

**Named value:** a named value can be a boolean, string, number, or a table of any combination of those three. Named values can ONLY be saved when saving the game, and can ONLY be loaded when loading the game.

**Saved value:** a saved value can be a boolean, string, or number. Saved values can be saved or loaded at ANY time.

Using a saved value is most common with booleans that should be persistent between sessions. For instance, if you want to spawn an agent when the mod is enabled, not only on is_new-game - so it's save-game compatible - then you might use a saved value. A named value is more commonly used when saving tables of details, for instance - in vanilla, it's used to save the spawn locations of Chaos and Norsca, or a list of all grudges, stuff like that.

```lua
    -- define a local variable that will be used later
    local defenders = {}

    local function init()
        -- check if the saved value is "false"
        if not cm:get_saved_value("is_this_mod_initialized") then
            -- we haven't done this block yet, so run it! It'll only ever run once
            cm:create_agent(
                "i'm not filling this out for the example"
            )
            
            cm:set_saved_value("is_this_mod_initialized", true)
        end

        core:add_listener(
            "example_listener",
            "BattleCompleted",
            true,
            function(context)
                defenders = {} -- reset to default
                for i = 1, cm:pending_battle_cache_num_defenders() do
                    local this_cqi, this_force_cqi, this_faction_name = cm:pending_battle_cache_get_defender(i)
                    table.insert(defenders, this_faction_name) -- add the faction name of each defender to the table
                end
            end,
            true
        )
    end

    cm:add_first_tick_callback(
        function()
            init()
        end
    )

    -- this is the ONLY SPOT we can save a named value
    cm:add_saving_game_callback(
        function(context)
            -- named value key; the variable that's being saved into it; just put `context` 
            cm:save_named_value("this_is_our_table", defenders, context)
        end
    )

    -- this is the ONLY SPOT we can load a named value
    cm:add_loading_game_callback(
        function(context)
        -- named value key; the DEFAULT value of this variable; just put `context` 
            table = cm:load_named_value("this_is_our_table", {}, context)
        end
    )
```

That was a lot of code, but that's where we're getting at! Lots of code! These are the two common examples of when either of these things are used. My example of the named value wasn't *perfect*, since that's still something you can easily get through the Lua environment, but that's the best example I can come up with right now. I'll probably have something better later on, yay.

