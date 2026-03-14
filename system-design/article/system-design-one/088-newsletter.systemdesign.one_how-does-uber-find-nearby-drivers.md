[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How Uber Finds Nearby Drivers at 1 Million Requests per Second

### #31: And How Proximity Service Works Explained Like You’re Twelve (5 minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Jan 04, 2024

∙ Paid

419

12

39

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines how Uber finds nearby drivers at 1 million requests per second. If you want to learn more, scroll to the bottom and find the references._

  * _[Share this post](https://newsletter.systemdesign.one/p/how-does-uber-find-nearby-drivers/?action=share) & I'll send you some rewards for the referrals._

March 2016 - Istanbul, Turkey.

Charles and his family are on a tourist visit.

Yet it was difficult for them to travel around because the nearest taxi stand was 3 km away from the hotel.

So they were sad.

[![How Does Uber Find Nearby Drivers](https://substackcdn.com/image/fetch/$s_!t9wf!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa71a7971-3694-4236-a8bf-60d9bb6781e2_800x500.png)](https://substackcdn.com/image/fetch/$s_!t9wf!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa71a7971-3694-4236-a8bf-60d9bb6781e2_800x500.png)

They hear about the ride-sharing app called Uber from the hotel receptionist.

They installed it immediately and were stunned by the easiness of finding a driver.

* * *

## How Does Uber Find Nearby Drivers?

Finding nearby drivers with accuracy and scalability is a hard problem. 

Here’s how Uber solves it:

### 1\. Location Indexing

It’s difficult to find nearby drivers only based on latitude and longitude. 

So they index locations to find nearby drivers efficiently.

[![Uber; Finding Nearby Drivers](https://substackcdn.com/image/fetch/$s_!W2z2!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F858c7112-34fd-4a69-ab34-dda938505513_800x500.png)](https://substackcdn.com/image/fetch/$s_!W2z2!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F858c7112-34fd-4a69-ab34-dda938505513_800x500.png)Finding Nearby Drivers

They use the _H3_ library for location indexing.

**H3** is a hexagonal-shaped hierarchical geospatial indexing system created at Uber.

H3 divides Earth’s surface into cells on a flat grid, giving each cell a unique identifier with a 64-bit integer.

An H3 index is a reference to a specific cell on the grid.

[![An H3 Index Referencing a Specific Cell](https://substackcdn.com/image/fetch/$s_!Xx97!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F99695104-77de-42d7-a909-351fc51c1f7e_800x500.png)](https://substackcdn.com/image/fetch/$s_!Xx97!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F99695104-77de-42d7-a909-351fc51c1f7e_800x500.png)An H3 Index Referencing a Specific Cell

They find nearby drivers by identifying the relevant cells covering the rider's area. And then listing the drivers in those cells sorted by the [estimated time of arrival](https://newsletter.systemdesign.one/p/uber-eta) (**ETA**).

H3 offers the benefits of a _hierarchical indexing system_ and __ a _hexagonal grid system_.

Here’s how H3 works:

#### i) Hierarchical Index

They need the index of the smallest possible area to find the nearby drivers accurately. Put another way, a high data resolution is necessary.

A small cell provides high data resolution. Yet storage costs increase if there are many smaller-sized cells.

While bigger-sized cells hide details in data.

[![Data Resolution Based on Grid Size](https://substackcdn.com/image/fetch/$s_!T_uF!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F63f06a50-7437-494d-b4bd-9c75a8ca0155_800x500.png)](https://substackcdn.com/image/fetch/$s_!T_uF!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F63f06a50-7437-494d-b4bd-9c75a8ca0155_800x500.png)Data Resolution Based on Grid Size

So they created a hierarchical grid. And defined the smaller hexagonal cells as subdivisions of bigger hexagonal cells.

A hierarchical grid allows data compression. Because the areas with lower details are represented with fewer cells. While areas with higher details are represented by many cells.

[![Fewer Cells Representing Large Areas Without Features in Hierarchical Grid](https://substackcdn.com/image/fetch/$s_!8KA6!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F740d683b-1fef-41f1-ba26-bda7f083614c_800x500.png)](https://substackcdn.com/image/fetch/$s_!8KA6!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F740d683b-1fef-41f1-ba26-bda7f083614c_800x500.png)Fewer Cells Representing Large Areas Without Details in Hierarchical Grid

Also changing data resolution became easier with a hierarchical grid. Because the identifier of child cells could be truncated to find ancestor cells.

They created a global grid on H3 with 122 base cells. Yet 12 out of 122 base cells are _pentagons_ because it’s not possible to tile a sphere (Earth) only with hexagons. H3 treats the pentagon as a hexagon without a part.

Besides hexagons don’t subdivide perfectly. So they subdivide a hexagon into 7 hexagons with a known amount of error. And the child hexagons get rotated to fit the shape of the parent hexagon.

[![H3 Subdividing Areas Into Smaller Hexagons](https://substackcdn.com/image/fetch/$s_!dkpQ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5b75d70a-373f-4962-989a-74cc124ddb60_800x500.png)](https://substackcdn.com/image/fetch/$s_!dkpQ!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5b75d70a-373f-4962-989a-74cc124ddb60_800x500.png)H3 Subdividing Areas Into Smaller Hexagons

The H3 library supports 16 data resolution types. And can index an area as small as 1 square meter.

They use the cell identifier as the _sharding key_ to partition the H3.

#### ii) Hexagonal Grid

They need distance between cells to find nearby drivers. 

It's possible to create a grid with triangle, square, or hexagon-shaped cells.

[![Distance Measurement in Grid](https://substackcdn.com/image/fetch/$s_!jOcx!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Feedcd446-9416-4935-9289-be24a22e2c41_800x500.png)](https://substackcdn.com/image/fetch/$s_!jOcx!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Feedcd446-9416-4935-9289-be24a22e2c41_800x500.png)Distance Measurement in Grid

But H3 uses a hexagon as the cell shape to make it easier to measure the distance between 2 cells. Because each neighboring cell is the same distance away from a cell's center. It _simplifies distance measurement_.

While triangles have 3 distanced neighbors and squares have 2 distanced neighbors. Because some neighbors share an edge and others share a vertex. 

So distance measurement between cells needs extra coefficients in triangles and squares.

#### iii) Hierarchical Algorithms

H3 supports the _indexing function_ to find a cell in the grid efficiently. It accepts latitude, longitude, and resolution to return the cell identifier.

[![Finding a Cell Identifier Using Indexing Function](https://substackcdn.com/image/fetch/$s_!yuHT!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3a5e877a-0300-4eb2-8aa6-ef56e8045695_800x500.gif)](https://substackcdn.com/image/fetch/$s_!yuHT!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3a5e877a-0300-4eb2-8aa6-ef56e8045695_800x500.gif)Finding a Cell Identifier Using Indexing Function

H3 finds the neighboring cells around a specific cell using the _kRing_ _function_.

Besides H3 does _constant-time bitwise operations_ to truncate the cell index. It makes switching between data resolutions easy.

* * *

### 2\. Location Storage

Each driver app sends its location to the server every few seconds. But the GPS signals can get noisy and sparse due to a poor network. 

The exact location is necessary to find nearby drivers. So they do _map matching_.

[![Map Matching](https://substackcdn.com/image/fetch/$s_!V029!,w_1456,c_limit,f_auto,q_auto:good,fl_lossy/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1122625f-15dc-4b1c-978e-17488e1e2ae6_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!V029!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1122625f-15dc-4b1c-978e-17488e1e2ae6_1200x630.gif)Map Matching

**Map matching** transforms raw GPS signals into actual road segments.

They use Apache Cassandra to store raw locations for long-term durability.

Apache Cassandra is a distributed database. Yet Cassandra is optimized for write operations.

So they add a Redis cache layer on top of it to shed the _read operations_.

[![Redis Buffering Data Points for Map Matching](https://substackcdn.com/image/fetch/$s_!ErPM!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb3c26801-188d-49f7-9152-be2879a9fd13_800x500.png)](https://substackcdn.com/image/fetch/$s_!ErPM!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb3c26801-188d-49f7-9152-be2879a9fd13_800x500.png)Redis Buffering Data Points for Map Matching

Redis stores the recent locations of each driver. Also it buffers enough data points to do map matching.

The map-matched data then gets stored in a separate schema on Cassandra.

* * *

### 3\. Scalability

## This post is for paid subscribers

[Subscribe](https://newsletter.systemdesign.one/subscribe?simple=true&next=https%3A%2F%2Fnewsletter.systemdesign.one%2Fp%2Fhow-does-uber-find-nearby-drivers&utm_source=paywall&utm_medium=web&utm_content=139367048)

[Already a paid subscriber? **Sign in**](https://substack.com/sign-in?redirect=%2Fp%2Fhow-does-uber-find-nearby-drivers&for_pub=systemdesignone&change_user=false)

PreviousNext

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture