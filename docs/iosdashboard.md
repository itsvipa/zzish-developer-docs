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
