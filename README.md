# BeeSwift
Add Python to a SwiftUI application with the BeeWare Briefcase

## Setup Virtual Environment

```
python3 -m venv venv
source venv/bin/activate
pip install sympy
python -m pip install briefcase
```

## Start a New Project

```
briefcase new
```

Responses ...

```
Formal Name [Hello World]: Bee Swift
App Name [beeswift]:
Bundle Identifier [com.example]:
Project Name [Bee Swift]: 
Description [My first application]: Add Python to a SwiftUI application with the BeeWare Briefcase
Author [Jane Developer]:
Author's Email [jane@example.com]:
Application URL [[https://example.com/beeswift]:
Project License [1]:
GUI Framework [1]:
```

## Set Python Requirements

```
cd beeswift
vim pyproject.toml
```

Type `a` to append text, then scroll to the line that containes the requirements for the iOS app.
Specifically make sure this is under the `# Mobile deployments` line, and is preceeded by `[tool.briefcase.app.beeswift.iOS]`.
Add `sympy` and the version number to the `requires = ... ` section.

```
# Mobile deployments
[tool.briefcase.app.beeswift.iOS]
requires = [
    'toga-iOS>=0.3.0.dev34',
    'std-nslog~=1.0.0',
    'sympy==1.11.1'
]
```
Press the `esc` button and then type `:wq` and `enter` to write and quit.


## Create the iOS Project

```
briefcase create iOS
```

## (Optional) Add PythonKit to theiOS Project

Start XCode, and open the project at `iOS/XCode/Bee Swift/Bee Swift.xcodeproj`.
Go to `File -> Add Package`s, click GitHub on the left hand side, click the plus button on the lower left.
In the search field in the upper right, enter the following

```
https://github.com/pvieito/PythonKit.git
```

Click `Add Package` in the lower right corner.


## Create Objective C AppDelegate Header

In the tree on the left hand side, right click `Supporting Files` and then `New File`.
Select `Header File` in the menu, then click `Next`.
Enter the name `AppDelegate` then click `Create`, without modifying any defaults.
Under the commented header section, delete any uncommented code, and replace with the following.

```objc
#import <UIKit/UIKit.h>

@interface AppDelegate : UIResponder <UIApplicationDelegate>

@property (strong, nonatomic) UIWindow *window;


@end
```

## Create Objective C AppDelegate Class

In the tree on the left hand side, right click `Supporting Files` and then `New File`.
Select `Objective C File` in the menu, then click `Next`.
Enter the name `AppDelegate` then click `Next`, and finally `Create` on the next window, without modifying any defaults.

```objc
#import "AppDelegate.h"
#include <Python.h>
#import "Bee_Swift-Swift.h"

@interface AppDelegate ()

@end

@implementation AppDelegate

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    [self swiftuiExtension];
    return YES;
}

@end
```

## Create Swift AppDelegate Extension

In the tree on the left hand side, right click `beeswift` and then `New File`.
Select `Swift File` in the menu, then click `Next`.
Enter the name `AppDelegateExtension` then click `Create`, without modifying any defaults.
On the subsequent popup, click the option to `Create Bridging Header`.
Inside the newly created `Bee Swift-Bridging-Header.h` file add the following.

```
#include <Python.h>
#include <dlfcn.h>
#include "AppDelegate.h"
```

Inside the new `AppDelegateExtension.swift` file, add the following.

```swift
import Foundation
import SwiftUI
import UIKit
import PythonKit

@objc public extension AppDelegate {
    @objc func swiftuiExtension() {
        // Do some Python stuff
        let math = Python.import("math")
        let pi = math.pi
        let piStr = "pi = \(pi)"
        let answer = "sin(1) = \(math.sin(1))"

        // Do some sympy stuff
        let sympy = Python.import("sympy")
        let a = sympy.symbols("a")
        let mechanics = Python.import("sympy.physics.mechanics")
        let b = mechanics.dynamicsymbols("b")
        let ab = a*b
        let abStr = "x = \(ab)"
        
        // Build a SwiftUI
        self.window = UIWindow(frame: UIScreen.main.bounds)
        window.makeKeyAndVisible()
        window.rootViewController = UIHostingController(
            rootView: VStack {
                Text(piStr)
                Text(answer)
                Text(abStr)
            }
        )
    }
}
```

## Update the Objective C Main File

Modify the call to `UIApplicationMain` in the `main.m` script, replacing `@PythonAppDelegate` with `@AppDelegate`.

```objc
UIApplicationMain(argc, argv, nil, @"AppDelegate");
```

## Run the app

![Result Displayed on iPhone](img/iphoneGraphic.png)

