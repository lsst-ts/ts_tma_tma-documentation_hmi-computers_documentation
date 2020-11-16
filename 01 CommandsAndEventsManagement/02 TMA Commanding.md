
## TMA Commanding

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

### CppAppCommand.lvclass

This class is the one that generates the TCP message for each specific command
and sends them to the TMA using the TCP Client. To do so, each subsystem has a
specific class that inherits from the CppAppCommand class and has the specific
commands available for that subsystem. The methods in the parent class, the
CppAppCommand class, are dynamic dispatch and overridden by the child classes.

#### Methods

Here each of the available methods are listed.

##### CppAppCommand_Init

Initialize the command father object, this means initialize the ID repo (for
having an incremental ID different every time), the source repo and the repo for
the TCP Client (the object from the Sender.lvclass that corresponds to the TCP
Client).

Having a repo for each of this values allows the system to work without needing
to pass the parent object to all the windows, it is initialized at the
application launch and the values are stored in the repos and are available just
with an empty object of the parent class.

![CppAppCommand.lvclass_CppAppCommand_Init.vi context help.\label{figuretwenty-two70937cbe32ad787c123bf2984e769e84}](../Resources/figures/70937cbe32ad787c123bf2984e769e84.png)

##### CleanUp

Release queues from the different repos.

![CppAppCommand.lvclass_CleanUp.vi context help.\label{figuretwenty-three1836b222ffadf2c349e501ca25451728}](../Resources/figures/1836b222ffadf2c349e501ca25451728.png)

##### Enable

Send Enable Command to TMA.

![CppAppCommand.lvclass_Enable.vi context help.\label{figuretwenty-four0bd68a2221b292b0572a819716bae693}](../Resources/figures/0bd68a2221b292b0572a819716bae693.png)

##### Home

Send Command to Perform Home operation

![CppAppCommand.lvclass_Home.vi context help.\label{figuretwenty-five0c97331a0b955daf960053c6034fdca9}](../Resources/figures/0c97331a0b955daf960053c6034fdca9.png)

##### Move

Send Move command to TMA

![CppAppCommand.lvclass_Move.vi context help.\label{figuretwenty-sixbaffd692dbdffd8bfe59a54cb57983d6}](../Resources/figures/baffd692dbdffd8bfe59a54cb57983d6.png)

##### MoveVelocity

Send Move Velocity command to TMA

![CppAppCommand.lvclass_MoveVelocity.vi context help.\label{figuretwenty-sevenfdae7fa9ce09673d7fdde8e999f8ee16}](../Resources/figures/fdae7fa9ce09673d7fdde8e999f8ee16.png)

##### ResetAlarm

Send Reset Alarm Command to TMA

![CppAppCommand.lvclass_ResetAlarm.vi context help.\label{figuretwenty-eight99f085b41c6679388638168c4d447f6b}](../Resources/figures/99f085b41c6679388638168c4d447f6b.png)

##### SendCMD

This VI generates the TCP message for the specified command and with the
specified parameters and sends it to the TCP Client module to send it over TCP
to the TMA. Is meant to be used by the child classes for each of the commands
they have to send.

![CppAppCommand.lvclass_SendCMD.vi context help.\label{figuretwenty-nine8610d62c6fe583826dfca68b92b003a9}](../Resources/figures/8610d62c6fe583826dfca68b92b003a9.png)

##### SendPowerCMD

Send power command to TMA

![CppAppCommand.lvclass_SendPowerCMD.vi context help.\label{figurethirtyfc49ec768230affd87d663d6d4795c6e}](../Resources/figures/fc49ec768230affd87d663d6d4795c6e.png)

##### Stop

Send Stop to TMA

![CppAppCommand.lvclass_Stop.vi context help.\label{figurethirty-one959233c7aec15e1abbb3d7479ed42ae9}](../Resources/figures/959233c7aec15e1abbb3d7479ed42ae9.png)

##### Tracking

Send Tracking Command to TMA

![CppAppCommand.lvclass_Tracking.vi context help.\label{figurethirty-two9768ab5b5a9ad349d0fa8a047fb9e841}](../Resources/figures/9768ab5b5a9ad349d0fa8a047fb9e841.png)

#### Childs

The list of the available childs and the methods that use each of them
correspond to the existing subsystems and the corresponding commands for each
subsystem.

##### CppAppACWCMD.lvclass

This class corresponds to the commands available for the Azimuth Cable Wrap
subsystem, the methods in this class are:

-   EnableTrack

-   Move

-   MoveVelocity

-   ResetAlarm

-   SendPowerCMD

-   Stop

-   TrackTarget

##### CppAppACWDrivesCMD.lvclass

This class corresponds to the commands available for the Azimuth Cable Wrap
drives subsystem, the methods in this class are:

-   Enable

-   ReadDriveIdent

-   ResetAlarm

-   WriteDriveIdent

##### CppAppAZCMD.lvclass

This class corresponds to the commands available for the Azimuth subsystem, the
methods in this class are:

-   Home

-   Move

-   MoveVelocity

-   ResetAlarm

-   SendPowerCMD

-   Stop

-   Tracking

##### CppAppAZDrivesCMD.lvclass

This class corresponds to the commands available for the Azimuth drives
subsystem, the methods in this class are:

-   Enable

-   ReadDriveIdent

-   ResetAlarm

-   WriteDriveIdent

##### CppAppAZThermalCMD.lvclass

This class corresponds to the commands available for the Azimuth drives thermal
subsystem, the methods in this class are:

-   ControlMode

-   ReadAZThermalIdent

-   ResetAlarm

-   SendPowerCMD

-   WriteAZThermalIdent

##### CppAppBALCMD.lvclass

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

##### CppAppCabinet0101CMD.lvclass

This class corresponds to the commands available for the Cabinet 0101 subsystem,
the methods in this class are:

-   ControlMode

-   ResetAlarm

-   SendPowerCMD

##### CppAppCabinetCMD.lvclass

This class corresponds to the commands available for the Main Cabinet subsystem,
the methods in this class are:

-   ResetAlarm

-   TrackExtTemp

##### CppAppCCWCMD.lvclass

This class corresponds to the commands available for the Camera Cable wrap
subsystem, the methods in this class are:

-   EnableTrack

-   Move

-   MoveVelocity

-   ResetAlarm

-   SendPowerCMD

-   Stop

-   TrackTarget

##### CppAppCCWDrivesCMD.lvclass

This class corresponds to the commands available for the Camera Cable Wrap
drives subsystem, the methods in this class are:

-   Enable

-   MoveVelocity

-   ReadDriveIdent

-   ResetAlarm

-   WriteDriveIdent

##### CppAppCommaderCMD.lvclass

This class corresponds to the commands available for the Commander subsystem,
the methods in this class are:

-   AskCommand

-   PublishOnly

-   SystemReady

##### CppAppDPCMD.lvclass

This class corresponds to the commands available for the Deployable Platform
subsystem, the methods in this class are:

-   LockExtension

-   MoveVelocity

-   ReadPlatformIdent

-   ResetAlarm

-   SendPowerCMD

-   Stop

-   WritePlatformIdent

##### CppAppEIBCMD.lvclass

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

##### CppAppELCMD.lvclass

This class corresponds to the commands available for the Elevation subsystem,
the methods in this class are:

-   Home

-   Move

-   MoveVelocity

-   ResetAlarm

-   SendPowerCMD

-   Stop

-   Tracking

##### CppAppELDrivesCMD.lvclass

This class corresponds to the commands available for the Elevation drives
subsystem, the methods in this class are:

-   Enable

-   ReadDriveIdent

-   ResetAlarm

-   WriteDriveIdent

##### CppAppELThermalCMD.lvclass

This class corresponds to the commands available for the Elevation drives
thermal subsystem, the methods in this class are:

-   ControlMode

-   ReadELThermalIdent

-   ResetAlarm

-   SendPowerCMD

-   WriteELThermalIdent

##### CppAppLPCMD.lvclass

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

##### CppAppMainAxesCMD.lvclass

This class corresponds to the commands available for the Main Axes subsystem,
the methods in this class are:

-   MoveToTarget

-   Stop

-   TrackTarget

##### CppAppMCCMD.lvclass

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

##### CppAppMCLCMD.lvclass

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

##### CppAppModbusTempControllersCMD.lvclass

This class corresponds to the commands available for the Modbus Temperature
Controllers subsystem, the methods in this class are:

-   FanPower

-   ReadDriveIdent

-   ResetAlarm

-   Setpoint

-   WriteDriveIdent

##### CppAppMPSCMD.lvclass

This class corresponds to the commands available for the Main Power Supply
subsystem, the methods in this class are:

-   ResetAlarm

-   SendPowerCMD

##### CppAppOSSCMD.lvclass

This class corresponds to the commands available for the Oil Supply System
subsystem, the methods in this class are:

-   ResetAlarm

-   SendAbortPoweringCMD

-   SendChangeModeCMD

-   SendCoolingPowerCMD

-   SendMainPumpPowerCMD

-   SendOilPowerCMD

-   SendPowerCMD

##### CppAppSafetyCMD.lvclass

This class corresponds to the commands available for the Safety subsystem, the
methods in this class are:

-   SafetyReleaseOverride

-   SafetyReset

-   SafetySetOverride

##### CppAppTECCMD.lvclass

This class corresponds to the commands available for the Top End Chiller
subsystem, the methods in this class are:

-   ResetAlarm

-   SendPowerCMD

-   TrackExtTemp

##### CppAppTFCMD.lvclass

This class corresponds to the commands available for the Transfer Function
subsystem, the methods in this class are:

-   ExcitationConfig

-   ResetAlarm

GetEventfromTMA.lvclass
-----------------------

This class is responsible of parsing the events from the TMA received by the TCP
Client and generate the corresponding events inside the HMI application.

#### Task process

This task was created using the NI GOOP Developing Suite, this task is object
oriented and the communication between methods is done using queues and user
events. The task main is contained in the process.vi, here there is one loop.
The loop is used for CMD reception, see Figure \ref{figurethirty-three69459e04fb4b90c71b521567c9afc8ef}. This process has only one
instance that manages all the received events from the TMA.

![GetEventFromTMA task\label{figurethirty-three69459e04fb4b90c71b521567c9afc8ef}](../Resources/figures/69459e04fb4b90c71b521567c9afc8ef.png)

#### Task methods

Here the available methods for this task are explained.

##### GetEVENTFromTMA_Init.vi

Initialize the process to hear from TMA OMT.

The initialized process will get ACK, DONE, ERROR and WARNINGS that come from
the TMA (operation_manager). It will also get the state change and error from
the TMA OMT (operation_manager) itself.

![Task method: GetEventFromTMA_Init\label{figurethirty-four74a6d5ce8acd1e1bde61e9197fd16bad}](../Resources/figures/74a6d5ce8acd1e1bde61e9197fd16bad.png)

##### CleanUp

Stop task process and destroy user events.

![Task method: CleanUp\label{figurethirty-five625255128b48dfab8cc2ef1aa628e8ab}](../Resources/figures/625255128b48dfab8cc2ef1aa628e8ab.png)

##### ControlProcessWindow

This VI is used to show or hide the process front panel. Depending on the
ShowProcessWindow control value.

![Task method: ControlProcessWindow\label{figurethirty-six5b09030f1c224f409c548c97e07cd083}](../Resources/figures/5b09030f1c224f409c548c97e07cd083.png)

##### GetEventRefs

This VI is used to return the references to the different events: ack, done,
error and warning.

![Task method: GetEventRefs\label{figurethirty-sevend440b222f78f8f6b8dc91f280e9453c3}](../Resources/figures/d440b222f78f8f6b8dc91f280e9453c3.png)

#### CMD Reception loop

This loop receives the commands from the public methods and executes the
corresponding actions. This loop has a state for each method as well as some
other states used for loop managing, each state is explained in the next
sections.

##### Init

This state is just executed once and is the first executed one. Here the
initialization actions are executed. These are:

-   Initialization of local variable

-   Registration of events for the event structure

-   Updating the displayed name of the VI for debug purposes

![GetEVENTFromTMA.lvclass_Process.vi Init\label{figurethirty-eight7594b113e7a74001bbb6be57518ab3a3}](../Resources/figures/7594b113e7a74001bbb6be57518ab3a3.png)

##### Idle

This state is executed constantly after executing every new CMD, here the events
created at the methods are received and executed in the next iteration, in the
event called "EventsToProcess" see Figure \ref{figureforty0ffa46f585c98b683a6134b3b50cf30d}.

![GetEVENTFromTMA.lvclass_Process.vi Idle\label{figurethirty-ninefd6414a2c920162a8a200737a0b5767b}](../Resources/figures/fd6414a2c920162a8a200737a0b5767b.png)

![GetEVENTFromTMA.lvclass_Process.vi EventsToProcess\label{figureforty0ffa46f585c98b683a6134b3b50cf30d}](../Resources/figures/0ffa46f585c98b683a6134b3b50cf30d.png)

In addition to this, the TCP messages received from the TCP Client as user
events are received and parsed, the event that receives the TCP messages is
called “DataFromTCP” see Figure \ref{figureforty-onef00e25525f869863dacc2c4672994d39}. The TCP messages are parsed and the
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

![GetEVENTFromTMA.lvclass_Process.vi DataFromTCP\label{figureforty-onef00e25525f869863dacc2c4672994d39}](../Resources/figures/f00e25525f869863dacc2c4672994d39.png)

##### Timeout

This state is executed when there is something that must be executed in the
specified timeout of the Idle state event structure, see Figure \ref{figureforty-twoffea54a30df54a93c2d371689872356d}.

![Timeout state from Idle state\label{figureforty-twoffea54a30df54a93c2d371689872356d}](../Resources/figures/ffea54a30df54a93c2d371689872356d.png)

![GetEVENTFromTMA.lvclass_Process.vi Timeout\label{figureforty-three584b251ad792650692f863fd0386d08b}](../Resources/figures/584b251ad792650692f863fd0386d08b.png)

##### ShowWindow

This state is used to show the front panel of the process.

![CMD Receiver states: ShowWindow\label{figureforty-foure69d2d974ee18af4bb649e21db92269b}](../Resources/figures/e69d2d974ee18af4bb649e21db92269b.png)

##### HideWindow

This state is used to hide the front panel of the process.

![CMD Receiver states: HideWindow\label{figureforty-five4804276518cb9f6e2c8f0ccbdc7abf31}](../Resources/figures/4804276518cb9f6e2c8f0ccbdc7abf31.png)

##### Shutdown

This state is reached when the shutdown CMD is received. This loop is used to
stop the CMD receiver loop.

![GetEVENTFromTMA.lvclass_Process.vi shutdown\label{figureforty-sixb2b82c4abc519e2523f469bacf952aa8}](../Resources/figures/b2b82c4abc519e2523f469bacf952aa8.png)

##### Error

This state is reached when an error occurs at the task, here the error is
published and cleared for the next iteration.

![GetEVENTFromTMA.lvclass_Process.vi Error\label{figureforty-seven16549f5ba20f1abe2e0b7bb142b70442}](../Resources/figures/16549f5ba20f1abe2e0b7bb142b70442.png)

TMAOMTMonitoring task
---------------------

To be updated

This task is responsible of monitoring the TMA OMT status.

Here the CSC connection status, CSC commands, CSC Events and TMA OMT actual
state are received and saved.

#### Task process

This task was created using the NI GOOP Developing Suite, this task is object
oriented and the communication between methods is done using queues and user
events. The task main is contained in the process.vi, here there is a single
loop. This loop contains both the CMD reception and the CMD actions execution.
This process has only one instance that manages all the received data.

![TMAOMTMonitoring task\label{figureforty-eight61555585148d675193b203cfbb113f09}](../Resources/figures/61555585148d675193b203cfbb113f09.png)

#### Task methods

Here the available methods for this task are explained.

##### TMAOMTMonitoring_Init.vi

This VI is used to launch the process.

![Task method: TMAOMTMonitoring \_Init\label{figureforty-nine591d405c4f711bb40656637b9a885def}](../Resources/figures/591d405c4f711bb40656637b9a885def.png)

##### CleanUp

This VI is used to stop the task and release all the references generated for
this task.

![Task method: CleanUp\label{figurefifty66ac09dc52f60ca146973034097794b8}](../Resources/figures/66ac09dc52f60ca146973034097794b8.png)

##### ControlProcessWindow

This VI is used to show or hide the process front panel. Depending on the
ShowProcessWindow control value.

![Task method: ControlProcessWindow\label{figurefifty-one97c7b83d936bca9900fdff7fe8ee0ced}](../Resources/figures/97c7b83d936bca9900fdff7fe8ee0ced.png)

##### NewTMAState

This VI is used to send the new TMA OMT state string to the process and update
the memory.

![Task method: NewTMAState\label{figurefifty-two98e0fabce511f820847f3a6cc4ce20f4}](../Resources/figures/98e0fabce511f820847f3a6cc4ce20f4.png)

##### SetTCSCommStatus

This VI is used to send the TCS connection status to the process and update the
memory.

![Task method: SetTCSCommStatus\label{figurefifty-three10fc86376d1e4c4d13a8829a84b41074}](../Resources/figures/10fc86376d1e4c4d13a8829a84b41074.png)

##### GetTMAStatus

This VI is used to get the status of the TMA OMT from the task.

![Task method: GetTMAStatus\label{figurefifty-four56e46ec31ea69914ca986d8406542515}](../Resources/figures/56e46ec31ea69914ca986d8406542515.png)

##### NEWTCSCmd

This VI is used to send the last command received from the TCS to the process
and update the memory.

![Task method: SendDONE\label{figurefifty-fivee5b59d1fdb07ed4928c03eb607ce114d}](../Resources/figures/e5b59d1fdb07ed4928c03eb607ce114d.png)

##### GetCMDHistory

This VI is used to get the last 100 CMDs received from the TCS.

![Task method: GetCMDHistory\label{figurefifty-sixa25363da62efc80d885ff6dab7c0f329}](../Resources/figures/a25363da62efc80d885ff6dab7c0f329.png)

##### GetEventHistory

This VI is used to get the last 100 events received from the TCS.

![Task method: GetEventHistory\label{figurefifty-seven07e3f8bbdee245d20ef852de67f1f7dd}](../Resources/figures/07e3f8bbdee245d20ef852de67f1f7dd.png)

##### NEWTCSEvent

This VI is used to send the last event received from the TCS to the process and
update the memory. This is not used for error or waring events.

![Task method: 4.7.2.10 NEWTCSEvent\label{figurefifty-eight2cc603df606ab033bc958043cf61fb45}](../Resources/figures/2cc603df606ab033bc958043cf61fb45.png)

#### Main loop

This loop receives the CMDs from the methods and executes them in the same loop.
The different states of the loop are explained in the following sections.

##### Init

Here the initialization actions are executed.

![Loop states: Init\label{figurefifty-ninee404f638668268d3073bc082a1b7e253}](../Resources/figures/e404f638668268d3073bc082a1b7e253.png)

##### Idle

This state is executed constantly after executing every new CMD, here the events
created at the methods are received and executed in the next iteration.

![Loop states: Idle\label{figuresixty3f276b7d1d394e2dbddc70e78ad4a42d}](../Resources/figures/3f276b7d1d394e2dbddc70e78ad4a42d.png)

##### Timeout

This state is executed when there is something that must be executed in the
specified timeout of the Idle state event structure, see Figure \ref{figuresixty-one5a9d7a30173c7114968eb1d6689ee85b}.

![Timeout state from Idle state\label{figuresixty-one5a9d7a30173c7114968eb1d6689ee85b}](../Resources/figures/5a9d7a30173c7114968eb1d6689ee85b.png)

![Loop states: Timeout\label{figuresixty-twoea096d17d2afe10dc90a83d631c2de5a}](../Resources/figures/ea096d17d2afe10dc90a83d631c2de5a.png)

##### ShowWindow

This state is used to show the front panel of the process.

![Loop states: ShowWindow\label{figuresixty-three2fc66861285c454098d66de610dc1d43}](../Resources/figures/2fc66861285c454098d66de610dc1d43.png)

##### HideWindow

This state is used to hide the front panel of the process.

![Loop states: HideWindow\label{figuresixty-fourb2c602d1f37e364f2519be97641fca53}](../Resources/figures/b2c602d1f37e364f2519be97641fca53.png)

##### NEWTMAStateSendEVENT

This state is executed when the NEWTMAState method is used. Here the received
string from the calling method is saved into the TMA Status Local Var register
as current state.

![Loop states: NEWTMAStateSendEVENT\label{figuresixty-five7539d8c63f7a1e88c8810c815c73e6c0}](../Resources/figures/7539d8c63f7a1e88c8810c815c73e6c0.png)

##### SetConnStatus

This state is executed when the SetTCSCommStatus method is used. Here the
received boolean from the calling method is saved into the TMA Status Local Var
register as current connection status.

![Loop states: SetConnStatus\label{figuresixty-six774331c6ab5684de8240990d7f7ba975}](../Resources/figures/774331c6ab5684de8240990d7f7ba975.png)

##### NewTCSCMD

This state is executed when the NewTCSCMD method is used. Here the received
structure from the calling method is saved into the CMD history queue and TMA
Status Local Var register as last CMD.

![Loop states: NewTCSCMD\label{figuresixty-seven403bb5144ba657673d60392a0fe8fb44}](../Resources/figures/403bb5144ba657673d60392a0fe8fb44.png)

##### NewTCSEvent

This state is executed when the NewTCSEvent method is used. Here the received
structure from the calling method is saved into the Event history queue and TMA
Status Local Var register as last CMD.

![Loop states: NewTCSEvent\label{figuresixty-eight305ef9ff7e5a8239909ad5dc6dc96e20}](../Resources/figures/305ef9ff7e5a8239909ad5dc6dc96e20.png)

##### GetStatus

This state is executed when the GetTMAStatus method is used. Here the TMA Status
Local Var register value is used as response to the calling method, the response
is given at the “QueueFromProcess” queue.

![Loop states: GetStatus\label{figuresixty-nine8465a63d0dc07265ad781d432b22c197}](../Resources/figures/8465a63d0dc07265ad781d432b22c197.png)

##### GetCMDHistory

This state is executed when the GetCMDHistory method is used. Here the CMD
History queue elements are used as response to the calling method, the response
is given at the “QueueFromProcess” queue.

![Loop states: GetCMDHistory\label{figureseventy7033fdb8f196da0925147de5dbc07ebe}](../Resources/figures/7033fdb8f196da0925147de5dbc07ebe.png)

##### GetEventHistory

This state is executed when the GetEventHistory method is used. Here the Event
History queue elements are used as response to the calling method, the response
is given at the “QueueFromProcess” queue.

![Loop states: GetEventHistory\label{figureseventy-one559d9e03a710c7c09fb57fa908b722fd}](../Resources/figures/559d9e03a710c7c09fb57fa908b722fd.png)

##### Error

This state is reached when an error occurred at the loop. Here the error is
posted to the event from process to be handled by the general error handler.

![Loop states: Error\label{figureseventy-two2278779df52a382906053f19e6d48960}](../Resources/figures/2278779df52a382906053f19e6d48960.png)

##### Shutdown

This state is reached when the shutdown CMD is received. This loop is used to
stop the loop and close the connection to the TMA OMT.

![Loop states: Shutdown\label{figureseventy-three8cd88c931dc08cc2714ec3df401d5a0f}](../Resources/figures/8cd88c931dc08cc2714ec3df401d5a0f.png)

CommanderCheck task
-------------------

To be updated

This task will be waiting for commands form outside and, in the timeout, will
check the actual commander var. If the commander has changed an event will be
placed in Events from process with new data.

#### Task process

This task was created using the NI GOOP Developing Suite, this task is object
oriented and the communication between methods is done using queues and user
events. The task main is contained in the process.vi, here there is a single
loop. This loop contains both the CMD reception and the CMD actions execution.
This process has only one instance that manages all the received data.

![CommanderCheck task\label{figureseventy-four7363c779e5996d2aacc057f4407333e2}](../Resources/figures/7363c779e5996d2aacc057f4407333e2.png)

#### Task methods

Here the available methods for this task are explained.

##### CommanderCheck_Init

This VI is used to launch the process, to do so some inputs are required:

-   Commander Variable URL: a string with the URL of the CommanderVariable that
    the process will use to know if the commander has changed and how is the new
    commander.

-   Status Variable URL: a string with the URL of the Status Variable that the
    process will use publish the last TMA OMT status.

![Task method: CommanderCheck_Init\label{figureseventy-five6445a816c30524906950111a5620ae4a}](../Resources/figures/6445a816c30524906950111a5620ae4a.png)

##### CleanUp

This VI is used to disconnect vars, stop the task and release all the references
generated for this task.

![Task method: CleanUp\label{figureseventy-six4e9fbe23e7cb1efe91ee0810167ea38c}](../Resources/figures/4e9fbe23e7cb1efe91ee0810167ea38c.png)

##### ControlProcessWindow

This VI is used to show or hide the process front panel. Depending on the
ShowProcessWindow control value.

![Task method: ControlProcessWindow\label{figureseventy-seven30d5ec3d899eeea67e04c0db3267d019}](../Resources/figures/30d5ec3d899eeea67e04c0db3267d019.png)

##### GetCommander

This VI is used to get the current commander. It also tells if the status is
valid or not, but only after the first read of the variable is done.

![Task method: GetCommander\label{figureseventy-eight7b472a80d4577e51c0a8e1adabd81b67}](../Resources/figures/7b472a80d4577e51c0a8e1adabd81b67.png)

##### DisconnectVars

This VI is used to disconnect the shared variables used by the task.

![Task method: DisconnectVars\label{figureseventy-nine1a136d96bfac47b2d0a766d0447251d9}](../Resources/figures/1a136d96bfac47b2d0a766d0447251d9.png)

##### ConnectVars

This VI is used to connect the shared variables used by the task.

![Task method: ConnectVars\label{figureeighty6a4c984904a47ab42fa91a0a42a09e38}](../Resources/figures/6a4c984904a47ab42fa91a0a42a09e38.png)

##### GetCommanderEventRef

This VI is used to get the ref to the events triggered by the task.

![Task method: GetCommanderEventRef\label{figureeighty-one90a3e010b6f5fa114a117c02d96f31f8}](../Resources/figures/90a3e010b6f5fa114a117c02d96f31f8.png)

##### PublishTMAOMTState

This VI is used to send the last TMA OMT state to the process and update the
Status Variable.

![Task method: PublishTMAOMTState\label{figureeighty-two3454716cf4495c3c41b9fde5d6d892de}](../Resources/figures/3454716cf4495c3c41b9fde5d6d892de.png)

##### ActivateChecking

This VI is used to activate or de activate the checking of the commander.

![Task method: ActivateChecking\label{figureeighty-threeefce932ef4109ace1691c863d16817fb}](../Resources/figures/efce932ef4109ace1691c863d16817fb.png)

#### Main loop

This loop receives the CMDs from the methods and executes them in the same loop.
The different states of the loop are explained in the following sections.

##### Init

Here the initialization actions are executed.

![Loop states: Init\label{figureeighty-four1ea4095f5e2b5e5782690ca248d4f8aa}](../Resources/figures/1ea4095f5e2b5e5782690ca248d4f8aa.png)

##### Idle

This state is executed constantly after executing every new CMD, here the events
created at the methods are received and executed in the next iteration.

![Loop states: Idle\label{figureeighty-five94dff4cabdad59c7848c32bf5205d93e}](../Resources/figures/94dff4cabdad59c7848c32bf5205d93e.png)

##### Timeout

This state is executed when there is something that must be executed in the
specified timeout of the Idle state event structure. In this timeout, see Figure
87, the commander is checked by reading the commander variable. If this variable
has new data a commander change event is triggered.

![Timeout state from Idle state\label{figureeighty-six54afc2c7b6ee04f85475d3177f166395}](../Resources/figures/54afc2c7b6ee04f85475d3177f166395.png)

![Loop states: Timeout\label{figureeighty-sevene204a041042024141355e9d762050a89}](../Resources/figures/e204a041042024141355e9d762050a89.png)

##### ShowWindow

This state is used to show the front panel of the process.

![Loop states: ShowWindow\label{figureeighty-eight2fc66861285c454098d66de610dc1d43}](../Resources/figures/2fc66861285c454098d66de610dc1d43.png)

##### HideWindow

This state is used to hide the front panel of the process.

![Loop states: HideWindow\label{figureeighty-nineb2c602d1f37e364f2519be97641fca53}](../Resources/figures/b2c602d1f37e364f2519be97641fca53.png)

##### ConnectVars

This state is executed when the ConnectVars method is used. Here the variable
references specified at the init are connected.

![Loop states: ConnectVars\label{figureninety86d3fa8d2bb302e9f0a6efee90b87e06}](../Resources/figures/86d3fa8d2bb302e9f0a6efee90b87e06.png)

##### DisconnectVars

This state is executed when the DisconnectVars method is used. Here the variable
references are disconnected.

![Loop states: DisconnectVars\label{figureninety-onee03edfd2a04d8a7ae3375169b16d81ee}](../Resources/figures/e03edfd2a04d8a7ae3375169b16d81ee.png)

##### GetCommander

This state is executed when the GetCommander method is used. Here the actual
commander register is used as response to the calling method, the response is
given at the “QueueFromProcess” queue.

![Loop states: GetCommander\label{figureninety-two2556b04ad4da512c4ae8a85a3355c6b8}](../Resources/figures/2556b04ad4da512c4ae8a85a3355c6b8.png)

##### PublishState

This state is executed when the PublishTMAOMTState method is used. Here the
string specified at the method is written to the status connection variable.

![Loop states: PublishState\label{figureninety-three8d24f3d5fa9066546bd43ce0765083aa}](../Resources/figures/8d24f3d5fa9066546bd43ce0765083aa.png)

##### ActivateChecking

This state is executed when the ActivateChecking method is used. Here the
Boolean value specified to the method is used to set the value of the
CheckCommander? Local variable.

![Loop states: ActivateChecking\label{figureninety-four42d9e52be937bdc9663b176537c2f2b4}](../Resources/figures/42d9e52be937bdc9663b176537c2f2b4.png)

##### Error

This state is reached when an error occurred at the loop. Here the error is
posted to the event from process to be handled by the general error handler.

![Loop states: Error\label{figureninety-five82d9701342dc8c37e25aa2f0c3cc12d9}](../Resources/figures/82d9701342dc8c37e25aa2f0c3cc12d9.png)

##### Shutdown

This state is reached when the shutdown CMD is received. This loop is used to
stop the loop and close the connection to the variables.

![Loop states: Shutdown\label{figureninety-sixc842dfbf798506dcf19cd11d7f084de6}](../Resources/figures/c842dfbf798506dcf19cd11d7f084de6.png)
