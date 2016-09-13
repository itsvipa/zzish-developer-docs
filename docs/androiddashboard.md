
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
