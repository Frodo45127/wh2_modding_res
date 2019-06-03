# Custom Vistas
## By Maruka

#### Introduction
Have you always wanted to make your own custom vista in Terry? No? Then you have come to the wrong place! This guide will teach you how to make your own custom vista. Think of environments like creating a river, a coastline, adding a mountain range, etc.

This guide will teach you how to create your own custom vista. This isn't officially supported by CA, but it works very well. If you have any questions, it is best to ask them on the Map Makers Discord channel, which you can find [here][gccm].

You don't need any additional programmes, you'll only need to make use of Terry and Bob.


#### Preparation

Open your file explorer and go to:

```
Total War WARHAMMER II\assembly_kit\raw_data\terrain\vistas\
```

and look for one of the vista's with a terry file. I'll be using lizardmen_02 as the basis of this guide.

Copy whichever map you chose and rename it to something else. I called mine lizardmen_04 because I'm original like that.

Go into that folder and rename the .terry and .layer file as well. 

Open the terry file in a text editor. You'll see a section which still refers to the original vista, change that to your new name (in my case I changes lizardmen_02 to lizardmen_04 here).

```
<data terrain_setup="terrain/vistas/lizardmen_04/"  world_width="0" global_lighting="" ambient_light_environments="" water_plane_material="materials/campaign_water_plane_default.xml.material"/>
```

Save the file.

Open the vista.xml and change the display_name, and any other changes you might want to make. This is only used for the display in Terry later.

You can also change the vista.png if you want, that's also only used in Terry.

#### Editing the Vista in Terry

Open Terry. Once opened, close the wizard popup.

Drag the .terry file into Terry. It'll now load the Vista. You can now make any changes you like, such as adding props, or changing the terrain.

![test][img1]

Ocne you have finished with your changes, go to File -> Save As, and save your edited vista in its own folder (with the same name).

![test][img2]

It is vital that you always do 'save as', don't use the normal save option. If you use the normal save Terry will crash. If you decide to make any further changes to your vista, just use 'save as' again and override your own vista. That works perfectly fine.

Once you're done editing you can close Terry.

#### Exporting the vista

***Note:*** *If you closed Terry and Steam shows it is stil running, you will need to open up your Task Manager to close it yourself. `CRTL+ALT+DEL` to pull up the manager, and scroll down to the Other Programs. There, you should find a program called TweaK. Right click -> End Task and now you should be able to continue with the guide.*

Open Bob.

Select your vista map in the leftmost panel, as demonstrated in this lovely image:

![test][img3]

Click start.

Wait.

Click exit.

Congratulations, you're done! Next time you start up Terry you should see your new shiny vista.


[gccm]: http://discord.io/tww_mapmakers

[img1]: images/chapter_4_extra_1/01.png
[img2]: images/chapter_4_extra_1/02.png
[img3]: images/chapter_4_extra_1/03.png