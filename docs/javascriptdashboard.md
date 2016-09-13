
#Sending data to the Zzish Dashboards

You can configure your app to send student Data to the Zzish Dashboard.
 
###**STEP 1 - Creating a Zzish User (OPTIONAL)**

You do not need this step if you use the Zzish Login or Zzish Launcher option. Both these methods will give the user uuid (as shown above). If you have your own login mechanism, you can attach a Zzish user account to your user.
```
var user = Zzish.saveUser(USER_ID, "Ed", function(err, resp) {
    //user has been saved
});
```

The `USER_ID` field is YOUR unique id to represent the user. This response will need to be successful before you can start tracking user data.

###**STEP 2 - Request a Group Code from the user.**
 
A group code is a unique code representing a class on the Zzish Learning Hub. You do not need this step if you use the Zzish Login or Zzish Launcher option. Both these methods will give the group code (as shown above). Otherwise you can request it yourself from the user. We provide a simple validation process

```
Zzish.validateClassCode("12345", function(err, resp) {
    if (!err) {
        //valid unverified class code
    }
});
```

###**STEP 3 - Create an activity for the user**

A Zzish activity represents an event (e.g. Quiz) with a duration which contains a series of interactions (e.g. Questions). 

```
Zzish.startActivity(userId, "UNIQUE NAME OF ACTIVITY", groupCode, function(err, resp) {

});
```

###**STEP 4 - Log User Actions**

```
var parameters = {
    definition: {
        "type": "UNIQUE ACTION ID",
        "name": "What is 3*3"
    },
    result: {
        response: "5" //String
        score: 100.0 //float
        correct: true //boolean
        duration: 3000 //in milliseonds
        attempts: 3; //number of attempts 
    }
}
Zzish.logActionWithObjects(activityId, parameters, callback) {
});
```

###**STEP 5 - Stop or Cancel the Activity**

Once the user has finished or the user arbitrarily cancels the activity, you can user the following two methods. You can get the `activityId` from the `startActivity` method.
```
Zzish.stopActivity(activityId, {}, function(err, resp) {
});
Zzish.cancelActivity(activityId, function(err, resp) {
});
```
