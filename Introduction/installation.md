# Installation

The three WPS packages are listed below, in order of complexity of installation.

## Geoserver

Geoserver can be installed from [Geoserver.org](http://geoserver.org), where you should choose the latest stable release (2.5.1 at the time of writing). There are several different installation methods, including pre-compiled installers for windows and mac, a platform independent binary, and a Web Archive (war) file for use in java servlet environment on a web server.

The method of starting geoserver once you have it installed will depend on the installation method. Instructions can be found on the [geoserver website](http://docs.geoserver.org/stable/en/user/installation/index.html).

WPS capability is provided as an extension to the core program, and as such must be installed separately, from the extensions section of the geoserver download page. The downloaded zip file should be unzipped, and the constituent files moved to the lib directory of your geoserver install.

Although the folder paths may change, regardless of which installation method you choose, the actual geoserver program will be held within a folder called [installation path]/webapps/geoserver. The lib directory is in webapps/geoserver/WEB-INF/lib. When you restart geoserver you should see WPS available as a service.

## PyWPS

PyWPS is under development and the only way to install it is from the [GitHub Repository](https://github.com/jachym/pywps-4) using pip, the tool for installing and managing python packages. So before you get started with PyWPS you will need to ensure you have Python 2.7 or newer installed, and Pip, which you can find instructions for [here](https://pip.pypa.io/en/latest/).

The [documentation](http://pywps.readthedocs.org/en/latest/) for PyWPS suggests that it can be installed along with all of its dependencies with the following command:
&lt;pre&gt;&lt;code&gt;$ pip install -e git+https://github.com/jachym/pywps-4.git@master#egg=pywps-dev&lt;/code&gt;&lt;/pre&gt;
If this fails, then the dependencies (lxml and werkzeug) can also be installed using pip, then re-run the above command.

## ZOO

There are installation instructions for every platform in the [ZOO documentation](http://www.zoo-project.org/docs/). On windows the recommended approach is to use the [OSGeo4W installer](http://trac.osgeo.org/osgeo4w/). This should also install all dependencies, but if not then as a minimum you will require a web server with FastCGI, libxml, Python and cURL.

For linux there are instructions on for each major distribution. There are a large number of dependencies, but all are listed, along with the repository where they can be downloaded. The ZOO Kernel itself must be compiled from source from the [subversion repository](http://svn.zoo-project.org/svn/trunk zoo-project), but again instructions are provided for this.


