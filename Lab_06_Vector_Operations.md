**Learning Objective**

To introduce vector operations, including buffer, dissolve, and overlay
operations.

Vector operations refers to a variety of tools that operate on vector
data. While far from exhaustive, this lab tutorial walks through a set
of extremely commonly used operations. Each section of the tutorial is
essentially a standalone mini-tutorial that introduces how a specific
operation works, using data from Philadelphia, Pennsylvania. You will
notice that each of these operations has a variety of settings and
capabilities beyond what you are instructed to do. You are encouraged to
explore the various functionalities of these tools. In the assignment,
you will be asked to work with several of these tools in sequence,
performing a classic **site suitability analysis**, through which these
tools identify optimal locations based on a set of spatial criteria.

First, complete the tutorial by following the steps below. Then, using
the skills you’ve learned in the tutorial, complete the assignment given
following the tutorial.

## TUTORIAL

## Acquiring the Data

We will work with data for Philadelphia (and one data set for
Pennsylvania) made publicly available by PASDA, the Pennsylvania
Geospatial Clearinghouse ([PASDA](http://www.pasda.psu.edu)). From
PASDA, use the search function to find and download the following files
layers:

  - SEPTA High Speed Stations (2012)  
    These are the **subway stations**.
  - SEPTA Regional Rail Stations (2016)  
    These are the **regional rail stations**.
  - SEPTA Routes Spring (2016)  
    These are the **bus routes**.
  - Philadelphia Health – Healthy Corner Stores (2016)  
    These are **corner stores** participating in the Food Trust’s
    Healthy Corner Store Initiative –
    <http://thefoodtrust.org/what-we-do/corner-store>.
  - Philadelphia Health – Farmers Markets (2016)  
    These are **farmers markets**.
  - Philadelphia Empowerment Zones (2012)  
    These are **empowerment zones** – areas targeted by the City of
    Philadelphia for investment –
    <https://www.phila.gov/programs/empowerment-zones/>.
  - 2015 Cartographic Boundary File, State-County-Census Tract for
    Pennsylvania, 1:500,000 (2015)  
    These are Pennsylvania **census tracts** – US Census Bureau
    enumeration units.

Some of these files will be used for the tutorial portion of the lab.
Others will be used for the assignment.

Move all files to your workspace folder and unzip any zip files.

## Explore and Integrate the Data into a Common CRS

Open ArcGIS Pro and create a new project. Explore each of the downloaded
data sets in the catalog view, including the maps and tables.

Pay particular attention to the CRS of each data set. Note that most are
in Pennsylvania State Plane South, which is the CRS used by the City of
Philadelphia for its GIS data. The exceptions are the regional rail
stations, the bus routes, and the census tracts. Using the **Project** tool, 
convert these data sets to `NAD 1983 StatePlane Pennsylvania South FIPS 3702 (US Feet)`. 
Use these converted data sets in the remainder of the tutorial and
assignment.

**Note:** If you are converting to the same CRS multiple times,
you may wish to it to your **Favorites** for easy access.
To do this, once you have located the appropriate CRS in the **Coordinate System**
window, click on its name, and then click on the star to the right of it.
It will now be in your favorites for the next time you wish to select it.

![](images/crs_favorites.png)

## Adjust the Environment Settings

Now, we will set up the Environment in ArcGIS. This will specify a
workspace folder for all of the data files generated when running
different operations and make it easier to save output files.

1.  On the **Analysis** tab, choose **Environments**.
2.  Under **Workspace**, set the **Current and Scratch Workspaces** to your
    workspace folder (See example below; your path will differ.) You do
    not need to modify any of the other fields. Click OK. Each time you
    run an operation in this project, the output will be saved to your workspace. Be
    sure to set up your **Environment** in ArcGIS each time you begin a new project.

![](images/lab6_fig1.png) 

## Retrieve Line Length and Polygon Area Measurements

Here, you will learn how to use ArcGIS to calculate line length and
polygon area and encode these values in an attribute table.

We will start by adding a new field and populating the field with new
length values in feet.

1.  Add the bus routes layer to a new map (be sure to add the new data set
    you created that is in Pennsylvania State Plane CRS).
2.  Open the attribute table. Explore the data by selecting some
    different routes so you understand that each record in the table
    represents one bus route.
3.  Add a new field to the table. Name the new field `Lengthft`. Make it
    a double data type.
4.  Right-click on the header for the new field (where it says
    `Lengthft`) and click **Calculate Geometry**.
5.  In the **Calculate Geometry Attributes** pane make sure the **Target Field**
    is set to `Lengthft`, the **Property** is set to 'Length', and the **Length Unit** is set to 'Feet (United States)'.
6.  Click Run. You should see the new length values appear in the field. 
    This is the length of each bus route in feet.

It is also possible to calculate the area of polygons.

1.  Turn off the bus routes layer, and add the empowerment zones
    shapefile to the map.
2.  Open the empowerment zones attribute table.
3.  Add a new field called `Areaft`. Calculate the area of each
    empowerment zone for this new field by following the analogous
    procedures as above, but instead of length choose to calculate the
    area in square feet.
4.  Close the map.

## Buffer

Here, you will learn how to use ArcGIS to perform a buffer operation. We
will buffer points here but it is also possible to buffer lines and
polygons in the same way.

1.  Add the farmers markets shapefile to a new map.
2.  On the **Analysis** tab, scroll down in the tool gallery in the **Tools** group 
and click on **Buffer**.  (The Buffer tool can
    also be accessed from the Toolbox by going to **Analysis
    Tools→Proximity→Buffer**.)

![](images/lab6_fig2.png)

3.  In the Buffer pane, for the **Input Features** choose the farmers
    markets. Name the output shapefile `farmark_1320ft` and make sure it
    saves to your workspace. For the **Distance** box, 
    enter 1320, with a **Linear Unit** of feet. Leave the rest of the options as the defaults. Press Run.
4.  View the new shapefile, which has been added automatically to the map. Zoom in to where there are
    clusters of farmers markets to see how the buffer polygons overlap.
    Open the attribute table and see that each buffer disk is
    represented as a single record in the attribute table.  
    ![](images/lab6_fig3.png) 
5.  Repeat the buffer analysis, but this time, for Dissolve Type
    choose 'Dissolve all output features into a single feature'. Call the output `farmark_1320ft_dissolve`.
6.  View the new shapefile in the map. Zoom in to a cluster of farmers
    markets to see how the buffers merge together (i.e. are dissolved as
    single polygons in these locations). Open the attribute table and
    see that *all* polygons are represented by a single record in the
    attribute table. This is called a **multipart object** (in this
    specific case a **multipolygon**) because a single feature (i.e. the
    one record in the attribute table) has multiple parts (i.e. multiple
    individual polygons).  
    ![](images/lab6_fig4.png) 

It is often useful to have an individual record in the attribute table
for each polygon. Here, we will convert this multipart layer to a
singlepart layer using a **Multipart to Singlepart** operation.

1.  In the toolbox, go to **Data Management Tools→Features→Multipart
    to Singlepart**. In the **Multipart to Singlpart** pane, for **Input
    Features** select `farmark_1320ft_dissolve`. Name the output
    `farmark_1320ft_dissolve_single`. Press Run.
2.  View the resulting shapefile. Notice it appears graphically the same
    as the original `farmark_1320ft_dissolve` shapefile. Open the
    attribute table. Notice that it now has 43 records—one for each separate
    polygon.
3.  Explore this new shapefile by selecting certain records in the
    attribute table to see which polygon is associated with a single
    record.  
    ![](images/lab6_fig5.png) 
4.  Close the map.


## Map Dissolve

Here, you will learn how to use ArcGIS to generalize a data layer by
dissolving the boundaries of adjacent polygons.

1.  Add the census tracts shapefile (be sure to add the one in
    Pennsylvania State Plane CRS).
2.  First we will dissolve all the tract boundaries to yield the
    boundary of the state. Go to the tool gallery on the **Analysis** pane and select
    **Dissolve**. (The Dissolve tool can also be accessed from the **Toolbox**
    (**Toolbox→Data Management Tools→Generalization→Dissolve**).
3.  For **Input Features** select the census tracts shapefile. Name the
    output file `PA_bnd`. Press Run.
4.  View the resulting shapefile. Note that the new shapefile is simply
    a boundary of Pennsylvania – the operation has dissolved all the
    boundaries between the tracts, resulting in an outline of the entire
    set of features (i.e. the entire state of Pennsylvania). You’ll
    notice that the resulting feature has one record in the attribute
    table.  
    ![](images/lab6_fig6.png) 

Now we will compare what you have just done to what happens when you
dissolve by an attribute.

5.  Open the attribute table of the original census tracts shapefile and
    find the field `COUNTYFP`. This field is a 3-digit identifier for
    each individual county (e.g. `091` for Montgomery County) in the
    state. The code contained in the `GEOID` field for each tract is composed of the state code (`STATEFP`), the county code (`COUNTYFP`), and the tract code (`NAME`).  In Philadelphia, all tract `GEOID` codes begin with `42101` (`42` for Pennsylvania; `101`
    for Philadelphia County).  It is thereford possible to perform a map
    dissolve operation on the PA tracts shapefile, using the
    `COUNTYFP` field as the dissolve field.
6.  Perform another dissolve operation on the original census tracts
    shapefile, but this time under **Dissolve Field(s)** check
    the COUNTYFP box. This will dissolve the boundaries among adjacent
    tracts that share the same county identification. Name the output
    shapefile `pa_counties`. Press Run.
7.  View the resulting shapefile. You’ll see that the tracts
    have been dissolved by the county identifier, resulting in a
    shapefile of county outlines.  
    ![](images/lab6_fig7.png) 
8.  Close the map.

## Point in Polygon Overlay

Here, you will learn how to use ArcGIS to create a **spatial join**
based on a point in polygon overlay. Point in polygon is a type of
spatial relationship used to find points that are geographically
contained by a polygon. Other types of spatial relationships are
possible. A **spatial join** is used when you want to attach data in one
vector layer to another layer based on a spatial relationship. It is common to use a **spatial join** to attach the attributes from a polygon data layer
to the points in a point layer. It is possible to do spatial joins with other
vector data types, for example to attach county names (from a polygon
layer) to road segments (in a line layer), or attach a voting precinct
(from a polygon layer) to residential buildings (in a polygon layer).

Here, as an example, we will join data from the polygon counties layer
onto the regional rail station points.

1.  Add the regional rail stations and the counties shapefiles (which
    you just created) to a new map. Review each of their attribute tables
    and note the fields that are included in each.
2.  Right-click the regional rail stations shapefile and choose **Joins
    and Relates→Spatial Join**. (You can also access the **Spatial Join** tool by clicking the button on the contextual **Data** tab in the **Feature Layer** section.)
    ![](images/spatial_join.png)
3.  In the **Spatial Join** Geoprocessing pane, for **Target Features** make sure the `regional_rail_sps` layer is selected.  We are aiming to move data into these features from another layer's features.
4.  For **Join Features** select the counties layer. Name the output shapefile `rail_stations_w_counties`.


5.  You are also given a choice of how to associate each polygon with a given point. For **Match Option**, choose 'Within'. The choices in this list  
describe the relationship of the **Target Features** to the **Join Features**.  In this case, we want to trigger a match if the rail stations are **within** a county.  When you are specifying a spatial relationship, be sure to read the descriptions of the types of relationships carefully to choose the correct one. More information about spatial relationships can be found at "[Select by location graphic examples](https://pro.arcgis.com/en/pro-app/tool-reference/data-management/select-by-location-graphical-examples.htm)"

6.  Press Run.
7.  View the new shapefile. Note that spatially it is identical to the
    original regional rail stations shapefile. Open the new shapefile’s
    attribute table. You will observe that the attributes from the
    counties file are now appended to the attributes of the original
    regional rail stations shapefile, based on the county within which
    each station resides.
    
    ![](images/lab6_fig8.png)

You can also join points to polygons. The problem, however, is that many
points can fall within a single polygon. Since we cannot store multiple
values of a single attribute for a single polygon (a violation of first
normal form), the software provides a few options for transforming
multiple point values to a single value for each polygon.

1.  Right-click on the counties shapefile and choose **Joins and
    Relates→Spatial Join**.
2.  In the **Spatial Join** pane, choose the regional rail stations shapefile as the layer to join to the counties layer.  Name the output file `counties_w_railstations`.
3.  You are given a choice under the **Field Map of Join Features** for
how to summarize the point data.  Each item in the **Output Fields** list
corresponds to a column in the resulting shapefile. When you click on a field in this list, you can specify the **Merge Rule** for that field,
which defines what will happen when there is more than one matching join
feature for a target feature.  The default choice is 'First', which simply
takes the first point within the polygon that the algorithm comes to.  You 
can also choose 'Count', which would total the number of points that fall within the polygon. 

![](images/field_map_join_features.png)
4. Change the **Merge Rule** for the `Line_Name` attribute to 'Count', and for the rest of the attributes leave the default choice of 'First'. Click Run.

5.  View the new shapefile and open its attribute table. You should see that the `Line_Name` attribute contains the number of regional rail station points that fall within each county polygon. The other attributes contain their corresponding data from the first matching point in each county. Most counties
    have no rail stations, of course, since these are counties for all of
    Pennsylvania, but there should be many regional rail stations in
    Philadelphia county and the surrounding counties.
6.  Close the map.

## Polygon Overlay: Clip

A **Clip** operation is akin to a 'cookie cutter' operation whereby one
layer (points, lines, or polygons) is clipped to the boundaries of
another polygon data layer. As an example, we will clip the bus routes
lines to the empowerment zones polygons.

1.  Add the empowerment zones and bus routes shapefiles to a new map.  
    ![](images/lab6_fig9.png) 
2.  Go to the tool gallery on the **Analysis** pane and select
    **Clip**. 
    
    ![](images/clip.png)
    (The **Clip** tool can also be accessed from the **Toolbox**
    (**Toolbox→Analysis Tools→Extract→Clip**).

3.  In the **Clip** pane, for **Input Features** select the bus routes
    layer. For **Clip Features** select the empowerment zones layer. Name
    the output shapefile `bus_routes_clip`. Press Run.
4.  View the new shapefile, which should include only the bus routes
    within the empowerment zones polygons. It may be helpful to turn off
    the original bus routes layer.  
    ![](images/lab9_fig10.png) 
5.  Open the attribute tables for the original bus routes layer and the
    new clipped bus routes layer. Note that they have the identical set
    of fields.

## Polygon Overlay: Erase

An **Erase** operation is akin to a 'pencil eraser' operation whereby one
polygon layer use to erase (or eliminate) the features of another point,
line, or polygon layer which fall within its boundaries. As an example,
we will erase the bus routes lines that fall within the empowerment
zones polygons (i.e. the complement of the previous clip operation).

1.  Turn on only the empowerment zones and the original bus routes
    layers (or add them to the map if you have removed them).
2.  Open the toolbox. In
    the 'Find tools' search box, type in 'erase' and choose **Erase (Analysis)**. Note that
    the erase tool can also be accessed from the toolbox tree
    (**Toolbox→Analysis Tools→Overlay→Erase**).
3.  In the **Erase** pane, for **Input Features** select the bus routes
    layer. For **Erase Features** select the empower zones layer. Name the
    output shapefile `bus_routes_erase`. Press Run.
4.  View the new shapefile, which should include only the bus routes
    which are NOT within the empowerment zones polygons. It may be
    helpful to turn off the original bus routes layer and give the 
    empowerment zones a 'No Color' fill.  
    ![](images/lab6_fig11.png) 
5.  Close the map.

## Polygon Overlay: Intersect

The intersect overlay operation creates a new data layer from the areas
that overlap when you overlay multiple polygons shape files (that is,
the output data layer only includes areas that are spatially coincident
to all the input layers). As an example we will intersect the
empowerment zones with a buffer of the corner stores which we perform
here.

1.  Add the empowerment zones layer and the corner stores layers to
    a new map.
2.  Perform a buffer operation on the corner stores using a buffer
    distance of 1320 feet and using the 'Dissolve all...' option. Name the
    new shapefile `stores_1320ft`.
3.  Turn off the corner stores layer. Change the symbology of the
    empowerment zones so it has a 'No Color' fill. Then order the layers so that the
    empowerment zones is on top. You can see the area of intersection –
    the area where the stores\_1320ft and the empowerment zones are
    spatially coincident, or overlap each other.  
    ![](images/lab6_fig12.png) 
4.  Go to the tool gallery and choose Intersect. 
    ![](images/intersect.png)
Note that the intersect tool can also be accessed from the Toolbox
    (**Toolbox→Analysis Tools→Overlay→Intersect**).
5.  For **Input Features** first add the empowerment zones and then enter
    the `stores_1320ft` layer (the order of entry does not matter). Name
    the output layer `zones_storebuf_intersect`. Press OK.
6.  View the resulting shapefile. It may be helpful to toggle the other
    layers on and off to see it – the area within both the empowerment
    zones and the store buffer layers.  
    ![](images/lab6_fig13.png) 
7.  Open the attribute table for the new shapefile. Note that it has
    fields from both the empowerment zones and the `stores_1320ft`
    attribute tables. It also has only two records, though there are
    more than two polygons, indicating it is a multipart layer.

## Polygon Overlay: Union

The union overlay operation creates a new data layer representing the
combination of all the areas from all the input layers. As an example we
will union the empowerment zones with the buffer of the corner stores
you created in the last step.

1.  In the map, turn on the empowerment zones and the `stores_1320ft`
    layers and turn off the other layers.

2.  Go to the tool gallery and choose **Union**. 
![](images/union.png)
Note that the
    union tool can also be accessed from the toolbox (**Toolbox→Analysis
    Tools→Overlay→Union**).

3.  For **Input Features** first add the empowerment zones and then enter
    the `stores_1320ft` layer (the order does not matter). Name the
    output layer `zones_storebuf_union`. Press Run.

4.  View the resulting shapefile. It may be helpful to toggle the other
    layers on and off to see it – the area that includes all of both the
    empowerment zones and the `stores_1320ft` layers.
    
    ![](images/lab6_fig14.png) 

5.  Open the attribute table. Note that it contains fields from the both
    the input layers.

6.  Note there are only 5 records in the attribute table. Select each of
    the records and view which polygons are selected in the map. There
    are clearly multiple polygons associated with certain records,
    i.e. it is a multipart shapefile.

7.  Note the `FID_stores` field, and its values - -1 and 0. These values
    indicate whether a polygon is inside or outside the original
    stores\_1320ft buffer. A value of -1 indicates the feature is
    outside the buffer; a value of 0 indicates the feature is inside the
    buffer. To demonstrate this, select those records in the table that
    equal -1. Those polygons outside the 1320 foot buffer distance of a
    corner store, but within the empowerment zones, will be selected.
    
    ![](images/lab6_fig15.png) 

It often helpful to convert the result of a union operation from
multipart to a singlepart shapefile. Here, we will convert the
`zones_storebuf_union` shapefile.

1.  Clear your selection, if you have any features still selected.
2.  In the Toolbox, search for and
    execute the **Multipart to Singlepart** tool. Perform the multipart to
    singlepart operation on the `zones_storebuf_union` shapefile.
3.  View the resulting shapefile. It should now have 29 records in the
    attribute table – one for each separate polygon.

# ASSIGNMENT

## Objective

Consider the city government is choosing a location to encourage the
development of a new store selling healthy foods. The target location
should be: within an area targeted for investment, accessible by public
transit, and not nearby current farmers markets or corner stores
participating in the Healthy Corner Store Initiative.

The following specific spatial criteria are given:

1.  Within a Philadelphia empowerment zone
2.  Within 2000 feet of a subway station or regional rail station
3.  NOT within 1200 feet of a farmers market or corner store
    participating in the Healthy Corner Store Initiative.
4.  A larger contiguous area is preferable to increase the likelihood of
    securing a potential property.

## Deliverables

**Turn in a report in the format described in the syllabus.**

Be sure to include the following information:

1.  One or more maps illustrating the data sets used and a step in the
    methodology.
2.  A map showing the area(s) that meet the spatial criteria.
3.  State the area in square feet for each area that meets the criteria
    and make a recommendation for the best area for a new store selling
    healthy foods.

The **Introduction** section should state the research objective and the
relevant spatial criteria for locating the new store.

The **Data and Methods** section should state the data sets used in the
analysis, from where those data were acquired, and the GIS operations
employed. The maps illustrating the data sets used and a part of the
methodology should be cited in the text here (e.g. Figure 1, 2, etc.)

The **Results** section should state the results (i.e. a brief
description of the areas that meet the criteria, the square footage of
each, and your recommendation as to the best area for the store). The
map of the potential area(s) for the store should be cited in the text
here.

The **Discussion** section should state an interpretation of the results
(i.e. where in Philadelphia is the recommended area, what is
advantageous about this particular location), limitations of the
analysis, and how the analysis could be improved or expanded.

The **Tables** **and Figures** section should contain the maps, each on
a separate page with a caption. The maps should be cited in the text.

## Getting Started

Make sure all layers and your map are in State Plane Pennsylvania
South CRS.

There are many ways to complete this analysis (but only one correct
answer). All of the tools that you need to complete the lab are
described in the lab tutorial. Here is one strategy.

**Start by creating an inclusive layer of the areas where the new store
MUST be located.**

1.  Buffer the regional rail stations and the subway stations (be sure
    to use the 'Dissolve all...' option in the buffer operation).
2.  Union the resulting regional rail station buffer and the subways
    station buffer.
3.  Intersect the resulting regional rail station/subway station buffer
    union layer with the empowerment zones. Name this layer `inclusive`
    because it describes the area where the new store MUST be located.

**Then create an exclusive layer of the areas where the new store CAN’T
be located.**

4.  Buffer the farmers markets and the corner stores.
5.  Union the resulting farmers markets buffer and the corner stores
    buffer. Name this layer `exclusive` because it describes the area
    where the new store CAN’T be located.

**Then create a layer of the candidate areas where the new store MUST be
located, but without the areas where it CAN’T be.**

6.  Erase the exclusive layer from the inclusive layer. Name this layer
    `candidates` because it represents the candidates for the final
    area you recommend.

**Clean up the candidates layer.**

7.  Use a multipart to singlepart operation on the candidates layer so
    that each polygon is represented by one record in the attribute
    table (clear your selections first).

**Calculate the area of each candidate polygon.**

8.  Add a field to the resulting layer's attribute table to hold the
    area value of each polygon. Calculate the area of each polygon in
    square feet.
