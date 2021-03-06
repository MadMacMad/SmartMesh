# Bluetooth SmartMesh Controller
Implemented with the Ionic framework, the Bluetooth SmartMesh Controller lets you interact with the smart mesh from your iPhone, Android, or from Chrome.
For limited time, you can try the app from our server in [Chrome Canary] on OSX or [Chrome DEV] on Android.

Just click here: [Run](https://aircable.net:8101)

### Setup
Setup the ionic environment from [Ionic]. See [Ionic Instructions].

```sh
sudo npm install -g cordova ionic gulp bower
```
### Other Packages
We use several other software packages to support the application.
* ionic material
* RobotoDraft, font face kit
* ionic material input plugin
* angular slider
* material design icons
```sh
bower install ionic-material
bower install robotodraft
bower install ion-md-input
bower install --save angularjs-slider
bower install material-design-icons
```

### Icons
Many icons are used and modified from [Google Icons]. These are SGV file and preloaded.

### Plugins
The following Ionic and [Cordova] plugins are used. Specifically the ble-central plugin has to be modified to allow processing of scan results in arrays.
```sh
ionic plugin add ionic-plugin-keyboard@1.0.8
ionic plugin add cordova-plugin-device@1.1.1
ionic plugin add cordova-plugin-console@1.0.2
ionic plugin add cordova-plugin-splashscreen@3.1.0
ionic plugin add cordova-plugin-whitelist@1.2.1
ionic plugin add cordova-plugin-transport-security@0.1.1
ionic plugin add cordova-plugin-inappbrowser@1.1.1
ionic plugin add com.unarin.cordova.beacon
ionic plugin add cordova-plugin-ble-central
ionic plugin add cordova-plugin-x-toast
```

### iBeacon plugin 
Install the iBeacon plugin and the angular wrapper.
```sh
cordova plugin add https://github.com/petermetz/cordova-plugin-ibeacon
cordova plugin add https://github.com/nraboy/ng-cordova-beacon
```
Fix ble.js in plugin cordova-plugin-ble-central. Modify, so that  stringToArrayBuffer return Uint8Array and not ArrayBuffer.

### MQTT 
The code uses the [MQTT JavaScript] implementation. To make it usable in the browser, we browserify it.
```sh
npm install mqtt
browserify mqtt.js -s mqtt > ../../SmartMesh/www/js/mqtt.js 
```

### MQTT server
Run your own MQTT server, or install is via brew on OSX. Building your MQTT server can be a bit challenging. See [Compiling Mosquitto].

```sh
/usr/local/opt/mosquitto/sbin/mosquitto -c /usr/local/etc/mosquitto/mqtt.conf
```

# Splash Screen and App Icon
Ionic automatically makes all icon and splash screen files for publication of the native app on iOS. Use the icon.psd and splash.psd files and run:
```sh
ionic resources --icon --splash
```

## Running on IOS 
We often get into debugging issues when running the app on the device via Ionic. Like that:
```sh
ionic run ios -device -debug
```
I recommend to prepare the app with Ionic and then run it on Xcode
### Run with Xcode 
```sh
ionic prepare ios --minify
```
Open the project in Xcode using the generated Xcode project file. Minify compresses all the files and images to make a smaller image for distribution and faster runtime.

# Serve for Chrome browser
In order to be able to use Web Bluetooth, the app must be served through a secure web server. There are two ways to do that.
```sh
ionic platform add browser
ionic build browser --minify
```
Then deploy on your web server. 
The second option is to run the app under Ionic Lab with debug.
```sh
ionic serve -lc
```
This is a build-in web server that opens up a screen with iOS and Android screen simulations, but it is not a secure server. To do that, use a https proxy redirector. See file httpsproxy.js and run it with node, after the ionic serve is running.
```sh
node httpsproxy.js
```
# Building Native App
Here are some resources how to publish app in the iTune Connect using XCode. It is the usual detail work Apple wants you to do for screenshots, permissions, entitelements. We found that the app needs PUSH to be enabled. We have not tried to publish on Google Play.
```sh
http://ionicframework.com/docs/guide/publishing.html
```
### More Information
For more information consult the [Documentation].

[Ionic]: <https://www.ionicframework.com/>
[Ionic Instructions]: <https://www.airpair.com/javascript/posts/a-year-using-ionic-to-build-hybrid-applications>
[Google Icons]: <https://design.google.com/icons>
[MQTT JavaScript]: <https://github.com/mqttjs/MQTT.js.git>
[Compiling Mosquitto]: <http://goochgooch.co.uk/2014/08/01/building-mosquitto-1-4>
[Documentation]: <https://aircable.co/blog/aircable-news-1/post/bluetooth-smartmesh-15>
[Chrome Canary]: <https://www.google.com/chrome/browser/canary.html>
[Chrome DEV]: <https://play.google.com/store/apps/details?id=com.chrome.dev&hl=en>