
#Use the Zzish launcher

To improve the student experience in the Zzish Learning Hub, you can provide a hook so that your app can be directly launched from the student's planner.
 
###**STEP 1 - Create a Custom Intent**

In your Android Manifest, create a Custom Intent

```
<activity android:name=".SecondActivity_" android:label="@string/title_activity_main" >
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data android:scheme="zzish" android:host="appnamehost" />
    </intent-filter>
</activity>
```

###**STEP 2: Update your Zzish App Details**

In the Developer console, enter the scheme with a colon and two slashes (e.g. zzishmycoolapp://) into the "iOS Launch Url" of your app. 
 
###**STEP 3 - Request User from Token**

Within the activity specified in your intent, in the `onCreate` method, add the following

```
zzish = zzishFactory.initWithToken(this, "API KEY", getIntent(), new ZZCallback() {
    @Override
    public void processResponse(int status, Object message) {
        user = (ZZUser) message;
    }
});
```

This will instantiate your Zzish instance and set up the user.
