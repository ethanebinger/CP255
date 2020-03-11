# Assignment 2: Initial Analysis
## Spatio-Temporal Mapping of Bus Bunching with Real-Time GTFS

#### Achievements

My project explores how to use real-time GTFS data to identify bus bunching. The primary data set for this project is the GTFS data published by AC Transit. There are two types of GTFS data, static and real-time, and both are be used for this project. I received an API developer key from AC Transit and have built the following notebooks to support this assignment:

- [ACTransit_API_Routes_Stops.ipynb](https://github.com/ethanebinger/CP255/blob/master/code/ACTransit_API_Routes_Stops.ipynb)
    + This notebook calls the AC Transit API and collects static GTFS data of routes, stops, and trips. It then parses this data into pandas dataframes and saves them .csv files, which will be used later as lookup tables for routes and as identifiers for bus stop locations.
    + This notebook also displays some geospatial visualizations of the route and stop pattern data.

- [ACTransit_API_VehicleLocations.ipynb](https://github.com/ethanebinger/CP255/blob/master/code/ACTransit_API_VehicleLocations.ipynb)
    + This notebook calls the API Transit API and collects real-time GTFS data of vehicle locations. Every active vehicle at the time of the call is collected and saved into a .CSV file with its route, trip, location, and speed.
    + The API can be called manually or run on a schedule. Currently the code is written to call the API (and save the data) every minute.

- [ACTransit_API_BusBunching.ipynb](https://github.com/ethanebinger/CP255/blob/master/code/ACTransit_API_BusBunching.ipynb)
    + This notebook is still in progress. The aim is to merge together the static, descriptive data with the real-time vehicle locations. Then, spatial calculations will be applied to find the distance between buses.

My biggest achievement for this project is getting the AC Transit API to work. I am familiar with JSONs but the real-time GTFS was structured in Protocol Buffer format. It was confusing at first how to parse this data, but I found a [helpful resource](https://github.com/MobilityData/gtfs-realtime-bindings/tree/master/python) and now have iterable code that returns organized CSVs.

Another achievement is plotting the bus stops using fiona and matplotlib. Visualizing the data on a map is going to a major component of this project so I am glad to have some preliminary maps created already.

#### Challenges

I have two main hurdles to overcome. The first is in data collection. I was able to create a function that calls and collects real-time vehicle locations from the AC Transit API. However, this script runs on my local machine and is limited by its availability. I want to gather data from a whole day for this project so I can explore where buses are bunching through time. To do this I could keep my laptop open all day and have the script running in the background or I could use a remote server to run this script for me. I created an account with the Open Computing Facility, but I am unfamiliar with Unix and the shell environment so have not gotten anything set up. I am working through the [Software Carpentry Unix Shell Tutorial](http://swcarpentry.github.io/shell-novice/) and getting help from a co-worker so this will work but requires more time. For now, I have data from an afternoon when I ran the script uninterrupted on my machine. This will work for testing my code, but I would prefer a larger dataset to work with for my final results.

The second hurdle is identifying bus bunching. I have started playing around with geopandas and geopy but need to critically approach this step because these geospatial functions sometime trip me up. I need to join the real-time data with the static, descriptive data to a) identify where the nearest stop is and b) identify the nearest bus on the same route. Getting the nearest point is simple, but I do not want to join the 6 bus to the 80 bus or to a stop for a different route. I also want to avoid unnecessary loops that will slow down my processing and analysis. 

#### Next Steps

My next step is to get a .py version of *ACTransit_API_VehicleLocations.ipynb* running on the Open Computing Facility servers and collect more data. However, the bulk of the work will be creating an algorithm to identify bus bunching. As noted above I might get held up here because of the complexity involved in identifying bus locations and joining nearby buses together. I plan to explore the methods available with [geopy](https://geopy.readthedocs.io/en/stable/) to complete this task.

Following the creation of a bus bunching identifying tool I will generate heat maps that highlight the times and locations where buses group together. I believe that this step will not be as complicated as the one before because I know how to create maps of my data. If time permits, I want to create an interactive visualization online where users could query a route/time of day and see the data they choose instead of static maps.

#### References

“AC Transit API Documentation.” https://api.actransit.org/transit/Help. Accessed Feb. 4, 2020.

“GTFS Static Overview.” https://developers.google.com/transit/gtfs. Accessed Feb. 4, 2020.

“GTFS Realtime Overview.” https://developers.google.com/transit/gtfs-realtime. Accessed Feb. 4, 2020.

Lehe, Lewis, and Dennys Hess. “Bus Bunching Explained Visually.” http://setosa.io/bus/. Accessed Feb. 4, 2020.