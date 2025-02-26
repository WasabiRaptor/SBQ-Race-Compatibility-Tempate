# SBQ Race Compatibility Tempate
Template to quickly add SBQ compatibility to most modded races.

Put this folder within your mods folder and then follow the instructions.

You'll need to unpack the files for the modded race you wish to add compatibility with, check this [guide](https://steamcommunity.com/sharedfiles/filedetails/?id=745239455) to learn how to do so.

## Instructions

### Step 1: Replace Species Name
replace `speciesName` in all files and filenames in this mod's folder with the internal name of your respective modded species, if you don't know what that is, find the `speciesName.species` file in the `species/` folder and open it to get the `kind` value of that species within its file, make sure to get this value rather than just assuming it matches the filename, because starbound has some cases where it's case sensitive and some where it isn't, the `kind` value is what's generally the 'correct' one.

Make sure to check if your filesystem or editor has a find and replace to have an easier time doing mass edits!

### Step 2: Assign Base Color Map
Starbound replaces colors on the base sprites of a species using color replace directives which are uniquely defined per species, this makes it difficult to use any assets interchangibly because most species have entierly different color palettes, therefore we have to define some extra data for SBQ's scripts to recolor its animations to work with your species, else the animations will just look like they're human skin colored.

Open the images in the `humanoid/` folder for your respective species in the unpacked mod, and open the `speciesName.species.patch` file in the `species/` folder in this mod, within that file, you will see `baseColorMap` with tables of strings for color hex codes, you are going to want to define the color palette for each color in the images of your species, first index in each table being the dark outline shade, second being the shading or light outline, third being the main shade, and fourth being the highlight. If their base palette has more then 4 shades for a color, then just define whatever is closest for how SBQ will use it.

The highlight shade is optional, if your species doesn't have it for that color, remove it or duplicate the previous hex color, however if you're missing one of the other color indexes, I suggest duplicating one of the others that you think fits best into its slot, **you cannot simply define new colors here** colors which are not set by the species replace color directives will be unchanged and therefore would appear the same regardless of the individual's customization.

Further colors can be defined for use in SBQ's replacing, however 'primary' and 'secondary' are what are used for the color replacements on the digest drop items as well as the infuse colors, so it's important to define them.

### Step 3: Assign base colors to body parts

Within the `speciesName.species.patch` You'll find the `colorRemapGlobalTags` table, each entry is the name of an animation tag, and a list of replacements to do based on the base color map, for each one you'll see something like `["primary","secondary"]` check each of those for each part, colors are being re-mapped from the color of base human part to the color the part should have for your species, you can basically ignore the first part of every color pair, that simply what defines the "human" color getting replaced in the base animations, as you're only concerned with what you're replacing it with.

for example, if this was for the belly part, if your species belly was say a 'teritary' color, you'd want to change it to this `["primary","tertiary"]` that is the basics of what to do for each part.

You might also see operations in those lists that are meant to replace a single color, or only do its operation if the requisite settings are set, I think these are self explanitory.

### Step 4: Give good colonytags
Check the `.tenant` files in the folders within `tenants/` make sure they require the correct `colonytags` for your species, check the unpacked mod's tenants to compare, or define your own if the unpacked mod didn't have any, as a unique feature to SBQ tenants, you can order furniture from their deed, and it is defined per tenant, it should be self explainitory how it works when looking at the file.

### Step 5: Include the source race mod
In the unpacked mod's files (you may need to enable viewing hidden files) there should be a `_metadata` file. Open it as well as the one inside this mod, copy the `name` value from the source mod, it should be a string, then look in the metadata for this mod, replace `"Race Mod Name"` in the `includes` with the value of your race mod's name, this will make sure the patches load after the mod, and correctly add the relevant data.

You should also change this mod's `name` value while you're there.

## Additional Notes
There are additional values in the `.species` file which can be used to configure how the species will behave as a pred, as well as how they animate.

### voreConfig
A full explaination of everything here is well outside the scope of a simple compatibiltiy patch, just know this is what defines the capabilities and actions a species can do with SBQ, ranging from what vore actions they're allowed to do, to what status effects their body can apply, and a lot more. If one is looking into giving their race more unique properties, I suggest unpacking Starbecue's assets and taking a look at `/humanoid/any/vore.config` to see how it's structured.

### animationConfig
If left undefined, will simply default to an animation file in SBQ's engine assets that imitates how the oringal humanoids in the retail build function, no need to bother with this value if your species already works how you want it to in retail. Configureable animations are a feature of SBQ-Engine and will not do anything in retail starbound. A full explanation of this feature is outside the scope of this tutorial, I suggest reading the included documentation with SBQ-Engine to learn more about it if you're interested.

### animationCustom
A list of paths to animation files or objects to merge together over the base animationConfig. This loads in the animation parts for the vore animations as well as the occupants. This may be useful for replacing the paths to certain image parts if your species has different markings on the belly and etc. as long as they share the same frame structure with the base.
