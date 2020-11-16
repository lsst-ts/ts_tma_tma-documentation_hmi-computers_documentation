# TCP Client

This component is the one that connects to the TMA over TCP to send and receive
the TCP messages. The message to send is specified to the task by a public
method of the TCP client object, and the received messages are published in a
user event created when the object is initialized. These events are then
registered by another task to filter then and generate the proper user events
within the application.

For this component, the Sender.lvclass is used. This class is explained in
section 3.2 in a generic way, as this task can be used in many different ways
depending on the configuration set to it.

## Component Configuration

As explained in section 3.2.1 the Sender.lvclass requires a *\*.xml* file to
specify the configuration of the task. For this specific case, the used
configuration is specified in the “*HMIConfig.xml*” file located at the
Configuration folder inside the HMIComputers repo. In this file there is a
section called “*TMAConnectionData*” that is the one used for this component, by
specifying this name to the Sender_Init.

The values of this configuration can be modified to improve the performance or
if the IP or port of the machine running the TMA OMT (operation_manager)
changes. The values that must be maintained are:

-   ReadResponses: this must be set to FALSE.

-   ReadDataFromTCP: this must be set to TRUE.

## Sender.lvclass

This class when initialized launches a task that contains a TCP client. The
class main elements are explained in the following sections.

### Configuration file explained

This task requires from a configuration file for initialization, in*\*.xml*
format that contains the following sections:

-   Remote_Adress: the IP of the target you want to connect to.

-   Remote_Port: the port of the target you want to connect to.

-   Connect_Timeout_in_ms: timeout for the connect.

-   Send-Receive_Timeout_in_ms: timeout for the send receive, this is used when
    the read response is configured.

-   ReadResponses: when this is TRUE, every time a message is sent it waits for
    a response with the timeout (Send-Receive_Timeout_in_ms).

-   bytes_to_read: bytes to read from TCP.

-   ReadMode: here the read mode is configured, the options are:
    sel='Standard'\>0; sel='Buffered'\>1; sel='CRLF'\>2; sel='Immediate'\>3

-   Check_Connection_time_ms: this is the timeout for check the connection
    status, this timeout also sets the frequency for checking the TCP for
    reading responses.

-   ReadDataFromTCP: when this is TRUE the task reads the TCP messages with the
    check connection timeout frequency and publishes an event with the received
    data.

### Task process

This task was created using the NI GOOP Developing Suite, this task is object
oriented and the communication between methods is done using queues and user
events. The task main is contained in the process.vi, here there is one loop.
The loop used for CMD reception and for TCP listening, see Figure 1.

![](media/8b66f61d71b3541ebd69adacf70551e6.png)

Figure 1. Sender.lvclass_Process.vi block diagram

### Task methods

Here the available methods for this task are explained.

#### Sender_Init

Init the sender TCP class.

typeDefName is the name of the data that will be read from the ConfigurationFile
specified in the path

![](media/051a2e73e70264de756807e6fb1f596d.png)

Figure 2. Sender.lvclass_Sender_Init.vi context help.

#### CleanUp

CleanUp the sender TCP class.

![](media/5a345beac8a3e935f25b639ea22db54e.png)

Figure 3. Sender.lvclass_CleanUp.vi context help.

#### ControlProcessWindow

This VI is used to show or hide the process front panel. Depending on the
ShowProcessWindow control value.

![](media/a3550841e831fb3c2a014f57f2593b32.png)

Figure 4. Sender.lvclass_ControlProcessWindow.vi context help.

#### GetConnected

Check if the sender is connected to the TCP server

![](media/6ff59eaeefc59fd553434273b7485b4e.png)

Figure 5. Sender.lvclass_GetConnected.vi context help.

#### SendCloseConn

Send the command to the process to close the connection.

![](media/7d6ff118a7d1d2b138d7296f20add9b2.png)

Figure 6. Sender.lvclass_SendCloseConn.vi context help.

#### SendString

This VI sends the "SendString In" message through TCP and waits for a response
message.

This response message depends on the configuration of the task:

\- If the task is set to NOT wait for TCP responses it will give an empty string
as "Response".

\- If the task is set to wait for TCP responses it will give the response
message as "Response".

For either configuration if an error occurs in the task while trying to send the
data there is no error set in the error line and the following message is
obtained as "Response": Fault when sending data

![](media/5e0aaffd386f4275151785f5d4d5e585.png)

Figure 7. Sender.lvclass_SendString.vi context help.

### CMD Reception loop

This loop receives the CMDs from the public methods and executes the required
actions depending on the received command. This loop has a state for each method
as well as some other states used for loop managing, each state is explained in
the next sections.

#### Init

Here the local variables are initialized to the default values.

![](media/2d08352ebc2451865f650d8c4b9dd251.png)

Figure 8. Sender.lvclass_Process.vi Init

#### Idle

This state is executed constantly after executing every new CMD, here the events
created at the methods are received and executed in the next iteration.

![](media/743a2f92dbb5039388a081f74391a8a9.png)

Figure 9. Sender.lvclass_Process.vi Idle

#### Timeout

This state is executed when the CheckConnection time ms time is passed without
any new commands. When this happens the case Timeout is executed and depending
on the configuration it does:

-   If ReadDataFromTCP is set to TRUE: reads the TCP messages and publishes an
    event with the data.

-   If ReadDataFromTCP is set to FALSE: it just checks if the connection is ok.

-   In both cases if the connection is not OK it sends a reconnect to itself,
    see Figure 12.

![](media/091f9552d65a830b8536f8f845c6039d.png)

Figure 10. Sender.lvclass_Process.vi Timeout from Idle

![](media/e1fa8e5be479eb2529180eb7a3520b8d.png)

Figure 11. Sender.lvclass_Process.vi Timeout

![](media/1be04df075f3f54aa02c427bd084055d.png)

Figure 12. Sender.lvclass_Process.vi connection not OK

#### CMD-ShowWindow

This state is used to show the front panel of the process.

![](media/b71badfccde11bb93fb6f36633d84229.png)

Figure 13. Sender.lvclass_Process.vi CMD-ShowWindow

#### CMD-HideWindow

This state is used to hide the front panel of the process.

![](media/5aa1b581bea9842f60e7b1d8f75f8d90.png)

Figure 14. Sender.lvclass_Process.vi CMD-HideWindow

#### CMD-SendString

This case corresponds to the public method SendString, here the message
specified to the method is sent over TCP. Depending on the configuration:

-   If ReadResponses is set to TRUE, the task waits for the time specified as
    “Send-Receive Timeout in ms” to a response message from TCP and sends it to
    the public method in the response queue. See Figure 16.

-   If ReadResponses is set to FALSE, %NoResponse% is sent to the public method
    in the response queue.

![](media/77dee0dd617fe4f24b6ce93de2993037.png)

Figure 15. Sender.lvclass_Process.vi CMD-SendString

![](media/eb10377e7b097486e35430db315340c4.png)

Figure 16. Sender.lvclass_Process.vi block CMD-SendString wait for TCP response

#### CMD-Connect

This case opens the TCP connection to the TCP server, this case is called by the
reconnect private method used within the task.

![](media/c1c7c4368482d4e6c692fbc766736d2b.png)

Figure 17. Sender.lvclass_Process.vi CMD-Connect

#### CMD-CloseConnection

This case corresponds to the public method SendCloseConn, here the TCP
connection is closed.

![](media/0663c541a8cfe02851bee573e1185132.png)

Figure 18. Sender.lvclass_Process.vi CMD-CloseConnection

#### CMD-GetConnected

This case corresponds to the public method GetConnected, here the status of the
TCP connection is sent to the public method as a response in a queue.

![](media/0863454e6f316c874bafe9e3f0500609.png)

Figure 19. Sender.lvclass_Process.vi CMD-GetConnected

#### CMD-shutdown

This case is used to stop the task, called by the cleanup, here if the TCP
connection is still open it is closed. After that the task stops.

![](media/4004a4695f398916c1e6675b6a88524e.png)

Figure 20. Sender.lvclass_Process.vi CMD-Shutdown

#### Error

This case is used when there is an error in the error line, here the error from
the error line is cleared and if the TCP connection was active it is closed and
the reconnect method is used to open the connection again.

![](media/2731e97c5ffe676eb7678b889e90b1b2.png)

Figure 21. Sender.lvclass_Process.vi Error

