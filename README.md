# Android Resource File Provider

[![API](https://img.shields.io/badge/API-14%2B-orange.svg?style=flat)](https://developer.android.com/about/dashboards/index.html#Platform)
[![license](https://img.shields.io/github/license/mashape/apistatus.svg)](LICENSE.md)
[![Build Status](https://travis-ci.org/federicoboschini/Android-Resource-File-Provider.svg?branch=master)](https://travis-ci.org/federicoboschini/Android-Resource-File-Provider)

Easily share audio, video, image and document files from `raw`, `assets` and `drawable` folders **without any specific permission**.

## Installation

Add the repository at the top of your app `build.gradle`:

```groovy
repositories {
    maven {
        url  "https://dl.bintray.com/boschini/android-libraries"
    }
}
```

Include the dependency in your app `build.gradle` file:

```groovy
implementation 'it.federicoboschini:resource-file-provider:1.0.0'
```

If you are experiencing any issue with resource merging or support-library compatibility add the dependency excluding `com.android.support`:

```groovy
implementation('it.federicoboschini:resource-file-provider:1.0.0') {
    exclude group: 'com.android.support'
}
```

## Configuration

Add this to your app `AndroidManifest.xml`:

```xml
<application>
<!--    ...     -->
    <provider
        android:name="android.support.v4.content.FileProvider"
        android:authorities="@string/rfp_provider_authority"
        android:exported="false"
        android:grantUriPermissions="true">
        <meta-data
            android:name="android.support.FILE_PROVIDER_PATHS"
            android:resource="@xml/file_provider_paths" />
    </provider>
<!--    ...     -->
</application>
```

**Optional** Add this (provider authority) to your app `strings.xml`:

```xml
<resources>
    <!--    ...     -->
    <string name="rfp_provider_authority">it.my_company.my_app.my_authority</string>
    <!--    ...     -->
</resources>
```

**Optional** Define custom file paths by creating a `file_provider_paths` in `res/xml`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<paths>
    <!-- Define the desired file paths -->
    
    <!--    The default is:     -->
    <!-- <files-path name="all" path="/"/> -->
</paths>

```

## Example: share a mp3 file

```java
try {
    ResourceFileProvider.Builder
        .from(this)
        .setDirectory(FOLDER_RAW)
        .setFileName("my_sound")
        .setFileExtension("mp3")
        .setFileType(TYPE_AUDIO)
        .build()
        .shareFile();
} catch (FileNotFoundException e) {
    e.printStackTrace();
}
```

***Note:*** `setFileName` parameter is **without** the extension.

### Current supported and tested extensions:

* `.mp3`
* `.jpg`
* `.png`
* `.pdf`
* `.mp4`

### Current supported and tested folders:

* `drawable`
* `raw`
* `assets`

***Note:*** if your project contains density-dependent drawables, the nearest to the device density will be selected.

### Known issues:

* Can't get images from `mipmap` folder.
