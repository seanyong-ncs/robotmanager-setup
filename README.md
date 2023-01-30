# NCS Robot Manager Quick Start

Quick start guide to understanding how to set up Temi robot, how to navigate and use robotmanager, and how to perform the in person demo

## Section 1: Prepare Temi bot for operation

Quick Overview of section
- Navigate and access Temi controls
- Log into Temicenter (First party app)
- Update RobotAgent and Concierge

### A. Navigate and access Temi controls

When first accessing Temi, you will most likely be greeted by the home screen that will show a background and a list of locations that temi is pre-programmed to go to. There will be an app drawer icon in the **top-right hand corner**. Once opened, you will be able to find **Temi's settings and App launcher**. 

For the purposes of this guide, I will skip most of the utilities/descriptions of what you will find in these "Apps", instead listing out the most important functions/settings that you will findin the following sections
##### Temi Settings (Shows up as "Settings" in app drawer) #####
 - Wireless ADB Settings (Developer Tools > ADB Connection to temi) 
 - Map Editor
 - Apps & Permissions

##### App Launcher #####
 - Robot Agent
 - Native Settings

##### Native Settings (Accessed from App Launcher) #####
 - Bluetooth Devices
 - Apps (Used to manually force close apps on the temi)

##### Additional Remarks #####

###### Note that `Temi Settings` are NOT THE SAME as `Native Settings` ######

There will be many instances where you will want to be able to navigate Temi's UI without the need to interact with the app's controls. For example, force closing the app when the app hangs. In order to do this, you need to swipe upwards from the bottom a little bit to the right. Shown the in the picture below. From there, you can either __go back (Triangle button), go to the homepage (Circle button), open recent apps (Square button)__

[Place holder Image]

#### B. Log into Temicenter ####

Temicenter is used to manage the maps on the temi robot and to "test run" the robot in the absence of the robot manager platform

There will be a Samsung tablet that you will need to use to log into the Temicenter app. It is usually stored at the QB office locker __"L020"__, the passcode to access the locker is __"1020"__. Once the tablet is retrieved, open up the "temi" app on the tablet, which should be located on the home screen. Afterwards proceed to this link.

[Temi Center Link]

Then log into Temicenter by clicking the scan QR code button on the temi app on the tablet and scan the QR code on your computer screen with the tablet.

#### C. Update Robot Agent and Concierge
##### What's that? #####
What is RobotAgent and Concierge? RobotAgent is the interface/adapter that allows the Temi robot to talk to the RobotManager(RM) platform, and concierge is the front/user facing app that can be controlled by the Concierge CMS platform. Both are important to the proper functioning of the RM platform.

##### Pre-requisites #####
1. You must have ADB installed on your computer. Follow the official guide on [XDA Developers]. Skip ahead to the section on how to install ADB on your specific OS. This guide will assume that you use a linux distribution 
2. The `robotagent.apk` file and/or the `concierge.apk` file that you wish to install.
3. The `root-config.properties` file (How to retrieve this file will be addressed in Section 2: Robot Manager)

##### Flashing Steps #####
1. Go to `Settings` > `Developer Tools` > `ADB Connection to temi`
2. Click on the `Open Port` button (Open the port unsecurely)
3. Connect to the temi by entering (The IP will be shown on screen)
   ```sh
   adb connect [temi ip address] 
   ```
4. Once connected to temi via ADB, close the concierge app by going to `App Launcher` > `Settings (Native Settings)` > `Apps` > `Concierge`. Then click on the button that says `Force Stop` followed by `Uninstall` to uninstall the app __(You MUST uninstall the app this way. You cannot force install through ADB using the `-r` flag, doing so will result in corrupted data)__
5. Uninstall Robot Agent by following the same instructions in step 4, but instead of `Concierge` click on `RobotAgent`
6. Move `robotagent.apk`, `concierge.apk` and `root-config.properties` to an easily locatable directory and `cd` into that directory from terminal
7. Flash the `robotagent.apk` first in terminal
   ```sh
   adb install robotagent.apk
   ```
8. Then flash `concierge.apk` in terminal
   ```sh
   adb install concierge.apk
   ```
9. Push the `root-config.properties` to `sdcard/download`
    ```sh
    adb push root-config.properties sdcard/download
    ```

##### After Flashing Steps #####
1. Open `Temi Settings` > `Apps & Permissions` > `Permissions` and grant all permissions to all applications 
2. Open RobotAgent from `App Launcher` > `RobotAgent`
3. Click on `CHOOSE CONFIG FILE` and select the `root-config.properties` located in `sdcard/download`
4. Open the `Concierge` app and tap on the logo at the top 5 times to bring up the main menu, then click on `Settings`
5. In the settings page, you will need to set the fields `Username`, `Password`, `Company ID`, `Robot ID` and `Robot Access Key` fields. `Username` and `Password` are correspondent to the credentials used to log into robotmanager. To find the necessary strings for the `Company ID`, `Robot ID` and `Robot Access Key` fields, open up the `root-config.properties` file in any notepad application on your computer 
    
    Example snippet of the `root-config.properties` file - **DO NOT COPY THE KEYS FROM THIS EXAMPLE**
    ```
    # Change to 'true' if there is a running MQTT broker in the machine
    existMqttBroker=false
    
    robotId=xxxxxxxx-yyyy-zzzz-aaaa-abcdabcdabcd
    accessKey=xxxxyyyyzzzz1234xxxxyyyyzzzz1234
    companyId=xxxxxxx-yyyy-zzzz-aaaa-abcdabcdabcd
    
    ...
    ```
    To avoid mistakes when transfering the keys from the text file to the Temi Concierge app, a bluetooth keyboard is recommended. You can use your phone as a bluetooth keyboard to more accurately type the keys into the temi.
    
6. After setting up the config. Return to the concierge main menu and click `Download Profile` to load the changes.

# Section 2: Concierge and Mapping

Creating a map is essential to the functionality of the temi in the context of Robotmanager. As such new maps will have to be created whever new venues are used for demo purposes. The process to create a map for robotmanager is largely similar to the first-party method, albeit with some caveats.

#### Set Temi up for mapping ####
1. Put the charging dock in a location that is unobstructed and is braced against a wall or hard surface.
2. Return the temi to dock manually
3. Ensure that the Concierge app and the Robot Agent app are both running in the background

#### Start Mapping ####

1. Once above instructions are complete, follow this [mapping guide] and continue mapping as per the instructions in the video
2. When mapping is complete, log back in to the first-party temi center app to define points of interest and virtual walls
   
#### Concierge Virtual Tours ####
Visit https://dev.concierge.robotmanager.io/ to create and edit concierge profiles


   [Temi Center Link]: <https://center.robotemi.com/>
   [XDA Developers]: <https://www.xda-developers.com/install-adb-windows-macos-linux/#what-is-android-debug-bridge-adb>
   [mapping guide]: <https://www.robotemi.com/tv/how-to-map/>
