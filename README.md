# Experiment 11: Google Maps Android Application

**Course**: Mobile Application Development Lab  
**Subject **: Mobile Application Development  
**Experiment No**: 11  

---

## 👤 Student Information

| Field | Details |
| :--- | :--- |
| **Student Name** | Spencer Aaron Fernandes |
| **USN** | 25MCAR0123 |
| **Course** | MCA General |
| **Semester** | 3rd Semester |
| **Academic Year** | 2025–2027 |

---

## 🎯 Aim / Objective

To develop an Android application that displays an interactive **Google Map** using the **Google Maps SDK for Android**, adds custom location markers, adjusts camera zoom levels, and manages API authentication.

---

## 📚 Theory & Key Concepts

1. **Google Maps SDK for Android**: A Google Play Services library that embeds interactive vector maps into Android applications.
2. **SupportMapFragment**: A subclass of `Fragment` that acts as the container for the map object and manages its lifecycle automatically.
3. **OnMapReadyCallback**: An interface providing a callback method (`onMapReady`) triggered when the map instance is fully loaded and ready to be manipulated programmatically.
4. **GoogleMap**: The primary class representing the map object in the app, allowing developers to add markers, polylines, overlays, and control camera movements.
5. **LatLng & MarkerOptions**: 
   - `LatLng`: Represents a geographical point defined by latitude and longitude coordinates.
   - `MarkerOptions`: Defines properties for a map marker (position, title, snippet, icon, etc.).
6. **CameraUpdateFactory**: Generates camera movements such as zooming in/out or moving to specific `LatLng` coordinates.

---

## 🛠️ Software & System Requirements

- **IDE**: Android Studio (Ladybug / Jellyfish or higher)
- **Programming Language**: Kotlin
- **JDK Version**: Java Development Kit (JDK) 11 or higher
- **Min SDK Version**: API Level 24 (Android 7.0 Nougat)
- **Target / Compile SDK Version**: API Level 37
- **Google Play Services**: `com.google.android.gms:play-services-maps:20.1.0`
- **Internet Connection**: Required to load map vector tiles and tilesets from Google servers.

---

## 📝 Step-by-Step Procedure / Algorithm

1. **Project Setup**:
   - Create a new project in Android Studio using the **Empty Views Activity** template with Kotlin language support.
2. **Add Dependencies**:
   - Add `com.google.android.gms:play-services-maps` to `gradle/libs.versions.toml` and `app/build.gradle.kts`.
3. **Configure Permissions & Metadata**:
   - Open `AndroidManifest.xml` and add permissions for `INTERNET`, `ACCESS_FINE_LOCATION`, and `ACCESS_COARSE_LOCATION`.
   - Add the `<meta-data>` entry for `com.google.android.geo.API_KEY` under the `<application>` tag.
4. **Obtain and Store Google Maps API Key**:
   - Generate an API Key in the [Google Cloud Console](https://console.cloud.google.com/).
   - Add the API Key string resource inside `res/values/strings.xml`.
5. **Design User Interface**:
   - In `res/layout/activity_main.xml`, add a `FragmentContainerView` referencing `com.google.android.gms.maps.SupportMapFragment`.
6. **Implement Map Logic in Activity**:
   - Extend `MainActivity` with `OnMapReadyCallback`.
   - Obtain a reference to `SupportMapFragment` and invoke `getMapAsync(this)`.
   - Override `onMapReady(googleMap: GoogleMap)` to add a marker (`LatLng`) and adjust the camera zoom level.
7. **Build and Run**:
   - Compile and execute the application on an Android Emulator or physical device.

---

## 💻 Source Code Implementation

### 1. `app/build.gradle.kts`
```kotlin
plugins {
    alias(libs.plugins.android.application)
}

android {
    namespace = "com.example.googlemaps"
    compileSdk { version = release(37) }

    defaultConfig {
        applicationId = "com.example.googlemaps"
        minSdk = 24
        targetSdk = 37
        versionCode = 1
        versionName = "1.0"
        testInstrumentationRunner = "androidx.test.runner.AndroidJUnitRunner"
    }

    compileOptions {
        sourceCompatibility = JavaVersion.VERSION_11
        targetCompatibility = JavaVersion.VERSION_11
    }
}

dependencies {
    implementation(libs.google.play.services.maps)
    implementation(libs.androidx.activity.ktx)
    implementation(libs.androidx.appcompat)
    implementation(libs.androidx.constraintlayout)
    implementation(libs.androidx.core.ktx)
    implementation(libs.material)
}
```

### 2. `AndroidManifest.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <!-- Permissions required for network access and location -->
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />

    <application
        android:allowBackup="true"
        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="@xml/backup_rules"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.GoogleMaps">

        <!-- Google Maps API Key Meta-Data -->
        <meta-data
            android:name="com.google.android.geo.API_KEY"
            android:value="@string/google_maps_key" />

        <activity
            android:name=".MainActivity"
            android:exported="true"
            android:windowSoftInputMode="adjustResize">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```

### 3. `res/values/strings.xml`
```xml
<resources>
    <string name="app_name">Google Maps</string>
    <string name="google_maps_key" translatable="false">AIzaSyAcLzg4TrnZ68tREMuCG9eWFn4OHVmVPm4</string>
</resources>
```

### 4. `res/layout/activity_main.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/app-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <androidx.fragment.app.FragmentContainerView
        android:id="@+id/map"
        android:name="com.google.android.gms.maps.SupportMapFragment"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

### 5. `MainActivity.kt`
```kotlin
package com.example.googlemaps

import android.os.Bundle
import androidx.activity.enableEdgeToEdge
import androidx.appcompat.app.AppCompatActivity
import androidx.core.view.ViewCompat
import androidx.core.view.WindowInsetsCompat
import com.google.android.gms.maps.CameraUpdateFactory
import com.google.android.gms.maps.GoogleMap
import com.google.android.gms.maps.OnMapReadyCallback
import com.google.android.gms.maps.SupportMapFragment
import com.google.android.gms.maps.model.LatLng
import com.google.android.gms.maps.model.MarkerOptions

class MainActivity : AppCompatActivity(), OnMapReadyCallback {

    private lateinit var mMap: GoogleMap

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContentView(R.layout.activity_main)

        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main)) { v, insets ->
            val systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars())
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom)
            insets
        }

        // Initialize the map fragment asynchronously
        val mapFragment = supportFragmentManager
            .findFragmentById(R.id.map) as SupportMapFragment
        mapFragment.getMapAsync(this)
    }

    override fun onMapReady(googleMap: GoogleMap) {
        mMap = googleMap

        // Set default location (Sydney, Australia)
        val defaultLocation = LatLng(-33.8688, 151.2093)
        mMap.addMarker(
            MarkerOptions()
                .position(defaultLocation)
                .title("Marker in Sydney")
        )
        mMap.moveCamera(CameraUpdateFactory.newLatLngZoom(defaultLocation, 12f))
    }
}
```

---

## 📱 Expected Output & Results

1. When the app launches, the `SupportMapFragment` initializes and fetches map vector tiles from Google servers using the provided API key.
2. An interactive Google Map renders on the full screen.
3. The map centers on Sydney, Australia (`LatLng(-33.8688, 151.2093)`) with a zoom level of `12f`.
4. A red pin marker appears over Sydney with the title **"Marker in Sydney"**. Tapping the marker displays the title pop-up.

---

## ❓ Common Errors & Troubleshooting

| Issue | Cause | Solution |
| :--- | :--- | :--- |
| **Blank map / Gray grid with Google logo** | Invalid API Key or API not enabled in GCP Console | Ensure **Maps SDK for Android** is enabled under Google Cloud Console -> APIs & Services -> Library. |
| **Map not loading / No internet** | Missing internet permission or offline device | Verify `<uses-permission android:name="android.permission.INTERNET" />` is present in `AndroidManifest.xml`. |
| **`NullPointerException` on `mapFragment`** | Mismatched ID or wrong fragment type | Ensure `R.id.map` in `activity_main.xml` uses `com.google.android.gms.maps.SupportMapFragment`. |

---

## ✅ Result & Conclusion

The Android application to display Google Maps using the Google Maps API (Experiment 11) was developed successfully. The application renders an interactive Google Map, places location markers, and handles map camera positioning.





<img width="818" height="1599" alt="1" src="https://github.com/user-attachments/assets/e4313bf8-2c6b-4664-b8d2-eb66710d1d6a" />


<img width="702" height="1600" alt="2" src="https://github.com/user-attachments/assets/43eb7dad-09dd-44df-b6d9-93c76e44a368" />



