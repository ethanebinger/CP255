# Assignment 3: Further Analysis
## Using Open-Source Bike Share Trip Data to Inform Social Distancing Street Closures

#### Background

Due to the ongoing circumstances regarding COVID-19 my previous project was no longer feasible. I was working on using real-time GTFS data to identify bus bunching within the AC Transit system. However, because of the shelter-in-place mandate and social distancing guidelines imposed by local jurisdictions and the state of California, AC Transit bus service has been impacted. Many routes are no longer in operation and many are running without scheduled stops or with alternate headways (“AC Transit Service Bulletins”, 2020). Additionally, because non-essential workers are staying at home to reduce the spread of COVID-19 there is low ridership and no traffic on the roads. The main causes of bus bunching are not present or cannot be properly measured.

I am still interested in transportation but changed my project topic to be more related to the ongoing pandemic. More specifically I am focusing on social distancing and active travel. For those that go outside it is recommended that they follow social distancing guidelines that suggest everyone remain 6 feet away from each other to avoid further spread of the novel coronavirus. However, this can often be difficult in many built environments where the space allocated to vehicles dwarfs the width of the sidewalk.

To encourage social distancing many cities around the world (Laker, 2020) have converted road space from automotive traffic to pedestrians and bicyclists. Although many of these are just temporary closures, the increased space for non-vehicular travel is allowing residents to get exercise, fresh air, and access still-open essential buildings in a more sustainable way.
Oakland has been receptive to these needs and recently identified 74 miles of streets (Rudick, April 2020) to be cordoned off for non-automotive travel. Not all cities in the Bay Area are following suit though. Despite the decrease in vehicular travel and the need for increased sidewalk space San Francisco has not closed streets (Rudick, March 2020) and a letter and petition to repurpose Berkeley’s bicycle boulevards has yet to gain traction (Walk Bike Berkeley, 2020).

This project builds on the need for additional street closures to create safe streets in the Bay Area during the pandemic. I use open-source bike share data from BayWheels to identify the most frequented biking paths by system users. Together with maps of existing and planned bicycle facilities these routes can serve as recommendations for jurisdictions in the Bay Area that are looking to close streets to vehicular traffic to promote social distancing.

The following datasets are used in this project:
•   [System data from Bay Wheels](https://www.lyft.com/bikes/bay-wheels/system-data) from February 2020 (the latest dataset available).
•   Metropolitan Transportation Committee’s [regional bicycle facilities](http://opendata.mtc.ca.gov/datasets/regional-bike-facilities?geometry=-122.400%2C37.800%2C-122.144%2C37.847).
•   Cartographic boundary files [from the Census](https://www.census.gov/geographies/mapping-files/time-series/geo/carto-boundary-file.html).


#### Achievements

The following notebooks were created for this project:

* [0_Process_BayWheels_Trips.ipynb](https://github.com/ethanebinger/CP255/blob/master/code/0_Process_BayWheels_Trips.ipynb)
    * This notebook cleans up the raw data that comes from Bay Wheels and puts it into a more usable format for this project. Possibly erroneous (trips that are many hours) and illogical (trips between Oakland and San Jose) trips are removed from the dataset, which is then grouped by unique origin-destination pair for easier data manipulation because it shortens the dataset from hundreds of thousands to tens of thousands of records.
    * Additionally, this notebook creates some new data products from the raw data. This includes a geospatial dataframe with unique stations, a geospatial dataframe with bounding boxes for each region where Bay Wheels operates, and a geospatial dataframe of routes between each origin-destination pair. The routes are identified using the OSMnx package (Boeing, 2017), which generates a street network with the OpenStreetMaps API and queries shortest path routes using NetworkX.
* [1_Explore_BayWheels_Trips.ipynb](https://github.com/ethanebinger/CP255/blob/master/code/1_Explore_BayWheels_Trips.ipynb)
    * This notebook does less data cleaning and more exploratory analysis to visualize the Bay Wheels origin-destination trips. A heatmap of trips by day of the week and time of day is created using Seaborn. This visualization highlights distinct weekday/weekend trip patterns and indicates that when exploring trip routes it may make sense to split up weekday and weekend travel because trip patterns may differ with trip purpose.
    * The bicycle facilities in the regions serviced by Bay Wheels are also explored to see where the most lane-miles are (San Francisco) and what type of bicycle facility is most common (unprotected bike lanes). 
* [2_Densify_BayWheels_Trips.ipynb](https://github.com/ethanebinger/CP255/blob/master/code/2_Densify_BayWheels_Trips.ipynb)
    * This notebook reads in the geospatial data created in the first notebook to identify the streets Bay Wheels riders frequent the most. This notebook uses complex geospatial manipulation methods (`gpd.geometry.unary_union`, `gpd.tools.jsoin`) to create a geodataframe with only unique trip segments and then intersect that with every Bay Wheels route to output counts of trips over each segment.
    * The end of this notebook begins to display this data but it is still in progress. I am looking to improve the section where the routes are displayed on a map using folium and also possibly build out the section that exports the routes to HTML files so they can be displayed on an external webpage (currently just a demo).

One big achievement is converting the coordinates for each bike share station into nodes to be read into OSMnx to create routes. The OpenStreetMaps network that is built using OSMnx does routing based on trips from nodes to nodes, where nodes are street intersections. The Bay Wheels dataset was not developed with OpenStreetMaps in mind, so all the stations are specified as latitude, longitude coordinates. I had to use the `ox.get_nearest_node` method to make this conversion. There were many trials and errors along the way due to mismatching projection systems and flipped specifications of coordinates (lat,lon vs lon,lat) so am very happy to have figured this out. It was also a challenge to convert the route created with the `nx.shortest_path` method back to coordinates because having a list of OpenStreetMaps nodes is not useful for this project.

Another achievement is identifying the number of trips per street segments. I used the geopandas and shapely packages to create unique route segments and then spatially joined that dataset to the routes. I needed very specific geospatial methods to accomplish this step. There may be a less computationally expensive way to accomplish this process but right now everything appears to be working so I am not reading to mess with the code just to improve runtime. 

#### Challenges and Next Steps

My next step is to is identifying the names of the streets (i.e. Hillegrass Ave from Dwight Way to Alcatraz Ave) that Bay Wheels riders use the most frequently for their trips. They can serve as recommendations for street closures to the jurisdictions with Bay Wheels service. I have counts of the trips on street segments but not the street names. This could be done manually by overlaying the route segments on the OpenStreetMaps baselayer, but I have not figured out how to use the `counts` column in the geodataframe of trip segments to color the lines based on the frequency with which they are used. It may be possible to intersect my street segments with those pulled from OSMnx, thereby appending descriptive data to each bicycle route and making this process more automated, but I have not figured out how to do that either.

I would also like to overlay my recommended street closures with the ones already closed in Oakland to see if there is any overlap. However, I could not find a geospatial dataset with that information and creating this dataset manually may be too time consuming for this project.

Another challenge I am having is not being able to process all the data in `2_Densify_BayWheels_Trips.ipynb`. The code should loop through all the regions serviced by Bay Wheels but has been edited to only select the routes in the 'East Bay' region. I am not sure why, but the `geometry.unary_union` method kept crashing when it was run on routes in the 'San Francisco' region. I would like to make street closure recommendations for all three service regions, but for now only have routes in the 'East Bay' region.

Additionally, I need to clean up the maps I have made so I can begin to tell a story for my presentation. Many of the visualizations could benefit from improved legends and stylizations as well. If time permits, I want to create an interactive web page with these maps so visitors can explore their neighborhood and see on a map what streets have been recommended for closure to vehicular traffic.


#### References

“AC Transit Service Bulletins.” http://www.actransit.org/servicebulletins/. Accessed Apr. 13, 2020.

Boeing, G. "OSMnx: New Methods for Acquiring, Constructing, Analyzing, and Visualizing Complex Street Networks." Computers, Environment and Urban Systems 65 (2017), pp. 126-139.

Laker, Laura. “World cities turn their streets over to walkers and cyclists.” The Guardian, Apr. 11, 2020. https://www.theguardian.com/world/2020/apr/11/world-cities-turn-their-streets-over-to-walkers-and-cyclists. 

Rudick, Roger. “People Desperate for Space, but so far Bay Area Won’t Open Streets.” Streetsblog, Mar. 30, 2020. https://sf.streetsblog.org/2020/03/30/people-desperate-for-space-but-so-far-bay-area-officials-wont-open-streets/. 

Rudick, Roger. “Oakland to Open 74 Miles of Streets for Walkers and Cyclists.” Streetsblog, Apr. 10, 2020. https://sf.streetsblog.org/2020/04/10/oakland-to-open-74-miles-of-streets-for-safe-biking-and-recreation/. 

“System Data.” Lyft. Accessed Apr 10., 2020. https://www.lyft.com/bikes/bay-wheels/system-data. 

Walk Bike Berkeley. “Letter requesting closure of streets for social distancing & COVID-19.”Accessed Apr. 13, 2020. https://docs.google.com/document/u/1/d/e/2PACX-1vQQkbtEb3s0bfUMcLUMeu1Oa4OWnmxK9hsRLIRfw3PznZYCsc7kpUZ9pjpHS1La9PWUswL7q9LGVG7q/pub. 
