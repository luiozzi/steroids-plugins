# PhoneGap SQLitePlugin

Integrated iOS plugin from https://github.com/pgsqlite/PG-SQLitePlugin-iOS
Integrated Android plugin from https://github.com/pgsqlite/PG-SQLitePlugin-Android (experimental version modified for Steroids, not offically supported)

Forum & community support at: http://groups.google.com/group/pgsqlite

## Usage with Steroids

The SQLite plugin is bundled in with AppGyver Scanner, so there's no need to install it separately. Simply copy the JavaScript files in the `www` directory to your project and load them in your app, e.g. with a `<script src="/plugins/sqliteplugin.js"></script>" tag. Note that you only need to load the `sqliteplugin.js` file – on Android, the `sqliteplugin.android.js` file is used automatically instead. Read more at [Steroids Guides](http://guides.appgyver.com/steroids/guides/android/android-extension/).

You also need to make sure that your `config.platform.xml` file has the correct tag defined:

* Android:
  `<plugin name="SQLitePlugin" value="com.phonegap.plugin.sqlitePlugin.SQLitePlugin"/>`
* iOS:
  `<plugin name="SQLitePlugin" value="SQLitePlugin" />`
