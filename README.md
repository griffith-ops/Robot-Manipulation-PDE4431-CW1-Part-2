# Robot-Manipulation-PDE4431
Assignment Submission
Epson-VT6-Pick-and-Place/
│
├── README.md
│
├── src/
│   └── PickAndPlace.prg
│
├── images/
│   ├── robot_setup.png
│   ├── execution_flowchart.png
│   └── pick_and_drop_positions.png
│
└── docs/
    └── coursework_report.pdf
    # Epson VT6 – Robotic Pick and Place Application

A robotic pick-and-place application developed using the **Epson VT6 6-axis industrial robot**, programmed in **Epson SPEL+** through the **Epson RC+ 8.2** development environment.

This project demonstrates automated object handling, robot motion programming, gripper control, coordinate frame offsets, modular programming, and repeated execution cycles.

## 1. Project Overview

The objective of this project is to develop a robotic pick-and-place application capable of picking up three balls from a designated pickup location and placing them individually at three predefined drop locations.

The application uses a modular programming approach, where separate functions are responsible for picking and placing objects.

The robot executes the following sequence:

1. Initialize the robot motors.
2. Move to the reference position.
3. Approach the pickup location.
4. Activate the gripper to pick up the object.
5. Retract from the pickup location.
6. Move to the designated drop location.
7. Release the object.
8. Retract from the drop location.
9. Repeat for the remaining objects.
10. Return to the home position and switch off the motors.

## 2. Hardware and Software

| Component | Description |
|---|---|
| Robot | Epson VT6 |
| Robot configuration | 6-axis industrial robot |
| Development software | Epson RC+ 8.2 |
| Programming language | Epson SPEL+ |
| End effector | Digitally controlled gripper |
| Gripper control | Digital output 11 |
| Objects handled | Three balls |
| Application | Automated pick-and-place |

## 3. Project Objectives

- Develop a functional pick-and-place application using an industrial robot.
- Understand robot motion commands such as `Go` and `Move`.
- Implement world-frame and tool-frame coordinate offsets.
- Configure and operate a robotic gripper using digital outputs.
- Implement modular programming using functions and subroutine calls.
- Develop repeated execution sequences using `Do...Loop Until`.
- Verify robot positioning, approach paths, and retract movements.

## 4. Program Structure

The program is organized into the following functions:

| Function | Description |
|---|---|
| `main` | Executes one complete three-ball pick-and-place sequence |
| `Pick` | Picks an object from the predefined pickup location |
| `Drop_1` | Places the first object at drop location 1 |
| `Drop_2` | Places the second object at drop location 2 |
| `Drop_3` | Places the third object at drop location 3 |
| `Loop_Cycle_count3` | Repeats the complete three-ball sequence three times |
| `Loop_Cycle_updated` | Executes each pick-and-drop operation once using individual counters |

### Main Function

The `main` function coordinates the complete operation by calling the required functions sequentially.

```text
Motor ON
    |
    v
Pick Ball 1
    |
    v
Drop_1
    |
    v
Pick Ball 2
    |
    v
Drop_2
    |
    v
Pick Ball 3
    |
    v
Drop_3
    |
    v
Motor OFF
```

## 5. Pick Operation

The `Pick` function is responsible for approaching, gripping, and retracting from the pickup location.

### Execution Sequence

1. Set robot speed to 70.
2. Move to `home_ref`.
3. Open the gripper using `Off 11`.
4. Wait for one second.
5. Reduce speed to 30.
6. Approach the pickup point using `Go pick_exam +Z(100)`.
7. Move to the pickup position using `Move pick_exam`.
8. Wait for one second.
9. Close the gripper using `On 11`.
10. Wait for one second.
11. Retract using `Move pick_exam -TLZ(100)`.

### SPEL+ Code

```vb
Function Pick
    Speed 70
    Go home_ref
    Off 11
    Wait 1

    Speed 30
    Go pick_exam +Z(100)
    Move pick_exam
    Wait 1

    On 11
    Wait 1

    Move pick_exam -TLZ(100)
Fend
```

## 6. Drop Operations

Three separate functions define the placement operations.

### Drop_1

The first object is placed at `drop1_exam`.

```vb
Function Drop_1
    Go drop1_exam -TLZ(100)
    Move drop1_exam
    Wait 1

    Off 11
    Wait 1

    Move drop1_exam +Z(100) CP
Fend
```

### Drop_2

The second object is placed at `drop2_exam`.

```vb
Function Drop_2
    Go drop2_exam +Z(100)
    Wait 1

    Move drop2_exam
    Wait 1

    Off 11
    Wait 1

    Move drop2_exam -TLZ(100) CP
Fend
```

### Drop_3

The third object is placed at `drop3_exam`. After placement, the robot returns to its home reference position.

```vb
Function Drop_3
    Go drop3_exam -TLZ(100)
    Move drop3_exam
    Wait 1

    Off 11
    Wait 1

    Go drop3_exam +Z(100) CP
    Go home_ref
Fend
```

## 7. Motion Commands Used

| Command | Purpose |
|---|---|
| `Motor On` | Enables robot motors |
| `Motor Off` | Disables robot motors |
| `Speed` | Sets the speed for applicable motion commands |
| `Go` | Performs point-to-point robot movement |
| `Move` | Performs linear robot movement |
| `Wait` | Introduces a programmed delay |
| `On 11` | Activates output 11 to close the gripper |
| `Off 11` | Deactivates output 11 to open the gripper |
| `Call` | Executes a specified function |
| `CP` | Enables continuous-path motion behavior where supported |
| `Print` | Displays messages in the run window |

## 8. Coordinate Frames and Offsetting

The application demonstrates two important coordinate-offset techniques.

### World Frame Offset

A world-frame offset is applied relative to the robot's reference coordinate system.

Example:

```vb
Go pick_exam +Z(100)
```

This command moves the robot to a position offset by 100 mm along the positive Z-axis of the applicable reference frame.

### Tool Frame Offset

A tool-frame offset is applied relative to the tool coordinate system.

Example:

```vb
Move pick_exam -TLZ(100)
```

This command applies a negative 100 mm offset along the tool's local Z-axis.

### Importance of Offsetting

Coordinate offsets are used to:

- Define approach and retract positions.
- Reduce the risk of collisions.
- Improve the consistency of object handling.
- Control robot movement relative to the tool orientation.
- Avoid unnecessary intermediate taught points.

**Note:** The original program uses both positive and negative offsets for different pickup and drop operations. Their suitability depends on the taught points, tool orientation, and physical robot configuration.

## 9. Gripper Control

The gripper is operated using digital output 11.

| Command | Gripper operation |
|---|---|
| `On 11` | Close gripper |
| `Off 11` | Open gripper |

A one-second delay is introduced after gripper activation and deactivation to allow sufficient time for mechanical operation.

The actual gripper response depends on the connected hardware and pneumatic or electrical control arrangement.

## 10. Loop-Based Automation

Two additional functions demonstrate alternative approaches to executing the application.

### Loop_Cycle_count3

This function uses a single counter to repeat the complete three-ball pick-and-place sequence three times.

```vb
Integer count
count = 1

Do
    Print "Current loop cycle count: ", count

    Call Pick
    Call Drop_1

    Call Pick
    Call Drop_2

    Call Pick
    Call Drop_3

    count = count + 1

Loop Until count > 3
```

This demonstrates:

- Counter initialization.
- Repeated function execution.
- Counter incrementation.
- Conditional loop termination.

### Loop_Cycle_updated

This function uses three independent counters:

- `count1` – First ball.
- `count2` – Second ball.
- `count3` – Third ball.

Each loop executes its respective pick-and-drop operation once.

The function also displays progress messages in the Epson RC+ run window.

This demonstrates the use of independent counters and sequential loop execution.

## 11. Robot Positions

The following robot positions must be defined and taught before executing the application.

| Position | Description |
|---|---|
| `home_ref` | Robot home/reference position |
| `pick_exam` | Pickup position |
| `drop1_exam` | First drop location |
| `drop2_exam` | Second drop location |
| `drop3_exam` | Third drop location |

The exact coordinates depend on the physical arrangement of the robot, workpiece, gripper, and work surface.

## 12. Testing and Verification

The following checks should be performed before running the complete application:

1. Verify all taught robot positions.
2. Confirm that the tool coordinate system is configured correctly.
3. Verify the gripper's opening and closing operations.
4. Check the approach and retract positions.
5. Validate the programmed coordinate offsets.
6. Test the motion sequence at a reduced speed.
7. Confirm that the robot can move between positions without collisions.
8. Verify that each object is placed at its intended location.
9. Test the complete three-object sequence.

## 13. Safety Considerations

Industrial robot operation requires appropriate safety precautions.

- Ensure the robot workspace is clear of obstacles and personnel.
- Verify that the emergency stop system is operational.
- Test new positions and trajectories at reduced speed.
- Confirm that the gripper is securely mounted.
- Check that the workpieces are correctly positioned.
- Verify tool-frame offsets before automatic operation.
- Follow the Epson VT6 operating manual and applicable laboratory safety procedures.

**Important:** This code is provided as an educational example. It must be verified against the actual robot configuration before execution. The additional loop functions enable the motors but do not explicitly disable them at the end; appropriate shutdown handling should be added before production use.

## 14. Future Improvements

Potential improvements include:

- Object detection using vision sensors.
- Automatic identification of pickup locations.
- Integration of proximity sensors.
- Gripper feedback to verify successful object pickup.
- Improved error handling and recovery.
- Configurable pick-and-place cycle counts.
- Dynamic selection of drop positions.
- Cycle-time optimization.

## 15. Learning Outcomes

Through this project, the following robotics programming concepts are demonstrated:

- Industrial robot programming using Epson SPEL+.
- Point-to-point and linear motion control.
- Tool Centre Point (TCP) configuration.
- World-frame and tool-frame coordinate offsets.
- Digital output-based end-effector control.
- Function-based program organization.
- Loop-based automation.
- Robot motion verification and safety considerations.

## 16. Development Environment

**Robot:** Epson VT6  
**Software:** Epson RC+ 8.2  
**Language:** Epson SPEL+  
**Application:** Three-object robotic pick-and-place  
**Project type:** Robotics coursework / Educational application

## 17. Disclaimer

This project was developed for educational purposes to demonstrate robotic pick-and-place programming concepts.

Robot positions, speed settings, gripper outputs, and coordinate offsets must be adapted and verified for the specific hardware configuration before use.

---

**Project:** Epson VT6 Robotic Pick-and-Place Application

**Technology:** Industrial Robotics | Epson SPEL+ | Robot Motion Control | Coordinate Frame Offsetting | Automation
