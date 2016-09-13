
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