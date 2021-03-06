## CUNY FemSTEM QGIS Hack: Subways and population in NYC - PART II

Welcome Back!


In this part of the Hack, we are going to answer questions about how many people live in close proximity to a subway, and make a heatmap of subway access.

Start by opening a **NEW** QGIS Project. For the next part, the CRS of all of our files must match from the beginning. Therefore, there was a fair amount of data cleaning before this step. 

### Set Up

For this project, we need two data layers:

- subway entrances and population by census tract

Click on the `Add Vector Layer` button and add the following data layers from the Data/Shape/Part2 Folder:
* nyctracts_popjoin
* subway_entrances_May2016
* (you can optionally add the subway lines if you would like)

Change the project projection to CRS 2263: NAD83 / New York Long Island (ftUS). Notice that this projection is in feet, this will be important for us later.


### Analysis

When we left off, we had visualized population, but had not yet done any analysis. To answer our questions about how many people have easy access to the subway, we will draw circles 1/3 mile in radius around each subway station. We will then figure out how many people are in the census tracts that touch those stations. 

We will see why that approximation doesn't really work. Then we will make an approximation that tries to take into account what percentage of each census tract is within that buffer.

#### Making Buffers
Let's start with setting up buffers around our stations. 

Click on the subway_entrances layer, and navigate to  Vector >> Geoprocessing Tools >> Buffers

![vector](https://github.com/michellejm/Intro-QGIS-CUNY-FemSTEM/blob/master/Images/femstem02_02.png)

And make the following selections:

* Input vector layer: subway_entrances
* Buffer Distance: 1584
* Output Shapefile: stationbuffers.shp

The CRS of the stations layer is in feet, and 1584 is 1/3 of a mile.


![vector](https://github.com/michellejm/Intro-QGIS-CUNY-FemSTEM/blob/master/Images/femstem02_13.png)


#### Basic Analysis 

First, we would like to know which tracts have more than 12000 people living in them. 

* **Right Click** on the nyc_tracts_popJoin layer and select `Open Attribute Table`. 
* Click on the `Select Features by Expression` button

![vector](https://github.com/michellejm/Intro-QGIS-CUNY-FemSTEM/blob/master/Images/femstem02_04.png)

* Select the `Population` Column from the Fields List
* Write the following equation:
	 "Total" > 12000
* **Click** `Select` and then `OK`
	 

![vector](https://github.com/michellejm/Intro-QGIS-CUNY-FemSTEM/blob/master/Images/femstem02_18.png)


Click on `Move selection to top`.

![vector](https://github.com/michellejm/Intro-QGIS-CUNY-FemSTEM/blob/master/Images/femstem02_07.png)


Which boroughs have census tracts with more than 12,000 people? Do any of those tracts stand out? Which ones?

Save these features as their own layer. Right click on the layer and select `Save As` and make the following selections:

File Name: nyctracts_greater12000
* YES - Save only selected Features
* YES - Add saved file to map.

![vector](https://github.com/michellejm/Intro-QGIS-CUNY-FemSTEM/blob/master/Images/femstem02_19.png)

Close the Attribute table and navigate to Vector >> Research Tools >> Select By Location

![vector](https://github.com/michellejm/Intro-QGIS-CUNY-FemSTEM/blob/master/Images/femstem02_05.png)

Select features in stationbuffers that interset features in nyctracts_greater12000

Select the first option (include input features that intersect the selection features).

![vector](https://github.com/michellejm/Intro-QGIS-CUNY-FemSTEM/blob/master/Images/femstem02_20.png)

Open the Attribute table to see which stations those are. 

This tells us what subway stations serve those highly populated census tracts. What stations are they?

Deselect all features ![vector](https://github.com/michellejm/Intro-QGIS-CUNY-FemSTEM/blob/master/Images/femstem02_21.png)



#### Our Question

Now we want to figure out how many people live within 1/3 mile of the subway stations.

Again navigate to Vector >> Research Tools >> Select By Location

Select features in nyctracts_popJoin that intersect features in subwaybuffers

![vector](https://github.com/michellejm/Intro-QGIS-CUNY-FemSTEM/blob/master/Images/femstem02_10.png)

We can already tell that adding up the people in these tracts will be a very rough estimate. Both because some people are counted twice, and people who are outside the boundaries are counted

![vector](https://github.com/michellejm/Intro-QGIS-CUNY-FemSTEM/blob/master/Images/femstem02_11.png)

Despite this, we want to add up the total population within the selected census tracts. We will use the Basic statistics tool. Navigate to Vector >> Analysis Tools >> Basic Statistics. Make the following selections: 
* Input vector layer: nyctracts_popJoin
* Target Field: Total

![vector](https://github.com/michellejm/Intro-QGIS-CUNY-FemSTEM/blob/master/Images/femstem02_12.png)

This is about 5.5 million people. This probably is not correct since there are about 8.5 million people in NYC. Let's see if we can do better.

**Proportional Split Estimation**

Now we will refine our estimate of the population near libraries using a method called *proportional split estimation*. A proportional split is a way to estimate the proportion of a quantitative attribute that falls within a portion of a polygon’s area. A proportional split is calculated in a few fairly simple steps. 

1. We calculate the area of each polygon unit 
2. Clip the polygons to the boundary of the study area (in our case the ¼ mile buffers)
3. Calculate the area of the polygons after clipping them to the study area
4. Divide the area of the polygons within the study area by their original area to determine the proportion of the original area that falls within the study area
5. Multiply the attributes (for us, population in 2014) we wish to estimate by the proportion in order to estimate the proportion of the attribute that falls within the study area. 

Note: proportional split estimation assumes that the attribute you are estimating is evenly distributed through out the polygon. In reality the population within each census tract is not evenly distributed. However, this is a common method of estimation.


**Calculating the area of the census tracts**

Open the nyctracts_popjoin layer attribute table and select the field calculator.
* Create a new field
Output field name: `Area`

Output field type: `Decimal number (real)`

Output field width:`10` 

Precision: `2`

Open the Geometry menu and double click on `$area`

Click `OK`

![new field](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities_2018/blob/master/Images/mappingdata08_18.png)


* Scroll to the right in the Census Tracts attribute table and see the new field that you added. Now select the `Toggle editing mode` button to exit the editing mode. You will be asked if you want to save your changes, say `yes`. 


**Clipping the census tracts to the ¼ mile buffers**
* Next we will use the `clip` tool to clip the Bronx census tracts with the 1/3 mile buffers around the stations. 
* Navigate to `Vector`>`Geoprocessing Tools`>`Clip`
Make the selections:

Input vector layer: nyctracts_popJoin

Clip layer: subwaybuffers

Add result to canvas

Save the Output shapefile  as `nycTracts_Clip`. 

Click `OK` and then `Close`

![vector](https://github.com/michellejm/Intro-QGIS-CUNY-FemSTEM/blob/master/Images/femstem02_14.png)


A new layer containing the census tracts clipped to the 1/3 mile buffers around the stations was added to your map. 

Toggle the visibility of all of of the layers on your map off except for `nycTracts_Clip`.
 ![vector](https://github.com/michellejm/Intro-QGIS-CUNY-FemSTEM/blob/master/Images/femstem02_15.png)

**Calculating the area of the clipped census tracts**

Now we will calculate the area of these new polygons. Open the attribute table for the clipped census tracts layer: `nycTracts_Clip`, and select the field calculator.
* Create a new field
Output field name: `AreaClip`

Output field type: `Decimal number (real)`

Output field width:`10` 

Precision: `2`

Open the Geometry menu and double click on `$area`

Click `OK`

![vector](https://github.com/michellejm/Intro-QGIS-CUNY-FemSTEM/blob/master/Images/femstem02_16.png)

* Notice the new field that has been added to the far right of the attribute table called `AreaClip`

![vector](https://github.com/michellejm/Intro-QGIS-CUNY-FemSTEM/blob/master/Images/femstem02_17.png)

**Dividing the the area of the clipped census tracts by their original area**

* Again open the field calculator and create a new field
Output field name: `Proportion`

Output field type: `Decimal number (real)`

Output field width:`10` 

Precision: `2`

Calculate `”AreaClip” / “Area”` -- i.e. the proportion of the original area that remained after the clip

Click `OK`

![clip](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities_2018/blob/master/Images/mappingdata08_22.png)

**Multiplying the population by the proportion** 

Now we will calculate one final field where we’ll multiply the attributes (for us, population) we wish to estimate by the proportion in order to estimate the proportion of the attribute that falls within the study area. 

* Again open the field calculator and create a new field
Output field name: `Pop_est`

Output field type: `Decimal number (real)`

Output field width:`10` 

Precision: `2`

Calculate `"Proportion" * "Total"`

Click `OK`

End the edit session and say yes to saving the changes. 


Now we will compare the total estimated population within the buffers to the original rough population estimate we made at the beginning of this exercise using the select by location tool. 

* Navigate to `Vector`>`Analysis`>`Basic Statistics`.
* Select Pop_est as the Target field and note the Sum: about 3.9 million people, a much better estimate than what we found before. 

______________________________________________________________________________________________________________

Tutorial written by Michelle McSweeney, PhD, a Research Fellow in the [Center for Spatial Research](http://cunywomeninstem.org) at Columbia University. Taught at *CUNY FemSTEM HackDay*, one day hack-a-thon by the [CUNY Women in STEM](http://cunywomeninstem.org). More information about the hack day is available [here](http://cunywomeninstem.org/hackday/).








