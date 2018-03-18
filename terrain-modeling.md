# Contents
[**Terrain modeling**](#terrain-modeling)
[**File Setup**](#File-Setup)
    1. [**Elevation data sources**](#Elevation-data-sources)
    2. [**Data Acquisition**](#Data-Acquisition)
    3. [**Creating a Geodatabase**](#Creating-a-Geodatabase)
[**2D Terrain Modeling**](#2d-terrain-modeling)
[**3D Terrain Modeling**](#3D-terrain-modeling)
[**Creating Contours from a Solid**](#creating-contours-from-a-solid)

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
* [Open Topography](http://opentopo.sdsc.edu/datasets)

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

**under development**

##Creating Contours from a Solid
This section is useful if you want to generate contours for a 3D form you have
sculpted by hand or virtually through Rhino or a 3D modeling program.

Open your object in Rhino.
Select object

  ``rhino terrain > cartography > create contour curves``

You can manipulate the contour interval by clicking the link in the dialogue box and
assign a new value.

For example:

  Click the number by `First interval = ` and change to 1
	Second interval = 5
	Third interval = 25

You will see your contours (in ft) added in the layer panel.

Add annotations to your contours by toggling to the drop down Rhino Terrain:

	``Rhino Terrain > Cartography> Annotate contour curves
	select layer to annotate > Dimension - set to foot-inch architectural``
