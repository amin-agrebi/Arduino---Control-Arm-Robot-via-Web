# Arduino---Control-Arm-Robot-via-Web

### User Interface

![](https://hackster.imgix.net/uploads/attachments/550036/user_interface_nWNHigoPHG.png?auto=compress%2Cformat&w=740&h=555&fit=max)

![](https://hackster.imgix.net/uploads/attachments/550039/user_interface_2_goGxnqRIkT.JPG?auto=compress%2Cformat&w=740&h=555&fit=max)

The robot arm has 6 motors.

-   Zone A: Control motor 2, 3, 4 (control three hand joints)
-   Zone B: Control motor 1 (control base)
-   Zone C: Control motor 5 (control rotation of gripper)
-   Zone D: Control motor 6 (control gripper)

### 

3. System Architecture

![](https://hackster.imgix.net/uploads/attachments/550042/system_architecture_BRu4bbYQ0J.JPG?auto=compress%2Cformat&w=740&h=555&fit=max)

### 

4. Working Flow

**Client Side** (web user interface - written in JavaScript + HTML + CSS)

When a user touches or sweeps finger (or clicks or moves mouse), we can get coordinate (x, y). The working flow is as follows:

![](https://hackster.imgix.net/uploads/attachments/550044/working_flow_client_3LhVrTQ0a3.JPG?auto=compress%2Cformat&w=740&h=555&fit=max)

In case of Zone A, to calculate angles of motor 2, 3, 4, we need to do some **_geometric calculation_**. You can refer to it at the end of this page.

**Server Side** (Arduino code)**:**

Once receiving a set of angles from clients, six motors moves from the current angles to the new angles gradually. Six motor should move and reach new angles at the same time. Before going into detail how to control all motors, let’s look at how to control a single motor. Suppose that we want to move a motor from current angle (angle) to new angle (new_angle). Since speed of motor is high, **_we should slow down it._** To do like that, two following steps are repeated until the motor reach new angle:

-   Move motor with a small step.
-   Pause a small time and then move another step.

The following diagram illustrates the above scheme in case new angle is greater than current angle:

![](https://hackster.imgix.net/uploads/attachments/550048/working_flow_server_1_yYdYpW56lI.JPG?auto=compress%2Cformat&w=740&h=555&fit=max)

_Wherestep_numis number of steps the motor has to take.__`step and time`__is predefined values. Two later ones decide the speed and smoothness._ The above is just only for one robot. To make robots start to move and reach destination at the same time, we can do as follows: Six motors take the same _step_num_, but `step` of each motor is different from each other. So we have to choose _step_num_ in this project is maximum.

Generally, working flow of Arduino is as follows:

![](https://hackster.imgix.net/uploads/attachments/550053/working_flow_server_2_etFSeO55q8.JPG?auto=compress%2Cformat&w=740&h=555&fit=max)

### 

5. Geometric Calculation

Let’s make a robot arm calculation into the following geometry problem:

![](https://hackster.imgix.net/uploads/attachments/550055/file.php?auto=compress%2Cformat&w=740&h=555&fit=max)

**_Known_**

-   C is fixed
-   A known point - D is the input from user
-   A known point - CB, BA, AD (denoted by b, a, d respectively)
-   Lengths of each arm segments **Find:** angles C, B, A **Solution:**
-   Make assumption that angle B and A are the same
-   Add some additional points and segment

![](https://hackster.imgix.net/uploads/attachments/550054/file.php?auto=compress%2Cformat&w=740&h=555&fit=max)

**_Calculation_**

-   We knew points C and D => we can calculate the length of DC (denoted by c)
-   We can also calculate the δ
-   Look at triangle ABE, we can infer that AE = BE and ∠E = π - 2α.
-   So:

![](https://hackster.imgix.net/uploads/attachments/550060/file.php?auto=compress%2Cformat&w=740&h=555&fit=max)

-   The Law of Cosines in triangle CDE:

![](https://hackster.imgix.net/uploads/attachments/550059/file.php?auto=compress%2Cformat&w=740&h=555&fit=max)

-   Change (1) and (2) into (3), we have:

![](https://hackster.imgix.net/uploads/attachments/550056/file.php?auto=compress%2Cformat&w=740&h=555&fit=max)

_**Simplify**_

-   Simplify the above:

![](https://hackster.imgix.net/uploads/attachments/550058/file.php?auto=compress%2Cformat&w=740&h=555&fit=max)

-   Since we know a, b, c and d, solve the above quadratic equation, we can calculate the value of α. - And β = π – α - Until now we found β, let’s find γ - The Law of Cosines in triangles BDC and BDA:

![](https://hackster.imgix.net/uploads/attachments/550057/file.php?auto=compress%2Cformat&w=740&h=555&fit=max)

-   Solve this set of equations, we can calculate γ.
-   So, their required angles is: (δ+γ), β and β. These are angles of motors 2, 3 and 4, respectively.

### 

6. Source Code

Source code inlude two files:

-   **_RobotArmWeb.ino_**: Arduino code
-   **_Remote_arm.php_**: Web app code, which is uploaded to PHPoC WiFi Shield or PHPoC Shield. (See instruction in [this article](https://www.hackster.io/phpoc_man/arduino-dynamic-web-control-8da805).)

You also need to upload the image file flywheel.png to PHPoC Shield.

![flywheel.png](https://hackster.imgix.net/uploads/attachments/550281/flywheel_6OvyCUsQ6p.png?auto=compress%2Cformat&w=740&h=555&fit=max)

flywheel.png

### 

The Best Arduino Starter Kit for Beginner

If you are looking for an Arduino kit, see [The Best Arduino Kit for Beginners](https://arduinogetstarted.com/hardware/best-arduino-kit-for-beginner)

  

### 

Function References

-   [Arduino - Servo Library](https://arduinogetstarted.com/reference/library/arduino-servo-library)
-   [Servo.attach()](https://arduinogetstarted.com/reference/library/servo-attach)
-   [Servo.write()](https://arduinogetstarted.com/reference/library/servo-write)
-   [Servo.writeMicroseconds()](https://arduinogetstarted.com/reference/library/servo-writemicroseconds)
-   [Servo.read()](https://arduinogetstarted.com/reference/library/servo-read)
-   [Servo.attached()](https://arduinogetstarted.com/reference/library/servo-attached)
-   [Servo.detach()](https://arduinogetstarted.com/reference/library/servo-detach)
-   [Serial.begin()](https://arduinogetstarted.com/reference/serial-begin)
-   [Serial.println()](https://arduinogetstarted.com/reference/serial-println)

## [](https://www.hackster.io/phpoc_man/arduino-control-arm-robot-via-web-379ef3#schematics)Schematics

### Schematic

It needs to provide the external power source for 6 motors

![](https://hackster.imgix.net/uploads/attachments/550083/schematic_gKypi8o4xH.JPG)
