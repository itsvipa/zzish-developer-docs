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