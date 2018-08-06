# Config-Multi-Environments
## Manage different environments and configurations for iOS projects
![](https://cdn-images-1.medium.com/max/1600/1*qZuI2QUxoEvSlLqrqO7UPA.png)
As iOS developers, we are already aware of managing different environments like Development, QA, Beta, and Production. For these different environments, there are different server URLs, app icons, and configurations.
So before creating a new build pointing to an environment, we need to keep in mind that we also have to change the server URL. We could do this by changing some hardcoded flag value in the constant file or using macros, but it makes everything more complicated.
But if we think for a little while, we can come up with an idea. And by applying this idea, we can easily handle any scenario. So the idea is, if we create different schemas and configurations, then it allows us to change the application server URLs, App icon, Plist file, and configuration.
In this tutorial, I will show you how to manage different environments using schemas and configuration.
These are the steps:
### Project Setup:
Open XCode and create a new single view application with a proper name.
### Add Schema and Configurations:
Before adding a schema, we need to know that every XCode schema comes with two different build configurations: Debug and Release. Then if we want, we can make changes specific to a particular build configuration.
Now to add our build configurations, select the project in the Project Navigator pane on the left. Then select Info from the two options (Info and Build Settings). In the Configurations, we have to add our own configuration for the five environments (Development, Production, QA, Beta, and UAT) there.
![](https://cdn-images-1.medium.com/max/1600/1*5IHSpl7Pm7qdzjJ0VXpO9w.png)
First of all, double click on Debug and rename it as Debug (Development). Similarly, double click on Release and rename it as Release (Development). Now click +, and select Duplicate Debug (Development) and Duplicate Release (Development), then change the duplicate environment name with the others available names.
![](https://cdn-images-1.medium.com/max/1600/1*ytKEoM7Fp-pi6eXdVde8XA.png)
For the schema creation, go to manage schema in top left corner of XCode. There you can see that one schema is already available. Rename it as Development — or you can delete the existing and add a new one with the name Development. Then add the rest of the four schemas for the other environments.
![](https://cdn-images-1.medium.com/max/1600/1*JVJ5eY0P-wkS2ViI5rj4-w.png)
Oops, don’t forget to check the shared box there. After adding all the schemas, the manage schema screen should look like this:
![](https://cdn-images-1.medium.com/max/1600/1*v_rGZmAYjZe_p4R6usHwTg.png)
### Add Configuration Settings File:
Right click on Project, select new file, then add Configuration Settings File and give it the same name as the environment.
![](https://cdn-images-1.medium.com/max/1600/1*EWfTg9WN09lbX8v-KcHDQw.png)
After adding all the config files, your Project Navigator left pane should look like this:
![](https://cdn-images-1.medium.com/max/1600/1*53B3UEp-ZMzRvQdj1EEL-w.png)
Now the most important part starts: add your server URL and other customized key value in the corresponding configuration file.
![](https://cdn-images-1.medium.com/max/1600/1*xECXRXHZGrqvL9ijPLgGiQ.png)
### Add Plist Files:
Rename the info.plist file as development.plist. Copy and paste the same plist file for the different environments inside the project, and rename each of the plist file with the environment’s name. You can set some environment-specific keys and values in the plist files. After that, add the keys from the configuration file to the plist files like this:
![](https://cdn-images-1.medium.com/max/1600/1*hMBtu6KdmmIgybiF53dSOw.png)
Now we have to set the appropriate plist path for each build configuration. From Targets, just select a plist file, and rename it with the same name for the Debug and Release configuration.
![](https://cdn-images-1.medium.com/max/1600/1*eFuPiN80FOC4Y3Ea4G2N1Q.png)
### Linking Build Configuration with the Configuration File:
Select all the build configurations (Debug and Release) in Projects Info one by one. Then set the appropriate configuration file, which you have added to the project.
![](https://cdn-images-1.medium.com/max/1600/1*W8_auNFU_NRDcsA21Msa0w.png)
After adding all the configuration files, your Build settings should look like this:
![](https://cdn-images-1.medium.com/max/1600/1*sNPBwjaXWmYZSAe_3CdFpw.png)
So we have now successfully linked all the configuration files to the respective build configurations.
### Linking Schema with the Build Configuration:
Now the last step is linking the schema with the build configuration. To do this, select any schema, go to edit schema, and set the appropriate build configuration there.
![](https://cdn-images-1.medium.com/max/1600/1*o-OY3Yzv7GzmlSs09o2uyA.png)
### Ready to Run the Project:
Now all the set up is done. The only thing you have to do select the schema and run — the environment will be automatically selected for you. So for fetching the server URL and other values, I have created an Environment.swift file. Check it out:
```swift
import Foundation

public enum PlistKey {
    case ServerURL
    case ConnectionProtocol
    
    func value() -> String {
        switch self {
        case .ServerURL:
            return "server_url"
        case .ConnectionProtocol:
            return "protocol"
        }
    }
}
public struct Environment {
    
    fileprivate var infoDict: [String: Any]  {
        get {
            if let dict = Bundle.main.infoDictionary {
                return dict
            }else {
                fatalError("Plist file not found")
            }
        }
    }
    public func configuration(_ key: PlistKey) -> String {
        switch key {
        case .ServerURL:
            return infoDict[PlistKey.ServerURL.value()] as! String
        case .ConnectionProtocol:
            return infoDict[PlistKey.ConnectionProtocol.value()] as! String
        }
    }
}
```

To fetch the server URL or other settings in ViewController.Swift or any other file, you have to write only one line of code:
```swift
let server_url = Environment().configuration(PlistKey.ServerURL)
     print(server_url)
```
You can also manage different app icons for different environments from build settings. Then, you will only have to look for a second to see which environment build is installed on your device.

# Reference
https://medium.freecodecamp.org/managing-different-environments-and-configurations-for-ios-projects-7970327dd9c9
