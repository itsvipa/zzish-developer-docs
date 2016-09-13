#Using the Zzish Javascript SDK

###**STEP 1 - Download and Install the SDK**

```
<script src="https://zzish.github.io/zzishsdk-js/zzish.js">
    <script>
        Zzish.init("API_KEY");
        window.onbeforeunload = function (e) {
            Zzish.stop();
        };        
    </script>
```
Use the API KEY you received when you registered your app with Zzish (see developer console):

#Using the Zzish login inside your app

Zzish provides a login mechanism so that you don't need to create a user management system. Simply allow your users to log in with Zzish where they can use an email/password, Google or Office 365.
 
###**STEP 1 - Create a Zzish Login Button**

Create a button to allow the user to log in using Zzish.
 
###**STEP 2 - Configure Zzish hooks to login**

```
Zzish.login("redirect", {redirectURL: REDIRECT_URL});
```

The `REDIRECT_URL` is the URL where you would like the Zzish user to be returned once they have completed the login. 
 
###**STEP 3 - Configure Zzish hooks to process login**

In the page where the user is returned, add the following code to get the user information.
```
Zzish.getCurrentUser(token, function(err, response) {
    if (!err) {
        var userId = response.uuid;
        var groupCode = response.attributes.groupCode;
    }
}
```

The two key pieces of information that you get from the response callback is the user's uuid and the group code. These are the two key pieces of information that you need to send to us when sending data.

#Use the Zzish Launcher

To improve the student experience in the Zzish Learning Hub, you can provide a hook so that your app can be directly launched from the student's planner.

###**STEP 1 - Update your Zzish App Details**

In the Developer console, enter the URL where you would like the user to be sent to when they launch your app from the Zzish Learning hub.
 
###**STEP 2 - Add code to process the app when launched**

Add the following code to process the login.

```
Zzish.getCurrentUser(token, function(err, response) {
    if (!err) {
        var userId = response.uuid;
        var groupCode = response.attributes.groupCode;
    }
}
```

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
