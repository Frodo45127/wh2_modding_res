# Lua Keywords

We've covered the majority of the Lua keywords, but there are a few left that we have to cover. We'll cover the final keywords that aren't loop/table-based in this tutorial, and we'll cover tables and loops in the next! We only have three to touch on.

#### If, Elseif, Else

So, we've seen conditional statements - if these things, then do this. But we haven't covered their full usage!

First thing's first - if you want stuff to happen based on a condition, you use that word I just used twice - `if`.

```lua
    local var = 5

    if var == 5 then
        -- [[ do stuff ]]
    end
```

Note that `then` defines a new scope here, and that scope ends at, well, `end`. The conditional is the statement between `if` and `then`.

```lua
    local var = 5

    function example()
        if var == 5 then -- if var is equivalent to 5 then...
            out("This is 5!")
            return
        end
        out("Not 5!")
    end

    example() → This is 5!

    var = 10

    example() → Not 5!
```

In this example, we're checking whether var is `5`. If it is, we print "This is 5!"; if it isn't, we print "Not 5!". We prevent the printing of *both* of them by using the `return` - otherwise, if the var were `5`, *both* outs would be called.

We can also do this by using the `else` keyword:

```lua
    local var = 5

    function example()
        if var == 5 then
            out("This is 5!")
        else
            out("Not 5!")
        end
    end

    example() → This is 5!

    var = 10

    example() → Not 5!
```

Functionally, exactly the same. However, it's prettier to look at, and makes more sense to read. Do note that `else` also creates its own scope; in this instance, there is a scope between `then` and `else`; and another between `else` and `end`.

What if we wanted to check for more than one specific condition? We can use `elseif`!

```lua
    local var = 5

    function example()
        if var == 5 then
            out("This is 5!")
        elseif var == 7 then
            out("This is 7!")
        else
            out("Not 5 or 7!")
        end
    end

    example() → This is 5!

    var = 7

    example() → This is 7!

    var = 10

    example() → Not 5 or 7!
```

We can use more than one elseif, too. Only one `if` or `else` within that block between `if` and the first `end`, though.

Do keep in mind that whichever "path" works first is the one that will be used, and the ones following it will be ignored, until the `end` of the conditional.

```lua
    local var = 10

    function example()
        if var > 5 then
            out("This is greater than 5!")
        elseif var == 5 then
            out("This is 5!")
        elseif var == 7 then
            out("This is 7!")
        else
            out("Less than 5, not 7!")
        end
    end

    example() → This is greater than 5!

    var = 5

    example() → This is 5!

    var = 7

    example() → Less than 5, not 7!
```

Even though the var matches the second conditional statement - `var == 7` - for the final assignment, it also matches the first condition, so that's the one and only path the code takes here.

Do note that you can do just one if and elseif, or an if and an else, or all three, or just an if, or like twenty elseif's. Though that's bad code, don't do that.

#### End, Then, Do

The keyword `end` is needed to denote the end of a scope, when that scope is defined by something other than the entire file. Each of the following keywords need an `end` to finish their scope:

* then
* do
* function

And the keyword `then` is needed after the conditional statement for an `if` or `elseif`.

The keyword `do` is used in two instances; one, to create a new scope without needing any other keywords (see example below). 

```lua

    local var = 5

    do
        local var = 10
        out(var) → 10
    end

    out(var) → 5

```

The other, and more common, usage of `do` is to attach it to loop keywords. Read on to the next chapter!