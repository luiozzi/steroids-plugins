BarcodeScanner
==============

Cross-platform BarcodeScanner for Cordova / PhoneGap. Verified to work with [AppGyver Steroids](http://www.appgyver.com/steroids).

From [iOS](https://github.com/phonegap/phonegap-plugins/tree/master/iOS/BarcodeScanner) and [Android](https://github.com/phonegap/phonegap-plugins/tree/master/Android/BarcodeScanner) repos

## Using the plugin ##

### Configuration for Steroids

The BarcodeScanner plugin is bundled in with AppGyver Scanner, so there's no need to install it separately. Simply copy the JavaScript files in the `www` directory to your project and load them in your app, e.g. with a `<script src="/plugins/barcodescanner.js"></script>" tag.

Note that you only need to load the `barcodescanner.js` file – on Android, the `barcodescanner.android.js` file is used automatically instead. Read more at [Steroids Guides](http://guides.appgyver.com/steroids/guides/android/android-extension/).

You also need to make sure that your `config.platform.xml` file has the correct tag defined:

* config.android.xml:
  `<plugin name="BarcodeScanner" value="com.phonegap.plugins.barcodescanner.BarcodeScanner"/>`
* config.ios.xml:
  `<plugin name="org.apache.cordova.barcodeScanner" value="CDVBarcodeScanner" />`

