# npm-vwgolfapi
NPM package for Volkswagen app API based on https://github.com/TA2k/ioBroker.vw-connect,
https://github.com/nightsha-de/npm-vwconnectapi and

https://github.com/adhyh/npm-vwconnectidapi

This API is taken entirely from that of ADHYH. I simply modified the query interval to avoid crashes on my golf. it's only for Homebridge plugin 

TUTORIAL:
If you have installed the Homebridge plugin "https://github.com/adhyh/homebridge-vwconnectid.git" but you encounter query problems (error 429). You must use this API.
To do this, install
" sudo npm install -g https://github.com/Libar19/vwgolfapi.git ".

Then find the path where the api is installed and go to the package.json of the plugin "Homebridge-vwconnectid" you should have ""dependencies": {
"npm-vwconnectidapi": "1.1.4"
edited on 1.1.4 by the path to your api download previously.

Save, Reboot Homebridge and Enjoy :)

