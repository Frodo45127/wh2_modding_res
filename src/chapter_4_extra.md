# Extras

Here it goes extra stuff that's good to know about Terry. So... here we go:

- The stuff you put outside the map **doesn't have his height relative to 0**. Instead, their *0* is the height of the terrain in the place you put them. For example, put a building at 30 height. If the ground it's at 60 in that exact place, then once the map loads **you'll find a floating building at 90 height**. This can only be seen ingame, not in Terry.

- Of the 8x8 green square map, the border row/columns are special. The game will **autolevel any terrain** from the middle of those squares to the border of the map to 0 height, but that it's not shown in Terry, only in-game. So, if you place stuff there, account for the height change once the game loads the map, because that part of the map acts like the outside of the map.

- Vegetation on prefabs you put outside your map dissapears.

- If your deployment zone goes outside your playable area, the game will **expand the playable area** to cover the full size of the deployment zone.

- The priority of Go/No-Go Regions is: `Walkable Terrain` > `Automatic No-Go Region` > `Go Region` > `No Go Region`. The thing **most to the right** will take priority over the rest.

- If you don't let any walkable connections with the map's border, the game will load your map as **a fully unwalkable map**, and it'll crash on loading/deploying.