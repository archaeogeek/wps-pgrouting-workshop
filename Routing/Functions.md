# PgRouting Functions Overview

## Understanding a routing network

The following terminology is often used within pgrouting documentation:

 * edge_table: the table containing the network
 * id: primary key column for the edge_table
 * source: the start node for calculations
 * target: the target node for calculations
 * cost: a user-calculated value for the 'cost' of travelling in the default direction along the line. Usually (eg the default with osm2po) this is a length/speed calculation
 * reverse_cost: a user-calculated value for the 'cost' of travelling in the reverse direction along a line. If one-way, it will generally be an arbitrarily high value, if two-way the default will be the same as the cost.

## Routing Functions

These are well-documented at [docs.pgrouting.org](http://docs.pgrouting.org/2.0/en/doc/index.html). The basic function format is:

    select pgr_algorithm(SQL for edges, start, end, additional options)

This returns records of type **pgr\_costresult[]** or **pgr_geomresult[]**. These are arrays (or graphs) of nodes and edges (running from source to target), with a cost or geometry accordingly. 

To use these in a map, they must be joined to the original edge and node data.

### Shortest Path

Several variations on shortest path functions are available within pgRouting. For example pgr_dijkstra:

    pgr_dijkstra(sql, source, target, directed, has_rcost);

 * sql: a SQL query, eg SELECT id, source, target, cost [,reverse_cost] FROM edge_table. This MUST return the columns, id, source, cost, target, reverse_cost from the edge_table
 * source: integer identifier of the source node
 * target: integer identifier of the target node
 * cost: floating point value of the edge cost. A negative cost will prevent the edge from being inserted in the graph. 
 * reverse_cost: (optional) the cost for the reverse traversal of the edge. This is only used when directed = true and has_rcost = true 
 * directed: true if you will be taking direction of travel along an edge into account
* has_rcost: true if you wish to use the value of the reverse_cost column

Returns pgr_costresult[]

### Driving Distance

Note that the term "distance" is a misnomer, as it actually refers to a maximum cost of travelling from the source node. 

    pgr_drivingDistance(sql, source, distance, directed, has_rcost);

* has\_rcost: true if you wish to use the value of the reverse_cost column
* sql: a SQL query, eg SELECT id, source, target, cost [,reverse_cost] FROM edge_table. This MUST return the columns, id, source, cost, reverse_cost from the edge_table
* source: int4 identifier of the source vertex
* cost: float8 value, of the edge traversal cost. A negative cost will prevent the edge from being inserted in the graph.
* reverse_cost: float8 (optional) the cost for the reverse traversal of the edge. This is only used when directed = true and has_rcost = true
* directed: true if you will be taking direction of travel along an edge into account
* has_rcost: true if you wish to use the value of the reverse_cost column

Returns pgr_costresult[]

### pgr_costresult[]
This is an array, so each row returned in the query result is a set of values in brackets:

 * seq:    sequential ID indicating the path order
 * id1:    the node ID
 * id2:    the edge ID
 * cost:   cost, as defined by the function returning this result


