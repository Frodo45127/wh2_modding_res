# Lua Tutorial

Howdy, internet stranger, and welcome. My name's Vandy, and I'll be guiding you through this new chapter of your life. This section of the Total War Modding Resources is dedicated to the learning and appreciation of the Lua language. 

#### What Is Lua?

One of the primary types of modding, in Total War, is the use of the programming language `Lua`. In case you were wondering, Lua is Portugese for 'moon', and the dude who made the language was born in 1960, and at the time of this writing the language is only 26 years old.

Using Lua, you can establish chains of events, create flexibility on when units can be recruited or when lords are spawned, and you can set up new quest battles. The entire Chaos Invasion mechanic from WH2 is scripted; from 3K, the randomly-spawned ancillaries from master-craftsmen buildings are scripted.

Unlike db entries, which are compiled during the initial loading screen of the game and turned into engine data, and are often static with few exceptions, Lua is a dynamic energy within the game, and is the only direct way to access a multitude of game features, background data, and more.

#### From Poop To OOP

From here to the end of this series, the goal is to get you from writing poop - or, in some of your cases, absolutely nothing - to writing in OOP, like a true-blooded programmer, though that's by no means necessary and you can stop any time prior to that.

To begin, I'm going to take you on a quick field-trip as we download some new software that will help us write better code using smart programs, and we'll talk about some really easy ways to test Lua code.

After that, our next journey is through the Lua basics. We'll learn about strings, and booleans, and functions, and tables, and loops, and return values. All of the basic details of the knowledge that are necessary for even the simplest scripts.

Next, we turn to Total War specifics. There are some concepts that we need to cover while writing Lua in Total War, such as events, interfaces, when to call your scripts, stuff like that.

Towards the end, I'll progress from simple scripting to some more high-minded scripting, using what the biz refers to as "Object-Oriented Programming". It's a more advanced topic that won't do well being talked about here, but getting a firm grasp on it will help propel your scripting to a new level. 

All throughout this series, I'll seek to keep the information as game-agnostic as possible, I'll seek to cover all gaps of knowledge and point out any common troubleshooting tips where necessary, and I'll be covering these topics in the order of which I believe they should be learned. It is highly recommended you don't skip around or skip ahead, the order of these tutorials are deliberate.