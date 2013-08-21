# Cordova/PhoneGap SQLitePlugin

Native interface to sqlite in a Cordova/PhoneGap plugin, working to follow the HTML5 Web SQL API as close as possible.

Created by @joenoon and @davibe

API changes by @brodybits (Chris Brody)

iOS nested transaction callback support by @ef4 (Edward Faulkner)

Cordova 2.7+ port with background processing by @j3k0 (Jean-Christophe Hoelt)

Steroids compatibility by AppGyver

License for this version: MIT

## Usage with Steroids

The SQLite plugin is bundled in with AppGyver Scanner, so there's no need to install it separately. Simply copy the JavaScript files in the `www` directory to your project and load them in your app, e.g. with a `<script src="/plugins/sqliteplugin.js"></script>" tag. Note that you only need to load the `sqliteplugin.js` file – on Android, the `sqliteplugin.android.js` file is used automatically instead. Read more at [Steroids Guides](http://guides.appgyver.com/steroids/guides/android/android-extension/).

You also need to make sure that your `config.platform.xml` file has the correct tag defined:

* Android:
  `<plugin name="SQLitePlugin" value="com.phonegap.plugin.sqlitePlugin.SQLitePlugin"/>`
* iOS:
  `<plugin name="SQLitePlugin" value="SQLitePlugin" />`

## Announcements

- Forum & community support at: http://groups.google.com/group/pgsqlite

## Highlights

As described in [a recent posting](http://brodyspark.blogspot.com/2012/12/cordovaphonegap-sqlite-plugins-offer.html):
- Keeps sqlite database in a known user data location that will be backed up by iCloud on iOS. This Cordova/PhoneGap SQLitePlugin continues to show excellent reliability, compared to the problems described in [CB-1561](https://issues.apache.org/jira/browse/CB-1561), in [this thread](https://groups.google.com/forum/?fromgroups=#!topic/phonegap/eJTVra33HLo), and also [this thread](https://groups.google.com/forum/?fromgroups=#!topic/phonegap/Q_jEOSIAxsY)
- No 5MB maximum, more information at: http://www.sqlite.org/limits.html

Some other highlights:
- Drop-in replacement for HTML5 SQL API: the only change is window.openDatabase() --> sqlitePlugin.openDatabase()
- Fail-safe nested transactions with batch processing optimizations
- [integration with SQLCipher for iOS](http://brodyspark.blogspot.com/2012/12/integrating-sqlcipher-with.html)

## Known limitations

- The db version, display name, and size parameter values are not supported and will be ignored.
- The sqlite plugin will not work before the callback for the "deviceready" event has been fired, as described in **Usage**.

# Usage

The idea is to emulate the HTML5 SQL API as closely as possible. The only major change is to use window.sqlitePlugin.openDatabase() (or sqlitePlugin.openDatabase()) instead of window.openDatabase(). If you see any other major change please report it, it is probably a bug.

## Opening a database

There are two options to open a database:
- Recommended: `var db = window.sqlitePlugin.openDatabase({name: "DB"});`
- Classical: `var db = window.sqlitePlugin.openDatabase("Database", "1.0", "Demo", -1);`

**IMPORTANT:** Please wait for the "deviceready" event, as in the following example:

    // Wait for Cordova to load
    document.addEventListener("deviceready", onDeviceReady, false);

    // Cordova is ready
    function onDeviceReady() {
      var db = window.sqlitePlugin.openDatabase({name: "DB"});
      // ...
    }

**NOTE:** The database file is created with `.db` extension.

## Background processing

To enable background processing open a database like:

    var db = window.sqlitePlugin.openDatabase({name: "DB", bgType: 1});

# Sample with PRAGMA feature

This is a pretty strong test: first we create a table and add a single entry, then query the count to check if the item was inserted as expected. Note that a new transaction is created in the middle of the first callback.

        // Wait for Cordova to load
        document.addEventListener("deviceready", onDeviceReady, false);

        // Cordova is ready
        function onDeviceReady() {
          var db = window.sqlitePlugin.openDatabase({name: "DB"});

          db.transaction(function(tx) {
            tx.executeSql('DROP TABLE IF EXISTS test_table');
            tx.executeSql('CREATE TABLE IF NOT EXISTS test_table (id integer primary key, data text, data_num integer)');

            // demonstrate PRAGMA:
            db.executeSql("pragma table_info (test_table);", [], function(res) {
              console.log("PRAGMA res: " + JSON.stringify(res));
            });

            tx.executeSql("INSERT INTO test_table (data, data_num) VALUES (?,?)", ["test", 100], function(tx, res) {
              console.log("insertId: " + res.insertId + " -- probably 1");
              console.log("rowsAffected: " + res.rowsAffected + " -- should be 1");

              db.transaction(function(tx) {
                tx.executeSql("select count(id) as cnt from test_table;", [], function(tx, res) {
                  console.log("res.rows.length: " + res.rows.length + " -- should be 1");
                  console.log("res.rows.item(0).cnt: " + res.rows.item(0).cnt + " -- should be 1");
                });
              });

            }, function(e) {
              console.log("ERROR: " + e.message);
            });
          });
        }

## Sample with transaction-level nesting

In this case, the same transaction in the first executeSql() callback is being reused to run executeSql() again.

        // Wait for Cordova to load
        document.addEventListener("deviceready", onDeviceReady, false);

        // Cordova is ready
        function onDeviceReady() {
          var db = window.sqlitePlugin.openDatabase("Database", "1.0", "Demo", -1);

          db.transaction(function(tx) {
            tx.executeSql('DROP TABLE IF EXISTS test_table');
            tx.executeSql('CREATE TABLE IF NOT EXISTS test_table (id integer primary key, data text, data_num integer)');

            tx.executeSql("INSERT INTO test_table (data, data_num) VALUES (?,?)", ["test", 100], function(tx, res) {
              console.log("insertId: " + res.insertId + " -- probably 1");
              console.log("rowsAffected: " + res.rowsAffected + " -- should be 1");

              tx.executeSql("select count(id) as cnt from test_table;", [], function(tx, res) {
                console.log("res.rows.length: " + res.rows.length + " -- should be 1");
                console.log("res.rows.item(0).cnt: " + res.rows.item(0).cnt + " -- should be 1");
              });

            }, function(e) {
              console.log("ERROR: " + e.message);
            });
          });
        }

This case will also works with Safari (WebKit), assuming you replace window.sqlitePlugin.openDatabase with window.openDatabase.

# Common traps & pitfalls

- The plugin class name starts with "SQL" in capital letters, but in Javascript the `sqlitePlugin` object name starts with "sql" in small letters.
- Attempting to open a database before receiving the "deviceready" event callback.

# Support

Community support is available via the new Google group: http://groups.google.com/group/pgsqlite

If you have an issue with the plugin please check the following first:
- You are using the latest version of the Plugin Javascript & Objective-C source from this repository.
- You have installed the Javascript & Objective-C correctly.
- You have included the correct version of the cordova Javascript and SQLitePlugin.js and got the path right.
- You have registered the plugin properly in `config.xml`.

If you still cannot get something to work:
- Make the simplest test program necessary to reproduce the issue and try again.
- If it still does not work then please make sure it is prepared to demonstrate the issue including:
  - it completely self-contained, i.e. it is using no extra libraries beyond cordova & SQLitePlugin.js;
  - if the issue is with *adding* data to a table, that the test program includes the statements you used to open the database and create the table;
  - if the issue is with *retrieving* data from a table, that the test program includes the statements you used to open the database, create the table, and enter the data you are trying to retrieve.

Then please post the issue to the [pgsqlite forum](http://groups.google.com/group/pgsqlite).

# Contributing

- The original repos of the Cordova plugin are at https://github.com/pgsqlite/PG-SQLitePlugin-iOS and https://github.com/pgsqlite/PG-SQLitePlugin-Android 
- Testimonials of apps that are using this plugin would be especially helpful.
- Reporting issues to the [pgsqlite forum](http://groups.google.com/group/pgsqlite) can help improve the quality of this plugin.
- Patches with bug fixes are helpful, especially when submitted with test code.
- Other enhancements will be considered if they do not increase the complexity of this plugin.
- All contributions may be reused by [@brodybits (Chris Brody)](https://github.com/brodybits) under another license in the future. Efforts will be taken to give credit for major contributions but it will not be guaranteed.
