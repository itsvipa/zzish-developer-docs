
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
