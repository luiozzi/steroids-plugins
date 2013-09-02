# Push plugin

Forked from https://github.com/phonegap-build/PushPlugin – see repo README for usage instructions.

Adapted for Steroids by AppGyver, Inc.

##Configuration in Steroids

The Push plugin is bundled in with AppGyver Scanner for iOS, so there's no need to install it separately. Simply copy the JavaScript file in the `www` directory to your project and load them in your app, e.g. with a `<script src="/plugins/PushNotification.js"></script>" tag.

You also need to make sure that your `config.ios.xml` file has the correct plugin tag defined:

* config.ios.xml:
  `<plugin name="PushPlugin" value="PushPlugin" />`
