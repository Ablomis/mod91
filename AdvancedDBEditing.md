# And guide on how to change starting calendar and initial car performance

## Objective of this guide

Objective of this guide is to show a solution to two fairly complicated problems of F1 Manager 2022 Modding

## What this guide WILL NOT cover

1. Basics like how to unpack and pack things
2. How to use DB Browser, i.e. how to import data.
3. What part values are, how they work etc. 

## Pre-requisites

1. Modified FModel: https://drive.google.com/file/d/1s9QZz_T6hHqueRX35UOBf7MhhIuK83m5/view?usp=sharing
2. AssetEditor: https://github.com/kaiheilos/Utilities
3. DB Browser for SQLite: https://sqlitebrowser.org/
4. Basic understanding of SQL (or ability to figure it out)
5. Knowledge of how to unpack and pack resources into paks (covered by many other guides)

## Baseline

The most important nuance about the calendar and initial car performance is that they are generated at run-time, 
so even you you know how to edit the database, you wont be able to simple change these paramteres as there are no records to change.
Still, we can achieve what we want with a couple of tricks.

## Database packing/unpacking

To get the starting game database you need to extract the Volta.uasset file from the pack with your prefered unpacking method.
Make sure you are extracting the last version of the database - it is included in the _0_P.pak
To do this make sure that this pak is IS THE ONLY file in the folder that your Fmodel is looking into.
After that you can export/import DB from FModel with right click.

Now the DB can be edited with DB Browser.

## Changing calendar

This is a simplier task of the two. To do this you need to:
1. Create a new calendar in the DB for all the seasons upfront
2. Change the DB query so that the game doesnt overwrite your calendar when you create new season

### Creating new calendar

1. In the database go to the Seasons table and create as many seasons as you plan to have in the mod (anywhere 10+ should be enough for sure).
You can edit prize pool, etc - it doesnt matter for our purpose.
![Capture3](https://user-images.githubusercontent.com/6393266/197652727-4d2e9f3c-649d-4361-abe1-ab67845511cd.PNG)

2. Now you need to add races for each of the seasons to the Races table. You need to provide only SeasonID, TrackID and Day, 
the rest of the values should be zeros. The calendar can be the same for each year or different - its up to you.
![Capture2](https://user-images.githubusercontent.com/6393266/197652714-c2b7935a-7fa6-4c79-8447-654de9ae1093.PNG)


3. Now in the pack finde the SeasonPS file, unpack it and open it with the Asset Editor.
There edit the highlighted field to contain following query: 
"UPDATE Races SET State = 0, RainPractice = ?4, TemperaturePractice = ?5, WeatherStatePractice = ?6, RainQualifying =?7, TemperatureQualifying = ?8, WeatherStateQualifying =?9, RainRace =?10, TemperatureRace=?11, WeatherStateRace=?12 WHERE SeasonID = ?2 AND TrackID = ?3;"
![Capture](https://user-images.githubusercontent.com/6393266/197652695-bbd9991d-09ac-4eb0-ac19-892f83e3e19d.PNG)


4. Save everything and repack. Now whenever a new season starts - the game will update your calendar with new weather instead of generating new calendar.

Thats it, you can enjoy your new calendar!

### Changing part values

1. You need to create a new table in the database with the name Parts_InitialValues. It should contain following columns: PartType, StatID, TeamID, NewValue, NewUnitValue. It shouldcontain all the stats for the initial parts for every tam in your mod. To get the initial values you can start a save and export the data from the Parts_DesignStatValues table.
![Capture4](https://user-images.githubusercontent.com/6393266/197653162-b41f4d03-0346-45b0-a987-fa1b991c950e.PNG)

2. After you have the initial data in the db, go to the "ExecuteSQL" tab and run this query:
```
CREATE TRIGGER set_initial_parts_stats
   AFTER INSERT ON Parts_DesignStatValues
   WHEN new.DesignID=91 AND new.StatID = 9
BEGIN
UPDATE Parts_DesignStatValues
    SET
        Value = (SELECT newValue 
            FROM (SELECT * FROM Parts_DesignStatValues 
                LEFT JOIN Parts_Designs 
                    ON Parts_DesignStatValues.DesignID = Parts_Designs.DesignID
                LEFT JOIN Parts_InitialValues 
                    ON Parts_InitialValues.PartType = Parts_Designs.PartType 
                    AND Parts_InitialValues.TeamID = Parts_Designs.TeamID
                    AND Parts_InitialValues.StatID = Parts_DesignStatValues.StatID)
            WHERE DesignID = Parts_DesignStatValues.DesignID
            AND StatID = Parts_DesignStatValues.StatID),
        UnitValue = (SELECT newUnitValue 
            FROM (SELECT * FROM Parts_DesignStatValues 
                LEFT JOIN Parts_Designs 
                    ON Parts_DesignStatValues.DesignID = Parts_Designs.DesignID
                LEFT JOIN Parts_InitialValues 
                    ON Parts_InitialValues.PartType = Parts_Designs.PartType 
                    AND Parts_InitialValues.TeamID = Parts_Designs.TeamID
                    AND Parts_InitialValues.StatID = Parts_DesignStatValues.StatID)
            WHERE DesignID = Parts_DesignStatValues.DesignID
            AND StatID = Parts_DesignStatValues.StatID);
END;
```
In nutshell this query creates a trigger which runs whenever the game creates initial set of Parts in the DB and updates these parts with the alues from the Parts_InitialValues table.

That's i, was not that hard, eh?)





