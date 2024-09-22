# Useful methods in Wrapper for testing and functionality
Before running any of the programs:
### Check for hardware version 
    robot.get_hardware_version() is ['ned2']
### Make sure you are turning off the learning mode with 
    robot.set_learning_mode(False)
### robot = NiryoRosWrapper()
    This will allows us to use any of the methods to control the arm
-----

### robot.set_arm_max_velocity(100); robot.set_arm_max_acceleration(100)
    Like the name suggests, set the velocity and acceleration to the value in percentage

### robot.led_ring.flashing(BLUE); robot.led_ring.solid(color = [255,255,255]); robot.led_ring.set_led_color(led_id,color); robot.led_ring.rainbow_cycle()
    Like the name suggests turns the ring led light to a flashing/solid of the color that we choose. 
    Note that led_id are values from 0 to 29, to set multi color to the ring leds as seen in line 524 in the program.
    Fun fact only ned2 and ned3 have leds
**try a test program to cycle through all the led colors fow when working with arm.**

### robot.custom_button.wait_for_any_action(timeout = 600)
    Line 868 under wait_custom_button_press() method. 
    Waits for any action to occur until time out. after which returns a ButtonAction.NoAction. 
    This is part of the Button Action Enum class that is in the ros_wrapper_enums, which contains the id numbers for all the actions that are performed

-----
# JOINT MOVEMENT
### robot.calibrate_auto(); robot.calibrate_manual()
    Calls service to calibrate the motos. But don't know the different between auto and manual tho. 

### robot.move_joints(6*[0],1)
    this calls the function move_joints with 6 lists with base value of 0 and wait until each of the list performs an action = 1. Basically move the joints to the 0th position, which I am assuming is the center.

Additionally not sure why move_joints(6*[0],1) is necessary, there is a function
### robot.enable_TCP()
    which is not the internet protocol
    TCP refers to TOLL CENTER POINT, which makes it pretty self explanatory for the method

**What this action is? Need to look into Button Action or test in real time.**

### robot.get_joints()
    Returns the current positions of all the joints

### robot.move_pose(x,y,z,roll,pitch,yaw); get_pose()
    Moves the arm to the given coordinates
    get_pose() is similar to get_joints

### robot.pick_from_pose(x,y,z,roll,pitch,yaw)
    Executes a picking from a position.
    Picking is considered as over the object, going to the height z, grasping with the tool and grasping the object

### robot.place_from_pose(x,y,z,roll,pitch,yaw)
    Similar to the pick from pose, but diffferent 

**Dont understand the difference between joints and pose. Need experiment**
### robot.move_without_moveit(target,duraation) 
Unknown
But, I believe its for the simulation space in the right of the screen

**Here is the code from the demo program**
It is imparitive that you realize that the arguments sent in are lists
and that the call to the acutally funciton(move_pose/pick_from_pose) is not done here but done in report.execute()
where it is called with **function(*args)**  which is a less known python syntax 
where it splits the list into its individual arguments (and it is **NOT** a pointer)

        z_offset = 0.15 if self.__robot.get_current_tool_id() <= 0 else 0.02
        sleep_pose = [0.3, 0, 0.3, 0, 1.57, 0]
        home_pose = [0.3, 0, 0.3, 0, 0, 0]
        pick_1 = [0, 0.2, z_offset, 0, 1.57, 0]
        pick_2 = [0, -0.2, z_offset, 0, 1.57, 0]
        place_1 = [0.15, 0, z_offset, 0, 1.57, 0]
        place_2 = [0.22, 0, z_offset, 0, 1.57, 0]

        report.execute(self.__robot.move_pose, "Move to sleep pose", sleep_pose)

        report.execute(self.__robot.pick_from_pose, "Pick 1st piece", pick_1)
        report.execute(self.__robot.place_from_pose, "Place 1st piece", place_1)

        report.execute(self.__robot.move_pose, "Move to sleep pose", sleep_pose)

        report.execute(self.__robot.pick_from_pose, "Pick 2nd piece", pick_2)
        report.execute(self.__robot.place_from_pose, "Place 2nd piece", place_2)

        report.execute(self.__robot.move_pose, "Move to sleep pose", sleep_pose)

        report.execute(self.__robot.pick_from_pose, "Pick 1st piece from center", place_1)
        pick_1[5] = 1.57
        report.execute(self.__robot.place_from_pose, "Replace 1st piece", pick_1)

        report.execute(self.__robot.move_pose, "Move to sleep pose", sleep_pose)

        report.execute(self.__robot.pick_from_pose, "Pick 2nd piece from center", place_2)
        pick_2[5] = -1.57
        report.execute(self.__robot.place_from_pose, "Replace 2nd piece", pick_2)
Additionally, from this code what I see is that you need to use pose for moving the robot, not sure what the point of joint is.
-----


### robot.get_current_tool_id(), robot.update_tool()
    I believe it is refering to the grabber since in the documentation, it is making references to grasp_with_tool() and release_with_tool() method.
    But, from the demo code, I believe that update_tool() is scanning to see if there is a tool and then changing the saved id to the tool that it finds, and then gets the tool id with get_current_tool_id().

