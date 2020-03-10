# Assignment 2: Initial Analysis
## Spatio-Temporal Mapping of Bus Bunching with Real-Time GTFS

#### Achievements

My project explores how to use real-time GTFS data to identify bus bunching. The primary data set for this project is the GTFS data published by AC Transit. There are two types of GTFS data, static and real-time, and both are be used for this project. I received an API developer key from AC Transit and have built the following notebooks to support this assignment:

- `ACTransit_API_Routes_Stops.ipynb`
    + This notebook calls the AC Transit API and collects static GTFS data of routes and stops. It then parses this data into pandas dataframes and saves them .CSV files, which will be used later as lookup tables for routes and as identifiers for bus stop locations.

- `ACTransit_API_VehicleLocations.ipynb`
    + This notebook calls the API Transit API and collects real-time GTFS data of vehicle locations. Every active vehicle at the time of the call is collected and saved into a .CSV file with its route, trip, location, and speed.
    + The API can be called manually or run on a schedule. Currently the code is written to call the API (and save the data) every minute.

- `ACTransit_API_Visualization.ipynb`
    + Exploring different geospatial visualizations of the collected data.

#### Challenges



#### Next Steps




#### References

“AC Transit API Documentation.” https://api.actransit.org/transit/Help. Accessed Feb. 4, 2020.

“GTFS Static Overview.” https://developers.google.com/transit/gtfs. Accessed Feb. 4, 2020.

“GTFS Realtime Overview.” https://developers.google.com/transit/gtfs-realtime. Accessed Feb. 4, 2020.

Lehe, Lewis, and Dennys Hess. “Bus Bunching Explained Visually.” http://setosa.io/bus/. Accessed Feb. 4, 2020.