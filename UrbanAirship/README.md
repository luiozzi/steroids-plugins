# Urban Airship plugin for Steroids

This verison of the Urban Airship plugin is forked from an earlier version of [the offical repo](https://github.com/urbanairship/phonegap-ua-push), and will be updated to the latest version once the migration to Cordova 3.0 is complete.

##Configuration in Steroids

The plugin is bundled in with AppGyver Scanner for iOS, so there's no need to install it separately. Copy the JavaScript file in the `www` directory of this repository to your project and load it in your app, e.g. with a `<script src="/plugins/PushNotification.js"></script>` tag. Also make sure that your `www/config.ios.xml` file has the correct plugin tag defined:

`<plugin name="PushNotificationPlugin" value="PushNotificationPlugin" />`

You then need to set up the correct Urban Airship App Key and App Secret in the **Configure iOS Build Settings** page for your app in [AppGyver Cloud](http://cloud.appgyver.com). Next, make sure your app is set up to receive push notifications in the Apple Developer Portal and update your `.mobileprovision` and `.p12` files if necessary. Finally, request a build for your app.


##Test app

In the `test/` folder is the Urban Airship test app, runnable as a Steroids project. Go to the project root and write `$ steroids connect`. Note that you need a Scanner app with properly configured Urban Airship App Key and App Secret (see above). Push notifications do not work on the iOS Simulator.
