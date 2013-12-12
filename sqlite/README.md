# PhoneGap SQLitePlugin

Readme and usage instructions at the [official GitHub repo](https://github.com/lite4cordova/Cordova-SQLitePlugin)

Forum & community support at: http://groups.google.com/group/pgsqlite

## Usage with Steroids

The SQLite plugin is bundled in with AppGyver Scanner, so there's no need to install it separately. Cordova also automatically loads the JavaScript file and creates a `window.sqlitePlugin` object in all WebViews.

## Resources

There's an [SQLite Guide](http://guides.appgyver.com/steroids/guides/phonegap_on_steroids/prepopulated-sqlite/) available on using a local prepopulated database.

You can also generate a CRUD scaffold that can be configured to use the SQLite plugin by typing

    $ steroids generate ng-sql-scaffold

in your Steroids project directory.
