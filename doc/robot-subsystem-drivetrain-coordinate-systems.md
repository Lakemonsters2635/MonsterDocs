
# Drivetrain Coordinate System Documentation

##### This page will go through the main coordinate systems and definitions of systems to synthesize drivetrainSubsystem's many coordinate system changes into one document.

<br>

## Coordinate Systems Covered

  - [Joystick Coordinate System](#joystick-coordinate-system)
  - [Drive Method Inputs (Stick Shaping)](#drive-method-inputs-stick-shaping)
  - [Connecting Stick Shaping to Robot & Field](#connecting-stick-shaping-to-robot-&-field)
  - [Field-Relative Coordinates](#field-relative-coordinates)
  - [Robot-Relative Coordinates](#robot-relative-coordinates)
  - [NavX Gyroscope Coordinates](#navx-gyroscope-coordinates)
  - [Swerve Module Coordinates](#swerve-module-coordinates)
  - [Red vs. Blue Alliance Reflection](#red-vs-blue-alliance-reflection)
  - [Vision Coordinate System (Oak-D / AprilTag)](#vision-coordinate-system-oak-d--apriltag)
  - [Coordinate System Transform](#coordinate-system-transform-flow)
  - [Summary Table: +X, +Y, +Z by Frame](#summary-table-x-y-z-by-frame)

<br>

## Joystick Coordinate System

### Raw axes:
- `+X`: Joystick pushed **right**
- `-Y`: Joystick pulled **forward**
- `+Z`: Twist **clockwise** (right)

These come directly from:
```java
joystick.getX();     // +X = right
joystick.getY();     // +Y = back
joystick.getTwist(); // +Z = clockwise
```
<div style="display: flex; justify-content: space-between;">
  <img src="/assets/images/swerve-coordinate-systems/JoystickAxis.png" alt="Joystick Axes" style="width: 49%;">
  <img src="/assets/images/swerve-coordinate-systems/JoystickCoordinateSystem.png" alt="Joystick Coordinates" style="width: 49%;">
</div>

<br>

## Drive Method Inputs (Stick Shaping)

To align joystick output with robot or field-relative motion:
Raw Code: 
```java
 if (rightJoystick.getY()>0.05 || rightJoystick.getY()<-0.05) {
        yPowerCommanded = rightJoystick.getY() * -1;
      }

      if (rightJoystick.getX()>0.05 || rightJoystick.getX()<-0.05) {
        xPowerCommanded = rightJoystick.getX();
      }

      if (Math.pow(rightJoystick.getTwist(),3)>0.05 || Math.pow(rightJoystick.getTwist(),3)<-0.05) {
        rotCommanded = rightJoystick.getTwist()*-1;
      }
```
Simplified version:
```java
double x = joystick.getX();            // +X = right
double y = -joystick.getY();           // +Y = forward
double rot = -joystick.getTwist();     // +Z = counterclowckwise
```

<p align="center">
  <img src="/assets/images/swerve-coordinate-systems/StickshapingCoordinateSystem.png" alt="Stick Shaping" width="25%">
</p>

These are passed to:
```java
drive(x, y, rot, fieldRelative);
```

<br>

## Connecting Stick Shaping to Robot & Field 
Connecting `Stick Shaping` coordinate systems to the `Robot` & `Field` `Coordinate Systems` is **key**. Looking back at the diagrams for input (Stick Shaping), it is clear that the deseired coordinate system is `+Y` moving **forward**, `+X` moving **right**, and `+Z` to move counterclockwise. This **needs** to match the Robot & Field coordinate systems. If you scroll below and you look at those diagrams, it is clear that the rotation counterclockwise is the same but `+X` is forward and `+Y` is left. This issue is resolved in the code in one key area through the use of `ChassisSpeeds.fromFieldRelativeSpeeds(xSpeed, ySpeed, rot, m_gyro.getRotation2d())`. The specific place to look at is `m_gyro.getRotation2d()`. Lets simplify this for a second and lets just talk about moving forward. Moving forward is positive Y speed and it works because `+X` is aligned with gyro 0 degrees by default WPIlib coordinate systems. Since our 0 for gyro is on the right side of the robot, `+X` is actually facing right and `+Y` is left of that by the robot coordinate system and is actually forward. This is how the input gets connected toward actual robot coordinates. 

<br>

## Field-Relative Coordinates

Defined relative to the **field**, not the robot:

- `+X`: **Away from driver station**
- `+Y`: **Left from driver station POV**
- `+Z`: **Clockwise rotation**

<p align="center">
  <img src="/assets/images/swerve-coordinate-systems/FieldRelativeAxis.png" alt="Field Relative Axis" width="80%">
</p>

Used to make driver control consistent regardless of heading:
```java
ChassisSpeeds.fromFieldRelativeSpeeds(
  xSpeed, ySpeed, rotSpeed, navx.getRotation2d()
);
```

<br>

## Robot-Relative Coordinates

The robot's frame of reference is:
- `+X`: **Forward**
- `+Y`: **Left**
- `+Z`: **Up**
- Yaw rotation (`+Z`): **Clockwise**.

<p align="center">
  <img src="/assets/images/swerve-coordinate-systems/BotAxis.png" alt="Bot Axis" width="30%">
</p>

Used in:
- `ChassisSpeeds` (robot-relative speeds)
- `SwerveDriveKinematics.toSwerveModuleStates(...)` (turns robot-relative speeds into individual wheel angle and speeds)

```java
ChassisSpeeds chassisSpeeds = new ChassisSpeeds(xSpeed, ySpeed, rot);
SwerveModuleState[] moduleStates = kinematics.toSwerveModuleStates(chassisSpeeds);
```

<br>



## NavX Gyroscope Coordinates

The NavX reports orientation and speeds in a standard aerospace format:
- **Yaw** = rotation around Z (clockwise is positive)
- **Pitch** = rotation around Y
- **Roll** = rotation around X

**Note that the coordinate system for NavX depends on the direction/orientation you place it on the roboRio so it is important to align it relative to your other coordinate systems.**

  Use:
```java
navx.getYaw();         // Returns +clockwise
navx.getRotation2d();  // Used for heading
```

<br>

## Swerve Module Coordinates

Modules are defined in robot-relative space.

Layout:
```java
public final Translation2d m_frontLeftLocation = 
            new Translation2d(-m_drivetrainWheelbaseWidth/2, m_drivetrainWheelbaseLength/2);
    public final Translation2d m_frontRightLocation = 
            new Translation2d(m_drivetrainWheelbaseWidth/2, m_drivetrainWheelbaseLength/2);
    public final Translation2d m_backLeftLocation = 
            new Translation2d(-m_drivetrainWheelbaseWidth/2, -m_drivetrainWheelbaseLength/2);
    public final Translation2d m_backRightLocation = 
            new Translation2d(m_drivetrainWheelbaseWidth/2, -m_drivetrainWheelbaseLength/2);
```
<p align="center">
  <img src="/assets/images/swerve-coordinate-systems/DefinedModulePositions.png" alt="Stick Shaping" width=50%">
</p>

Used in:
```java
new SwerveDriveKinematics(
  frontLeftLocation, frontRightLocation,
  backLeftLocation, backRightLocation
);
``` 

Each module receives:
- **Angle** (rotation in radians)
- **Speed** (m/s)

<br>

## Red vs. Blue Alliance Reflection

The field is mirrored depending on alliance:

- **Blue alliance**: +X = away from driver
- **Red alliance**: must flip field poses to match blue-side logic

Flipping example:
```java
if (DriverStation.getAlliance() == Alliance.Red) {
      //Default is Blue so if Red Alliance then negate the X value and Rotation converts to Red Alliance
      startPose = new Pose2d(-startPose.getX(), startPose.getY(), new Rotation2d(Math.toRadians(toRedHead(startPose.getRotation().getDegrees())))); 
      middlePose = new Translation2d(-middlePose.getX(), middlePose.getY());
      endPose = new Pose2d(-endPose.getX(), endPose.getY(), new Rotation2d(Math.toRadians(toRedHead(endPose.getRotation().getDegrees()))));
      endRot *= -1;
    }
}
```

PathPlanner support automatic mirroring.

<br>

## Vision Coordinate System (Oak-D / AprilTag)

Typical 3D camera uses right-handed coordinate frame:

- `+Z`: Forward
- `+X`: Right
- `+Y`: **Down** (due to camera mounting)

To transform to robot-relative:
- Use a known robot-to-camera transform
- Convert vision pose to `Pose2d`

<br>

## Coordinate System Transform 

| Step                    | Input Frame       | Output Frame         |
|-------------------------|-------------------|-----------------------|
| Joystick Input          | Joystick          | Driver Control Logic  |
| Stick Shaping           | Joystick          | Robot-relative Speeds |
| Field-Relative Drive    | Field             | Robot-relative Speeds |
| Gyro Correction         | NavX              | Robot Heading         |
| Kinematics              | Robot-relative    | Module States         |
| Vision Estimation       | Camera            | Field Pose Estimate   |

<br>

## Summary Table: +X, +Y, +Z by Frame

| Frame       | +X Direction     | +Y Direction     | +Z Rotation     |
|-------------|------------------|------------------|-----------------|
| Joystick    | Right            | Back             | CW (twist)      |
| Robot       | Forward          | Left             | CCW (yaw)        |
| Field       | Away from DS     | Left from DS POV | CW              |
| NavX        | Forward          | Left             | CW              |
| Vision      | Forward (Z)      | Down (Y)         | CW              |



