Note for Unity implement

- Guide convert Unity project to IOS : https://drive.google.com/file/d/0B9DX-vYf8HJPNDNvLVo3Q3djckU/view?usp=sharing

- Follow the standard guide.
[a link](https://github.com/xctcorporation/XlauncherIOS/blob/master/README.md)

- Add these lines of code in StartUnity function After [self createDisplayLink]; 
	
- Delete the codes below in applicationdidloadfinish(). 

            XLauncher *launcher = [XLauncher getInstance];

            [launcher setupWithWindow:self.window];

            [launcher setDomainDebug:NO]; 

            return YES;

	
