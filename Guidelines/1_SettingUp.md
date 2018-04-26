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

#### Notes on data

When working with census data from the US Census, there are a few levels of granularity that you can look at: State, County, Census Tracts, and Block Group. Some areas have intermediate levels that may be tabulated or reported by the census, such as Neighborhood Tabulation Areas, or Metropolitan Regions. Usually, these are the result of combining smaller areas to make intermediate areas in regions of high population density. 

Census Tracts are areas created by the census for analytic purposes, and Block Groups are, just as described, groups of city blocks, smaller than census tracts. When trying to make population estimates of any kind, it is best practice to use the smallest area available for the data you are interested in (to protect privacy, some data is only available at the Tract or County level). However, even though population data is available at the block group level, we are going to use census tracts because this project comes out of a larger project on languages, education, and income in New York City, and those datasets are only available at the census tract level. 

#### Finding Data
Census data is published by the US Government in their annual American Community Survey Report. You can find historic census data by doing a data query at NHGIS. However, because we are really only interested in New York City, we are able to use the NYC Open Data Portal. In recent years, many municipalities have established these data portals to allow students, researchers, and enthusiasts easy acces to relatively (sometimes very) clean data. 



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

![vector](https://github.com/michellejm/Intro-QGIS-CUNY-FemSTEM/blob/master/Images/femstem01_01.png) 

**Select** the `acs_shape.shp` file. A map should appear in the data window of QGIS (yours will likely be a different color)

![map](https://github.com/michellejm/Intro-QGIS-CUNY-FemSTEM/blob/master/Images/femstem01_02.png)


### Projections

Our map looks a little skewed - this isn't quite how we are used to looking at New York City. There is an issue with the projection. Projections are sets of mathematical transformations that turn a bumpy 3D figure (usually the Earth) into a 2D map. Something is always sacrificed in this transformation, but we can limit the distortion by selecting a projection that is optimized to the region we are concerned with. 

We want to optimize our map by selecting a projection best suited to New York City. For a detailed discussion of projections, see [here](http://www.geo.hunter.cuny.edu/~jochen/gtech201/lectures/lec6concepts/map%20coordinate%20systems/how%20to%20choose%20a%20projection.htm). 

Navigate to Project >> Project Properties

![properties](https://github.com/michellejm/Intro-QGIS-CUNY-FemSTEM/blob/master/Images/femstem01_06.png)

Click on CRS at the left. Make sure that `Enable 'on the fly' CRS transformation` is enabled. Search for `NAD_1983_StatePlane_New_York_East_FIPS_3101_Feet` (ESPG: 102715). **Click** `Apply` and then `OK`

![selections](https://github.com/michellejm/Intro-QGIS-CUNY-FemSTEM/blob/master/Images/femstem01_07.png)

Notice that the number in the bottom right hand corner reflects the new CRS. 

![crs](https://github.com/michellejm/Intro-QGIS-CUNY-FemSTEM/blob/master/Images/femstem01_08.png)

Now we will add all of the other shapefiles. 

Again, **click** on the `Add Vector Layer` button, and add the 
* `nyc_nta.shp`,  
* `mta_stations`, and 
* `mta_lines`.


Once you add all of the files, your map should look something like this. If all 4 layers appear in the Layers Panel, but you can't see the subway lines or the stations, rearrange the order by dragging and dropping them in the Layers Panel.

![map](https://github.com/michellejm/Intro-QGIS-CUNY-FemSTEM/blob/master/Images/femstem01_09.png)

Now we want to add the population data. 

Click on the `Add Delimited Text Layer` button, and make the following selections:
* File Name: FemSTEM/Data/Tabular/nyc_population-by-tract_2013.csv
* File format: CSV
* Geometry definition: No geometry
* `OK`

![map](https://github.com/michellejm/Intro-QGIS-CUNY-FemSTEM/blob/master/Images/femstem01_10.png) 

New





















