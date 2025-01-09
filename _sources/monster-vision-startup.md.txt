## Vision Startup   

### Startup Robot

1. Ensure breaker is off
1. Swap to new battery
1. Turn breaker on
1. Position robot in a safe start state 
   * For Eclipse Teleop this is arm down
   * For Eclipse auto arm is up
1. Robot is now ready for deploying

### Start Up Robot Vision For Competition
1. Deploy clean version of code if necessary
1. Ensure driver station on if not already
1. Ensure shuffleboard is on
1. Go to [wpilibpi.local](http://wpilibpi.local)
1. Go to vision statues tab
1. Check that the console says "up"
1. Check that enable is turned on
1. Click "down" and then "up" again
1. Make sure no errors are present, if not contact the nearest vision member
1. Go to [wpilibpi.local:1181](http://wpilibpi.local:1181) and refresh until image shows up. This is to display the cameras to the driver 



### Set Up Robot Vision For Development
1. Do Start Up Robot for Vision(Competition)
1. Click "down" on the [wpilibpi.local](http://wpilibpi.local) vision status page
1. Open up PuTTY to the "pi" saved session
1. Type the password "raspberry"
1. Go back to [wpilibpi.local](http://wpilibpi.local) status tab click "writable", ignore the error (not an important error)
1. Run the command `cd MV4.5`
1. Run the command `./MonsterVision4.5.py` 
1. Refresh [wpilibpi.local:1181](http://wpilibpi.local:1181) until the camera shows up
1. Develop!


### Reset Raspberry Pi if Vision Crashes
1. Go to [wpilibpi.local](http://wpilibpi.local)
1. Go to vision statues tab
1. Check that the console says "up"
1. Check that enable is turned on
1. Click "down" and then "up" again
1. Make sure no errors are present, if not contact the nearest vision member

### Monitor Vision Results on wpilibpi.local
1. Go to [wpilibpi.local](http://wpilibpi.local)
1. Go to vision statues tab
1. Go to [wpilibpi.local:1181](http://wpilibpi.local:1181) and refresh until image shows up. This is to display the cameras to the driver 

### Monitor Vision results in Shuffleboard
1. Refer to "Startup Robot Vision For Competition"
1. Go to shuffleboard
1. Click the double chevron in the top left corner
1. Look for the MonsterVision tab in the NetworkTables menu 


### Notes:
* MAKE SURE THE CAMERA, RASPBERRY PI, AND RADIO HAVE ALL CABLES CONNECTED SECURELY IF YOU REALLY CANNOT FIGURE OUT WHAT IS WRONG!!!!!
* If you change runCamera then click "down", "terminate", and "up" in the vision status tab
* Ignore resolution errors and calibration data
* If it says "bind() to port 1181 failed..." then run `htop`. Then click on the instance of MonsterVision4.5.py with the longest run time. Then type F9. Then type 9, then hit enter. Press "q" to exit. 