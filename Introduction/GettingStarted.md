# Getting Started

We are going to be using ZOO for our WPS during this demo. A small amount of preparation work is required to get all the files we need.

1. Right-click on a blank bit of desktop and choose 'open terminal'
2. Copy the following commands one line at a time and paste them into your terminal using ctrl-v:
        wget http://www.zoo-project.org/dl/ws2013-1.tar.bz2
        sudo tar -xvjpf ws2013-1.tar.bz2 -C /
        psql -f /var/www/temp/ws2013.sql pgrouting
If asked for the sudo password, it's user
3. You also need to install an additional library (utility package) for ZOO to run. Again in your terminal type the following:
        sudo apt-get install libsvg-cairo
If asked for the sudo password, it's user
4. Restart the apache service to ensure it picks up the changes. Again in your terminal, type the following:
        sudo /etc/init.d/apache2 restart
... and guess what the sudo password is?

