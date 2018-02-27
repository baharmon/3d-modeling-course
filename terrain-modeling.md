# Contents
[**Terrain modeling**](#terrain-modeling)
[File Setup](#File-Setup)
    1. [**Elevation data sources**](#Elevation-data-sources)
    2. [**Data Acquisition**](#Data-Acquisition)
    3. [**Creating a Geodatabase**](#Creating-a-Geodatabase)
[**2D Terrain Modeling**](#2d-terrain-modeling)
[**3D Terrain Modeling**](#3D-terrain-modeling)

# Terrain modeling
In this section you will learn how to
acquire elevation data,
re-project and process the data in GIS,
and then model the data as
3D meshes and surfaces in Rhino.

## File Setup

### Elevation data sources
* [National Map Viewer](http://nationalmap.gov/viewer.html)
* [US Interagency Elevation Inventory](https://coast.noaa.gov/inventory/)
* [Open Topography](http://www.opentopography.org/)

### Data Acquisition

__This lesson uses data from the National Map. You can find data from other
sources, such as the links above.__

Navigate to the [National Map Viewer](https://viewer.nationalmap.gov/basic/).

![alt text](https://github.com/baharmon/3d-modeling-course/blob/master/images/arcmap/Lecture%201/National_map_1.png "National Map")

1. Search for Baton Rouge, Louisiana and zoom to the area of study.

2. Check the `box/point` selection tool and use it to draw a box around your study  
area. The searches you will run will look for data in this area.

3. Select `Elevation Products (3DEP)` under _Data_ in your left-hand Dataset menu.

Under `Elevation Products (3DEP)`, check the boxes for [1 meter DEM and 1/9 arc-second DEM](https://nationalmap.gov/3dep_prodserv.html) and click `Find Products`.

Your data will appear in the left hand search panel. Click the `Footprint` link
next to the data see its footprint on the map. _You can always deselect by
clicking footprint again_

Click the `download` link next to each piece of data you wish to download. For this exercise, download:

```
USGS NED ned19_n30x75_w091x25_la_statewide_2006 1/9 arc-second 2009 15x 15 minute IMG and
USGS NED ned19_n30x50_w091x00_la_statewide_2006 1/9 arc-second 2009 15x 15 minute IMG.
```

Find your zipped file folders for this data in your downloads folder.

Extract your data. Select one of your data folders, `right click > 7-zip > Extract to the folder of your choice.` _Suggested to save in your user folder. Windows C: drive > Users >
find your username._ Repeat this step for the second folder.

Delete the original zipped folders.

### Creating a Geodatabase for Your Project

Start ARC CATALOG.

Click `Connect to Folder` button:

![alt text](https://github.com/baharmon/3d-modeling-course/blob/master/images/arcmap/Lecture%201/arccatalog_connect_to_folder.png)

Browse to your username. Create a new folder for this data. Name it arcgis_data
(_Option to repeat this step and connect to your downloads folder here so you can navigate easily to your files you downloaded_)

Right click in or on new folder > New > File Geodatabase:

![alt test](https://github.com/baharmon/3d-modeling-course/blob/master/images/arcmap/Lecture%201/create_geodatabase.png)

Name your file `baton_rouge.gdb`

## 2D terrain modeling
In this section you will use the digital elevation file you gathered
and import it into Arc Map to create elevation and slope maps.

### Import Data

Launch ARC MAP.

Open a new blank map.

Check your map settings.
```
Geoprocessing > Geoprocessing Options > Check the first two optionsand  Disable or uncheck Background Processing
```

![alt text](https://github.com/baharmon/3d-modeling-course/blob/master/images/arcmap/Lecture%201/set%20up%20for%20arc%20catalog.PNG)

```
Customize > Extensions > Check Spatial Analyst
```

1. Click `Add New Data` button and browse to your downloads.

2. Select the .img file from your dataset > Add.

![alt text](https://github.com/baharmon/3d-modeling-course/blob/master/images/arcmap/Lecture%201/add_img_data.PNG)

3. A dialogue box will appear. Select `Bilinear Interpolation` in the drop down menu > `Yes`
(_Bilinear Interpolation is for continuous data like topographic maps. Nearest Neighbor is for discrete data_)

Repeat steps 1-3 for the other dataset you downloaded from the National Map.

Click the `Toolbox` button:
![alt text](https://github.com/baharmon/3d-modeling-course/blob/master/images/arcmap/Lecture%201/toolbox%20icon.PNG)

You will see your Toolbox pop up:

![alt text](https://github.com/baharmon/3d-modeling-course/blob/master/images/arcmap/Lecture%201/toolbox.PNG)

Dock your Toolbox by clicking the Toolbox window and hovering over one of the blue buttons that appear to snap to your workspace.

### Reproject the Dataset
```
Toolbox Data Management > Projections and Transformations > Raster > Project
```
![alt text](https://github.com/baharmon/3d-modeling-course/blob/master/images/arcmap/Lecture%201/getting%20to%20raster%20projection.PNG)

Select your data to reproject.

Name your file `reprojected_ned_1`.

Save your file to your Geodatabase.

Set your Projection.
```
Projected Coordinate Systems > State Plan > NAD 1983 (US Feet) > NAD 1983 State Plan Louisiana North FIPS 1701 (US Feet)
```

Click `Add to Favorites` before clicking `OK`.

`Resampling Technique` > `BILINEAR` > `OK`

Repeat these steps for other set of data.

Once completed delete original 'ned' data you downloaded.

### Combine via Mosaic

```
Toolbox > Raster > Raster Dataset > Mosaic To New Raster
```

Select both data sets.

`Output Location` find your geodatabase.

Name your file.

Select projection `NAD_1983`.

Pixel Type: 32_BIT_FLOAT

Cellsize: 10

Number of Bands: 1

`OK`

Delete older layers since you've now combined them as one mosaic. Right click on layer title in your layers box > Delete.

### Convert Vertical 'Z' Data From Meters to Feet
You've reprojected your dataset into feet, but the process above only converts x and y data. You need to convert your vertical data, or 'z' data, separately so your data is correct.

Customize > Extensions > Ensure Spatial Analyst is checked

``Toolbox > Spatial Analyst Tools > Map Algebra > Raster calculator``

Find your output folder, name it `elevation`

Convert your NED data to feet.

``Double click NED data to add it to the formula line > * 3.28084 > OK``

![alt_text](https://github.com/baharmon/3d-modeling-course/blob/master/images/arcmap/Lecture%201/calcpic.PNG)

``Right Click on elevation layer > Properties > Symbology > select a new range of color values to display your data.
Type: `Percent Clip`
Display Tab > Bilinear Interpolation (for continuous data) > OK``

To add definition to your elevation, use Hillshade.

``Toolbox > Spatial Analyst > Surface > Hillshade
Input Raster: elevation
Output Raster: your geodatabase and name your file 'Hillshade'
Select how much you want to exaggerate your z values and emphasize the hills here e.g. 10
OK``

In your Table of Contents, drag your Hillshade layer over your Elevation layer to see the changes.

Right click on `Hillshade > Display > Transparency > 75%`

You should see a more defined and 3 Dimensional-looking topography.

## 3D terrain modeling
In this section you will export
a digital elevation model from ARC GIS
and import it into Rhino for 3D modeling and visualization.

### <title>

Start Rhino5.

Open the template `Large Objects - Meters.3dm`.

Create a layer called `region` and make it the current layer.

Turn on `Grid Snap` and `Ortho`.
Create a 450m x 450m rectangle the size of our study landscape
with the corner-to-corner
[Rectangle](http://docs.mcneel.com/rhino/5/help/en-us/commands/rectangle.htm)
command.
```
_Rectangle
First corner of rectangle: 0,0
Other corner or length: 450,450
```

Create contours with the [Contour](http://docs.mcneel.com/rhino/5/help/en-us/commands/contour.htm) command.
```
_Contour
Select objects for contours
_Enter
Contour plane base point: 0,0,0
Direction perpendicular to contour planes: 0,0,1
Distance between contours: 1.00
_Enter
```

Save as `heightfield.3dm`.
```
_SaveAs
```

### Point cloud patching
Start ARC GIS.
Export `elevation_2016` as a comma delimited xyz point cloud.
```
INSERT INSTRUCTIONS
```

Start Rhino5.

Open the template `Large Objects - Meters.3dm`.

Create a layer called `point_cloud` and make it the current layer.

Import the elevation data.
Check `Create point cloud`.
Then zoom all viewports to the extent of the data.
```
_Import
Zoom
All
Extents
```

Use the
[Scale1D](http://docs.mcneel.com/rhino/5/help/en-us/commands/scale1d.htm)
command to vertically exaggerate your elevation data by a factor of 3.
```
Scale1D
Origin point: 0,0,0
Scale factor: 3
Scale direction: 0,0,1
```

Create a layer called `plane` and make it the current layer.

Create a corner to corner rectangular plane
with the [Plane](http://docs.mcneel.com/rhino/5/help/en-us/commands/plane.htm)
command.
Designate opposite corners of the point cloud.
Then use the Gumball to move the plane beneath the lowest point.
```
_Plane
```

Create a layer called `surface` and make it the current layer.

Use the [Patch](http://docs.mcneel.com/rhino/5/help/en-us/commands/patch.htm)
command to create a NURBS surface.
Set `Sample point spacing` to `1.0`,
set `Surface U spans` to `150`,
set `Surface V spans` to `150`,
and set the `Starting surface` to the plane.
```
Patch
```

Hide or delete the point cloud layer.

Set all viewports to `Rendered` mode.

Make the plane larger with the
[Scale2D](http://docs.mcneel.com/rhino/5/help/en-us/commands/scale2d.htm)
command
```
Command: Scale2D
Origin point
Scale factor: 1.25
```

Create a layer called `solid` and make it the current layer.

Use the
[Extrude surface to boundary](http://docs.mcneel.com/rhino/5/help/en-us/commands/extrudesrf.htm)
command to extrude the topographic NURBS surface to the plane
to create a solid model with a base.
Select the plane as the boundary surface.
```
_ExtrudeSrf
_Solid=_Yes
_ToBoundary
Select a boundary surface
```

Hide or delete the plane layer.

Save as `nc_spm_evolution_3m.3dm`.
```
_SaveAs
```



Create a layer called `mesh` and make it the current layer.

Create a triangulated mesh using the RhinoTerrain command `Create Terrain Mesh`.
Select the point cloud when prompted `Select objects for triangulation`.
Accept the previewed result.
```
RtMeshTerrainCreate
_Accept
```

Turn off or delete the `point_cloud` layer.
Set viewports to `Rendered` mode.

Create a 50m base for the terrain model
using the RhinoTerrain command `Create Terrain Base`
```
RtMeshTerrainBase
Select mesh (BaseHeightStyle=Relative  BaseHeight=50)
_Enter
```

Optionally use the RhinoTerrain `Create contour curves` command
to compute contours.  
```
RtCartographyContoursCurvesCreate
Select mesh
_Enter
Select mesh (FirstInterval=1  SecondInterval=10  ThirdInterval=0  FourthInterval=0  ContourSmoothness=0  Complete)
_Complete
```

Turn on the `Sun`,
set `Date and time` to `Now`,
and set `Location` to `Here`.
```
Sun
```

Save as `rhinoterrain.3dm`
```
_SaveAs
```
