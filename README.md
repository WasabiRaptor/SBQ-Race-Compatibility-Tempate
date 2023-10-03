# SBQ Race Compatibility Tempate
template to quickly add compatibility to most modded races

put this folder within your mods folder and then follow the instructions

you will also have to have unpacked the files for the modded race you wish to add compatibility with so you can view them, you can search how to do this

## Instructions

### Step 1: Replace Species Name
replace `speciesName` in all files and filenames with the internal name of your respective modded species, if you don't know what that is, it would be the name of its `.species` file in the `species/` folder and the `kind` value of that species within its file, if these are different, then the mod creator wasn't being standard with file naming conventions and are thus incompatible, since anim overrides won't be able to get the path to the species file correctly

### Step 2: Assign Base Color Map
Starbound replaces colors on the base sprites of a species using color replace directives which are uniquely defined per species, this makes it difficult to use any assets interchangibly because most species have entierly different color palettes, therefore we have to define some extra data for anim overrides so we can have colors replaced in a smart way

open the images in the `humanoid/` folder for your respective species in the original mod, and open the `.patch` file in the `species/` folder in this mod, within that file, you will see tables of strings for color hex codes, you are going to want to define the color palette for each color in the images of your species, first index in each table being the outline color, second being the shading color, third being the main color, and fourth being the highlight color

the fourth color is optional, if your species doesn't have it for that color, no need to define it, however if you're missing one of the other color indexes, I suggest duplicating one of the others that you think fits best into its slot, **you cannot simply define a new color here**

Primary and Secondary colors *must* be defined, if your species does not have any visible flesh to define for the flesh color, feel free to make up your own or leave it default, if there is no tertiary color then just delete that row, if more are needed add them and give them a relevant name as you see fit.

### Step 3: Assign base colors to body parts
Open `humanoid/sbqAnimOverrideParts.config` for each part of the body there is a table of `remapColors`

You'll see entries in the table, something like `["primary","secondary"]` check each of those for each part, and make it so colors are being re-mapped from the colors of base human parts to the color the part should have for your species, you can basically ignore the first part of every color pair essentially, as you're only concerned with what it should become

for example, if this was for the belly part, if your species belly was the teritary color, you'd want to change it to this `["primary","tertiary"]` that is the basics of what to do for each part, ignore other parts you don't need to mess with unless you know what you're doing
