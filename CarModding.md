# Car Modding Guide
This is a guide on how to change any cars in the F1 Manager 2022.

## Requirements
- Unreal Engine 4.27 (install it from the EPIC launcher)
- 3D modelling software (i.e. Blender or 3DS Max)
- UE asset Editor: https://github.com/kaiheilos/Utilities

## How game manages cars
Paath's to the various car meshes are in the F1Manager22/Content/Cars/DA_CarDataAsset.uasset file.
There you have a list of all teams linked to all components. For example:
```
        "EF1Team::McLaren": {
          "CarSkeletonMesh (912)": {
            "AssetPathName": "/Game/Cars/BaseCars/BaseCar2022/Skeleton/SK_BaseCar2022.SK_BaseCar2022",
            "SubPathString": ""
          },
          "CarBodyMesh (949)": {
            "AssetPathName": "/Game/Cars/BaseCars/BaseCar2022/CarParts/Body/Body_A/Models/SK_BC2022_c_Body_A.SK_BC2022_c_Body_A",
            "SubPathString": ""
          },
```
This tells the game to look for the McLaren body mesh down a specific path.This means you can easily make every car in the game using a unique body.
pen the file in the UE Asset Editorand change the corresponding line to:
```
/Game/Cars/BaseCars/BaseCar2022/CarParts/Body/Body_McLaren/Models/SK_BC2022_c_Body_McLaren.SK_BC2022_c_Body_McLaren
```

Boom, done

## Car model
The car consists of following components:
- Body
- Nose cone
- Front Wing Left, Right
- Rear Wing Left, Right
- Susspension Front/Rear Left, Right
- Brakes Front/Rear Left, Right
- DRS
So you need to split your models into these elements to load it. If you don't need an element you can load an empty object. There are also damaged variants of the models if you need them, if you dont have damaged options-you can just use the same model everywhere.
For car elements other then the body, the paths to meshes are written in the data files in team folders, i.e.:
```
F1Manager22/Content/Cars/RaceTeams/McLaren/DataAssets
```

## Materials and textures
The game pretty much allows only one custom material and texture per car. Game material controller only uses these files:
- basecolor texture file, i.e. T_McLaren_Livery_basecolor
- material mask, i.e. T_McLaren_Livery_materialmask
- normal mask, i.e. T_McLaren_Livery_normal
- mask, ie. T_McLaren_Livery_basecolor
Plus same files for cockpit
Plus sponsors file, i.e T_McLaren_Sponsors_basecolor.uasset
This means that if you have a model with 20 materials and 20 textures you won't be able to import it. You need to bake all the textures in a single file.This is an example of how to do it for Blender:
https://www.youtube.com/watch?v=wG6ON8wZYLc
The files can be found here: 
```
F1Manager22/Content/Cars/RaceTeams/McLaren/Textures
```

## Importing the car into the game
1.To import the car into the game each imported element **MUST HAVE** material named "MI_Default_MaterialMask" in the slot named "Team_MaterialSlot".
The game looks for this material and replaces it with a specific material for a specific team.
// screen to be added
2. The element needs to be exported as an .fbx file
3. You drag and drop the file into the UE editor. The file MUST have a skeleton mesh to be displayed in the game.(I dont think the game actually uses it anyway, becuase it is defined separately in game)
4. Obviously create materials
5. Check that the element is displayed correctly in the preview window
6. Check that material names are correct as per #1
7. Check the element name. (Physics and skeleton assets don't really matter)
8. Check your baked texture name 

Now save all, cook the assets, put them in the right directories and package the game.

That was not that hard, right?

