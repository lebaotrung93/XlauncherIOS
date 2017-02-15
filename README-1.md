# XlauncherIOS For Unity
Get Started

Xlauncher SDK for iOS is the most simple way to intergrate user and payment to XCT system.Xlauncher SDK provide solution for payment such as: SMS, card, internet banking và Apple Payment.

Steps to integrate SDK

    1. Convert Unity to IOS game/application.

    2. Setup Xlauncher SDK

    3. Config SDK - Payment function

    4. Xlauncher SDK flow


1. Convert Unity to IOS game/application

    Follow 1-iOS-sdk-export video guide in: https://drive.google.com/drive/u/0/folders/0B9DX-vYf8HJPT3pxMmNSS3g5UEU

2. Setup Xlauncher SDK

        1.1. Import Xlauncher.framework into project

    - Drag and drop Xlauncher.framework into your project.

    - Tick on checkbox: “Copy items into destination group's folder (if needed)”.

    - Embedded Binaries with SDK

    ![alt tag](https://github.com/xctcorporation/XlauncherIOS/blob/master/Images/addEmbled.png)

        1.2. Add url schemes

    - Add the following url schemes for Facebook(“fb” + facebook app id) and Google sign in (Reverse client id) from XlauncherConfig.plist file
    
    ![alt tag](https://github.com/xctcorporation/XlauncherIOS/blob/master/Images/addFbSchemes.png)

    - Add facebook app id, facebook display name and application queries scheme as below. Please replace app id and display name with the value in the XlauncherConfig.plist file 
     ![alt tag](https://github.com/xctcorporation/XlauncherIOS/blob/master/Images/addFbId.png)

    - Add file XlauncherConfig.plist to your root project


          1.3. Coding

             - Import SDK : #import <XLauncher/XLauncher.h> into AppDelegate.m

    - Add these lines of code in Application didFinishLaunchingWithOptions function in AppDelegate class, after window setup and before return line. You can get Google Signin client ID in the XlauncherConfig.plist. Handle callback : There are two callback functions you can handle including: login success and logout success. You may use these data to call login or logout to your server

            (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    
            // Project configure
    
            XLauncher *launcher = [XLauncher getInstance];
    
            [launcher setupWithWindow:self.window];
            
            // Handle login callback

            [launcher handleLoginWithCompletion:^(NSDictionary *data) { 

            NSString *userID = data[kParamUserID];

            NSString *userName = data[kParamUserName];

            NSString *accessToken = data[kParamAccessToken]; 

            }]; 
            
            // Handle logout callback

            [launcher handleLogoutWithCompletion:^{ }];
    
            [launcher setDomainDebug:NO]; // if you want to build in the TEST mode, pass it to TRUE

            NSDictionary *dict = @{kParamApplication: ATNonNilObject(application), kParamOptions: ATNonNilObject(launchOptions)}; 

            ATDispatchEvent(Event_AppDidFinishLaunching, dict);    

            return YES;
            }
            
    - Add function handle Facebook schemes 

            - (BOOL)application:(UIApplication *)application openURL:(NSURL *)url sourceApplication:(NSString *)
            sourceApplication annotation:(id)annotation { 

            NSDictionary *dict = @{kParamApplication: ATNonNilObject(application), kParamUrl: ATNonNilObject(url), 
            kParamSourceApplication: ATNonNilObject(sourceApplication), kParamAnnotation: ATNonNilObject(annotation)}; 

            ATDispatchEvent(Event_AppOpenUrl, dict); 

            return YES; 
            }

    - This example code is apply for landscape mode. Base on your game orientation, if your game support both portrait and landscape then you must replace UIInterfaceOrientationMaskLandscape with UIInterfaceOrientationMaskAll, if you game is only support portrait mode, then you don’t need to add this function

            - (UIInterfaceOrientationMask)application:(UIApplication *)application supportedInterfaceOrientationsForWindow:(UIWindow *)window
            { 

            if ([[XLauncher getInstance] isScreenRotateToPortrait]) { 

            return UIInterfaceOrientationMaskPortrait; 

            } else 	
            
            return UIInterfaceOrientationMaskLandscape; 
            
            } 


    - Public functions

        Here is the list of public functions you can call to customize the Xlauncher in your game: 
        - setLauncherStickySide: You can specific the side that launcher can stick to via the or 
        bitwise. Ex: ATButtonStickySideTop | ATButtonStickySideBottom 
        - silentLogin: When open the app, maybe user is already logged in. Call this function to check if user is logged in or not, if not, you must call showLoginScreen function to show the login screen. 
        
                if([[Xlauncher getInstance] silentLogin])
                
                    // Move direct to game
                        
                else {
                
                    // Show login screen
                    
                    [[Xlauncher getInstance] showLoginScreen];
            
                }
        - showButtonLauncherWithAnimation 
        - hideButtonLauncherWithAnimation
        - showLoginScreen: Show the login screen, if user not logged in yet
        - showPaymentScreen: You may want to show payment screen from your game

3. Implement payment extra data

    Payment extra data (PED) is the data you send to game server when user make payment success. For example: if your game have multiple servers or multiple characters, you may want to send this data to game server, so its will know which character get the gold. The format is defined on your demand. 
    
    Note*: 
    It must be unique string
    Maximum is 50 characters
    There's no special character in string

    To implement PED, you create a class that implement PaymentExtraDataProtocol with function

        - (NSString *)getPaymentExtraData { 

        return @“server12-id1001”; // ex: server 12, userid = 1001

        }

    Then you create an PaymentExtraDataImp object and set it to XLauncher

          [launcher setPaymentExtraDataObject:[PaymentExtraDataImp new]];

    
4. Flow

- Login flow : 
![alt tag](https://github.com/xctcorporation/XlauncherIOS/blob/master/Images/loginFlow.png)

- Payment flow :
![alt tag](https://github.com/xctcorporation/XlauncherIOS/blob/master/Images/PaymentFlow.png)