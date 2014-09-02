# Introduction to Geoprocessing with WPS

Now that we understand how WPS services are created, we can progress to using them for useful tasks!

The ZOO demo comes with a large number of services pre-built. We will look at the Buffer service.

## The .zcfg file

Navigate to /usr/lib/cgi-bin and right click the file buffer.zcfg to open it in medit.

The top section of this file is the metadata section, as it was with the Hello World service. In this section you will spot a new parameter:

* **Profile**: This is an optional unique identifier for the process, from the OGC URN namespace [see here for an in depth description](http://www.opengeospatial.org/ogcna)

The DataInputs section has two parameters:

* **InputPolygon**: The polygon you wish to buffer;
* **BufferDistance**: The width of the buffer that you wish to create.

The InputPolygon parameter has minOccurs =1 , so is mandatory. It has a ComplexData data type, with three supported types, namely 2 types of GML and JSON. UTF-8 encoded GML is the default type.

The BufferDistance parameter has minOccurs = 0, so is optional. In the case where the BufferDistance parameter is not provided, there is a default 10 metres (note the US spelling). It has  LiteralData data type, with a type float, which means that it will accept numbers with decimal places. Although the default Unit of Measurement (uom) is meters, it will also accept feet.

The DataOutputs section uses the same ComplexData definitions to describe the format of the output buffer polygon.

You'll also notice a different type of supported output data:

    <Supported>
     mimeType = image/png
     asReference = true
     msStyle = STYLE COLOR 125 0 105 OUTLINECOLOR 0 0 0 WIDTH 3 END
     useMapServer = true
    </Supported>

* **mimeType = image/png**: allow the saving of the output as an image;
* **asReference**: (true/false) allow the output to be saved on the server;
* **msStyle**: a style definition in MapServer format for the output polygon;
* **useMapServer**: (true/false) instructs the ZOO kernel to make the output available in MapServer wms/wfs format for use in a web mapping application.

## The Service Provider file

The serviceProvider line in buffer.zcfg indicates that the python script is in the service.py file. Navigate to this in /usr/lib/cgi-bin and right-click to open it in a text editor.

This python file contains a number of defined functions, and at top we can also see the modules that are used. Alongside the zoo module that we used in our Hello World service, the geoprocessing functions also use the ogr and gdal libraries, some system (sys) libraries, and libxml2, for parsing xml.

NOTE: we're not going to try and write our own buffer function in this workshop, as Geoprocessing with Python is a course all of it's own! For reference material you could look at [OSGeo's wiki page](http://trac.osgeo.org/gdal/wiki/GdalOgrInPython), and in particular the [Utah State University tutorial](http://www.gis.usu.edu/~chrisg/python), and also [Python Geosptial Development](http://www.packtpub.com/application-development/python-geospatial-development) (from Packt Publishing).

This particular service provider file contains a number of helper functions defined at the top, and then the named functions lower down. This allows better code reuse, but can make it difficult to follow what goes on in an individual process.

    def Buffer(conf,inputs,outputs):
        print >> sys.stderr, "Starting service..."
        bdist=float(inputs["BufferDistance"]["value"])
        print >> sys.stderr, bdist
        print >> sys.stderr, inputs["InputPolygon"]
        geometry=extractInputs(conf,inputs["InputPolygon"])
        i=0
        rgeometries=[]
        while i &lt; len(geometry):
            tmp=geometry[i].Clone()
            resg=geometry[i].GetGeometryRef().Buffer(bdist)
            tmp.SetGeometryDirectly(resg)
            rgeometries+=[tmp]
            geomtry[i].Destroy()
            resg.thisown=False
            tmp.thisown=False
            i+=1
        outputResult(outputs["Result"],rgeometries)
        i=0
        return zoo.SERVICE_SUCCEEDED

This function has the same arguments as our Hello World script, namely the config from main.cfg, and the inputs and outputs dictionaries. The inputs dictionary contains two KVP- InputPolygon and BufferDistance, as defined in buffer.zcfg.

The first 5 lines output some messages to the standard output and create variables to hold the values for InputPolygon and BufferDistance.

Line 6 uses the first helper function- extractInputs. However, this is merely a gateway function to a number of other functions that do the real legwork to create a layer and associated features in memory from the input data.

These associated features are returned to the Buffer function in line 6, then the following lines iterate through each feature, clone it, buffer it, and return it to a dictionary called rgeometries. When each feature has been buffered, rgeometries is appended to the outputResult dictionary and the script returns the SERVICE_SUCCEEDED value.

 



