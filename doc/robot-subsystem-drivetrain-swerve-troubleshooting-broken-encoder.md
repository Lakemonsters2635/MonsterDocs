# Problems

## One of the Swerve Modules Isn't Turning the Correct Way

### Initial Symptoms

* When the robot is ran one of the swerve modules is not rotating correctly

* Other three modules was facing the right direction

### Trouble Shooting Safety

* glasses
* robot raised
* anything else to make sure you safely do trouble shooting and do not damage yourself, other students, electronics, motors, or hardware.

### Trouble Shooting Steps

Initial Trouble Shooting

* Recalibrate the swerve modules.
  * Even though we calibrated the modules several times we were still getting the same error.

Additional Trouble Shooting

* Make sure that the module desired states and the position of each module are in correct order (in the code)
  * Desired states were correct as expected, positions were also correct. Also if this was the case the issue would be inconsistent.

* Check the mark on the magnet inside of module's encoder:
  * Turns out the mark is broken, the magnet was moved:

    <img src="/MonsterDocs/assets/images/swerve-troubleshooting-broken-encoder/unaligned-magnet.jpg" width=150>

Final Resolution

* To fix the movement of encoder magnet:
  * Take out the encoder magnet and apply superglue to prevent it from moving. Then wait for a while.
  
    <img src="/MonsterDocs/assets/images/swerve-troubleshooting-broken-encoder/gluing-encoder-magnet.jpg" width=150>

  * Add a new mark on the magnet to be able to check it i the future.
  * Mount back the swerve module.
  * Re-align the wheels.
  * To ensure that the problem is fixed try to drive the wheen towards front, right, back and left. If the wheel id turning 90 degrees when it is moved in the given order it means the solution works. With arrangements in code it would become functional.


Contributors: 
* Suleyman Sade