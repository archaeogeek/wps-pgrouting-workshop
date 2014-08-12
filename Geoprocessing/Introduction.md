# Introduction to Geoprocessing with WPS

Now that we understand how WPS services are created, we can progress to using them for useful tasks!

The ZOO demo comes with a large number of services pre-built. We will look at the Buffer service.

## The .zcfg file

Navigate to /usr/lib/cgi-bin and right click the file buffer.zcfg to open it in a text editor.

The top section of this file is the metadata section, as it was with the Hello World service. In this section you will spot a new parameter:
* Profile: This is an optional unique identifier for the process, from the OGC URN namespace [see here for an in depth description](http://www.opengeospatial.org/ogcna)

The DataInputs section has two parameters:
* InputPolygon: The polygon you wish to buffer
* BufferDistance: The width of the buffer that you wish to create

The InputPolygon parameter has minOccurs =1 , so is mandatory. It has a ComplexData data type, with three supported types, namely 2 types of GML and JSON. UTF-8 encoded GML is the default type.

The BufferDistance parameter has minOccurs = 0, so is optional. In the case where the BufferDistance parameter is not provided, there is a default 10 metres (note the US spelling). It has  LiteralData data type, with a type float, which means that it will accept numbers with decimal places. Although the default Unit of Measurement (uom)is meters, it will also accept feet.

The DataOutputs section uses the same ComplexData definitions to describe the format of the output buffer polygon.

You'll also notice a different type of supported output data:
    <Supported>
     mimeType = image/png
     asReference = true
     msStyle = STYLE COLOR 125 0 105 OUTLINECOLOR 0 0 0 WIDTH 3 END
     useMapServer = true
    </Supported>

* mimeType = image/png: allow the saving of the output as an image
* asReference: (true/false) allow the output to be saved on the server
* msStyle: a style definition in MapServer format for the output polygon
* useMapServer: (true/false) instructs the ZOO kernel to make the output available in MapServer wms/wfs format for use in a web mapping application

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
        while i < len(geometry):
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

## Testing this out

It's a bit more difficult to fully test this function in a browser, but we will use the DescribeProcess call to understand how it works. As before, type in a new browser tab:
    http://localhost/cgi-bin/zoo_loader.cgi?Service=WPS&Version=1.0.0&Request=DescribeProcess&Identifier=Buffer

We will use the QGIS WPS Plugin to Execute this process, to save trying to pass the nodes of a polygon via a URL!

Go to the applications button in the top left of your screen and go to Education\QGIS (not QGIS browser or server). This will load QGIS 2.0, which is a little out of date but the same process should hold for later versions. Chances are, the WPS plugin is not yet installed, so click on Plugins on the top menu, and then "Manage and Install Plugins". Select "Get more from the list on the left, then in the Search bar, type WPS. In the results window you should see "WPS Client". Select it, then click on "Install plugin". Hopefully you will receive a message that the plugin installed successfully, then click on "Close" in the bottom right to close the plugin manager.

If you do not already see a WPS section in the bottom-left of QGIS, under the main Layers browser window, click on "Web" and then "WPS Client" in the top toolbar.

Click on "connect" in the WPS section, and then the "New" button to add a new WPS server. In the Name box, give your server a memorable name, such as ZOO OSGIS Demo. In the URL box type the path to the zoo_loader.cgi:
    http://localhost/cgi-bin/zoo_loader.cgi
Click OK to make this dialogue box disappear, and then with your new connection visible in the Server Connections box, click "Connect" to connect to the server.

If the URL was correct, you should now see a long list of processes or Identifiers, with their Title and Abstract. If the list of Identifiers is not in alphabetical order, click the column title to sort it to your liking.

From the list, find the Buffer service, and either highlight it and click "OK", or double-click it. This will bring up a simple form with the Input Vales in it, but we have got a bit ahead of ourselves because we haven't provided any input geometries yet! Click "back" to close the process dialogue box, and then "close" to exit from the WPS plugin.

The OSGeo Live DVD contains plenty of spatial data for us to use as an example. 



