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

#Using the Zzish Login inside your app

###**STEP 1: Create a Zzish Login Button**

Create a Button using your preferred method (e.g  programatically or graphically).

###**STEP 2 - Configure Zzish hooks**

In the Activity that you are working in, on the `buttonClick` event of the button you created, add the following code:
 
 ```
if(zzish != null){
    zzish.zzishLogin(new ZZCallback() {
@Override
    public void processResponse(int status, Object token) {
        if(status == Zzish.STATUS_OK){
            WebView webView = zzish.initWebView(context, new ZZCallback() {

                @Override
                public void processResponse(int status, Object message) {
                    ZZUser user = (ZZUser) message;
                }    
            });                        
        }
    }
});
}
```
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

#Sending data to the Zzish dashboards
You can configure your app to send student Data to the Zzish Dashboard.

###**STEP 1 - Creating a Zzish User (OPTIONAL)**
You do not need this step if you use the Zzish Login or Zzish Launcher option. Both these methods will give the user uuid (as shown above). If you have your own login mechanism, you can attach a Zzish user account to your user.

```
ZZUser user = Zzish.getUser("USER_ID");
user.setName("Charles");
user.save(new ZZCallback() {
    @Override
    public void processResponse(int status, Object message) {
        //TODO
    }
});
```

The `USER_ID` field is YOUR unique id to represent the user. There is an optional `saveWithBlock` method if you need to wait for the response. This response will need to be successful before you can start tracking user data.
 
###**STEP 2 - Request a Group Code from the user.**

A group code is a unique code representing a class on the Zzish Learning Hub. You do not need this step if you use the Zzish Login or Zzish Launcher option. Both these methods will give the group code (as shown above). Otherwise you can request it yourself from the user. We provide a simple validation process

```
zzish.validateClassCode("GROUP_CODE", new ZZCallback() {
    @Override
    public void processResponse(int status, Object message) {
        //TODO
    }
});
```

###**STEP 3 - Create an activity for the user**

A Zzish activity represents an event (e.g. Quiz) with a duration which contains a series of interactions (e.g. Questions).

```
ZZActivity activity = user.createActivity("UNIQUE ACTIVITY NAME");
activity.setGroupCode("GROUPCODE");
activity.start(new ZZCallback() {
    @Override
    public void processResponse(int status, Object message) {
        if (status==Zzish.STATUS_OK) {
            //message will potentially be an object returned by the server
        }
        else {
            //message will be a string with error message
        }
    }
});
```

###**STEP 4 - Log User Actions**

```
ZZActiion action = activity.createAction("What is 3*3");
action.setResponse("8");
action.setScore(84F);
action.setCorrect(false);
action.save(new ZZCallback() {
    @Override
    public void processResponse(int status, Object message) {
        if (status==Zzish.STATUS_OK) {
            //message will potentially be an object returned by the server
        }
        else {
            //message will be a string with error message
        }
    }
});
```

###**STEP 5 - Stop or Cancel the Activity**

Once the user has finished or the user arbitrarily cancels the activity, you can user the following two methods.

```
activity.stop(new ZZCallback() {
    @Override
    public void processResponse(int status, Object message) {
        if (status==Zzish.STATUS_OK) {
            //message will potentially be an object returned by the server
        }
        else {
            //message will be a string with error message
        }
    }
});
activity.cancel(new ZZCallback() {
    @Override
    public void processResponse(int status, Object message) {
        if (status==Zzish.STATUS_OK) {
            //message will potentially be an object returned by the server
        }
        else {
            //message will be a string with error message
        }
    }
});
```
