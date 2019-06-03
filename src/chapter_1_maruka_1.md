# Custom Campaign Settlement Skins
## By Maruka

#### Before we begin
This guide explains how you can make custom settlement skins for the campaign. This is not officially supported by CA, so the process is a little bit awkward. It might look complicated at first, but that's mostly because I've tried to be as thorough as possible in the explanation. If you have any questions you can ask them in the Map Makers discord channel, which you can find [here][gccm].

You will need [Rusted Pack File Manager][rpfm] for this.

Before we begin, make a folder -somewhere- on your computer called 'campaign', in a folder called 'prefabs'. The location of this folder does not matter, as long as you can find it again.

#### Preparation in Terry

Create a new map in Terry.

If you'd like your settlement to change with each level, you'll need to create five layers and make each version in one layer, as you can see in the layer panel in the pictures in section 2.

Before we continue, make five layers (name them 01-05 or something along those lines), and save your map.

Now go to the raw terry files in 
```
Total War WARHAMMER II\assembly_kit\raw_data\terrain\tiles\battle\_assembly_kit\(yourmap)
```

and open the .terry file of the map, and look for the ECFileLayer.

Change `<ECFileLayer export="true" bmd_export_type=""/>` to `ECFileLayer = export="true" bmd_export_type="catchment_01"/>`

Do this for all five layers.

Save the file, re-open terry & reopen the map. The layers should now have 'catchment - 01' or equivalent after their name, as you can see in the pictures in section 2.

#### Making the settlement in Terry

Build your settlement around the origin of the map 0,0,0. Standard CA settlements cover an area of roughly 4x4 units. You can make it larger than that (as we did for the faction capitals), but armies will stand in the settlement skin if you do that.

**Each layer will need to contain a copy of the previous levels.**

Kislev at level 1:

![KISLEV][img1]

Kislev at level 5

![KIIIISLEEEEV][img2]

#### Exporting the prefab

Save, and then export the map.

The description, author, and max players don't matter. You can close Terry now.

Go to the working data folder and find the exported files, the path is: 

`Total War WARHAMMER II\working_data_terrain\tiles\batle\_assembly_kit\[yourmap]`

*(replace `[yourmap]` with the key of your map)*

Copy the catchment_01_layer_bmd_data.bin (and 02-05), and paste it into the 'campaign' folder which you made earlier.

Rename catchment_01_layer_bmd_data.bin to `[nameofyourprefab].bmd`. In the case of Kislev that was gccm_kislev_emp_1.bmd.

That means that we're changing the extension of the file from BIN to BMD. Your computer might ask you if you're sure that you want to change the extension. You are sure, so go ahead and change it.

**These are the actual prefabs as they show up ingame.**

I'll add more information later if people have questions, for now you can just download [GCCM: Unique Faction Capitals][yay] and open that in PFM to see what I did to replace the prefabs.

[gccm]: https://discord.gg/NyrXErg
[rpfm]: https://github.com/Frodo45127/rpfm/releases
[yay]: https://steamcommunity.com/sharedfiles/filedetails/?id=1220118747

[img1]: images/chapter_1_maruka_1/01.png
[img2]: images/chapter_1_maruka_1/02.png