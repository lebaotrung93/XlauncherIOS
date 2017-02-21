# XlauncherIOS For Unity
## Get Started

Xlauncher SDK for iOS is the most simple way to intergrate user and payment to XCT system.Xlauncher SDK provide solution for payment such as: SMS, card, internet banking và Apple Payment.

## Steps to integrate SDK

    1. Convert Unity to IOS game/application.

    2. Setup Xlauncher SDK

    3. Config SDK - Payment function

    4. Xlauncher SDK flow


### 1. Convert Unity to IOS game/application

   Follow the video guide below
    
   [![How To Convert Unity to IOS game/application](http://img.youtube.com/vi/dZV1wjXS7QU/0.jpg)](http://www.youtube.com/watch?v=dZV1wjXS7QU "How To Convert Unity to IOS game/application")

### 2. Setup Xlauncher SDK

#### 2.1. Import Xlauncher.framework into project

    - Drag and drop Xlauncher.framework into your project.

    - Tick on checkbox: “Copy items into destination group's folder (if needed)”.

    - Embedded Binaries with SDK

    ![alt tag](https://github.com/xctcorporation/XlauncherIOS/blob/master/Images/addEmbled.png)

#### 2.2. Add url schemes

   - Add the following url schemes for Facebook(“fb” + facebook app id) and Google sign in (Reverse client id) from XlauncherConfig.plist file
    
    ![alt tag](https://github.com/xctcorporation/XlauncherIOS/blob/master/Images/addFbSchemes.png)

    - Add facebook app id, facebook display name and application queries scheme as below. Please replace app id and display name with the value in the XlauncherConfig.plist file 
     ![alt tag](https://github.com/xctcorporation/XlauncherIOS/blob/master/Images/addFbId.png)

    - Add file XlauncherConfig.plist to your root project

#### 2.3. Coding
- Import SDK: #import <XLauncher/XLauncher.h> into AppDelegate.m
- Add these lines of code in Application didFinishLaunchingWithOptions function in AppDelegate class, after window setup. You can get Google Signin client ID in the XlauncherConfig.plist.

```
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {

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
            [launcher handleLogoutWithCompletion:^{ 
	    		//do something
	    }];
    
            [launcher setDomainDebug:NO]; // if you want to build in the TEST mode, pass it to TRUE

            NSDictionary *dict = @{kParamApplication: ATNonNilObject(application), kParamOptions: ATNonNilObject(launchOptions)}; 

            ATDispatchEvent(Event_AppDidFinishLaunching, dict);    

            return YES;
            }

```

- Handle callback: There are two callback functions you can handle including login success and logout success. You may use these data to call login or logout to your server
