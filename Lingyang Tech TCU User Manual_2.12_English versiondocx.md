![](media/image2.png){width="3.722916666666667in"
height="3.722916666666667in"}

**TCU Operation Manual**

Version: V2.13

Date: Aug 2023

# **Table of Contents** {#table-of-contents .TOC-Heading}

[**1 Revision History** [1](#_Toc133163460)](\l)

[**2 Terms** [2](#_Toc133163461)](\l)

[**3 System Architecture** [3](#_Toc133163462)](\l)

[**4 TCU** [6](#_Toc133163463)](\l)

[**4.1 Product Instruction** [6](#_Toc133163464)](\l)

[**4.2 Operation Mode** [7](#_Toc133163465)](\l)

[4.2.1 Manual Mode [7](#_Toc133163466)](\l)

[4.2.2 Auto Mode [8](#_Toc133163467)](\l)

[4.2.3 Protection Mode [9](#_Toc133163468)](\l)

[**4.3 Manual Setting** [9](#_Toc133163469)](\l)

[**4.4 Software Commissioning** [16](#_Toc133163470)](\l)

[4.4.1 Communication Connection [16](#_Toc133163471)](\l)

[4.4.2 Overview of the TCU Tool software [22](#_Toc133163472)](\l)

[4.4.3 TCU tool chart function [28](#_Toc133163473)](\l)

[4.4.4 TCU Tool: Initialize equipment [29](#_Toc133163474)](\l)

[4.4.5 Others [30](#_Toc133163475)](\l)

1.  []{#_Toc133163460 .anchor}**Revision History**

  -------------- -------------- -------------------------------------------
  **Date**       **Revision**   **Description**

  2022.07        V1.0           First Edition

  2023.04        V2.0           Revised Edition

  2023.06        V2.1           Added Bluetooth connection

  2023.07        V2.11          Added Multi-level wind protection

  2023.07        V2.12          Updated New Self-powered TCU appearance

  2023.08        V2.13          English-only version released
  -------------- -------------- -------------------------------------------

**\
**

2.  []{#_Toc133163461 .anchor}**Terms**

> **TCU (Terminal Control Unit):** tracking controller
>
> **NCU (Network Control Unit):** communication box
>
> **SWM (Smart Weather Monitor):** smart weather station
>
> **SCADA (Supervisory Control And Data Acquisition):** data monitoring
> platform
>
> **LoRa (Long Range Radio):** A solution relates to an ultra-long
> distance wireless transmission scheme based on spread spectrum
> technology
>
> **ZigBee:** A wireless network protocol for low-speed and
> short-distance transmission
>
> **Modbus TCP:** Ethernet-based fieldbus protocol
>
> **Modbus RTU:** Fieldbus protocol based on serial 485 communication

**\
**

3.  []{#_Toc133163462 .anchor}**System Architecture**

![](media/image3.png){width="5.768055555555556in"
height="5.034722222222222in"}

**Figure 3-1 System architecture diagram**

The above diagram is the architecture diagram of the intelligent
tracking control system of Lingyang Technologies. A complete tracking
control system includes: 1 set of NCU, 1 set of SWM, multiple sets of
TCUs and the operation and maintenance platform (SCADA).

Aspects of data communication, mainly including data collection, data
transmission, data output storage and data recall. The information is
transmitted from TCU to NCU based on wireless LoRa or ZigBee, whose
underlying data acquisition protocol is based on Modbus. If the site
applies Lingyang intelligent NCU, the NCU aggregates all the information
and then transmits to the cloud SCADA through 4G with the MQTT protocol.
If the site applies traditional NCU, a general data protocol converter
is required to convert the bottom level aggregated device data into the
MQTT protocol, and the cloud SCADA presents it to the front-end of SCADA
through the MQTT protocol. Besides, the data on SCADA can also be called
by third-party users through the Restfull API interface. The specific
data communication flow chart is as follows.

![0b1bfe5855f636e1fb987fa84e8d06d](media/image4.png){width="5.768055555555556in"
height="3.986111111111111in"}

**Figure 3-2 Communication architecture diagram**

4.  []{#_Toc133163463 .anchor}**TCU**

[]{#_Toc133163464 .anchor}**4.1 Product Instruction**

The TCU is a motor drive controller of the PV trackers, which is the
core actuator. It will automatically calculate the sun's azimuth and
height angle of the location at any moment according to the local
longitude, latitude and time. In the auto mode, the system will
automatically issue tracking commands according to the current position
of the sun, and control the rotation of the motor, so that the PV
modules will always track the sun at the best tilt angle in real time
and thus increase the power generation.

The intelligent TCU also has several smart functions, such as wireless
one-key upgrade, wireless self-grouping network, anti-shadow tracking,
high wind, rain and snow protection, over-current and over-temperature
protection, etc. Meanwhile, it can also be seamlessly connected with NCU
and SCADA for fast configuration. There are two types of TCU, one is
powered by AC and the other one is powered by PV string and it's backup
battery. The AC power supply one requires AC 90V\~264V power supply from
the project site, and the PV string power supply one can take power
directly from the PV string, with input voltage range from 250V to1500V
DC, about it's backup battery, we have a built-in self-developed BMS
intelligent charge and discharge management module, it can charge the
battery with constant current and voltage under different ambient
temperatures and protect the battery intelligently to prevent the
battery from overcharging or over discharging, so as to improve the life
and efficiency of the battery, further enhance the stability and
reliability of the control system.

![img_v2_a4141ad0-cc81-402d-b3d6-ef57aac55aeg](media/image5.jpeg){width="5.761805555555555in"
height="2.504861111111111in"}

**Figure 4-1 TCU appearance**

[]{#_Toc133163465 .anchor}**4.2 Operation Mode**

The operation modes of TCU include manual mode, auto mode and protection
mode.

[]{#_Toc133163466 .anchor}**4.2.1 Manual Mode**

**(1) East**

In the manual mode, the TCU can be controlled to run eastward by
pressing the buttons on its surface or by operating the upper computer
debugging software. the TCU will keep rotating eastward until the east
limit is reached or other operations are performed.

(2) **West**

In the manual mode, the TCU can be controlled to run westward by
pressing the buttons on its surface or by operating the upper computer
debugging software. the TCU will keep rotating westward until the west
limit is reached or other operations are performed.

(3) **Flat**

In the manual mode, the TCU can be controlled to set flat by pressing
the buttons on its surface or by operating the upper computer debugging
software.

(4) **Stop**

In the manual mode, the TCU can be controlled to stop by pressing the
buttons on its surface or by operating the upper computer debugging
software.

[]{#_Toc133163467 .anchor}**4.2.2 Auto Mode**

The auto mode is the most common mode of operation, in which the TCU
will automatically control the tracker to follow the sun without human
intervention. Under normal operating conditions, the automatic mode
follows the following operating rules:

(1) After sunset and before sunrise:

-   The TCU will run to a preset night return protection angle
    (generally in a flat state)

(2) After sunrise and before sunset:

-   If there is no mutual shadow among panels, the tracker will work
    according to the angle calculated by the astronomical algorithm;

-   If there is a shadow among panels, the tracker will work according
    to the backtracking algorithm.

\(3\) Only the difference between the target angle and the actual angle
is bigger than the preset tracking accuracy, TCU starts controlling the
tracker to move.

[]{#_Toc133163468 .anchor}**4.2.3 Protection Mode**

**(1) Wind protection**

Wind protection mode includes normal wind protection mode and
multi-level wind protection mode. In order to ensure the safety of the
power station in windy weather, the controller can set the parameter of
start wind speed, stop wind speed and limited time, as well as hign wind
protection angle. The specific setting method can be referred to the
manually setting below and computer software debugging.

(2) **Over current protection**

The current value (mA) for over-current protection can be set, and the
TCU stops working when the operating current is greater than the pre-set
value.

(3) **Snow protection**

Snow protection angle can be set, the TCU operates to a preset angle in
snowy weather.

(4) **Night return protection**

When the TCU is in the night return mode, the TCU runs to the preset
angle (generally in a flat state).

[]{#_Toc133163469 .anchor}**4.3 Manual Setting**

  ------------ ----------- ---------------------- ------------------------------
  **Number**   **Range**   **Content**            **Description**

  001          85，170     Motor forward and      Calibrating east-west
                           reverse direction      direction

  002          1-255       Equipment number       Values are recommended set
                                                  below 168

  003          1-255       NCU network ID         Setting value according to the
                                                  corresponding communication
                                                  box

  004          0-15        NCU wireless frequency Setting value according to the
                                                  corresponding communication
                                                  box

  005          3，4        Node type              3 is relay routing, 4 is
                                                  endpoint (If there is no
                                                  ultra-long distance or
                                                  unstable TCU communication
                                                  case, the default value is 4)

  006          10-30       Protection wind speed  

  007          30-60       Maximum rotation angle 

  008                      calibreta the          Caliberate the angle deviation
                           deviation between TCU  between the north icon and
                           and true north         true north direction
  ------------ ----------- ---------------------- ------------------------------

**Table 4-1 TCU Parameter Setting Description Table**

(1) Calibrate motor direction

![](media/image6.png){width="3.7117180664916885in"
height="2.287373140857393in"}

-   Click the \"West\" or \"East\" button, the machine rotates to the
    corresponding west or east direction, which means the installation
    direction is correct. And then clicking the \"Set flat\" button, the
    machine will set flat.

![](media/image7.png){width="3.5615682414698164in"
height="2.210169510061242in"}

-   If the motor direction is wrong, please click the \"Stop\" and
    \"Auto\" buttons at the same time to enter the setting mode.

-   Click the \"West\" or \"East\" button to switch the motor direction
    (the number \"85\" and \"170\" represent two opposite directions).
    Click \"Set flat\" button to confirm the switch of motor direction,
    and then click \"Auto\" button to exit the setting mode.

-   Re-click the \"West\" or \"East\" button to test the motor
    orientation.

(2) Equipment number

![](media/image8.png){width="3.655939413823272in"
height="2.2874945319335085in"}

-   Click the \"Stop\" and \"Auto\" buttons at the same time to enter
    the setting mode, and then click the \"Stop\" button again to enter
    the second interface to set controller extension equipment number.

-   Click the \"West \" or \"East\" button to switch the value. The mode
    can be set from 1 to 255 in turn, and it is recommended not to
    exceed 168.

-   Click \"Set flat\" button to confirm the switch of equipment number,
    and then click \"Auto\" button to exit the setting mode.

Note: The TCU's equipment number should be unique under same NCU.

Note: (3)、(4)、(5) is network settings, please open the communication
box to check the network parameters before setting.

(3) NCU network ID

![](media/image9.png){width="3.5230063429571303in"
height="2.20587489063867in"}

-   After entering the setting mode, please click the \"Stop\" button to
    enter the third interface to set the communication box network ID.

-   Click the \"West \" or \"East\" button to switch the value. The mode
    can be set from 1 to 255 in turn.

(4) NCU wireless frequency

![](media/image10.png){width="3.557823709536308in"
height="2.201258748906387in"}

-   After entering the setting mode, please click the \"Stop\" button to
    enter the fourth interface to set the communication box wireless
    frequency point.

-   Click the \"West \" or \"East\" button to switch the value. The mode
    can be set from 0 to 15 in turn.

(5) Node type

![](media/image11.png){width="3.418264435695538in"
height="2.1136778215223098in"}

-   After entering the setting mode, please click the \"Stop\" button to
    enter the fifth interface to set the node type.

-   Click the \"West \" or \"East\" button to switch the value. 3 is
    relay route, 4 is terminal node (If there is no ultra-long distance
    or unstable communication case, the default value is 4).

-   Click the \"Set flat\" button to save the device communication
    settings. And then wait 5 seconds until the stop light on, which
    means that the network communication is set up successfully.

Note: (3)、(4)、(5)need to be confirmed together. It is needed to
complete the TCU's setting first, then turn on the communication box to
avoid signal interference causing failure of communication setting.

(6) Protection wind speed

![](media/image12.png){width="3.4560586176727908in"
height="2.144035433070866in"}

-   After entering the setting mode, please click the \"Stop\" button to
    enter the sixth interface to set the protection wind speed.

-   Click the \"West \" or \"East\" button to switch the value. The
    default value is 18m/s, and the setting range is from10 to 30m/s.

-   Click the \"Set flat\" button to confirm the protection wind speed
    setting.

(7) maximum rotation angle

![](media/image13.png){width="3.1021751968503937in"
height="1.9165583989501311in"}

-   After entering the setting mode, please click the \"Stop\" button to
    enter the 7th interface to set the maximum rotation angle.

-   Click the \"West \" or \"East\" button to switch the value, and the
    setting range is from 30 to 60 degrees.

-   Click the \"Set flat\" button to confirm the maximum rotation angle
    setting. Click the \"Auto\" button to exit the setting mode, and the
    TCU runs the auto mode.

(8) Beam angle correction

![](media/image14.png){width="3.164297900262467in"
height="1.9477712160979876in"}

-   After entering the setting mode, please click the \"Stop\" button to
    enter the 8th interface to set the caliberation angle between north
    direction icon and truth north icon.

-   Click the \"West\" or \"East\" button to switch the value.

-   Click the \"Set flat\" button to save the setting. Click the
    \"Auto\" button to exit the setting mode, and the device enters the
    auto mode.

[]{#_Toc133163470 .anchor}**4.4 Software Commissioning**

[]{#_Toc133163471 .anchor}The TCU Tool software is monitoring and
maintenance software platform for TCU, which enables users to control
the TCU more conveniently and comprehensively, so as to complete the
operation and maintenance of the tracker more efficiently and safely.
The software can achieve the real-time data collection and monitoring,
fault warning, parameter modifying, such as monitoring and modifying the
tracker real-time angle data, wind protection angle data , snow
protection angle data, limit protection angle data and so on.

**4.4.1 Communication Connection**

**(1) Wired connection**

Connect the computer and TCU by wired debugging cable. Open the TCU Tool
software and turn on the TCU.

![](media/image15.png){width="5.080588363954505in"
height="3.4157655293088363in"}

**Figure 4-2: TCU Tool wired connection step**

1.  Select the correct Port on TCU Tool；

2.  Write the equipment number of the TCU into TCU Tool；

3.  Click the \"Connect\" button

```{=html}
<!-- -->
```
(2) **Wireless connection**

![](media/image16.png){width="4.782701224846894in"
height="3.2377373140857393in"}

**Figure 4-3: TCU Tool wireless connection step**

Insert the USB port of the wireless debugging module into the computer.
Open the TCU Tool software:

1.  Select the correct Port；

2.  Click the![](media/image17.png){width="0.3541666666666667in"
    height="0.2777777777777778in"}button, follow the guide to set the
    network ID and wireless frequency points into the wireless debugging
    module according to the corresponding network ID and wireless
    frequency points on TCU, save and restart (Note: the first reading
    of the original configuration parameters of the wireless debugging
    module may not be successful, please try again)；

3.  Write the correct Equipment No；

4.  Click on the \"connect\" button

```{=html}
<!-- -->
```
3.  **Mobile phone bluetooth connection**

    Turn on the TCU with bluetooth module; Turn on the TCU Tool mobile
    phone software；(Please confirm with the supplier whether your TCU
    comes with a Bluetooth module)

```{=html}
<!-- -->
```
1.  Click on the \'My\' button at the bottom right.

    ![](media/image18.png){width="2.109722222222222in"
    height="2.704861111111111in"}

2.  Click on the \"Debugging mode \"

    ![](media/image19.png){width="2.1180555555555554in"
    height="4.423611111111111in"}

3.  Click on \"Scan QR Code \" and scan the QR code of the corresponding
    TCU to connect.

    ![](media/image20.png){width="2.0305555555555554in"
    height="3.4298611111111112in"}[]{#_Toc133163472 .anchor}

**4.4.2 Overview of the TCU Tool software**

![](media/image21.png){width="5.050648512685914in"
height="3.395113735783027in"}

**Figure 4-4: TCU Tool interface after successful connection**

**The upper right corners of the main interface are:**

![](media/image22.png){width="0.3472222222222222in"
height="0.3402777777777778in"}: Settings

![](media/image23.png){width="0.3402777777777778in"
height="0.3472222222222222in"}: Wireless debugging module configuration
tool

![](media/image24.png){width="1.2361111111111112in"
height="0.3402777777777778in"}: Language

![](media/image25.png){width="0.375in" height="0.3541666666666667in"}:
Dark background

![](media/image26.png){width="0.36875in"
height="0.31805555555555554in"}: Updating

If this icon appears, indicating that a new version exists, please
update it timely. This function needs to be connected to the Internet.

**The left side of the main interface:**

Interval: You can change the communication cycle between the computer
and the TCU here.

Action Logs: Record each modification action, the equipment number of
the TCU and the time of the modification.

**Main interface:**

-   Search: Look for the information you need

-   Chart: Enter the chart mode

-   Initialize equipment: Enter the corresponding interface to
    initialize control box.

-   Click ![](media/image27.png){width="0.2638888888888889in"
    height="0.2777777777777778in"} button to generate the chart 1 in
    Chart mode, click
    ![](media/image28.png){width="0.2847222222222222in"
    height="0.2777777777777778in"} button to generate the chart 2.

-   Work Mode: Show the current working mode of TCU, and you can also
    transform the working mode of TCU here.

-   Equipment Exception: Show the abnormal statement of the equipment

-   Theoretical Sun Angle: According to the built-in astronomical
    algorithm, real-time display of the current solar azimuth and height
    angle.

-   Equipment Angle Data:

```{=html}
<!-- -->
```
-   Target Angle: The target Angle is the best tracking Angle calculated
    > by the TCU's built-in algorithm

-   Measured Angle: the actual Angle of TCU

-   System time: Display the time information in TCU.

-   Geographical Information: Show the longitude, latitude and time zone
    > set in TCU.

-   Wireless Settings: Displays the network ID, wireless frequency
    > point, and node type set in TCU

-   Tracking Accuracy: Display the tracking accuracy information set in
    > the control box.

-   Remote target Angle :in AI mode, the control box tracks according to
    > the remote target Angle given by NCU. (Only can see after opening
    > the developer option)

-   Wind Speed: Displays the wind speed information.

-   Motor Current: Displays the current value to motor.

-   Motor Direction: Shows the motor rotation direction information,
    > here you can change the rotation direction of the motor

-   Motor Speed: Motor rotation speed can be controlled by adjusting the
    > motor speed value.

-   Protection Parameters: Show the TCU's target Angle in night return
    > mode, wind protection mode, heavy snow protection mode; and you
    > can see the default overcurrent protection value.

-   Panel parameters: Displays the width of the photovoltaic panel;
    > display the distance between each tracker. Upper slope and lower
    > slope can be configured in the future.

-   Wind protection and limits:

> Wind protection mode includes ordinary wind protection and multi-level
> wind protection; About multi-level wind protection mode, at most five
> level wind protection parameter can be set in multi-level wind
> protection mode, and the difficulty of entering these five wind speed
> Protection mode increases step by step. At the same time, the tracking
> angle limit of TCU will become larger and larger.

-   Wind mode start speed: A necessary condition for entering wind
    > protection mode

-   Wind mode start time: A necessary condition for entering wind
    > protection mode

-   Wind Mode Terminate Speed: A necessary condition for exiting wind
    > protection mode

-   Wind Mode Terminate Time: A necessary condition for exiting wind
    > protection mode

-   East limit: the maximum angle that trackers can rotate to east.

-   West limit: the maximum angle that trackers can rotate to west.

-   Azimuth deviation: the angle difference between the actual position
    > of tracker and the true north direction. Write the actual
    > deviation value can improve the tracking accuracy of TCU and
    > optimize the antishadow algorithm.

-   East-west slope deviation: the height difference between the panels
    > of two trackers in the east and west direction. Write the actual
    > deviation value can improve the tracking accuracy of TCU and
    > optimize the antishadow algorithm.

-   North-south slope deviation: the height difference between the
    > panels of two trackers in the north and south direction. write the
    > actual deviation value can improve the tracking accuracy of TCU
    > and optimize antishadow algorithm.

-   Clean Mode Angle: Target angle in Clean mode.

-   Calibration deviation: Due to the construction error, the TCU may
    > not be completely parallel to the photovoltaic panel after
    > installation. Input deviation value can improve the tracking
    > accuracy of the TCU and optimize the anti-shadow algorithm.

-   Communication timeout: if TCU cannot receive the signal of NCU
    > within the pre-set time, it will automatically set flat. If the
    > time is setted to 0s, this function is turned off.

[]{#_Toc133163473 .anchor}**4.4.3 TCU tool chart function**

Click the \"Chart\" button to enter the chart interface, and you can see
the data in a table.Click the
![](media/image29.png){width="0.3055555555555556in"
height="0.2569444444444444in"} button to export the data to the local
area.

![](media/image30.png){width="4.772636701662292in"
height="3.2311745406824146in"}

**Figure 4-6 TCU Tool's chart interface**

[]{#_Toc133163474 .anchor}**4.4.4 TCU Tool: Initialize equipment**

Users can set the parameters in this interface and save them locally.
After connecting the control box, we can click \"Apply\" on the right to
modify the corresponding parameters, or click the red \"Apply All\"
button to write all parameters into the control box at once.

When connecting the wireless debugging module, after setting the
corresponding network ID and the wireless frequency point, choose the
Equipment No then you can rewrite the parameters to corresponding TCU.

Note: we can send new parameters to all TCUs under the same network ID
and wireless frequency point if we set \"0\" to Equipment No.

![](media/image31.png){width="4.822852143482065in"
height="3.2521959755030623in"}

**Figure 4-7 TCU Tool device initialization interface**

[]{#_Toc133163475 .anchor}**4.4.5 Other**

Click![](media/image32.png){width="0.2902777777777778in"
height="0.2659722222222222in"}button to enter the setting mode, click
\"Display\" to select another page display form.

![](media/image33.png){width="4.954861111111111in"
height="3.3354166666666667in"}

**Figure 4-8 Setting mode page**

![](media/image34.png){width="4.9319444444444445in"
height="3.3159722222222223in"}

**Figure 4-9: Another page display form of TCU Tool**
