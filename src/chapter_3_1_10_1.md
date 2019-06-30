# Advanced Conditionals
## By DrunkFlamingo

I'm guest writing this to clear up some common confusions and problems I see with people writing conditional statements. As Vandy made clear, conditionals are what your script is built with, but as you work more with Lua you'll begin to see that tables are what makes the language work. Combining the two is the key to writing better script.

#### A Quick Refresher

A **conditional statement** is any statement of Lua that is either true or false. For our purposes, conditionals tend to look like this:

```lua
    return context:faction():name() == "wh_main_emp_empire"
```

Or like this:

```lua
    if context:faction():name() == "wh_main_emp_empire" then
```

In either case, it's either true or it isn't!

But those are pretty basic, and you can probably write those by the time you get here. The purpose of this chapter is to walk through doing some more advanced things with conditionals. Starting with the most common mistake I see being made by new scripters: how to check a large number of individually valid items. You see this when doing tasks like:

1. singling out groups of factions
2. singling out lord types
3. singling out specific regions

#### Using Tables and Loops to match many things

Let's take a pretty simple example. Imagine we have a scripted mechanic. This mechanic only applies to human players, and it is initiated by calling the function "my_mechanic" and giving it the faction key we want the mechanic to happen for.

A huge mistake I see in a lot of new scripters code is stuff like this:

```lua
    if cm:get_faction("wh_main_emp_empire"):is_human() then
        my_mechanic("wh_main_emp_empire")
    elseif cm:get_faction("wh_main_brt_bretonnia"):is_human() then
        my_mechanic("wh_main_brt_bretonnia")
    else
        -- this goes on for quite a while
    end
```

This is bad for a few reasons. Its bad for performance, but that doesn't matter as much as the fact that it requires a crapton of typing on your part. You've written 5 lines for only two factions! To get every good guy playable that's gonna be more than 30 lines. It also means if you want to check whether factions have a mechanic, you can't reuse any of this code. Finally, it means your code is a pain for other people to read. 

So how do we solve this? The simplest and best way is to use a table to check them. In this scenario, we're going to first introduce one that CA defines for us.

```lua
    local humans = cm:get_human_factions()
```

This returns a list of the humans in the game. That sounds much better, but let's write our code. 

```lua
    for i = 1, #humans do
        local current_human = humans[i]

        if current_human == "wh_main_emp_empire" then
            my_mechanic("wh_main_emp_empire")
        elseif current_human == "wh_main_brt_bretonnia" then
            my_mechanic("wh_main_brt_bretonnia")
        else
            -- blah blah blah
        end
    end
```

That's not any better. We're on the right track, but we haven't actually eliminated that many checks.

Remember that in our use case we need to know three things:
1. We need to know if a faction is human, this changes based on the game.
2. We need to know if a faction can use our mechanic at all. We define this ourselves.
3. We need to know the exact faction key by the end so we can start our mechanic.

So how can we improve this code? Well, we started by introducing the humans table to deal with condition #1, but condition #2 is still causing us grief. The answer, of course, is another table.

We will start with what that table looks like:

```lua
    local my_mechanic_factions = {}
```

local my_mechanic_factions = {} 
So what data do we need in this table? Probably the faction keys. When I said table, you might have wanted to try something like this:

```lua
    local my_mechanic_factions = {
        "wh_main_emp_empire",
        "wh_main_emp_brt_bretonnia"
    }
```

But now let's try to write a conditional statement using that. We'll probably end up with something like this:

```lua
    local humans = cm:get_human_factions()
    for j = 1, #humans do
        local current_human = humans[i]
        for j = 1, #my_mechanic_factions do
            local current_faction_key = my_mechanic_factions[j]
            if current_human == current_faction_key then
                my_mechanic(current_faction_key)
            end
        end
    end
```

That's much better than before. It saves us a lot of typing to add more factions, but it isn't very efficient. If we add, lets say, 25 factions to our mechanic list, then we're looping 25 times in a single player game and 50 times in a coop game. It probably won't be too noticeable, but we can do better.

#### The Right Answer

Instead of using an array table (one that is a list of items with integer keys; if you use Kailua you'll know this as a vector), lets try using a **map** table (one that associates a key and a value which we set explicitly) instead. These kinds of tables are extremely powerful when writing conditionals, so lets see why.

```lua
    local my_mechanic_factions = {
        ["wh_main_emp_empire"] = true,
        ]"wh_main_emp_brt_bretonnia"] = true
    }
```

How can that possibly help? Well, let's think about our condition #2. We want the smallest possible thing which can take a faction as an input and give back whether they are valid for our mechanic. These factions are true in relation to the mechanic. Any other faction will return "nil" if we ask for its entry in this table, because no entry for it exists. Lets see how that works in action:

```lua
    local humans = cm:get_human_factions()
    for i = 1, #humans do
        local current_human = humans[i]
        if my_mechanic_factions[current_human] then
            my_mechanic(current_human)
        end
    end
```

And there we go, finally something efficient, small, easy to modify, and easy to read. If the faction isn't in our table, then the "if" statement will be nil, which doesn't proceed. If the faction is there, we will proceed and fire off the mechanic. Now, no matter how many factions we want our mechanic to use, we'll only be looping once in a single player game, and twice in an MP game. For a mechanic which launches once or twice, that's the ideal number. 

We can also reuse this code elsewhere. Now, if we want to check whether a faction fits a mechanic or not at any time, it's only a single check away. We could even formalize this into a function:

```lua
    function does_faction_have_my_mechanic(faction_key)
        return (my_mechanic_factions[faction_key] and cm:get_faction(faction_key):is_human() 
    end
```

And check whether any given faction fits our criteria from any place in our code.