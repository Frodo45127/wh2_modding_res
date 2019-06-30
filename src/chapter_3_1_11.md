# Debugging

As you write Lua, you’ll find that errors aren’t uncommon. Something will go wrong, code is ignored, and you’re a sad panda. Thankfully, the language has a rigorous self-checking system implemented that can be used to catch any errors within lines and print out the exact line that caused an error. On top of this, it’s also important to include your own error checking systems as your write your own custom scripts to make sure your stuff is running properly!

#### Outputs!

As we've covered a bunch in this tutorial series, we can get some CA script logs using the script debug activator mod, and we can add stuff to that output log using the function `out()`.

A really remedial use of this function is to check exactly where in our script something is breaking. Your script just ISN'T working, and you don't know which exact line isn't working. Let's take an example (it's actually perfectly valid code, but let's pretend it isn't):

```lua
    function example()
        local empire = cm:get_faction("wh_main_emp_empire")
        local character = empire:faction_leader()
        local region = character:region():name()
        cm:apply_effect_bundle_to_region("testing_bundle", region, 10)
    end 
```

Say these lines haven’t been running for you, and you don’t know why! A very simple, very easy way to test is to edit it to the following, run the game, check the script logs and see where the block failed:

```lua
    function example()
        out("VANDY: example() called!")
        local empire = cm:get_faction("wh_main_emp_empire")
        out("VANDY: empire called!")
        local character = empire:faction_leader()
        out("VANDY: character called!")
        local region = character:region():name()
        out("VANDY: region called!")
        cm:apply_effect_bundle_to_region("testing_bundle", region, 10)
        out("VANDY: apply effect bundle called!")
    end 
```

You’ll notice a couple of things here. I called out() for every function that I called, for every line of "do something". This can be overkill, but in this situation, I want to see the exact very line where something went wrong, and the outputs will stop being called after this block is aborted due to failed code. I also added an easy-to-find tag to all my outputs, "VANDY", so I can quickly Ctrl+F them within the script_log.txt file.

This is a super simple, super easy, and super handy way to quickly debug failing code and find the EXACT line that isn’t working for you. But… it’s a little clunky. I mean, come on, you have to scroll through thousands of lines of CA outputs just to find your five lines? What are you, a CA employee? Thankfully, with just a few more lines, we can make our own output files.

#### Our Own Output Files

Yep! Now with 100% less fat!

I use custom output files all the time, for all of my mods. It takes no time at all to set one up, it doesn’t require any editing of CA scripts, and you can easily set it up so you have your own “dev mode”, which will be just like how CA does it - a boolean variable set to true when you want to have files printed, and false when you don’t.

Your local output function will look something like this. I’ll display it first, then we can start to take it apart.

```lua
local function Log(text)
	if type(text) == "string" then
		local file = io.open("name_of_file.txt", "a")
		file:write(text.."\n")
		file:close()
	end
end
```

And boom. Just like that, anytime within the local env that you call Log(), it will automatically open up a file called "name_of_file.txt", located in the same folder as the CA script log, and automatically write in your text. Pretty awesome, huh? Now, let’s break it down a little bit.

That first line here, we call a basic Lua library function, which is `type()`. This returns the type, obviously, of a variable. This isn’t needed here, as I could more-smartly use a `tostring()` call, but I’m doing some playful foreshadowing at better debugging practices. This line runs, checks to make sure the "text" parameter is a valid string, and if it is, it continues!

Following that, we call the Lua `io` library, which stands for “Input/Output”, a simple file manipulation library that gives us power over text editing files. io.open is used to open a file - which is named whatever the first arg is - and then the second argument determines the type of operation. We have several options here, [which can be found in this handy page][handy_page]. We are using “a”, which calls the append operation. Basically, we’re gonna add on to our file every time we call Log().

Next are two simple and easy function. `file:write()`, which just takes a string, does the actual typing for us like a sweetie; `file:close()` operates on self and closes the file.

And, just like that, we have our own, personalized log file. Whenever Log() is called, it’ll print that line for us on “name_of_file.txt”, and next time we call it it’ll be set on the very next line. Very cool. Let’s take a look at this in practice and take a look at some simple code and how we can de-clutter our output log by using our very own. Yay.


[handy_page]: http://lua-users.org/wiki/IoLibraryTutorial