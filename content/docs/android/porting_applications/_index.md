---
title: "Porting Applications to Android"
linkTitle: "Porting Applications to Android"
weight: 2
description: >
  Learn how to port your applications to the most widely used mobile platform
---

## Porting Applications to Android

### AndroidManifest.xml

Porting an application to Android requires adding an ```AndroidManifest.xml``` containing basic information about the app.

A basic AndroidManifest.xml may look like this:

```xml
<?xml version='1.0' encoding='utf-8'?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
          package="org.kde.alligator"
          android:versionName="0.0.1"
          android:versionCode="1588098483"
          android:installLocation="auto">
    <application android:name="org.qtproject.qt5.android.bindings.QtApplication" android:label="Alligator" android:icon="@drawable/alligator">
        <activity android:configChanges="orientation|uiMode|screenLayout|screenSize|smallestScreenSize|layoutDirection|locale|fontScale|keyboard|keyboardHidden|navigation"
                  android:name="org.qtproject.qt5.android.bindings.QtActivity"
                  android:label="Alligator"
                  android:windowSoftInputMode="adjustResize"
                  android:launchMode="singleTop">

            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>

            <meta-data android:name="android.app.lib_name" android:value="alligator"/>
            <meta-data android:name="android.app.qt_sources_resource_id" android:resource="@array/qt_sources"/>
            <meta-data android:name="android.app.repository" android:value="default"/>
            <meta-data android:name="android.app.qt_libs_resource_id" android:resource="@array/qt_libs"/>
            <meta-data android:name="android.app.bundled_libs_resource_id" android:resource="@array/bundled_libs"/>
            <meta-data android:name="android.app.extract_android_style" android:value="minimal"/>

            <!-- Deploy Qt libs as part of package -->
            <meta-data android:name="android.app.bundle_local_qt_libs" android:value="-- %%BUNDLE_LOCAL_QT_LIBS%% --"/>
            <meta-data android:name="android.app.load_local_libs_resource_id" android:resource="@array/load_local_libs"/>
            <!-- Run with local libs -->
            <meta-data android:name="android.app.use_local_qt_libs" android:value="-- %%USE_LOCAL_QT_LIBS%% --"/>
            <meta-data android:name="android.app.libs_prefix" android:value="/data/local/tmp/qt/"/>
            <meta-data android:name="android.app.load_local_libs" android:value="-- %%INSERT_LOCAL_LIBS%% --"/>
            <meta-data android:name="android.app.load_local_jars" android:value="-- %%INSERT_LOCAL_JARS%% --"/>
            <meta-data android:name="android.app.static_init_classes" android:value="-- %%INSERT_INIT_CLASSES%% --"/>
            <!--  Messages maps -->
            <meta-data android:value="@string/ministro_not_found_msg" android:name="android.app.ministro_not_found_msg"/>
            <meta-data android:value="@string/ministro_needed_msg" android:name="android.app.ministro_needed_msg"/>
            <meta-data android:value="@string/fatal_error_msg" android:name="android.app.fatal_error_msg"/>

            <!-- Splash screen -->
            <meta-data android:name="android.app.splash_screen_drawable" android:resource="@drawable/splash"/>

            <!-- Background running -->
            <meta-data android:name="android.app.background_running" android:value="false"/>

            <!-- auto screen scale factor -->
            <meta-data android:name="android.app.auto_screen_scale_factor" android:value="true"/>
        </activity>
    </application>

    <uses-sdk android:minSdkVersion="21" android:targetSdkVersion="28"/>
    <supports-screens android:largeScreens="true" android:normalScreens="true" android:anyDensity="true" android:smallScreens="true"/>

    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <uses-permission android:name="android.permission.INTERNET" />

</manifest>
```

Most of this file is just boilerplate code that can be pasted directly into your application. Every occurence of the applications name (in this case ```alligator``` or ```Alligator```) and the package (```org.kde.alligator```) must be changed to the name and package of the application being ported. If the application requires and permissions (for example if it accesses the internet, bluetooth or location services), thise must be added here.

### Icon

The application's icon must be included as an image file. The ```android:icon``` parameter in ```AndroidManifest.xml``` must be set accordingly.

### Splash screen

To create a splash screen for application startup, a ```splash.xml``` file must be created with this content:

```xml
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:width="128dp" android:height="128dp" android:gravity="center">
        <bitmap
            android:layout_width="fill_parent"
            android:layout_height="fill_parent"
            android:scaleType="fitXY"
            android:src="@drawable/alligator"/>
    </item>
</layer-list>
```

The ```android:src``` parameter must be set to the application's icon.


The AndroidManifest.xml, splash.xml and icon files should be stored in the application's source directory like this:

```
./android/AndroidManifest.xmlns
./android/res/drawable/alligator.png
./android/res/drawable/splash.xml
```

## Dependencies

If this is the first version of the application to be built on the Binary Factory, the dependency metadata should be added to the [Repo Metadata](https://invent.kde.org/sysadmin/repo-metadata) repository. If the project does not have any dependencies outside of Qt and the KDE frameworks, this is not required, since these dependencies are implicitely added. It is recommended however, to remove this dependency on the kf5umbrella and list the frameworks dependencies explicitely to stop the build infrastructure from building unnecessary frameworks.
