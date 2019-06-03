# Custom Portraits and Portholes
## By Marthenil

This guide will walk you through adding portraits and portholes to your custom characters.

#### Introduction
One of the major issues when making new custom characters has been the lack of working custom portraits. The portrait is the icon you see at the diplomacy screen, when recruiting a character, the skill window and more. 

Well, no more. In this relatively simple guide, we'll go through the steps required to add a working portrait to your new custom character. 

#### Things you will need
- [selfie.exe][selfie] by Crux3d 
- The XML template (included in the above)
- [HxD version 2.0 RC][hxd] (Optional)
- [Notepad++][npp] (Optional) 

#### Setting up and understanding the XML 

Extract selfie.exe and the XML template in a directory of your choice. 
Open up the XML template with Notepad ++ or the text editor of your choice. The template is already configured for a custom vanilla male dreadlord.
The settings themselves are pretty straightforward. Let's ignore Distance, Theta, Phi and FOV for now. 

Each **art_set_id** from the campaign_character_arts_table requires its own entry.

![test][img1]

The **variant** key from the variants table goes into the `Variant ID=` slot.

![test][img2]

The link to your **2D portrait porthole** is added in the `<FileDiffuse></File>` slot.

![test][img3]

Filemasks, Season, Level, Age, Politician, and Faction Leader values should be left **untouched.** Repeat for each campaign variant.

All of your characters in a single mod pack should use a single .xml file. Just keep adding entries.

#### Distance, Theta, Phi, FoV

These values essentially adjust the viewing angle of the porthole portrait. Unfortunately, there is no easy way to figure out what works best in each specific case. You can try the default values in the XML template, but they will not always work. 


In that case, you will have to find a unit that causes a similar model/portrait and copy the values from that. Extract either **portrait_settings__.bin** in ui/portraits in the data.pack or **portrait_setings__2.bin** in ui/portraits in data_2.pack. Total War: Warhammer 1 portraits are found in **portrait_settings__.bin** and Total War: Warhammer 2 portraits are found in **portrait_settings__2.bin.**

Open the newly exported bin file with HxD. Make sure that the data inspector is on (right hand panel with value translations) by making sure that it is ticked in View -> Toolbars -> Data Inspector. Hit `CRTL+F` to search for the unit with the portrait settings that you want to use. Notice that there is some text and some gibberish.

![test][img4]

The text is the **art_set_id** of the unit. The first 16 gibberish characters, in groups of 4, are the actual values for **Distance, Theta, Phi,** and **FoV** in Float. 

![test][img5]

Highlight the four first characters (bytes) and look at the data inspector (right panel) under float. This is your **Distance** value.

![test][img6]

The next four is your **Theta** value.

![test][img7]

And, you guessed it, the next four are your **Phi** Value and the last four are your **FOV** value.

![test][img8]

![test][img9]

#### Finishing Up
After you have successfully added your Distance, Theta, Phi, and FoV values to your XML, save it as `portrait_settings_[yourmodname].xml` in the directory that you extracted selfie.exe into in the previous steps. Simply drag the file onto `selfie.exe` and a `portrait_settings_[yourmodname].bin` should appear in the same folder.

To get your .bin into the game, simply add it to the UI/Portraits/Porholes folder in your mod pack.

Voila, your portaits and portholes are ready!

#### Warning

- **Do not** mess with the .bin files or filenames in HXD, other than reading the values for Distance,Theta, Phi and FOV. 
- **Do not** mess with the filename of the custom .bin too much, either. 
- **Do not** mess with the Filemasks, Season, Level, Age, Politician, and Faction Leader values in the XML either. 

***Failure to comply with the above can lead to severe game crashes, system-wide crashes and even the dreaded Blue Screen of Death.***

You have been warned.

#### Notes about selfie.exe

Decimals in your .xml must use decimal points `.` and not commas `,`, otherwise selfie will not recognise the float values. 

Selfie also supports error reporting, if your returned file is not working properly, try running selfie through the command prompt.

Open up a command prompt in the folder you have got selfie and your .xml file; usage is:

```
selfie.exe <filename>
```

#### Closing and Thanks
Major thanks to **Crux3d** for creating selfie.exe for Total War: Attila. Without his tool it would be close to impossible to figure the values out.

Thanks to both [Crynsos][cryn] and [Cataph][cataph] for their help with figuring some of the stuff out and testing the whole thing!

[selfie]: https://drive.google.com/open?id=1rOgdJBlaUr5S-CxuWKru3at2g0cVZFB7
[hxd]: https://mh-nexus.de/de/
[npp]: https://notepad-plus-plus.org/

[cryn]: https://steamcommunity.com/profiles/76561198001056290/myworkshopfiles/?appid=594570
[cataph]: https://steamcommunity.com/id/cataph/myworkshopfiles/?appid=594570

[img1]: images/chapter_1_marth_1/01.png
[img2]: images/chapter_1_marth_1/02.png
[img3]: images/chapter_1_marth_1/03.png
[img4]: images/chapter_1_marth_1/04.png
[img5]: images/chapter_1_marth_1/05.png
[img6]: images/chapter_1_marth_1/06.png
[img7]: images/chapter_1_marth_1/07.png
[img8]: images/chapter_1_marth_1/08.png
[img9]: images/chapter_1_marth_1/09.png
[img10]: images/chapter_1_marth_1/10.png