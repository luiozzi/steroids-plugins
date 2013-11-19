# Google Analytics Cordova plugin for iOS

## Usage with Steroids

The Google Analytics plugin is bundled in with AppGyver Scanner for iOS, so there's no need to install it separately. Simply copy the JavaScript files in the `www` directory to your project and load them in your app, e.g. with a `<script src="/plugins/GAPlugin.js"></script>" tag.

You also need to make sure that your `config.ios.xml` file has the correct tag defined:

    <plugin name="GAPlugin" value="GAPlugin" />
