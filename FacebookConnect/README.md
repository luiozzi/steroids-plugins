# Apache Cordova Facebook Plugin

The integrated plugin used is from https://github.com/phonegap/phonegap-facebook-plugin - see the repo for README

## Usage in Steroids

The Apache Cordova Facebook Plugin is bundled in with AppGyver Scanner for iOS, so there's no need to install it separately. Simply copy the JavaScript files in the `www` directory to your project and load them in your app, e.g. with tags

```
<script src="/plugins/cdv-plugin-fb-connect.js"></script>
<script src="/plugins/facebook-js-sdk.js"></script>
```
You also need to make sure that your `config.ios.xml` file has the correct tag defined:

`<plugin name="org.apache.cordova.facebook.Connect" value="FacebookConnectPlugin" />`

Finally, to connect to your Facebook app, you need to build a custom Scanner app with the correct Facebook URL protocol defined.

See the [Using the integrated Facebook Connect plugin guide](http://guides.appgyver.com/steroids/guides/phonegap_on_steroids/facebook-connect-plugin) for more information.
