# Guide 

- AotterTrek Android SDK contains trackiing and native advertising service.
- minSdkVersion 16 or later

# Install SDK
In order to use that repository, you need to reference it in the app's project-level build.gradle file. Open yours and look for an allprojects section:

Example project-level build.gradle (excerpt)
<pre>
allprojects {
    repositories {
        google()
        jcenter()
        <b>maven {
          <span style="color:#9ccc65">url "https://deps.aotter.net/artifactory/libs-release-local"</span>
        }</b>
    }
}
</pre>

Add the maven directive

Example app-level build.gradle (excerpt)
<pre>
dependencies {
    implementation fileTree(dir: 'libs', include: ['*.jar'])
    <b>implementation <span style="color:#9ccc65">'com.google.android.gms:play-services-ads:18.1.1'</span>
    implementation <span style="color:#9ccc65">'com.aotter.net:trek-sdk:3.1.0_rc2'</span></b>
}
</pre>

pull in the latest version of the AotterTrek Android SDK and additional related dependencies

# Update your AndroidManifest.xml and Application

Declare the following permissions:
```java
<uses-permission android:name="android.permission.INTERNET"/>
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>
```

```java
//If you are shipping an app, extend the Application class if you are not already doing so:

public class MyApplication extends Application {
    @Override
    public void onCreate() {
        super.onCreate();

        // init AotterTrek
        AotterTrek.initTrekService(this, YOUR_CLIENT_ID, YOUR_CLIENT_SECRET);
    }
}

```

- test key
  - CLIENT_ID : `DNgNhOwfbUkOqcQFI+uD`
  - CLIENT_SECRET : `1k+sYKMLZrclCRmgw/esYNZbjAhArT7Vn42cxfn3f/tgmT0XJZI4mNiNwBYLu9GOet7YtiT6`
