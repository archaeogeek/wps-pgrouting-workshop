# Installation

# Geoserver

Geoserver can be installed from [Geoserver.org](http://geoserver.org), where you should choose the latest stable release (2.5.1 at the time of writing). There are several different installation methods, including pre-compiled installers for windows and mac, a platform independent binary, and a Web Archive (war) file for use in java servlet environment on a web server.

The method of starting geoserver once you have it installed will depend on the installation method. Instructions can be found on the [geoserver website](http://docs.geoserver.org/stable/en/user/installation/index.html).

WPS capability is provided as an extension to the core program, and as such must be installed separately, from the extensions section of the geoserver download page. The downloaded zip file should be unzipped, and the constituent files moved to the lib directory of your geoserver install.

Although the folder paths may change, regardless of which installation method you choose, the actual geoserver program will be held within a folder called [installation path]/webapps/geoserver. The lib directory is in webapps/geoserver/WEB-INF/lib. When you restart geoserver you should see WPS available as a service.

