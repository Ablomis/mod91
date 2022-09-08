# F1Manager 2022 Images Modding Guide
This is a guide to making changes to the texutres in the F1 Manager 2022 game.

## Requirements
- Fmodel: Easily check contents of game files: https://github.com/4sval/FModel/releases
- UE Viewer: Show and extract resources (3D models, textures etc.) from game filesЖ https://www.gildor.org/en/projects/umodel
- Gimp: A free PhotoShop alternative for editing DDS files used by the game: https://www.gimp.org/downloads/
- Hex Editor when injecting back to Uasset: https://hhdsoftwaredocs.online/

## Preparing Modding Files
Follow Unpacking guide found [here](https://github.com/carefreeduck/F1ManagerModding/blob/main/Packing.md).

## Extracting the asset
If you don't want to alter the image but replace it completely - you can skip to the next session.
For the purpose of this tutorial we will use the OnboardingCgaracter.uasset.
Find the character .uasset file in the Fmodel, right click on t and select "Export Raw Data"
![image](https://user-images.githubusercontent.com/6393266/189229856-ce6cba60-7035-472c-a155-69326ec56e58.png)
After that, open the image in the Umodel editor, making sure the right enfine version is selected:
![image](https://user-images.githubusercontent.com/6393266/189230187-6f0ab11c-f653-4efc-91d9-56ffe3aad157.png)
After that export the image. Make sure you are exporting as .dds - the checkbox is selected:
![image](https://user-images.githubusercontent.com/6393266/189230523-22fa18cb-9a28-4094-a47c-9e37fb0df2fd.png)
Ok, you are done for now, you can make any changes as you see fit using GIMP or other editor that can work with DDS files.

## Creating final DDS file
Whether you are editing an existing image o a new one make sure it is exported with BC3/DXT5 compression and "No mipmaps" selectd.

## Replacing the asset
Now open your new DDS file in the hex editor and copy all data from offset 80 till the end. Take notice of the total size in the Information panel on the right side. In our case it is 409,600.
![image](https://user-images.githubusercontent.com/6393266/189231320-603986e7-5987-455b-8b27-381bcf783b37.png)
Now open the .uexp file that was exported from the .umodel (it is exported together with the .uasset file) in the hex editor as well. You will replace the original image data with the new data.
You need to be very precise now:
1) Go to the very bottom.
2) Don't touch the last 28 bytes - they are not a part of the image.
3) start selecting everything from before then
 ![image](https://user-images.githubusercontent.com/6393266/189232063-57b727e9-0d57-4124-9cf9-f5a07fcb0b87.png)
4) and go up untill the Total size in the information window is the same: 409,600
![image](https://user-images.githubusercontent.com/6393266/189232255-ff76d287-d5aa-40ca-8106-74f7fffde289.png)
5) Now replace the existing data with the copied data from the new image.
6) Save the .uexp file.

That's it! Now you can repack the .uasset and .uexp files to the pak and enjoy the updated texture in game.

## Credits
@carefreeduck for helping with the process.
robhal from modderbase.com for the tutorials that were used as the baseline.
