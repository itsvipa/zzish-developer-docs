#Using the Zzish SDK

###**STEP 1: Download the SDK**  
  
  
  
#####*Option 1: CocoaPods*  
  
Include the "ZzishSDK" pod in your POD file.

#####*Option 2: Source Files* 
  
You can just download and include our source code in your project. You can find the code at https://github.com/Zzish/zzishsdk-ios/tree/master/Pod/Classes. Create a Group in your project and then drag the files over into that group. (Optional) You'll also need to download Reachability and drag that into your folder. You can find the files  [here](https://developer.apple.com/library/ios/samplecode/Reachability/Introduction/Intro.html).
 
###**STEP 2: Installing the SDK**
 

1. Import the Zzish.h header file

```
#import <ZzishSDK/ZzishSDK.h>
```

2. Within your `applicationDidFinishLaunching` method in your application's delegate, simply call the following method using the API key you received when you registered your app with Zzish (see [developer console](https://developer.zzish.com/):

```
[Zzish startWithApplicationId:@"API_KEY" withBlock:^(NSDictionary *response) {
NSLog(@"Response after initialising %@",response);
}];
```

3. Within your `applicationWillTerminate` method in your application's delegate, simply call the following method.

```
[Zzish stop];
```


#Using the Zzish Login inside your app

Zzish provides a login mechanism so that you don't need to create a user management system. Simply allow your users to log in with Zzish where they can use an email/password, Google or Office 365.
 
###**STEP 1 - Create a Zzish Login Button**
Create a `UIButton` using your preferred method (e.g  programatically or on a storyboard. Add the `UIWebViewDelegate` to the Controller
 
###**STEP 2 - Configure Zzish hooks**
In the Controller that you are working in, do the following
 
1. Call the Zzish Login (on the Click event of your button)
```
[Zzish loginWithZzish:self WithBlock:^(NSDictionary *response) {
        NSLog(@"USER UUID %@", response[@"payload"][@"uuid"]);
        NSLog(@"GROUP CODE %@", response[@"payload"][@"attributes"][@"groupCode"]);
    }];
```
2. Add the WebView callback so we can ensure the WebView closes properly
```
#pragma mark - Optional UIWebViewDelegate delegate methods
- (BOOL)webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType
{
    return [Zzish webView:webView shouldStartLoadWithRequest:request navigationType:navigationType];
}
```

The two key pieces of information that you get from the response callback is the user's uuid and the group code. These are the two key pieces of information that you need to send to us when sending data.

#Use the Zzish Launcher

To improve the student experience in the Zzish Learning Hub, you can provide a hook so that your app can be directly launched from the student's planner.
 
###**STEP 1 - Set up a Scheme**
1. Open up your Info.plist file
2. Right click on the list of properties and click "Add Row". 
3. Select URL Types for the new item. 
4. Click the grey arrow next to "URL Types" to show "Item 0". 
5. Set your URL identifier to a unique string - something like "zzishmycoolapp"
 
###**STEP 2 - Update your Zzish App Details**
In the Developer console, enter the scheme with a colon and two slashes (e.g. zzishmycoolapp://) into the "iOS Launch Url" of your app. 
 
###**STEP 3 - Add code to process the app when launched**
Add the following code to your application delegate
```
- (BOOL)application:(UIApplication *)application handleOpenURL:(NSURL *)url {
    [Zzish loadUserByTokenUrl:url WithBlock:^(NSDictionary *response) {
        NSLog(@"USER UUID %@", response[@"payload"][@"uuid"]);
        NSLog(@"GROUP CODE %@", response[@"payload"][@"attributes"][@"groupCode"]);
}];
}
```

#Sending data to Zzish Dashboards
You can configure your app to send student Data to the Zzish Dashboard.
 
###**STEP 1 - Creating a Zzish User (OPTIONAL)**
You do not need this step if you use the Zzish Login or Zzish Launcher option. Both these methods will give the user uuid (as shown above). If you have your own login mechanism, you can attach a Zzish user account to your user.
```
ZZUser* user = [Zzish user:USER_ID];
user.name = @"Ed";
[user save];
```
The `USER_ID` field is YOUR unique id to represent the user. There is an optional `saveWithBlock` method if you need to wait for the response. This response will need to be successful before you can start tracking user data.
 
###**STEP 2 - Request a Group Code from the user**
A group code is a unique code representing a class on the Zzish Learning Hub. You do not need this step if you use the Zzish Login or Zzish Launcher option. Both these methods will give the group code (as shown above). Otherwise you can request it yourself from the user. We provide a simple validation process
```
[Zzish validateClassCode:@"GROUP CODE" withBlock:^(NSDictionary *response) {
    //result[@"status"] will be 200 if it's valid, 400 if not
}];
```
###**STEP 3 - Create an activity for the user**
A Zzish activity represents an event (e.g. Quiz) with a duration which contains a series of interactions (e.g. Questions). 
```
ZZActivity* activity = [user createActivity:@"UNIQUE NAME OF ACTIVITY"];
activity.groupCode = @"GROUP_CODE"
[activity startWithBlock:^(NSDictionary *response) {
    NSLog(@"Response after starting %@",response);
}];
```
###**STEP 4 - Log User Actions**
```
ZZAction* action = [activity createAction:@"What is 3*3"];
action.response=@"8";
[action score:84];
[action correct:false];
[action saveWithBlock:^(NSDictionary *response) {
    NSLog(@"Saved action with result %@",response);
}];
```
###**STEP 5 - Stop or Cancel the Activity**
Once the user has finished or the user arbitrarily cancels the activity, you can user the following two methods.
```
[activity stopWithBlock:^(NSDictionary *response) {
    NSLog(@"Response after stopping %@",response);
}];
[activity cancelWithBlock:^(NSDictionary *response) {
    NSLog(@"Response after stopping %@",response);
}];
```
