| **3151 LSST**               |                     |
|-----------------------------|---------------------|
| **Requested by:**           | **LSST**            |
| **Doc. Code / Version nº:** | Doc. Code / Version |
| **Editor:**                 | Julen Garcia        |
| **Approved by:**            |                     |
| **Date:**                   | 01/09/2019          |

\| INDEX \|

[1. Introduction 5](#introduction)

[2. Reference documents 6](#reference-documents)

[3. TCP Client 7](#_Toc56073546)

[3.1 Component Configuration 7](#_Toc56073547)

[3.2 Sender.lvclass 7](#_Toc56073548)

[3.2.1 Configuration file explained 7](#_Toc56073549)

[3.2.2 Task process 8](#_Toc56073550)

[3.2.3 Task methods 8](#_Toc56073551)

[3.2.4 CMD Reception loop 10](#_Toc56073552)

[4. TMA Commanding 23](#tma-commanding)

[4.1 CppAppCommand.lvclass 24](#cppappcommand.lvclass)

[4.1.1 Methods 24](#methods)

[4.1.2 Childs 27](#childs)

[4.2 GetEventfromTMA.lvclass 32](#geteventfromtma.lvclass)

[4.2.1 Task process 32](#task-process)

[4.2.2 Task methods 33](#task-methods)

[4.2.3 CMD Reception loop 34](#cmd-reception-loop)

[4.3 TMAOMTMonitoring task 41](#tmaomtmonitoring-task)

[4.3.1 Task process 41](#task-process-1)

[4.3.2 Task methods 42](#task-methods-1)

[4.3.3 Main loop 44](#main-loop)

[4.4 CommanderCheck task 59](#commandercheck-task)

[4.4.1 Task process 59](#task-process-2)

[4.4.2 Task methods 60](#task-methods-2)

[4.4.3 Main loop 62](#main-loop-1)

[5. Alarm Management 74](#_Toc56073569)

[5.1 Alarm Recpetion Task.lvclass 74](#_Toc56073570)

[5.1.1 Task process 75](#_Toc56073571)

[5.1.2 Methods 75](#_Toc56073572)

[5.1.3 CMD Reception loop 77](#_Toc56073573)

\| DOCUMENT HISTORY \|

| **Version** | **Date** | **Author** | **Comments** |
|-------------|----------|------------|--------------|
|             |          |            |              |
|             |          |            |              |
|             |          |            |              |
|             |          |            |              |
|             |          |            |              |

Introduction
============

This document contains the different components related to the commands and
events from the TMA that are:

-   TCP Client: this component is the one that connects to the TMA over TCP to
    send and receive the TCP messages. The message to send is specified to the
    task by a public method of the TCP client object, and the received messages
    are published in a user event created when the object is initialized.

-   TMA Commanding: this component is the one sending commands to the TMA and
    monitoring the events received from it. This is done using the TCP Client
    component.

-   Alarm Management: this component gets the alarm and warning events from the
    TCP client, logging the received alarms and warnings to have a record of the
    different events occurred while operating the system.

Reference documents
===================

| **Nº** | **Document** | **Code** | **Version** |
|--------|--------------|----------|-------------|
| **1**  |              |          |             |
| **2**  |              |          |             |
| **3**  |              |          |             |
| **4**  |              |          |             |
| **5**  |              |          |             |

TMA Commanding
==============

This component is the one sending commands to the TMA and monitoring the events
received from it. This is done using the TCP Client component and some other
tasks that will be explained in this section. This component has two main
different parts:

-   Command sending: this means generating the TCP message to send to the TMA
    using the TCP Client. This is done using the CppAppCommand.lvclass. This
    class is the one that generates the TCP message for each specific command
    and sends them to the TMA using the TCP Client.

-   Event reception: this means getting the messages from the TMA over the TCP
    Client and generating the corresponding events inside the HMI application.
    This is done using the GetEVENTFromTMA.lvclass. This class is responsible of
    parsing the events from the TMA received by the TCP Client and generate the
    corresponding events inside the HMI application.

In addition to these two classes there are two other classes inside the TMA
Commanding component. These are:

-   TMAOMTMonitoring.lvclass: this class is used to monitor the status of the
    TMA received in the “*StateInfo*” events.

-   CommanderCheck.lvclass: this class is used to monitor the actual commander
    and generate the corresponding events to change the user interface and block
    the command sending when the actual HMI is not the commander.

Each of the classes mentioned in above are explained in the upcoming sections.

CppAppCommand.lvclass
---------------------

This class is the one that generates the TCP message for each specific command
and sends them to the TMA using the TCP Client. To do so, each subsystem has a
specific class that inherits from the CppAppCommand class and has the specific
commands available for that subsystem. The methods in the parent class, the
CppAppCommand class, are dynamic dispatch and overridden by the child classes.

### Methods

Here each of the available methods are listed.

#### CppAppCommand_Init

Initialize the command father object, this means initialize the ID repo (for
having an incremental ID different every time), the source repo and the repo for
the TCP Client (the object from the Sender.lvclass that corresponds to the TCP
Client).

Having a repo for each of this values allows the system to work without needing
to pass the parent object to all the windows, it is initialized at the
application launch and the values are stored in the repos and are available just
with an empty object of the parent class.

![](media/70937cbe32ad787c123bf2984e769e84.png)

Figure 22. CppAppCommand.lvclass_CppAppCommand_Init.vi context help.

#### CleanUp

Release queues from the different repos.

![](media/1836b222ffadf2c349e501ca25451728.png)

Figure 23. CppAppCommand.lvclass_CleanUp.vi context help.

#### Enable

Send Enable Command to TMA.

![](media/0bd68a2221b292b0572a819716bae693.png)

Figure 24. CppAppCommand.lvclass_Enable.vi context help.

#### Home

Send Command to Perform Home operation

![](media/0c97331a0b955daf960053c6034fdca9.png)

Figure 25. CppAppCommand.lvclass_Home.vi context help.

#### Move

Send Move command to TMA

![](media/baffd692dbdffd8bfe59a54cb57983d6.png)

Figure 26. CppAppCommand.lvclass_Move.vi context help.

#### MoveVelocity

Send Move Velocity command to TMA

![](media/fdae7fa9ce09673d7fdde8e999f8ee16.png)

Figure 27. CppAppCommand.lvclass_MoveVelocity.vi context help.

#### ResetAlarm

Send Reset Alarm Command to TMA

![](media/99f085b41c6679388638168c4d447f6b.png)

Figure 28. CppAppCommand.lvclass_ResetAlarm.vi context help.

#### SendCMD

This VI generates the TCP message for the specified command and with the
specified parameters and sends it to the TCP Client module to send it over TCP
to the TMA. Is meant to be used by the child classes for each of the commands
they have to send.

![](media/8610d62c6fe583826dfca68b92b003a9.png)

Figure 29. CppAppCommand.lvclass_SendCMD.vi context help.

#### SendPowerCMD

Send power command to TMA

![](media/fc49ec768230affd87d663d6d4795c6e.png)

Figure 30. CppAppCommand.lvclass_SendPowerCMD.vi context help.

#### Stop

Send Stop to TMA

![](media/959233c7aec15e1abbb3d7479ed42ae9.png)

Figure 31. CppAppCommand.lvclass_Stop.vi context help.

#### Tracking

Send Tracking Command to TMA

![](media/9768ab5b5a9ad349d0fa8a047fb9e841.png)

Figure 32. CppAppCommand.lvclass_Tracking.vi context help.

### Childs

The list of the available childs and the methods that use each of them
correspond to the existing subsystems and the corresponding commands for each
subsystem.

#### CppAppACWCMD.lvclass

This class corresponds to the commands available for the Azimuth Cable Wrap
subsystem, the methods in this class are:

-   EnableTrack

-   Move

-   MoveVelocity

-   ResetAlarm

-   SendPowerCMD

-   Stop

-   TrackTarget

#### CppAppACWDrivesCMD.lvclass

This class corresponds to the commands available for the Azimuth Cable Wrap
drives subsystem, the methods in this class are:

-   Enable

-   ReadDriveIdent

-   ResetAlarm

-   WriteDriveIdent

#### CppAppAZCMD.lvclass

This class corresponds to the commands available for the Azimuth subsystem, the
methods in this class are:

-   Home

-   Move

-   MoveVelocity

-   ResetAlarm

-   SendPowerCMD

-   Stop

-   Tracking

#### CppAppAZDrivesCMD.lvclass

This class corresponds to the commands available for the Azimuth drives
subsystem, the methods in this class are:

-   Enable

-   ReadDriveIdent

-   ResetAlarm

-   WriteDriveIdent

#### CppAppAZThermalCMD.lvclass

This class corresponds to the commands available for the Azimuth drives thermal
subsystem, the methods in this class are:

-   ControlMode

-   ReadAZThermalIdent

-   ResetAlarm

-   SendPowerCMD

-   WriteAZThermalIdent

#### CppAppBALCMD.lvclass

This class corresponds to the commands available for the Balancing subsystem,
the methods in this class are:

-   Enable

-   Move

-   MoveVelocity

-   ReadElementIdent

-   ResetAlarm

-   SendPowerCMD

-   Stop

-   WriteElementIdent

#### CppAppCabinet0101CMD.lvclass

This class corresponds to the commands available for the Cabinet 0101 subsystem,
the methods in this class are:

-   ControlMode

-   ResetAlarm

-   SendPowerCMD

#### CppAppCabinetCMD.lvclass

This class corresponds to the commands available for the Main Cabinet subsystem,
the methods in this class are:

-   ResetAlarm

-   TrackExtTemp

#### CppAppCCWCMD.lvclass

This class corresponds to the commands available for the Camera Cable wrap
subsystem, the methods in this class are:

-   EnableTrack

-   Move

-   MoveVelocity

-   ResetAlarm

-   SendPowerCMD

-   Stop

-   TrackTarget

#### CppAppCCWDrivesCMD.lvclass

This class corresponds to the commands available for the Camera Cable Wrap
drives subsystem, the methods in this class are:

-   Enable

-   MoveVelocity

-   ReadDriveIdent

-   ResetAlarm

-   WriteDriveIdent

#### CppAppCommaderCMD.lvclass

This class corresponds to the commands available for the Commander subsystem,
the methods in this class are:

-   AskCommand

-   PublishOnly

-   SystemReady

#### CppAppDPCMD.lvclass

This class corresponds to the commands available for the Deployable Platform
subsystem, the methods in this class are:

-   LockExtension

-   MoveVelocity

-   ReadPlatformIdent

-   ResetAlarm

-   SendPowerCMD

-   Stop

-   WritePlatformIdent

#### CppAppEIBCMD.lvclass

This class corresponds to the commands available for the Encoder subsystem, the
methods in this class are:

-   ClearHeadsError

-   HardwareReset

-   ReadElementIdent

-   ResetAlarm

-   SendPowerCMD

-   SendReferenceStartCMD

-   SendReferenceStopCMD

-   WriteElementIdent

#### CppAppELCMD.lvclass

This class corresponds to the commands available for the Elevation subsystem,
the methods in this class are:

-   Home

-   Move

-   MoveVelocity

-   ResetAlarm

-   SendPowerCMD

-   Stop

-   Tracking

#### CppAppELDrivesCMD.lvclass

This class corresponds to the commands available for the Elevation drives
subsystem, the methods in this class are:

-   Enable

-   ReadDriveIdent

-   ResetAlarm

-   WriteDriveIdent

#### CppAppELThermalCMD.lvclass

This class corresponds to the commands available for the Elevation drives
thermal subsystem, the methods in this class are:

-   ControlMode

-   ReadELThermalIdent

-   ResetAlarm

-   SendPowerCMD

-   WriteELThermalIdent

#### CppAppLPCMD.lvclass

This class corresponds to the commands available for the Locking Pins subsystem,
the methods in this class are:

-   LP_Free

-   LP_Lock

-   LP_Test

-   Move

-   MoveVelocity

-   ReadPinIdent

-   ResetAlarm

-   SendPowerCMD

-   Stop

-   WritePinIdent

#### CppAppMainAxesCMD.lvclass

This class corresponds to the commands available for the Main Axes subsystem,
the methods in this class are:

-   MoveToTarget

-   Stop

-   TrackTarget

#### CppAppMCCMD.lvclass

This class corresponds to the commands available for the Mirror Cover subsystem,
the methods in this class are:

-   MCClose

-   MCOpen

-   Move

-   MoveVelocity

-   ReadSectorIdent

-   ResetAlarm

-   SendPowerCMD

-   Stop

-   WriteSectorIdent

#### CppAppMCLCMD.lvclass

This class corresponds to the commands available for the Mirror Cover Locks
subsystem, the methods in this class are:

-   MCL_Close

-   MCL_Open

-   Move

-   MoveVelocity

-   ReadMirrorLockIdent

-   ResetAlarm

-   SendPowerCMD

-   Stop

-   WriteMirrorLockIdent

#### CppAppModbusTempControllersCMD.lvclass

This class corresponds to the commands available for the Modbus Temperature
Controllers subsystem, the methods in this class are:

-   FanPower

-   ReadDriveIdent

-   ResetAlarm

-   Setpoint

-   WriteDriveIdent

#### CppAppMPSCMD.lvclass

This class corresponds to the commands available for the Main Power Supply
subsystem, the methods in this class are:

-   ResetAlarm

-   SendPowerCMD

#### CppAppOSSCMD.lvclass

This class corresponds to the commands available for the Oil Supply System
subsystem, the methods in this class are:

-   ResetAlarm

-   SendAbortPoweringCMD

-   SendChangeModeCMD

-   SendCoolingPowerCMD

-   SendMainPumpPowerCMD

-   SendOilPowerCMD

-   SendPowerCMD

#### CppAppSafetyCMD.lvclass

This class corresponds to the commands available for the Safety subsystem, the
methods in this class are:

-   SafetyReleaseOverride

-   SafetyReset

-   SafetySetOverride

#### CppAppTECCMD.lvclass

This class corresponds to the commands available for the Top End Chiller
subsystem, the methods in this class are:

-   ResetAlarm

-   SendPowerCMD

-   TrackExtTemp

#### CppAppTFCMD.lvclass

This class corresponds to the commands available for the Transfer Function
subsystem, the methods in this class are:

-   ExcitationConfig

-   ResetAlarm

GetEventfromTMA.lvclass
-----------------------

This class is responsible of parsing the events from the TMA received by the TCP
Client and generate the corresponding events inside the HMI application.

### Task process

This task was created using the NI GOOP Developing Suite, this task is object
oriented and the communication between methods is done using queues and user
events. The task main is contained in the process.vi, here there is one loop.
The loop is used for CMD reception, see Figure 33. This process has only one
instance that manages all the received events from the TMA.

![](media/69459e04fb4b90c71b521567c9afc8ef.png)

Figure 33. GetEventFromTMA task

### Task methods

Here the available methods for this task are explained.

#### GetEVENTFromTMA_Init.vi

Initialize the process to hear from TMA OMT.

The initialized process will get ACK, DONE, ERROR and WARNINGS that come from
the TMA (operation_manager). It will also get the state change and error from
the TMA OMT (operation_manager) itself.

![](media/74a6d5ce8acd1e1bde61e9197fd16bad.png)

Figure 34. Task method: GetEventFromTMA_Init

#### CleanUp

Stop task process and destroy user events.

![](media/625255128b48dfab8cc2ef1aa628e8ab.png)

Figure 35. Task method: CleanUp

#### ControlProcessWindow

This VI is used to show or hide the process front panel. Depending on the
ShowProcessWindow control value.

![](media/5b09030f1c224f409c548c97e07cd083.png)

Figure 36. Task method: ControlProcessWindow

#### GetEventRefs

This VI is used to return the references to the different events: ack, done,
error and warning.

![](media/d440b222f78f8f6b8dc91f280e9453c3.png)

Figure 37. Task method: GetEventRefs

### CMD Reception loop

This loop receives the commands from the public methods and executes the
corresponding actions. This loop has a state for each method as well as some
other states used for loop managing, each state is explained in the next
sections.

#### Init

This state is just executed once and is the first executed one. Here the
initialization actions are executed. These are:

-   Initialization of local variable

-   Registration of events for the event structure

-   Updating the displayed name of the VI for debug purposes

![](media/7594b113e7a74001bbb6be57518ab3a3.png)

Figure 38. GetEVENTFromTMA.lvclass_Process.vi Init

#### Idle

This state is executed constantly after executing every new CMD, here the events
created at the methods are received and executed in the next iteration, in the
event called “EventsToProcess” see Figure 40.

![](media/fd6414a2c920162a8a200737a0b5767b.png)

Figure 39. GetEVENTFromTMA.lvclass_Process.vi Idle

![](media/0ffa46f585c98b683a6134b3b50cf30d.png)

Figure 40. GetEVENTFromTMA.lvclass_Process.vi EventsToProcess

In addition to this, the TCP messages received from the TCP Client as user
events are received and parsed, the event that receives the TCP messages is
called “DataFromTCP” see Figure 41. The TCP messages are parsed and the
appropriate user events are generated, these events are:

-   Ack event: this event is used to transmit the received ack or no ack events.

-   Done event: this event is used to transmit the done events.

-   Error event: this event is used to transmit the received fault events.

-   Warning event: this event is used to transmit the received warning events.

-   Version event: this event is used to transmit the version from the TMA.

With some messages no user events are generated. These cases are:

-   StateInfo: when this message is received the state of the TMA is updated
    using the methods from the TMAOMTMonitoring.lvclass. For “State” submessages
    from the StateInfo messages the state of the TMA statemachine must be
    updated, to do so the PublishTMAOMTState method from the
    CommanderCheck.lvclass is used.

-   InPosition: not used in the HMI application.

![](media/f00e25525f869863dacc2c4672994d39.png)

Figure 41. GetEVENTFromTMA.lvclass_Process.vi DataFromTCP

#### Timeout

This state is executed when there is something that must be executed in the
specified timeout of the Idle state event structure, see Figure 42.

![](media/ffea54a30df54a93c2d371689872356d.png)

Figure 42. Timeout state from Idle state

![](media/584b251ad792650692f863fd0386d08b.png)

Figure 43. GetEVENTFromTMA.lvclass_Process.vi Timeout

#### ShowWindow

This state is used to show the front panel of the process.

![](media/e69d2d974ee18af4bb649e21db92269b.png)

Figure 44. CMD Receiver states: ShowWindow

#### HideWindow

This state is used to hide the front panel of the process.

![](media/4804276518cb9f6e2c8f0ccbdc7abf31.png)

Figure 45. CMD Receiver states: HideWindow

#### Shutdown

This state is reached when the shutdown CMD is received. This loop is used to
stop the CMD receiver loop.

![](media/b2b82c4abc519e2523f469bacf952aa8.png)

Figure 46. GetEVENTFromTMA.lvclass_Process.vi shutdown

#### Error

This state is reached when an error occurs at the task, here the error is
published and cleared for the next iteration.

![](media/16549f5ba20f1abe2e0b7bb142b70442.png)

Figure 47. GetEVENTFromTMA.lvclass_Process.vi Error

TMAOMTMonitoring task
---------------------

To be updated

This task is responsible of monitoring the TMA OMT status.

Here the CSC connection status, CSC commands, CSC Events and TMA OMT actual
state are received and saved.

### Task process

This task was created using the NI GOOP Developing Suite, this task is object
oriented and the communication between methods is done using queues and user
events. The task main is contained in the process.vi, here there is a single
loop. This loop contains both the CMD reception and the CMD actions execution.
This process has only one instance that manages all the received data.

![](media/61555585148d675193b203cfbb113f09.png)

Figure 48. TMAOMTMonitoring task

### Task methods

Here the available methods for this task are explained.

#### TMAOMTMonitoring_Init.vi

This VI is used to launch the process.

![](media/591d405c4f711bb40656637b9a885def.png)

Figure 49. Task method: TMAOMTMonitoring \_Init

#### CleanUp

This VI is used to stop the task and release all the references generated for
this task.

![](media/66ac09dc52f60ca146973034097794b8.png)

Figure 50. Task method: CleanUp

#### ControlProcessWindow

This VI is used to show or hide the process front panel. Depending on the
ShowProcessWindow control value.

![](media/97c7b83d936bca9900fdff7fe8ee0ced.png)

Figure 51. Task method: ControlProcessWindow

#### NewTMAState

This VI is used to send the new TMA OMT state string to the process and update
the memory.

![](media/98e0fabce511f820847f3a6cc4ce20f4.png)

Figure 52. Task method: NewTMAState

#### SetTCSCommStatus

This VI is used to send the TCS connection status to the process and update the
memory.

![](media/10fc86376d1e4c4d13a8829a84b41074.png)

Figure 53. Task method: SetTCSCommStatus

#### GetTMAStatus

This VI is used to get the status of the TMA OMT from the task.

![](media/56e46ec31ea69914ca986d8406542515.png)

Figure 54. Task method: GetTMAStatus

#### NEWTCSCmd

This VI is used to send the last command received from the TCS to the process
and update the memory.

![](media/e5b59d1fdb07ed4928c03eb607ce114d.png)

Figure 55. Task method: SendDONE

#### GetCMDHistory

This VI is used to get the last 100 CMDs received from the TCS.

![](media/a25363da62efc80d885ff6dab7c0f329.png)

Figure 56. Task method: GetCMDHistory

#### GetEventHistory

This VI is used to get the last 100 events received from the TCS.

![](media/07e3f8bbdee245d20ef852de67f1f7dd.png)

Figure 57. Task method: GetEventHistory

#### NEWTCSEvent

This VI is used to send the last event received from the TCS to the process and
update the memory. This is not used for error or waring events.

![](media/2cc603df606ab033bc958043cf61fb45.png)

Figure 58. Task method: 4.7.2.10 NEWTCSEvent

### Main loop

This loop receives the CMDs from the methods and executes them in the same loop.
The different states of the loop are explained in the following sections.

#### Init

Here the initialization actions are executed.

![](media/e404f638668268d3073bc082a1b7e253.png)

Figure 59. Loop states: Init

#### Idle

This state is executed constantly after executing every new CMD, here the events
created at the methods are received and executed in the next iteration.

![](media/3f276b7d1d394e2dbddc70e78ad4a42d.png)

Figure 60. Loop states: Idle

#### Timeout

This state is executed when there is something that must be executed in the
specified timeout of the Idle state event structure, see Figure 61.

![](media/5a9d7a30173c7114968eb1d6689ee85b.png)

Figure 61. Timeout state from Idle state

![](media/ea096d17d2afe10dc90a83d631c2de5a.png)

Figure 62. Loop states: Timeout

#### ShowWindow

This state is used to show the front panel of the process.

![](media/2fc66861285c454098d66de610dc1d43.png)

Figure 63. Loop states: ShowWindow

#### HideWindow

This state is used to hide the front panel of the process.

![](media/b2c602d1f37e364f2519be97641fca53.png)

Figure 64. Loop states: HideWindow

#### NEWTMAStateSendEVENT

This state is executed when the NEWTMAState method is used. Here the received
string from the calling method is saved into the TMA Status Local Var register
as current state.

![](media/7539d8c63f7a1e88c8810c815c73e6c0.png)

Figure 65. Loop states: NEWTMAStateSendEVENT

#### SetConnStatus

This state is executed when the SetTCSCommStatus method is used. Here the
received boolean from the calling method is saved into the TMA Status Local Var
register as current connection status.

![](media/774331c6ab5684de8240990d7f7ba975.png)

Figure 66. Loop states: SetConnStatus

#### NewTCSCMD

This state is executed when the NewTCSCMD method is used. Here the received
structure from the calling method is saved into the CMD history queue and TMA
Status Local Var register as last CMD.

![](media/403bb5144ba657673d60392a0fe8fb44.png)

Figure 67. Loop states: NewTCSCMD

#### NewTCSEvent

This state is executed when the NewTCSEvent method is used. Here the received
structure from the calling method is saved into the Event history queue and TMA
Status Local Var register as last CMD.

![](media/305ef9ff7e5a8239909ad5dc6dc96e20.png)

Figure 68. Loop states: NewTCSEvent

#### GetStatus

This state is executed when the GetTMAStatus method is used. Here the TMA Status
Local Var register value is used as response to the calling method, the response
is given at the “QueueFromProcess” queue.

![](media/8465a63d0dc07265ad781d432b22c197.png)

Figure 69. Loop states: GetStatus

#### GetCMDHistory

This state is executed when the GetCMDHistory method is used. Here the CMD
History queue elements are used as response to the calling method, the response
is given at the “QueueFromProcess” queue.

![](media/7033fdb8f196da0925147de5dbc07ebe.png)

Figure 70. Loop states: GetCMDHistory

#### GetEventHistory

This state is executed when the GetEventHistory method is used. Here the Event
History queue elements are used as response to the calling method, the response
is given at the “QueueFromProcess” queue.

![](media/559d9e03a710c7c09fb57fa908b722fd.png)

Figure 71. Loop states: GetEventHistory

#### Error

This state is reached when an error occurred at the loop. Here the error is
posted to the event from process to be handled by the general error handler.

![](media/2278779df52a382906053f19e6d48960.png)

Figure 72. Loop states: Error

#### Shutdown

This state is reached when the shutdown CMD is received. This loop is used to
stop the loop and close the connection to the TMA OMT.

![](media/8cd88c931dc08cc2714ec3df401d5a0f.png)

Figure 73. Loop states: Shutdown

CommanderCheck task
-------------------

To be updated

This task will be waiting for commands form outside and, in the timeout, will
check the actual commander var. If the commander has changed an event will be
placed in Events from process with new data.

### Task process

This task was created using the NI GOOP Developing Suite, this task is object
oriented and the communication between methods is done using queues and user
events. The task main is contained in the process.vi, here there is a single
loop. This loop contains both the CMD reception and the CMD actions execution.
This process has only one instance that manages all the received data.

![](media/7363c779e5996d2aacc057f4407333e2.png)

Figure 74. CommanderCheck task

### Task methods

Here the available methods for this task are explained.

#### CommanderCheck_Init

This VI is used to launch the process, to do so some inputs are required:

-   Commander Variable URL: a string with the URL of the CommanderVariable that
    the process will use to know if the commander has changed and how is the new
    commander.

-   Status Variable URL: a string with the URL of the Status Variable that the
    process will use publish the last TMA OMT status.

![](media/6445a816c30524906950111a5620ae4a.png)

Figure 75. Task method: CommanderCheck_Init

#### CleanUp

This VI is used to disconnect vars, stop the task and release all the references
generated for this task.

![](media/4e9fbe23e7cb1efe91ee0810167ea38c.png)

Figure 76. Task method: CleanUp

#### ControlProcessWindow

This VI is used to show or hide the process front panel. Depending on the
ShowProcessWindow control value.

![](media/30d5ec3d899eeea67e04c0db3267d019.png)

Figure 77. Task method: ControlProcessWindow

#### GetCommander

This VI is used to get the current commander. It also tells if the status is
valid or not, but only after the first read of the variable is done.

![](media/7b472a80d4577e51c0a8e1adabd81b67.png)

Figure 78. Task method: GetCommander

#### DisconnectVars

This VI is used to disconnect the shared variables used by the task.

![](media/1a136d96bfac47b2d0a766d0447251d9.png)

Figure 79. Task method: DisconnectVars

#### ConnectVars

This VI is used to connect the shared variables used by the task.

![](media/6a4c984904a47ab42fa91a0a42a09e38.png)

Figure 80. Task method: ConnectVars

#### GetCommanderEventRef

This VI is used to get the ref to the events triggered by the task.

![](media/90a3e010b6f5fa114a117c02d96f31f8.png)

Figure 81. Task method: GetCommanderEventRef

#### PublishTMAOMTState

This VI is used to send the last TMA OMT state to the process and update the
Status Variable.

![](media/3454716cf4495c3c41b9fde5d6d892de.png)

Figure 82. Task method: PublishTMAOMTState

#### ActivateChecking

This VI is used to activate or de activate the checking of the commander.

![](media/efce932ef4109ace1691c863d16817fb.png)

Figure 83. Task method: ActivateChecking

### Main loop

This loop receives the CMDs from the methods and executes them in the same loop.
The different states of the loop are explained in the following sections.

#### Init

Here the initialization actions are executed.

![](media/1ea4095f5e2b5e5782690ca248d4f8aa.png)

Figure 84. Loop states: Init

#### Idle

This state is executed constantly after executing every new CMD, here the events
created at the methods are received and executed in the next iteration.

![](media/94dff4cabdad59c7848c32bf5205d93e.png)

Figure 85. Loop states: Idle

#### Timeout

This state is executed when there is something that must be executed in the
specified timeout of the Idle state event structure. In this timeout, see Figure
87, the commander is checked by reading the commander variable. If this variable
has new data a commander change event is triggered.

![](media/54afc2c7b6ee04f85475d3177f166395.png)

Figure 86. Timeout state from Idle state

![](media/e204a041042024141355e9d762050a89.png)

Figure 87. Loop states: Timeout

#### ShowWindow

This state is used to show the front panel of the process.

![](media/2fc66861285c454098d66de610dc1d43.png)

Figure 88. Loop states: ShowWindow

#### HideWindow

This state is used to hide the front panel of the process.

![](media/b2c602d1f37e364f2519be97641fca53.png)

Figure 89. Loop states: HideWindow

#### ConnectVars

This state is executed when the ConnectVars method is used. Here the variable
references specified at the init are connected.

![](media/86d3fa8d2bb302e9f0a6efee90b87e06.png)

Figure 90. Loop states: ConnectVars

#### DisconnectVars

This state is executed when the DisconnectVars method is used. Here the variable
references are disconnected.

![](media/e03edfd2a04d8a7ae3375169b16d81ee.png)

Figure 91. Loop states: DisconnectVars

#### GetCommander

This state is executed when the GetCommander method is used. Here the actual
commander register is used as response to the calling method, the response is
given at the “QueueFromProcess” queue.

![](media/2556b04ad4da512c4ae8a85a3355c6b8.png)

Figure 92. Loop states: GetCommander

#### PublishState

This state is executed when the PublishTMAOMTState method is used. Here the
string specified at the method is written to the status connection variable.

![](media/8d24f3d5fa9066546bd43ce0765083aa.png)

Figure 93. Loop states: PublishState

#### ActivateChecking

This state is executed when the ActivateChecking method is used. Here the
Boolean value specified to the method is used to set the value of the
CheckCommander? Local variable.

![](media/42d9e52be937bdc9663b176537c2f2b4.png)

Figure 94. Loop states: ActivateChecking

#### Error

This state is reached when an error occurred at the loop. Here the error is
posted to the event from process to be handled by the general error handler.

![](media/82d9701342dc8c37e25aa2f0c3cc12d9.png)

Figure 95. Loop states: Error

#### Shutdown

This state is reached when the shutdown CMD is received. This loop is used to
stop the loop and close the connection to the variables.

![](media/c842dfbf798506dcf19cd11d7f084de6.png)

Figure 96. Loop states: Shutdown
