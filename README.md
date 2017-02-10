# XlauncherIOS
Get Started

Xlauncher SDK for iOS is the most simple way to intergrate user and payment to XCT system.Xlauncher SDK provide solution for payment such as: SMS, card, internet banking và Apple Payment.

Steps to integrate SDK

1. Setup Xlauncher SDK

2. Config SDK - Payment function

3. Xlauncher SDK flow



1. Setup Appota SDK

1.1. Import Xlauncher.framework into project

- Drag and drop Xlauncher.framework into your project.

- Tick on checkbox: “Copy items into destination group's folder (if needed)”.

- Embedded Binaries with SDK

![alt tag](https://github.com/xctcorporation/XlauncherIOS/blob/master/Images/addEmbled.png)

1.2. Add url schemes

- Add the following url schemes for Facebook(“fb” + facebook app id) and Google sign in (Reverse client id)

![alt tag](https://github.com/xctcorporation/XlauncherIOS/blob/master/Images/addFbSchemes.png)

- Add facebook app id, facebook display name and application queries scheme as below. Please replace app id and display name with the value in the config file

![alt tag](https://github.com/xctcorporation/XlauncherIOS/blob/master/Images/addFbId.png)

- Add file XlauncherConfig.plist to your root project


1.3. Coding

- Import SDK : #import <XLauncher/XLauncher.h> 

- Add these lines of code in Application didFinishLaunchingWithOptions function, after window setup and before return line. You can get Google Signin client ID in the config file. 

    XLauncher *launcher = [XLauncher getInstance];

    [launcher setupWithWindow:self.window];

    [launcher setDomainDebug:NO]; // if you want to build in the TEST mode, pass it to TRUE

    ...

    return YES; 

- Add these lines of code in Application didFinishLaunchingWithOptions before return

    NSDictionary *dict = @{kParamApplication: ATNonNilObject(application), kParamOptions: ATNonNilObject(launchOptions)}; 

    ATDispatchEvent(Event_AppDidFinishLaunching, dict);

- Add fucntion handle facebook schemes 

    - (BOOL)application:(UIApplication *)application openURL:(NSURL *)url sourceApplication:(NSString *)
    sourceApplication annotation:(id)annotation { 
    NSDictionary *dict = @{kParamApplication: ATNonNilObject(application), kParamUrl: ATNonNilObject(url), 
    kParamSourceApplication: ATNonNilObject(sourceApplication), kParamAnnotation: ATNonNilObject(annotation)}; 
    ATDispatchEvent(Event_AppOpenUrl, dict); 
    return YES; }

- This example code is apply for landscape mode. Base on your game orientation, if your game support both portrait and landscape then you must replace UIInterfaceOrientationMaskLandscape with UIInterfaceOrientationMaskAll, if you game is only support portrait mode, then you don’t need to add this function

    - (UIInterfaceOrientationMask)application:(UIApplication *)application supportedInterfaceOrientationsForWindow:(UIWindow *)window
    { 

    if ([[XLauncher getInstance] isScreenRotateToPortrait]) { 

    return UIInterfaceOrientationMaskPortrait; 

    } 

    else 	return UIInterfaceOrientationMaskLandscape; } 

- Handle callback : There are two callback functions you can handle including: login success and logout success. You may use these data to call login or logout to your server

    [launcher handleLoginWithCompletion:^(NSDictionary *data) { 

    NSString *userID = data[kParamUserID];

    NSString *userName = data[kParamUserName];

    NSString *accessToken = data[kParamAccessToken]; 

    }]; 

    [launcher handleLogoutWithCompletion:^{ }]; 

- Public functions

    Here is the list of public functions you can call to customize the launcher in your game: 
    - setLauncherStickySide: You can specific the side that launcher can stick to via the or 
    bitwise. Ex: ATButtonStickySideTop | ATButtonStickySideBottom 
    - silentLogin: When open the app, maybe user is already logged in. Call this function to check if user is logged in or not, if not, you must call showLoginScreen function to show the login screen. 
    * return false if user not logged in yet

    * return true if user already logged in, the callback will call later in 
    - handleLoginWithCompletion function. Keep in mind this process is async, cause we must verify and the get the newest access token from the server. 
    - showButtonLauncherWithAnimation 
    - hideButtonLauncherWithAnimation
    - showLoginScreen: Show the login screen, if user not logged in yet
    - showPaymentScreen: You may want to show payment screen from your game

2. Implement payment extra data

    Payment extra data(PED) is the data you send to game server when user make payment success. For example: if your game have multiple servers or multiple characters, you may want to send this data to game server, so its will know which character get the gold. The format is defined on your demand. 

    To implement PED, you create a class that implement PaymentExtraDataProtocol with function

    - (NSString *)getPaymentExtraData { 

    return @“server12-id1001”; // ex: server 12, userid = 1001

    }

    Then you create an PaymentExtraDataImp object and set it to XLauncher

    [launcher setPaymentExtraDataObject:[PaymentExtraDataImp new]];

    
3. Flow

- Login flow : 

![alt tag](https://github.com/xctcorporation/XlauncherIOS/blob/master/Images/PaymentFlow.png)

- Payment flow :

![alt tag](https://github.com/xctcorporation/XlauncherIOS/blob/master/Images/loginFlow.png)
