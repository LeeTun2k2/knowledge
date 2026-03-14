[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How Lyft Support Rides to 21 Million Users

### #43: Break Into Lyft Architecture (11 minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Apr 12, 2024

∙ Paid

100

2

14

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines Lyft's architecture. If you want to learn more, scroll to the bottom and find the references._

  * _[Share this post](https://newsletter.systemdesign.one/p/lyft-engineering/?action=share) & I'll send you some rewards for the referrals._

_Note: This post is based on my research and might differ from real-world implementation._

Oliver is backpacking across Canada.

He has to reach the railway station in 25 minutes.

But couldn't find public transport from his place to the railway station.

[![Lyft Engineering](https://substackcdn.com/image/fetch/$s_!YcZN!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe4ad0d8e-6886-46af-835b-d7ff6d5cf054_800x500.gif)](https://substackcdn.com/image/fetch/$s_!YcZN!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe4ad0d8e-6886-46af-835b-d7ff6d5cf054_800x500.gif)

So he uses a ride-sharing app called Lyft.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!4Qs3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)](https://substackcdn.com/image/fetch/$s_!4Qs3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)

### [Interview Master (Featured)](https://www.interviewmaster.io/subscribe?utm_source=substack&utm_medium=newsletter&utm_campaign=neo)

Interview Master is a free 5-day crash course to crack technical interviews. After finishing the course, you also get a weekly coding challenge with its solution. You can also win a Google resume template and cheat sheets for DSA and System Design.

[Master Interviews Now!](https://www.interviewmaster.io/subscribe?utm_source=substack&utm_medium=newsletter&utm_campaign=neo)

* * *

## Lyft Engineering

Here’s a simplified version of Lyft’s architecture:

### 1\. Map Data

Oliver adds his current location to the app.

They use OpenStreetMap (**[OSM](https://www.openstreetmap.org/)**) for internal map data. It gives a free and editable map of the world.

And use Google’s [S2](https://github.com/google/s2geometry) library on top of OSM to efficiently index and query map data. S2 is based on the Geohash algorithm. It divides the map into grids called cells and gives each cell a unique ID.

Also they store road metadata and road segment sequences in each cell. It helps to understand turn restrictions on the road.

[![Representing the World With S2 Cells](https://substackcdn.com/image/fetch/$s_!QJD5!,w_1456,c_limit,f_auto,q_auto:good,fl_lossy/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb9da6531-eea4-40aa-906d-dea6a11b2e1e_800x500.gif)](https://substackcdn.com/image/fetch/$s_!QJD5!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb9da6531-eea4-40aa-906d-dea6a11b2e1e_800x500.gif)Representing the World Map With S2 Cells

They let the client dynamically build a road network graph for low memory usage. This means the client downloads the S2 cells based on the user's location. Besides the client buffers nearby cells to have enough map data for navigation.

[![Downloading Map Data From DynamoDB](https://substackcdn.com/image/fetch/$s_!MZof!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F16e185b6-714d-406b-9ccb-4b65a8ec9e72_1755x415.png)](https://substackcdn.com/image/fetch/$s_!MZof!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F16e185b6-714d-406b-9ccb-4b65a8ec9e72_1755x415.png)Downloading Map Data From DynamoDB

They store map data in serialized format on DynamoDB. And query the S2 geospatial index to find the relevant DynamoDB partition. The map data then gets downloaded from DynamoDB. The map data gets deserialized and filtered before returning it to the client.

[![Caching Map Data in CDN for Low Latency](https://substackcdn.com/image/fetch/$s_!wPzW!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F14347788-d3ad-4033-b5ae-8366fe90fd34_1795x322.png)](https://substackcdn.com/image/fetch/$s_!wPzW!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F14347788-d3ad-4033-b5ae-8366fe90fd34_1795x322.png)Caching Map Data in CDN for Low Latency

Besides they run a CDN to cache the map data. It reduces latency and improves the reliability of the service.

[![SQLite Caching Layer on the Client to Reduce Duplicate Downloads of Map Data](https://substackcdn.com/image/fetch/$s_!D7OH!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F87d17a24-e37b-4372-a3fb-1015bcada8c5_1213x623.png)](https://substackcdn.com/image/fetch/$s_!D7OH!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F87d17a24-e37b-4372-a3fb-1015bcada8c5_1213x623.png)SQLite Caching Layer on the Client to Reduce Duplicate Download of Map Data

Yet there’s a chance of duplicate download of map data by drivers. And it will increase network data usage. So they set up an [SQLite](https://www.sqlite.org/index.html) database caching layer on the client. And update the in-memory cache only if the map data on the server changes.

[![Finding the Missing Roads With GPS Signals](https://substackcdn.com/image/fetch/$s_!51j5!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc7c5d610-1dd1-4fbf-a363-6bbcc57b9913_1141x788.png)](https://substackcdn.com/image/fetch/$s_!51j5!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc7c5d610-1dd1-4fbf-a363-6bbcc57b9913_1141x788.png)Finding Missing Roads With GPS Signals

Also there's a risk of missing data and errors in OSM. For example, new roads may be absent on maps and road directions could be wrongly labeled. So they use the [Kalman filter](https://en.wikipedia.org/wiki/Kalman_filter) to find missing roads. Weeks of driver location data is used to create a plot of missing roads and then update the map.

Think of the **Kalman filter** as a person who makes a correct guess about something's location. While the new and old information is taken into consideration for guessing.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!4Qs3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)](https://substackcdn.com/image/fetch/$s_!4Qs3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)

### 2\. Finding Nearby Drivers

Oliver searches for nearby drivers.

They store driver locations in a Redis cluster for scalability and low latency. A **Redis cluster** contains many Redis instances. This means driver locations are spread across many Redis instances. Thus preventing global write lock and contention issues when many rides get ordered at the same time.

But sharding Redis based on region causes a hot shard problem because of more drivers in big cities. So they use Google’s S2 library and divide the map into grids. And S2 is hierarchical. That means the cell size varies from square centimeters to square kilometers. They chose Geohash level 5 by default to find nearby drivers. It represents a square kilometer, so only a few cars will fit inside a single shard. And the hot shard problem wouldn't occur.

[![Time Bucket Tracking Active Drivers](https://substackcdn.com/image/fetch/$s_!sjlS!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4e360952-8258-4717-b6c1-f220fc646471_1170x673.png)](https://substackcdn.com/image/fetch/$s_!sjlS!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4e360952-8258-4717-b6c1-f220fc646471_1170x673.png)Time Bucket Tracking Active Drivers

Redis cluster may contain drivers who stopped driving for the rest of the day. But they want only active drivers. This means drivers who are still driving during the day.

A simple approach is to create in-memory time buckets periodically. Then store the list of active drivers in it. And remove old buckets every 30 seconds. So only active drivers will stay in the latest bucket. Yet it results in constant allocation and freeing up of memory.

[![Sorting Drivers by the Latest Timestamp Using a Sorted Set](https://substackcdn.com/image/fetch/$s_!qeTB!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc84c861f-760c-4eb1-ae06-0efcf81781e1_899x573.png)](https://substackcdn.com/image/fetch/$s_!qeTB!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc84c861f-760c-4eb1-ae06-0efcf81781e1_899x573.png)Sorting Drivers by the Latest Timestamp Using Sorted Set

So they use a Redis sorted set in each Geohash to [find nearby drivers](https://newsletter.systemdesign.one/p/how-does-uber-find-nearby-drivers). And store the last timestamp reported by the drivers in a sorted order. While inactive driver data is expired using the [ZREMRANGEBYSCORE](https://redis.io/commands/zremrangebyscore/) command. That means only data of those drivers who haven’t reported in the last 30 seconds will expire. Simply put, they overwrite memory instead of reallocating it. Imagine the **sorted set** as key-value pairs sorted by score.

Besides they store the driver location in a hash data structure. It's also queried to ensure that a driver doesn’t show up in 2 different Geohashes while driving through.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!4Qs3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)](https://substackcdn.com/image/fetch/$s_!4Qs3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)

### 3\. Matching Riders With a Driver

Oliver is shown a list of potential rides. 

Yet they were expensive for his budget.

So he tried Lyft's feature called Line. It allows riders to share a single car at a low price.

But they must match only the riders with overlapping routes for efficiency.

A simple approach is to create a matching system based on time buckets. This means putting all the rider requests in a matching pool for a minute and then matching them. It increases the probability of a better match.

[![There Are 6 Stops Between Pickup and Dropoff of Rider A](https://substackcdn.com/image/fetch/$s_!Zesf!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Faf5de6e0-2b13-48a5-8f22-33933a8c5c90_2696x1574.png)](https://substackcdn.com/image/fetch/$s_!Zesf!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Faf5de6e0-2b13-48a5-8f22-33933a8c5c90_2696x1574.png)6 Stops Between Pickup and Dropoff of Rider A

But the number of permutations of routes to calculate increases with number of riders sharing a single car. This means they must consider all riders in the system and predict future demand to find the best match. Put another way, it becomes a weighted maximal matching problem. For example, route ABCDBCDA would be a bad experience for rider A because it takes more time with many stops.

[![Route Swapping Rider a Into an Efficient Route Before Pickup](https://substackcdn.com/image/fetch/$s_!6OMc!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe4af84f0-037d-4294-870d-ddb2ac789a2f_996x559.png)](https://substackcdn.com/image/fetch/$s_!6OMc!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe4af84f0-037d-4294-870d-ddb2ac789a2f_996x559.png)Route Swapping Rider A Into an Efficient Route Before Pickup

So they do route swapping. Think of **route swapping** as moving a rider from one route to another route before pickup. That means they re-match a rider if an efficient route has been found. Also it increases the matching window as they could look for a better route before pickup.

The above figure shows that rider A got swapped from route 1 to route 2 due to a lower trip time. Besides they accumulate the matched riders for around 30 seconds before checking for better routes. They do it to make the matching algorithm more efficient.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!4Qs3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)](https://substackcdn.com/image/fetch/$s_!4Qs3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)

### 4\. Finding the Rider’s Location

Oliver selects a ride.

But wants a short walking distance for pickup.

So he tries to set an accurate pickup spot on the app.

[![Multipath Effect](https://substackcdn.com/image/fetch/$s_!L4Ew!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6261a0d5-6962-44e6-970d-78f9e9ba3a7a_800x500.gif)](https://substackcdn.com/image/fetch/$s_!L4Ew!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6261a0d5-6962-44e6-970d-78f9e9ba3a7a_800x500.gif)Multipath Effect

Yet Global Positioning System (**[GPS](https://en.wikipedia.org/wiki/Global_Positioning_System)**) isn’t reliable near tall buildings. Because the GPS signal might get reflected by a building and travel longer than it should. It’s called the **multipath effect**. This means triangle inequality and an incorrect position.

[![Wi-Fi Point Estimate and Probability Distribution](https://substackcdn.com/image/fetch/$s_!F4Ru!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd63e6c6f-b8b3-40ea-950a-a99d7d46b839_1552x882.png)](https://substackcdn.com/image/fetch/$s_!F4Ru!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd63e6c6f-b8b3-40ea-950a-a99d7d46b839_1552x882.png)Wi-Fi Point Estimate and Probability Distribution

So they use a Wi-Fi map to solve this problem. For example, knowing that a rider was at a specific shop gives more context to their position. **Wi-Fi** is a group of radio technologies used for wireless local area networking (**WLAN**).

## This post is for paid subscribers

[Subscribe](https://newsletter.systemdesign.one/subscribe?simple=true&next=https%3A%2F%2Fnewsletter.systemdesign.one%2Fp%2Flyft-engineering&utm_source=paywall&utm_medium=web&utm_content=143071410)

[Already a paid subscriber? **Sign in**](https://substack.com/sign-in?redirect=%2Fp%2Flyft-engineering&for_pub=systemdesignone&change_user=false)

PreviousNext

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture