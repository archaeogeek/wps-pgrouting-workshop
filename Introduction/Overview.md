
## Introduction to WPS

WPS stands for Web Processing Service(s), and at version 1.0 is an official [OGC standard](http://www.opengeospatial.org/standards/wps). The standard sets rules for how inputs and outputs (requests and responses) for geospatial processing services, for example a buffer, can be made. The standard also defines how a client such as a desktop GIS or web application can request the execution of a process, and how the output from the process is handled.  

While they may differ in how they look and how they are installed, WPS Servers are basically all the same. They are an application that runs on a web server (such as apache) and accepts http GET requests in the form of Key-Value-Pairs or http POST requests in xml format. The WPS server will respond with either LiteralData (such as a character string), ComplexData (such as a URL to a file that can be downloaded), or BoundingBoxData.

These requests and the response from the server must conform to the OGC WPS 1.0 standard. There are only three types of request under the standard:
* GetCapabilities: Asks the server for information about itself and what processes it has available;
* DescribeProcess: Asks the server for information about the inputs and outputs for one or all of the processes listed in the GetCapabilities response;
* ExecuteProcess: Asks the server to execute a named process and provides the expected inputs.

