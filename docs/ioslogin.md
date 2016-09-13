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

