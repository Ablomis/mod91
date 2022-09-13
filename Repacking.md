# F1Manager 2022 Packing Guide
This is a guide to repack assets back into the F1 manager game.

## Requirements
- Unreal Engine 4.27 (install it from the EPIC launcher)

## Preparing the files
Create a folder named "toPack" on your **working directory**.
Copy edited assets into the toPack folder, making sure the folder structure is the same as in the base game, for example 
"F1Manager22/Content/Characters/Drivers/ValtteriBottas/Textures/T_F1_Driver_ValtteriBottas_Hair_Alpha.uasset"
No in your working directory create a txt file with the name "_path.txt"
The content of the _file_ should be:
f:\XXX\toPack\ *" "../../../"
Where "f:\XXX" is the path to your working directory and the "toPack" folder is the one we created earlier.

## Packing the assets
1. Open command line.
2. Find your UnrealPack.exe and drag and drop it to the cmd
3. Now add "F:\XXX\pakchunk0-WindowsNoEditor_1_P.pak" where XXX is the path to your working directory
4. Now add "-Create=" and drag and drop the txt file we created earlier
5.You should have a string similar to this: C:\Users\Ablomis>D:\UE_4.27\Engine\Binaries\Win64\UnrealPak.exe F:\Workspace\mod91\pakchunk0-WindowsNoEditor_1_P.pak -Create=F:\Workspace\mod91\_path.txt
6. Hit enter

Congratulations, your are done. Now copy the pakchunk0-WindowsNoEditor_1_P.pak from the working directory to the game pack directory and enjoy your mod.
