# Provider
Provider connects iOS devices to ControlFloor. This sets up video streaming from iOS devices to the browser,
and also enables the devices to be controlled remotely.

# Basic Install Instructions
## Clone repos
1. `git clone https://github.com/nanoscopic/ios_remote_provider.git`
1. `git clone https://github.com/nanoscopic/controlfloor.git`
1. `git clone https://github.com/nanoscopic/ios_video_app.git`
1. `git clone https://github.com/nanomsg/nng.git`

## Build nng - https://github.com/nanomsg/nng
1. `cd nng`
1. `cmake`
1. `make`
1. `make install`

## Build ControlFloor

1. `cd controlfloor`
1. `make`
1. `./main run`

Open `https://yourip:8080` to see if controlfloor is running

## Build iOS Remote Provider and WDA
1. `cd ios_remote_provider`
1. Edit `config.json` to add your Apple developer details
1. `make`
1. `security unlock-keychain login.keychain` # to make sure developer details are there for xcode build
1. `make wda`

## Register Provider
1. `./main register`

## Build and setup CF Vidstream App
1. `cd ios_video_app`
1. Open the xcode project and install CF Vidstream on the device

## Start CF Vidstream App Manually
1. Open the app
1. Click "Broadcast Selector"
1. Click "Start Recording"

## Start Provider
1. `cd ios_remote_provider`
1. `./main run`

## Automatically starting CF Vidstream App
1. Figure out your device id  
    A. `./bin/iosif list`  
1. Figure out your device UI width/height  
    A. `./main winsize`
    B. -or- `./main winsize -id [your device id]` 
    C. Observe "Width" and "Height" displayed
1. Add device specific config block to `config.json`:  
    ```  
    {
        ...
        devices:[
            {
                udid:"[your device id]"
                uiWidth:[your device width]
                uiHeight:[your device height]
            }
        ]
    }
    ```
1. That's it. The video app will be started automatically when the provider is started.

## Using tidevice instead of go-ios

You may wish to use tidevice instead of go-ios to start WDA. Do the following to get it setup:  
  
1. Install tidevice. `pip3 install tidevice`

1. Add a WDA start method to your `config.json`:  
    ```
    {
        ...
        wda:{
            ...
            startMethod: "tidevice"
        }
    }
    ```

1. Run `make usetidevice` to auto-generate the `calculated.json` file containing the location of tidevice installed on your system.  
  
1. Start provider normally; tidevice will be used.
