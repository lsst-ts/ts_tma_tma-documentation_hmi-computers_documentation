Alarm Management
================

This component gets the alarm and warning events from the TCP client, logging
the received alarms and warnings to have a record of the different events
occurred while operating the system. This component has a different from the
previous ones, and this is that the code that runs on the MCC, also known as
EUI, and the one running on the HHD are different. This is due to the limited
capabilities of the HHD. Therefore, the alarm management and logging is done in
the MCC and then the HHD connects to it and obtains the alarms stored at the
MCC. To do so, there are three different classes, these are:

-   Alarm Recpetion Task.lvclass: this class receives the alarms/faults and
    warnings, from the GetEventsFromTMA.lvclass, and stores the notAcked ones in
    memory until they are acked. In addition to this, the received alarms and
    warnings are sent to the AlarmSavingTask.lvclass for storing them.

-   AlarmSavingTask.lvclass: this task manages the alarm and warning log file.

-   Alarm Recpetion Task HHD.lvclass: this class is the one used in the HHD
    instead of the Alarm Recpetion Task.lvclass. It just connects to the Alarm
    Recpetion Task.lvclass and requests for certain data.

Alarm Recpetion Task.lvclass
----------------------------

This class receives the alarms/faults and warnings, from the
GetEventsFromTMA.lvclass, and stores the notAcked ones in memory until they are
acked. In addition to this, the received alarms and warnings are sent to the
AlarmSavingTask.lvclass for storing them.

### Task process

This task was created using the NI GOOP Developing Suite, this task is object
oriented and the communication between methods is done using queues and user
events. The task main is contained in the process.vi, here there is one loop.
The loop is used for CMD reception, see Figure 97. This process has only one
instance that manages all the received alarms and warnings.

![](media/7cdf3b852338e4a91ad92f619e4760aa.png)

Figure 97. Alarm Recpetion Task.lvclass_Process.vi Process

### Methods

Here each of the available methods are listed.

#### Alarm Recpetion Task_Init.vi

Initialize the alarm Reception task:

\- Launch the TCP server, see section 5.1.4, used to communicate with the HHD.
For doing this a xml file is needed, in this file a section with the name
"AlarmsForHHDServerConnectionData" must be found to initialize the server. The
configuration used for this TCP server can be found in the “HMIConfig.xml” file
located at the Configuration folder inside the HMIComputers repo.

\- Add the needed object references to the class object.

\- Launch the alarm reception task process.

![](media/17a8b173c1717f27b0b1fbc78264c555.png)

Figure 98. Alarm Recpetion Task.lvclass_Alarm Recpetion Task_Init.vi context
help.

#### CleanUp

Stop the process

![](media/b82587dc9fa57163b1df254da3019604.png)

Figure 99. Alarm Recpetion Task.lvclass_CleanUp.vi context help.

#### CMD_AckAllNotACKED

This method acks all the not acked events stored in memory and are sends them to
the AlarmSavingTask.lvclass to save them as acked.

![](media/dcc25c2e69489332538bc74e0c9a110e.png)

Figure 100. Alarm Recpetion Task.lvclass_CMD_AckAllNotACKED.vi context help.

#### CMD_AckAllNotACKEDSubsystem

This method acks the not acked events stored in memory filtered by the specified
subsystem and are sends them to the AlarmSavingTask.lvclass to save them as
acked.

![](media/b565866ac9fcf54f0d82e133fddf36b4.png)

Figure 101. Alarm Recpetion Task.lvclass_CMD_AckAllNotACKEDSubsystem.vi context
help.

#### CMD_GetNotACKED

This method is used to get the not acked events stored in memory. This vi has a
timeout for the response of 180000 ms

![](media/c7fcfb324382b9c609888c98d627e3c7.png)

Figure 102. Alarm Recpetion Task.lvclass_CMD_GetNotACKED.vi context help.

#### CMD_GetNotACKEDSubsystem

This method is used to get the not acked events stored in memory filtered by the
specified subsystem. This vi has a timeout for the response of 180000 ms

![](media/ec5debae865fbd4f146361075b1c805c.png)

Figure 103. Alarm Recpetion Task.lvclass_CMD_GetNotACKEDSubsystem.vi context
help.

### CMD Reception loop

This loop receives the CMDs from the public methods and executes the required
actions depending on the received command. This loop has a state for each method
as well as some other states used for loop managing, each state is explained in
the next sections.

#### Init

This state is just executed once and is the first executed one. Here the
initialization actions are executed. These are:

-   Initialization of local variable

-   Registration of events for the event structure

    -   Alarm event from the GetEventFromTMA.lvclass

    -   Warning event from the GetEventFromTMA.lvclass

    -   TCP_Server_DataFrom event used by the TCP server to receive the TCP
        messages from the Alarm Recpetion Task HHD.lvclass and respond to them
        requests.

    -   TCP_Server_Error event that reports the errors from the TCP server.

-   Updating the displayed name of the VI for debug purposes

![](media/ac75019465328a2136ab6ea53721fae3.png)

>   Figure 104. Alarm Recpetion Task.lvclass_Process.vi Init

#### Idle

This state is executed constantly after executing every new CMD, here the events
created at the methods are received and executed in the next iteration, in the
event called “EventsToProcess” see Figure 106.

![](media/2eee17b3b907c8dae3868658e1289614.png)

Figure 105. Alarm Recpetion Task.lvclass_Process.vi Idle

![](media/c97367bfed5dc44c431073461f082020.png)

Figure 106. Alarm Recpetion Task.lvclass_Process.vi EventsToProcess

In addition to this, the events generated by the GetEventFromTMA.lvclass are
received, these are:

-   Error event: this event receives the fault events and sets the next state to
    be EV-ErrorEvent, see Figure 107.

-   Warning event: this event receives the warning events and sets the next
    state to be EV-WarningEvent, see Figure 108.

![](media/afa8a4f05207dde8ec45e93725761de2.png)

>   Figure 107. Alarm Recpetion Task.lvclass_Process.vi Error event

![](media/e35b74ee9bf49df5064a2b6ba88b2156.png)

>   Figure 108. Alarm Recpetion Task.lvclass_Process.vi Warning event

As said before, for working with the HHD, that has less computational power,
there is a TCP communication between them to get the alarms logged at the EUI in
the MCC. Therefore, there are two other events registered at the event structure
inside the Idle case, these are:

-   TCP_Server_DataFrom: event used by the TCP server to receive the TCP
    messages from the Alarm Recpetion Task HHD.lvclass and respond to the
    requests, see Figure 109.

-   TCP_Server_Error: event that reports the errors from the TCP server, see
    Figure 110.

![](media/68a2eacef6c3b86a2b93ec16c46f8897.png)

Figure 109. Alarm Recpetion Task.lvclass_Process.vi TCP_Server_DataFrom

![](media/663150cdd483bb932f7508038f274f71.png)

Figure 110. Alarm Recpetion Task.lvclass_Process.vi TCP_Server_Error

#### Timeout

This state is executed when there is something that must be executed in the
specified timeout of the Idle state event structure, see Figure 111.

![](media/ffea54a30df54a93c2d371689872356d.png)

Figure 111. Timeout state from Idle state

![](media/584b251ad792650692f863fd0386d08b.png)

Figure 112. Alarm Recpetion Task.lvclass_Process.vi Timeout

#### ShowWindow

This state is used to show the front panel of the process.

![](media/e69d2d974ee18af4bb649e21db92269b.png)

Figure 113. CMD Receiver states: ShowWindow

#### HideWindow

This state is used to hide the front panel of the process.

![](media/4804276518cb9f6e2c8f0ccbdc7abf31.png)

Figure 114. CMD Receiver states: HideWindow

#### CMD-GetNotACKED

This case executes when the public method CMD_GetNotACKED is used, here the not
acked events stored in memory, using the shift register of the while loop, are
sent as a response to the calling method.

![](media/a2e1daf777024597e9870811f9519d44.png)

Figure 115. Alarm Recpetion Task.lvclass_Process.vi CMD-GetNotACKED

#### CMD-GetNotACKEDSubsystem

This case executes when the public method GetNotACKEDSubsystem is used, here the
not acked events stored in memory, using the shift register of the while loop,
are filtered by the specified subsystem and are sent as a response to the
calling method.

![](media/e45a26619adec442a7b13da81bca451e.png)

Figure 116. Alarm Recpetion Task.lvclass_Process.vi CMD-GetNotACKEDSubsystem

#### CMD-AckAllNotACKED

This case executes when the public method CMD_AckAllNotACKED is used, here the
not acked events stored in memory, using the shift register of the while loop,
are sent to the AlarmSavingTask.lvclass to save them as acked and the memory is
cleared, wiring an empty array to the shift register of the while loop.

![](media/ecde3ca5c1ce81e3fa80ff6af271cc13.png)

Figure 117. Alarm Recpetion Task.lvclass_Process.vi CMD-AckAllNotACKED

#### CMD-AckAllNotACKEDSubsystem

This case executes when the public method CMD_AckAllNotACKEDSubsystem is used,
here the not acked events stored in memory, using the shift register of the
while loop, filtered by the specified subsystem are sent to the
AlarmSavingTask.lvclass to save them as acked and are removed from memory.

![](media/53cae98abef4239d62030a739740c41c.png)

Figure 118. Alarm Recpetion Task.lvclass_Process.vi CMD-AckAllNotACKEDSubsystem

#### Shutdown

This state is reached when the shutdown CMD is received. This loop is used to
stop the CMD receiver loop.

![](media/b2b82c4abc519e2523f469bacf952aa8.png)

Figure 119. Alarm Recpetion Task.lvclass_Process.vi shutdown

#### Error

This state is reached when an error occurs at the task, here the error is
published and cleared for the next iteration.

![](media/ea8890e7966c810c9e99924539fb496f.png)

Figure 120. Alarm Recpetion Task.lvclass_Process.vi Error

#### EV-ErrorEvent

This case is executed when an error event is received at the event structure
inside the Idle case. Here the received error (fault) event is sent to the
AlarmSavingTask.lvclass to save it and it is added to the notAcked events in
memory, indexing it into the shift register of the while loop.

![](media/7411d017323b878c954fcc8109cd2b86.png)

Figure 121. Alarm Recpetion Task.lvclass_Process.vi EV-ErrorEvent

#### EV-WarningEvent

This case is executed when a warning event is received at the event structure
inside the Idle case. Here the received warning event is sent to the
AlarmSavingTask.lvclass to save it and it is added to the notAcked events in
memory, indexing it into the shift register of the while loop.

![](media/040d78b4d8f9a28092bc4c1e73a5fa17.png)

Figure 122. Alarm Recpetion Task.lvclass_Process.vi EV-WarningEvent

### Server.lvclass

This class when initialized launches a task that contains a TCP server. The
class main elements are explained in the following sections.

#### Configuration file explained

This task requires from a configuration file for initialization, in*\*.xml*
format that contains the following sections:

-   Service_Name: the name you want to set to the server, default is empty.

-   Port: the port of the server the clients will connect to.

-   Listen_Timeout_in_ms: timeout for listening to new clients.

-   Receive_Timeout_in_ms: timeout for reading from TCP clients, this timeout is
    set to check every client.

-   BytesToRead: bytes to read from TCP.

-   ReadMode: here the read mode is configured, the options are:
    sel='Standard'\>0; sel='Buffered'\>1; sel='CRLF'\>2; sel='Immediate'\>3

#### Task process

This task was created using the NI GOOP Developing Suite, this task is object
oriented and the communication between methods is done using queues and user
events. The task main is contained in the process.vi, here there are three
loops, see Figure 123.

-   The loop on top is used for CMD reception.

-   The TCP Listening Loop, is the one reading the TCP connections from the
    clients for any new data.

-   New Connection Manager loop, is the one managing the new client connections.

![](media/f8028a78c1fca2eaaca83ba905948bed.png)

Figure 123. Server.lvclass_Process.vi block diagram

##### CMD Reception loop

This loop receives the CMDs from the methods and passes the required actions to
the corresponding loop.

###### Init

Here the local variables are initialized to the default values.

![](media/b2778629528983275c632aeb660a7abf.png)

Figure 124. Server.lvclass_Process.vi Init

###### Idle

This state is executed constantly after executing every new CMD, here the events
created at the methods are received and executed in the next iteration.

![](media/25dcf81b8d5d34d86edc9c07a79a5ba5.png)

Figure 125. Server.lvclass_Process.vi Idle

###### Timeout

This state is executed when there is something that must be executed in the
specified timeout of the Idle state event structure, see Figure 126.

![](media/1e2f937dc618605c620b5684a5813697.png)

Figure 126. Timeout state from Idle state

![](media/69c63cb7d417cfaee3a8fe4ce37ca62b.png)

Figure 127. Server.lvclass_Process.vi Timeout

###### ShowWindow

This state is used to show the front panel of the process.

![](media/e69d2d974ee18af4bb649e21db92269b.png)

Figure 128. Server.lvclass_Process.vi ShowWindow

###### HideWindow

This state is used to hide the front panel of the process.

![](media/4804276518cb9f6e2c8f0ccbdc7abf31.png)

Figure 129. Server.lvclass_Process.vi HideWindow

###### CMD-SendData

This case is called from the SendDataTCP method. Here the specified message is
sent to the specified TCP client, the identification of the client is done using
the connection reference ID (i32) data to the process. The different client
connections are stored in a DVR and there the specified connection is searched,
if found the message is sent if not an error is sent as a response to the
method. If any error occurred during the process they are sent to the process as
a response.

![](media/e0202f4c7bf706a3832d5c7f249edaf8.png)

Figure 130. Server.lvclass_Process.vi CMD-SendData

###### CMD-SendData2All

This case is called from the SendDataTCP2All method. Here the specified message
is sent to all the TCP clients. If any error occurred during the process they
are sent to the process as a response.

![](media/282ae05422f613021acaa33990a5dfe6.png)

Figure 131. Server.lvclass_Process.vi CMD-SendData2All

###### Shutdown

This state is reached when the shutdown CMD is received. This loop is used to
stop the process.

![](media/8f7648965940c4b510535f139760476f.png)

Figure 132. Server.lvclass_Process.vi shutdown

###### Error

This state is reached when an error occurs at the task, here the error is
published, into a user event that can be registered by the calling task, and
cleared for the next iteration.

![](media/483f07d3417caa5b5d10d4a632d8007a.png)

Figure 133. Server.lvclass_Process.vi Error

##### The TCP Listening Loop 

This loop is responsible of publishing the telemetry over TCP. This loop
contains a very simple state machine with 5 states, each of them is explained in
the upcoming sections.

###### Initialize

![](media/4828deb415a8db418cf241a49de04305.png)

Figure 134. Server.lvclass_Process.vi Initialize

###### StatusCheck

Here the connected clients are cheked:

-   If there are no clients connected the next state is StatusCheck

-   If there are clients connected the next state is ListenCommands

![](media/d0f96d781471d52ca329ae714ef90ea8.png)

Figure 135. Server.lvclass_Process.vi StatusCheck

###### ListenCommands

Here the messages from the connected clients are obtained, and if any client is
no longer available the client is removed from the active clients.

The next state is always StatusCheck.

![](media/1ef67b0e32d52d5f7fe352ceb4f5021e.png)

Figure 136. Server.lvclass_Process.vi ListenCommands

###### ErrorHandling

Here the same VI from the Error case in the CMD reception loop is used to
publish the error as an event.

The next state is always StatusCheck.

![](media/6ed8dc820a53171f26c2d9f561b50fbb.png)

Figure 137. Server.lvclass_Process.vi ErrorHandling

###### Exit

Here all the connections are closed and the loop is stopped.

There is no next state after this case.

![](media/8c9139f780eb476e4bed4698b627e0eb.png)

Figure 138. Server.lvclass_Process.vi Exit

##### New Connection Manager 

This loop is responsible of managing the new TCP client connections. This loop
contains a very simple state machine with 5 states, each of them is explained in
the upcoming sections.

###### Initialize

Here the TCP server is initialized.

The next state is always WaitNewConnection.

![](media/21bfef6d77132753e4aa803f07fa1471.png)

Figure 139. Server.lvclass_Process.vi Initialize

###### WaitNewConnection

Here the server waits for new TCP client connections.

The next state is always CheckExit.

![](media/761bd38beb5a1a21ffa6d9bd66be0f0b.png)

Figure 140. Server.lvclass_Process.vi WaitNewConnection

###### CheckExit

Here the local variable Stop is checked to stop the loop.

-   If TRUE the next state is Exit.

-   If FALSE the next state is WaitNewConnection.

![](media/072bbdd1544aa5c10b23f8a2e7b027de.png)

Figure 141. Server.lvclass_Process.vi CheckExit

###### ErrorHandling

Here the following actions are done:

-   All the connections are closed, and the server is stopped.

-   The error is publish using the same VI from the Error case in the CMD
    reception loop.

The next state is always Initialize.

![](media/e1a3df95a7639cf916416c6fb1e5455e.png)

Figure 142. Server.lvclass_Process.vi ErrorHandling

###### Exit

Here all the connections are closed and the loop is stopped.

There is no next state after this case.

![](media/1d95653b306ff02f78c047350ec21c8f.png)

Figure 143. Server.lvclass_Process.vi Exit

AlarmSavingTask.lvclass
-----------------------

This task manages the alarm and warning log file.

### Task process

This task was created using the NI GOOP Developing Suite, this task is object
oriented and the communication between methods is done using queues and user
events. The task main is contained in the process.vi, here there is one loop.
The loop is used for CMD reception, see Figure 144. This process has only one
instance that manages all the received alarms and warnings.

![](media/d08155d3845bd28604cd56590eb0f14f.png)

Figure 144. AlarmSavingTask.lvclass_Process.vi block diagram

### Methods

Here each of the available methods are listed.

#### AlarmSavingTask_Init.vi

Initialize the object:

\- Set the folder where the alarm log files will be stored into the object

\- Launch the process

![](media/b0f81e726139534eed570467c58d0d0e.png)

Figure 145. AlarmSavingTask.lvclass_AlarmSavingTask_Init.vi context help.

#### CleanUp

Stop the process

![](media/fda4e17703a63842eda838ce10c453d0.png)

Figure 146. AlarmSavingTask.lvclass_CleanUp.vi context help.

#### CMD_GetFiltered

Get the events from the AlarmHistory file based on the speficied filters. This
request has a response with a timeout of 180000 ms.

The available filters are:

\- System: these are the different subsystem available (Azimuth, Elevation,
balancing, cable wraps, ....)

\- Type: all, alarm, warning or info

\- Date interval: specifying from which date to which date.

![](media/ddf9db6f6c1bd5d0aef686c09dc6e6c6.png)

Figure 147. AlarmSavingTask.lvclass_CMD_GetFiltered.vi context help.

#### CMD_SaveEventArray

Save the specified event array to the log file of the actual day.

There is one AlarmHistory file per day, this file contains all the events that
ocurred in a certain day.

![](media/e0ea7f1dd6f13cf6a6b4943e231d1292.png)

Figure 148. AlarmSavingTask.lvclass_CMD_SaveEventArray.vi context help.

### CMD Reception loop

This loop receives the CMDs from the public methods and executes the required
actions depending on the received command. This loop has a state for each method
as well as some other states used for loop managing, each state is explained in
the next sections.

#### Init

This state is just executed once and is the first executed one. Here the
initialization actions are executed. These are:

-   Initialization of local variable

-   Registration of events for the event structure

-   Updating the displayed name of the VI for debug purposes

![](media/ed05759b3813585fdf3afba3164ab775.png)

>   Figure 149. AlarmSavingTask.lvclass_Process.vi Init

#### Idle

This state is executed constantly after executing every new CMD, here the events
created at the methods are received and executed in the next iteration.

![](media/d66fef97964c2e8769c6b0fd2b038dbf.png)

Figure 150. AlarmSavingTask.lvclass_Process.vi Idle

#### Timeout

This state is executed when there is something that must be executed in the
specified timeout of the Idle state event structure, see Figure 151.

![](media/ffea54a30df54a93c2d371689872356d.png)

Figure 151. Timeout state from Idle state

![](media/584b251ad792650692f863fd0386d08b.png)

Figure 152. AlarmSavingTask.lvclass_Process.vi Timeout

#### ShowWindow

This state is used to show the front panel of the process.

![](media/e69d2d974ee18af4bb649e21db92269b.png)

Figure 153. AlarmSavingTask.lvclass_Process.vi ShowWindow

#### HideWindow

This state is used to hide the front panel of the process.

![](media/4804276518cb9f6e2c8f0ccbdc7abf31.png)

Figure 154. AlarmSavingTask.lvclass_Process.vi HideWindow

#### CMD-SaveEventArray

This case is called from the CMD_SaveEventArray method. Here the specified event
array from the method is saved into the AlarmHistory file of the actual day.

There is one AlarmHistory file per day, this file contains all the events that
occurred in a certain day.

![](media/06c04d91822340597a716aec1b88283e.png)

Figure 155. AlarmSavingTask.lvclass_Process.vi CMD-SaveEventArray

#### CMD-GetFiltered

This case is called from the CMD_GetFiltered method. Here the AlarmHistory files
that correspond to the specified date interval are read and the events that
match the specified filters are sent to the method as a response.

The available filters are:

\- System: these are the different subsystem available (Azimuth, Elevation,
balancing, cable wraps, ....)

\- Type: all, alarm, warning or info

\- Date interval: specifying from which date to which date.

![](media/a2bd35e4930965e3d11c79d36f47d1e1.png)

Figure 156. AlarmSavingTask.lvclass_Process.vi CMD-GetFiltered

#### Shutdown

This state is reached when the shutdown CMD is received. This loop is used to
stop the CMD receiver loop.

![](media/b2b82c4abc519e2523f469bacf952aa8.png)

Figure 157. AlarmSavingTask.lvclass_Process.vi shutdown

#### Error

This state is reached when an error occurs at the task, here the error is
published and cleared for the next iteration.

![](media/94d26e2cc75248391e0965d35c852674.png)

Figure 158. AlarmSavingTask.lvclass_Process.vi Error

Alarm Recpetion Task HHD.lvclass
--------------------------------

This class is the one used in the HHD instead of the Alarm Recpetion
Task.lvclass. It just connects to the Alarm Recpetion Task.lvclass and requests
for certain data.

### Task process

This task was created using the NI GOOP Developing Suite, this task is object
oriented and the communication between methods is done using queues and user
events. The task main is contained in the process.vi, here there is one loop.
The loop is used for CMD reception, see Figure 159.

![](media/81f14738587b3624e4235ee988df923b.png)

Figure 159. Alarm Recpetion TaskHHD.lvclass_Process.vi Process

### Methods

Here each of the available methods are listed.

#### Alarm Recpetion TaskHHD \_Init.vi

Initialize the alarm Reception task for the HHD:

\- Launch the TCP client, used to communicate with the EUI in the MCC. For doing
this a xml file is needed, in this file a section with the name
"AlarmsForHHDClientConnectionData" must be found to initialize the server.

\- Launch the alarm reception task process.

![](media/1dab57ab36c08cee461362976065bf20.png)

Figure 160. Alarm Recpetion Task HHD.lvclass_Alarm Recpetion Task HHD_Init.vi
context help.

#### CleanUp

Stop the process

![](media/21fd22f0a0465890d08ed01b07c81d24.png)

Figure 161. Alarm Recpetion Task HHD.lvclass_CleanUp.vi context help.

#### CMD_AckAllNotACKED

Send the request to the AlarmRecption Task.lvclass process to ack all the
notAcked events

![](media/ee0dca42c0fe073955228065dc927ccf.png)

Figure 162. Alarm Recpetion Task HHD.lvclass_CMD_AckAllNotACKED.vi context help.

#### CMD_AckAllNotACKEDSubsystem

Send the request to the AlarmRecption Task.lvclass process to ack all the
notAcked events that correspond to the specified subsystem

![](media/1bfedee79cfcb2286bda5b3ec0d504d5.png)

Figure 163. Alarm Recpetion Task HHD.lvclass_CMD_AckAllNotACKEDSubsystem.vi
context help.

#### CMD_GetNotACKED

Send the request to the AlarmReception Task.lvclass to get all the not acked
events.

![](media/91e1bf208cce2d68e7fdd46c75001d40.png)

Figure 164. Alarm Recpetion Task HHD.lvclass_CMD_GetNotACKED.vi context help.

#### CMD_GetNotACKEDSubsystem

Send the request to the AlarmReception Task.lvclass to get the not acked events
that correspond to the specified system

![](media/93f5084716cde702204a7a32695ffa25.png)

Figure 165. Alarm Recpetion Task HHD.lvclass_CMD_GetNotACKEDSubsystem.vi context
help.

### CMD Reception loop

This loop receives the CMDs from the public methods and executes the required
actions depending on the received command. This loop has a state for each method
as well as some other states used for loop managing, each state is explained in
the next sections.

#### Init

This state is just executed once and is the first executed one. Here the
initialization actions are executed. These are:

-   Initialization of local variable

-   Updating the displayed name of the VI for debug purposes

![](media/2cbd5d75837396bb9fa9e8005a3b4dbd.png)

>   Figure 4. Alarm Recpetion Task HHD.lvclass_Process.vi block diagram

#### Idle

This state is executed constantly after executing every new CMD, here the events
created at the methods are received and executed in the next iteration.

![](media/7fe3bbf3a390f3ab2e60acc479d106d9.png)

Figure 150. Alarm Recpetion Task HHD.lvclass_Process.vi Idle

#### Timeout

This state is executed when there is something that must be executed in the
specified timeout of the Idle state event structure, see Figure 151.

![](media/ffea54a30df54a93c2d371689872356d.png)

Figure 151. Timeout state from Idle state

![](media/584b251ad792650692f863fd0386d08b.png)

Figure 152. Alarm Recpetion Task HHD.lvclass_Process.vi Timeout

#### ShowWindow

This state is used to show the front panel of the process.

![](media/e69d2d974ee18af4bb649e21db92269b.png)

Figure 153. Alarm Recpetion Task HHD.lvclass_Process.vi ShowWindow

#### HideWindow

This state is used to hide the front panel of the process.

![](media/4804276518cb9f6e2c8f0ccbdc7abf31.png)

Figure 154. Alarm Recpetion Task HHD.lvclass_Process.vi HideWindow

#### CMD-GetNotACKEDSubsystem

This case is called from the CMD_GetNotACKEDSubsystem method. Here the request
to the AlarmReception Task.lvclass to get the not acked events that correspond
to the specified system is sent and the response is sent to the calling method.

![](media/1e7a51c3c4badeecaf065cc934a64a2f.png)

Figure 14. Alarm Recpetion Task HHD.lvclass_Process.vi CMD-GetNotACKEDSubsystem

#### CMD-GetNotACKED

This case is called from the CMD_GetNotACKED method. Here the request to the
AlarmReception Task.lvclass to get all the not acked events is sent and the
response is sent to the calling method.

![](media/f7263a892e6c209ade37c76182d9b04a.png)

Figure 14. Alarm Recpetion Task HHD.lvclass_Process.vi CMD-GetNotACKED

#### CMD-AckAllNotACKED

This case is called from the CMD-AckAllNotACKED method. Here the request to the
AlarmReception Task.lvclass to ack all the not acked events is sent.

![](media/31d3dc1da5c8bdd74989b71a41709270.png)

Figure 16. Alarm Recpetion Task HHD.lvclass_Process.vi CMD-AckAllNotACKED

#### CMD-AckAllNotACKEDSubsystem

This case is called from the CMD-AckAllNotACKEDSubsystem method. Here the
request to the AlarmReception Task.lvclass to ack all the not acked events that
correspond to the specified subsystem is sent.

![](media/df687154f39384bec108bc069912aceb.png)

Figure 16. Alarm Recpetion Task HHD.lvclass_Process.vi
CMD-AckAllNotACKEDSubsystem

#### Shutdown

This state is reached when the shutdown CMD is received. This loop is used to
stop the CMD receiver loop.

![](media/b2b82c4abc519e2523f469bacf952aa8.png)

Figure 157. Alarm Recpetion Task HHD.lvclass_Process.vi shutdown

#### Error

This state is reached when an error occurs at the task, here the error is
published and cleared for the next iteration.

![](media/08df2dbb76173eb843703791e196f76d.png)

Figure 158. Alarm Recpetion Task HHD.lvclass_Process.vi Error
