# Overview

While they may differ in how they look and how they are installed, WPS Servers are basically all the same. They are an application that runs on a web server (such as apache) and accepts http GET requests in the form of Key-Value-Pairs or http POST requests in xml format. The WPS server will respond with either LiteralData (such as a character string), ComplexData (such as a URL to a file that can be downloaded), or BoundingBoxData.

These requests and the response from the server must conform to the OGC WPS 1.0 standard. There are only three types of request under the standard:
    * GetCapabilities: Asks the server for information about itself and what processes it has available;
    * DescribeProcess: Asks the server for information about the inputs and outputs for one or all of the processes listed in the GetCapabilities response;
    * ExecuteProcess: Askes the server to execute a named process and provides the expected inputs.

## Geoserver

[Geoserver](http://geoserver.org) is a fully featured Web Mapping Server, and WPS integration is available as an addition. It has the advantage of being easier to install and comes pre-configured with a number of WPS processes, but it is not possible to create your own processes.

## PyWPS

[PyWPS](http://pywps.wald.intevation.org/) is a small python-based WPS that was originally developed to provide access to modules from GRASS GIS, but now it offers an environment for programming your own processes.

Installation and usage is more complex than for GeoServer as you are required to write your own processes, but it is under active development and has good documentation. It is probably best suited for those with an intermediate or higher level of experience writing python code.

## ZOO

[ZOO](http://www.zoo-project.org/) is a WPS written in C, but with support for Python and other common programming languages. It is split into three conceptual parts:
 * ZOO-Kernel- this is the program itself
 * ZOO-Services- the web services that you wish to use
 * ZOO-API- a javascript library based on OpenLayers and Proj4js, designed to make the it easier to create services.

ZOO is the most complex package to install and configure, although the use of OSGeo4W for installation on windows makes it easier. There is a lot of good documentation and tutorials on the ZOO website.




