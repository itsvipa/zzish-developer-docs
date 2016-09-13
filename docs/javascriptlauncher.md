
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