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