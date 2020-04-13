# CP255
Repo for classwork and projects from CP255: Urban Informatics and Visualization (Spring 2020)

## Final Project: Using Open-Source Bike Share Trip Data to Inform Social Distancing Street Closures

Due to the ongoing circumstances regarding COVID-19 Bay Area jurisdictions and state of California have mandated residents shelter-in-place. For those that go outside it is recommended that people follow social distancing guidelines that suggest people remain 6 feet away from each other to avoid further spread of the novel coronavirus. However, this can often be difficult in many built environments where the space allocated to vehicles dwarfs the width of the sidewalk.

To encourage social distancing [many cities around the world](https://www.theguardian.com/world/2020/apr/11/world-cities-turn-their-streets-over-to-walkers-and-cyclists) have converted road space from automotive traffic to pedestrians and bicyclists. Although many of these are just temporary closures, the increased space for non-vehicular travel is allowing residents to get exercise, fresh air, and access still-open essential buildings in a more sustainable way.

Oakland has been receptive to these needs and recently [identified 74 miles of streets](https://sf.streetsblog.org/2020/04/10/oakland-to-open-74-miles-of-streets-for-safe-biking-and-recreation/) to be cordoned off for non-automotive travel. Not all cities in the Bay Area are following suit though. Despite the decrease in vehicular travel and the need for increased sidewalk space [San Francisco has not closed streets](https://sf.streetsblog.org/2020/03/30/people-desperate-for-space-but-so-far-bay-area-officials-wont-open-streets/) and a [letter](https://docs.google.com/document/u/1/d/e/2PACX-1vQQkbtEb3s0bfUMcLUMeu1Oa4OWnmxK9hsRLIRfw3PznZYCsc7kpUZ9pjpHS1La9PWUswL7q9LGVG7q/pub) and [petition](https://docs.google.com/forms/d/e/1FAIpQLSdIXHpAjaZVu9hTSbRgx7z5YAtl3ZiNsJTw3WA3-awvywsd9w/viewform) to repurpose Berkeley’s bicycle boulevards has yet to gain traction.

This project builds on the need for additional street closures to create safe streets in the Bay Area during the pandemic. I use open-source bike share data from BayWheels to identify the most frequented biking paths by system users. Together with maps of existing and planned bicycle facilities these routes can serve as recommendations for jurisdictions in the Bay Area that are looking to close streets to vehicular traffic to promote social distancing.

**Notebooks:**
* `0_Process_BayWheels_Trips`
* `1_Explore_BayWheels_Trips`
* `2_Densify_BayWheels_Trips`

**Datasets used:**
* [System data](https://www.lyft.com/bikes/bay-wheels/system-data) from Bay Wheels. We process only trips from February 2020 because it is the latest dataset available (most current station layout and trip flows).
* [Regional bicycle facilities](http://opendata.mtc.ca.gov/datasets/regional-bike-facilities?geometry=-122.400%2C37.800%2C-122.144%2C37.847) in the Bay Area. From the Metropolitan Transportation Commission.
* Census boundaries for California, including polygons for the state outline, census-designated places, and tracts.


## Project Idea: Spatio-Temporal Mapping of Bus Bunching with Real-Time GTFS

This project planned to focus on how to use real-time GTFS data to identify bus bunching. Bus bunching is a phenomenon that occurs when a bus gets delayed. Once the bus is behind schedule there are more people waiting at the next stop, which further delays the bus. This continues down the route, and if there is enough delay and short enough headway the bus behind catches up. Instead of being evenly spaced the buses are now platooning, resulting in unreliable service.

However, due to the ongoing circumstances regarding COVID-19 this project was no longer feasible. I was using real-time GTFS data from AC Transit, but because of the shelter-in-place mandate and social distancing guidelines imposed by local jurisdictions and the state of California, AC Transit bus service has been impacted. Many routes are no longer operating and many are running without scheduled stops or with alternate headways (http://www.actransit.org/servicebulletins/). Additionally, because non-essential workers are staying at home to reduce the spread of COVID-19 there is low ridership and no traffic on the roads. The main causes of bus bunching are not present or cannot be properly measured.

**Notebooks**:
* `ACTransit_API_Routes_Stops.ipynb`
* `ACTransit_API_VehicleLocations.ipynb`
* `ACTransit_API_BusBunching.ipynb`
