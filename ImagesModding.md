# F1Manager 2022 Texture Modding Guide
This is a guide to making changes to the texutres in the F1 Manager 2022 game.

## Requirements
- Fmodel: Easily check contents of game files: https://github.com/4sval/FModel/releases
- UE Viewer: Show and extract resources (3D models, textures etc.) from game filesЖ https://www.gildor.org/en/projects/umodel
- Gimp: A free PhotoShop alternative for editing DDS files used by the game: https://www.gimp.org/downloads/
- Unreal Engine 4.27 (install it from the EPIC launcher)

## Preparing Modding Files
Follow Unpacking guide found [here](https://github.com/carefreeduck/F1ManagerModding/blob/main/Packing.md).

## Extracting the asset
If you don't want to alter the image but replace it completely - you can skip to the next session.
For the purpose of this tutorial we will use the OnboardingCgaracter.uasset.
Find the character .uasset file in the Fmodel, right click on t and select "Export Raw Data"
![image](https://user-images.githubusercontent.com/6393266/189229856-ce6cba60-7035-472c-a155-69326ec56e58.png)
After that, open the image in the Umodel editor, making sure the right engine version is selected:
<img src="https://user-images.githubusercontent.com/6393266/189230187-6f0ab11c-f653-4efc-91d9-56ffe3aad157.png" width="50%" height="50%">

After that export the image. Make sure you are exporting as .dds - the checkbox is selected:
<img src="https://user-images.githubusercontent.com/6393266/189230523-22fa18cb-9a28-4094-a47c-9e37fb0df2fd.png" width="50%" height="50%">

Ok, you are done for now, you can make any changes as you see fit using GIMP or other editor that can work with DDS files.

## Creating final DDS file
Whether you are editing an existing image o a new one make sure it is exported with **no compression** (8888 BRGA). Add mip maps if necessary. As a rule of thumb - mip maps are needed for images that will be used as textures for 3D models.

## Replacing the asset
Go to the Unreal editor and create a folder for your texture in the content folder. Then drag your .DDS texture there.
Make sure the name of the texture is **identical** to the .uasset file.
![image](https://user-images.githubusercontent.com/6393266/189735622-5d2287b4-ca0a-4730-869a-931474c848c2.png)
Go File-> Save All
Then File-> Cook content for Windows
The output files will be in the "Unreal Projects\MyProject\Saved\Cooked\WindowsNoEditor\MyProject\Content" directory

That's it! Now you can repack the cooked .uasset and .uexp files to the pak and enjoy the updated texture in game.

## Credits
@carefreeduck for helping with the process.

robhal from modderbase.com for the tutorials that were used as the baseline:
- http://modderbase.com/showthread.php?tid=1448
- http://modderbase.com/showthread.php?tid=57
