## CUNY FemSTEM QGIS Hack: Subways and population in NYC

### Set Up
Make sure that you have QGIS 2.18 installed on your computer. If you do not, please refer to the [installation instructions](https://github.com/michellejm/Intro-QGIS-CUNY-FemSTEM/Install-QGIS.md). If you have 2.14, please update now by opening QGIS and clicking on the update link at the bottom. **Do NOT install 3.0** - that is the Early Adopter release, and it is not stable yet.

Download the GitHub repository for this course. Using the green button [here](https://github.com/michellejm/Intro-QGIS-CUNY-FemSTEM), select `Download ZIP`. The Data folder has all of the datasets you need for our hack today. 

### Premise

For this hack, we are going to estimate how many people live within a 1/2 mile radius of subway stations in New York City. We will use our findings to identify "transportation deserts", as well as "transportation hot spots". We will represent our findings with a heatmap. 

### Data 

For this hack, we will need four different datasets:
- A shapefile of New York City census tracts (provided)
- A shapefile of New York City neighborhoods (from NYC Open Data Portal)
- A shapefile of the subway stations (provided)
- Population data (from the Census) that can be used with our shapefile of census tracts (provided)


### Set Up QGIS

**Launch** QGIS. Your new blank map project will look like this:

![blank](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities_2018/blob/master/Images/mappingdata01_01.png)

Begin to familiarize yourself with the interface. For more information, refer to this [brief description](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities/blob/master/Resources/QGIS_InterfaceDescription.md) of the elements of the interface. 

#### Adding Layers

In order to construct our map within QGIS we will need to add our data layers to the map project. There are several ways to accomplish this. We will use the `Add Vector Layer` button. Click on `Add Vector Layer`

![vector](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities_2018/blob/master/Images/mappingdata01_02.png)

**Navigate** to the Data Folder. There are a number of different file extensions that may be unfamiliar. The files outlined in magenta are elements of the nyct2010 shapefile (which represents the 2010 census tracts. The files outlined in blue are components of the nynta shapefile (which represents the neighborhood tabulation areas). It is very important that all of these files stay together in the same folder otherwise QGIS will not be able to load the layer.

* .shp - The main file that stores the feature geometry (required).
* .shx - The index file that stores the index of the feature geometry (required).
* .dbf - The dBASE table that stores the attribute information of features (required).
* .sbn and .sbx - The files that store the spatial index of features.
* .prj - The file that stores the coordinate system information.
* For more information on these extensions and others see [this explanation by ESRI](http://webhelp.esri.com/arcgisdesktop/9.2/index.cfm?TopicName=Shapefile_file_extensions).

![vector](https://github.com/michellejm/Intro-QGIS-CUNY-FemSTEM/Images/femstem01_01.png) 

Select the `nyct2010.shp` file. A map should appear in the data window of QGIS (yours will likely be a different color)

![map](https://github.com/michellejm/Intro-QGIS-CUNY-FemSTEM/Images/femstem01_01.png)

