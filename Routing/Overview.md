## Introduction to Routing

Routing is the process by which we construct paths from Point A to B, or calculate how far you can travel from Point A in a car in 30 minutes. To do these sorts of calculations, it's not enough to have some vector data representing roads or paths, the data needs to be carefully prepared and loaded into a PostgreSQL database with the pgRouting extension enabled on it.

### pgRouting

pgRouting is an extension for PostgreSQL, that extends PostGIS to add the Routing functions. For PostGIS 2.0 onwards it can easily be added to any spatial database using the syntax:

        create extension pgrouting;

For older versions it is slightly more complicated, but documentation can be found at [pgrouting.org](http://pgrouting.org/).

Vector line data such as roads must be converted into a network or topology before it can be used for routing. This is the process by which the different **segments**, or sections of a road between junctions are converted into **edges**, and the junctions are converted into **nodes**. This terminology will be covered in more detail later.

### Loading your data

There are a number of packages available to take vector data such as OpenStreetMap, load it into PostgreSQL, and create the necessary network. These include:

 * [osm2pgsql](https://github.com/openstreetmap/osm2pgsql) Note that this tool simply loads the data into postgreSQL, you will need to use the pgRouting pgr_createTopology function to create the network yourself;
 * [osm2pgrouting](https://github.com/pgRouting/osm2pgrouting);
 * [osm2po](http://osm2po.de/).
There are pros and cons to each approach, but for large volumes or data and ease of installation and use, it's hard to beat osm2po.

[Ordnance Survey MasterMap ITN data](http://www.ordnancesurvey.co.uk/business-and-government/products/itn-layer.html) is a ready-made (non-free) network, supplied in gml format. This can be loaded into PostgreSQL using a tool such as [Loader](https://github.com/AstunTechnology/Loader) and then used directly.








