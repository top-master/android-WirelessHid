# WirelessHid

The hid implementation based on wireless wifi (api 21, android5.0 and above), uses the touch screen of the android device to control the mouse pointer and basic keyboard data input on the pc through the wifi network (local area network is best).

The whole architecture is C/S architecture, in which the android device is the client side, and the pc (windows/linux) is the server side. The Android mobile's input events get sent to the PC through network packaging. The current packaging uses Google's protocol buffer, which is an open source library based on binary data encapsulation and decapsulation. For details, please refer to:
https://developers.google.com/protocol-buffers/

Description of each directory:
  1. The `android` directory is the app source code for android, which is the client end of the entire architecture. The current project is an android studio project, which can be opened directly with android studio.
  2. pc This directory is the source code of the executable program on pc (linux/windows) (currently implemented by JAVA), and also contains the required protobuf library. You can use eclipse to import the project.
  3. In the bin directory are compiled binary files, including an apk file on android and an executable jar file independent of the platform system (can be executed on linux/windows like: `java -jar WirelessHidServer.jar`), users can run and use directly without compiling from source code.

2016.06.28 update:

  1. Modified the overall architecture, uses the PC side as the server side, and the Android side as the client side.
     The PC side will listen for messages and/or requests from the Android side,
     and once there is a link (or connection) request from the Android side,
     PC side will establish requested connection;
     Even if the Android side disconnects,
     the PC side will continue to monitor until there is a connection request.

  2. Added the automatic service discovery mechanism to Android;
     The PC side only needs to run the service program,
     and the Android side only needs to open the app,
     and then the Android app will perform multicast search for services,
     through the UDP address 224.0.0.1 range.
     If the service is found, Android will initiate a link to the server.

  3. Modified the key value corresponding to the volume key;
     The volume down key corresponds to the arrow key down key,
     and the volume up key corresponds to the arrow key up key.

The following functions are currently implemented:

  1. Mouse movement control
  2. Mouse Left/Right Click Control
  3. Mouse scroll axis control
  4. Set mouse movement and scrolling speed
  5. Keyboard main and/or common key controls.
  6. Keyboard long press continuous input control
  7. Pressing the up and down keys of the volume keys of the mobile phone,
     triggers the left and right arrow keys of the keyboard,
     it is used for PPT (Power-point) presentations.
  8. Trigger the right button of the keyboard by shaking the phone (realized by the sensor), which is used for PPT presentations

Current known issues:

  1. The performance on Windows is not good, there is a problem of data loss, the mouse and keyboard are relatively stuck,
   but it is very smooth on linux.
  2. Some of the current key values ​​are incorrect
   (such as the menu key, which cannot be used according to the javadoc),
   and no suitable key values have been found for the time being.
  3. At present, touchpad gesture operations
   (such as zoom-in gestures) cannot be supported,
   and keyboard combination key input cannot be supported.
  4. After setting the mouse speed, the mouse coordinates are incoherent

# Here are runtime snapshots:
## Android side (client):
  0. Searches for PC-side's service (multicast service discovery process).

  ![screenshot 0](./ScreenShot/Client_0.png)

  1. Touch-pad

  ![screenshot 1](./ScreenShot/Client_1.png)

  2. Main common keyboard

  ![screenshot 2](./ScreenShot/Client_2.png)

  3. Num-pad From the keyboard

  ![screenshot 3](./ScreenShot/Client_3.png)

## PC side (server):
  1. Listening for the service discovery package of the Android side

  ![screenshot 4](./ScreenShot/Server_1.png)

  2. Received the service discovery packet from the Android side,
     responds to the android service link request, and establishes a link.

  ![screenshot 5](./ScreenShot/Server_2.png)

  3. After receiving the disconnection request packet from the Android side, disconnects the connection, and re-listens for new connection requests.

  ![screenshot 6](./ScreenShot/Server_3.png)
