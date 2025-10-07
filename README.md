# SBQ Race Compatibility Tempate
Template to quickly add SBQ compatibility to most modded races.

Put this folder within your mods folder and then follow the instructions.

You'll need to unpack the files for the modded race you wish to add compatibility with, check this [guide](https://steamcommunity.com/sharedfiles/filedetails/?id=745239455) to learn how to do so.

## Instructions

### Step 1: Replace Species Name
Replace `speciesName` in the filename and contents of `/species/speciesName.species.patch` with the internal name of your respective modded species, if you don't know what that is, find the `/species/speciesName.species` file in the source mod and open it to get the `kind` value of that species. Make sure to get this value rather than just assuming it matches the filename, because starbound has some cases where it's case sensitive and some where it isn't, the `kind` value is what's the 'correct' one.

Make sure to check if your filesystem or editor has a find and replace to have an easier time doing mass edits!

### Step 4: Setup Tenant Data
Open `/species/speciesName.species.patch` and look for `sbqTenantData` near the top of the file, within it you'll find `colonyTagCriteria`.
You will want to set fitting colony tags for your race, check if the source mod has any `.tenant` files within its `tenants` folder if it has one. If you do find any, open them, and copy their `colonyTagCriteria` into our patch. If there aren't any tenants, check if the mod has any objects that have `colonyTags` and define amounts for those tags, do be careful about this! Lots of race templates have colony tags on their copy-pasted ship objects, but have no actual objects belonging to their species. If theres no objects aside from ship objects with the species colony tags, assign them a combination of colony tags from the base game assets that would fir their theme.

For example, I haven't created any racial objects for the Nickit race, so their `colonyTagCriteria` is as such:
```json
{
	"colonyTagCriteria" : {
		"hoard" : 8,
		"valuable" : 32
	}
}
```
After deciding their `colonyTagCriteria` you should make sure to assign them some furniture the player can order! Hunting for furniture is a little bit of a hassle in Starbound, so SBQ lets you order some for NPCs from the deed! Look for `orderFurniture`, this is a list of items that can be any length, and should probably contain some items that provide the tags the species requires.

For example, the order-able furniture for the Nickit:
```json
{
	"orderFurniture" : [
		{"name":"carbed", "count":1},
		{"name":"jukebox", "count":1},
		{"name":"royalthrone", "count":1},
		{"name":"diamonddisplay", "count":1},
		{"name":"goldenegg", "count":1},
		{"name":"goldenducky", "count":1},
		{"name":"treasurechest", "count":1},
		{"name":"barrelgoldfilled", "count":1},
		{"name":"woodencrategoldfilled", "count":1}
	]
}
```

### Step 2: Assign Base Color Palette
Starbound replaces colors on the base sprites of a species using color replace directives which are uniquely defined per species, this makes it difficult to use any assets interchangibly because most species have entierly different color palettes, therefore we have to define some extra data for SBQ's scripts to recolor its animations to work with your species, else the animations will just have funky colors.

Open the images in the `humanoid/` folder for your respective species in the unpacked mod, and open the `/species/speciesName.species.patch` file here. Look for `baseColorPalette`, you are going define a list of hex colors going from darkest to lightest, for each color in the sprite. These should be lists of 3-4 hex colors, where the first color is the outline and the fourth color is an optional highlight, if your color doesn't have a highlight, just remove it from the list. Do remember that **you cannot define 'new' colors here** colors which are not set by the species replace color directives will be unchanged and therefore would appear the same regardless of an individual's customization.

Further colors can be defined for use in SBQ's replacing, however 'primary' and 'secondary' are what are used for the color replacements on the digest drop items, so it's important to define them.

### Step 3: Assign Replace Colors to Generated Images
Within `/species/speciesName.species.patch` look for `sbqPartImages`, each entry is the relative path from `/humanoid/speciesName/` where an image will be generated when mods are loading, these images are used for SBQ's vore animations and are modified versions of the base species' images as well as recolored versions of animations from SBQ to match your species. Each entry has a `sourceImage` which is the image to copy and modify, `processingDirectives` a string of starbound's image processing directives to apply to the image, `patches` a list of image patching scripts to modify the image, and then lastly `remapDirectives` a list of pairs that are used to generate the color replace directives.

For most races, you will only need to concern yourself with the parts that have `remapDirectives`, simply go through the list and read the comments, replacing the second entry in each pair with the name of the color in the pallete it should be for your species.

for example, if this was for the belly part, if your species belly was say a 'teritary' color, you'd want to change it to:
```json
	[
		"primary",
		"tertiary"
	]
```

### Step 5: Include the Source Race Mod
In the unpacked mod's files (you may need to enable viewing hidden files) there should be a `_metadata` file. Open it as well as the one inside this mod, copy the `name` value from the source mod, it should be a string, then look in the metadata for this mod, replace `"Race Mod Name"` in the `includes` with the value of your race mod's name, this will make sure the patches load after the mod, and correctly add the relevant data.

You should also change this mod's `name` value while you're there.
