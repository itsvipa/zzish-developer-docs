
#Using the Zzish Android SDK

###**STEP 1: Download the SDK**

*[Click here to download the SDK](https://github.com/Zzish/zzishsdk-android/raw/master/dist/zzish-release.aar) and complete the following instructions:*

1. Right click on your application and select "Open Module Settings"
2. Click "+" in top left hand corner of the dialog.
3. Next select "Import JAR/ARR package"
4. Select the downloaded SDK.
5. Now reselect your app in project structure and go to dependencies.
6. Click "+" the bottom of the dialog and select "module dependency" 
7. Select "zzish-release".

*The Zzish SDK also requires that you install GSON as an extra dependency. In the same window as you added zzish-release as a dependency.*

1. Click "+" at the bottom of the dialog and select "library dependency"
2. Select "GSON"

###**STEP 2: Update your configuration**

Enable network access in you Android Manifest

```
<uses-permission android:name=“android.permission.ACCESS_NETWORK_STATE” />

```

###**STEP 3: Initialize the SDK**

1. Create a `ZzishService` (with the appropriate import statements) in the Activities/Fragments that you would like to use the Zzish Service.
```
private ZzishService zzish;
```
2. In your Main Activity, Initialise your Zzish instance by calling the following method using the API key you received when you created your app with Zzish (see [developer console](https://developer.zzish.com)):

```
zzish = ZzishFactory.getInstance(activity, "API_KEY", new ZZCallback() {
        @Override
        public void processResponse(int status, Object message) {
            if (status== Zzish.STATUS_OK) {

            }
            else {
            }
        }
    });
```