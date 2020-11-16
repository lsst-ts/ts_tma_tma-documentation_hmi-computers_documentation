### Power Supply Window

This window manages the power supply of main axes.

#### CMDs

This window sends two commands:

##### Power

This command is used to power on/off the subsystem.

- Command name: CppAppMPSCMD.lvclass:ResetAlarm.CMD.

- Command number: 601

- Command Parameters string:

  - Boolean: Command to power on (1) or power off (0).

##### Reset Alarm

This command is used to reset subsystem.

- Command name: CppAppMPSCMD.lvclass:SendPower.CMD.

- Command number: 602

#### Front Panel

Following the generic window layout, the front panel is divided into two main
sections: monitoring and control sections. The monitoring area will show the
relevant information for the subsystem. Navigation across windows will be
sometimes possible by pressing the window section that shows the information of
a given subsystem. The control area will show the control actions available for
the subsystem.

![Front Panel of the Power Supply_EUI.vi\label{figureonehundredeighty-fiveb100c545173fdabc8d44b5e86156ebfe}](../Resources/figures/b100c545173fdabc8d44b5e86156ebfe.png)

##### Monitoring

Within this part, all the indicators shown in Figure \ref{figureonehundredeighty-fived56fcea25c158a2e8dcc9d06ec6d4c8d} are described.

- The LED indicator is indicating the subsystem status.

  - Green: on.

  - Grey: off.

  - Red: fault.

- Status: this indicator does the same as the LED, monitoring the subsystem’s
    status by text.

- Current and DC voltage: those indicators, show the value of the current and
    the voltage.

- Graph. There are two buttons for stop updating values and for restart
    updating them:

  - Freeze graph: to stop the graph update.

  - Update graph: to continue the graph update.

##### Control

The control part is made up by three buttons:

- On button. Asks the subsystem to power on. When this button is pressed, the
    “On button” event is launched.

- Off button. Asks the subsystem to power off. When this button is pressed,
    the “Of button” event is launched.

- Reset button. Asks the subsystem to reset. When this button is pressed, the
    “Reset Alarm button” event is launched.

#### Connector pane

The required inputs for the VI are the following:

- Stop panel user event in: this is an input that has the reference to the
    event that stops the VI.

- VI Data: this input contains the following events’ references:

  - Sub Panel Window: reference to the subpanel where windows are loaded.
        Not used in this window.

  - Home and Window Selection User Event: event used to send the window
        selection to the menu from the loaded window. Not used in this window.

  - Main VI Events: event to communicate information to the main VI, the
        options available are: window name change, operation mode, alarm box and
        exit. Not used in this window.

  - Disable Menu? Event: event used to hide the menu when a CMD is active.
        used in SendDisableMenuEvent.vi at the InitSequence and EndSequence
        cases of the Consumer Loop.

  - HMIMain2Window Event: event to tell the actual window that the commander
        has changed from the main to the actual window. Used at the
        GetHMIMain2WindowEventRef.vi within LoopInitializationAction.vi.

#### Block Diagram

The block diagram is divided in three main areas: the init actions, the three
loops (the consumer loop, the main event loop and the updating graph loop) and
the exit actions.

##### Init Actions

![Block diagram of the VI.\label{figureonehundredeighty-six4a6ba29f15456f4301c6bd4ddfb5eaf4}](../Resources/figures/4a6ba29f15456f4301c6bd4ddfb5eaf4.png)

Within the initialization:

- ActivateDeactivateControls.vi: activates or deactivates front panel
    controls.

- Launch the event of the Update Graph control to ensure that the graph is
    updated from the VI initialization.

- Hide ack progress bar.

![Init part of the block diagram\label{figureonehundredeighty-seven5fead12919308a862083dbe0d8ed60fa}](../Resources/figures/5fead12919308a862083dbe0d8ed60fa.png)

###### Loop Initialization Actions

This VI does the following, using InitGeneralHMIRefs.vi:

- Each of the possible user events are registered:

  - Some of them are obtained from the TMA using
        GetEVENTFromTMA.lvclass:GetEventRefs.vi (explained with more details in
        the list of subVIs). Ack/Done/Error/Warning event out are registered, so
        that they could be managed in the Main Event Loop, depending on the
        response received from the TMA.

  - The reference of the HMIMain2Window event, to use it to communicate the
        commander change from the Main VI to the deferent windows.

  - Stop panel user event, to stop the window manually.

  - An error event is created using CreateErrorEvent.vi, creating the error
        event used to send the event from the consumer loop to the main loop.

  - GetHMIMain2WindowEventRef.vi returns the reference of the HMIMain2Window
        event to use it to communicate information from the Main VI to the
        different windows.

- Also, a buffer is initialized and registered, to use it at the Update Graph
    Loop.

- URLs for telemetry are registered, as well.

- Done/Ack Sync Queue is registered, to use it to synchronize the ack and done
    events with the TMA.

- Publish the local STO to false, has this system has no specific STO.

- Following information is obtained from this VI:

  - Consumer Loop Queue

  - Initialized References (Buffer object, Done/Ack Sync Queue, URL data and
        Event Registration Ref)

![LoopInitializationActions.vi\label{figureonehundredeighty-eight838e90d9f56cdf264d3332c91c53bb96}](../Resources/figures/838e90d9f56cdf264d3332c91c53bb96.png)

##### Exit Actions

At the exit part, the errors from different loops are appended. In the
HMIExitActions VI following actions are executed:

- The Destroy Error Event VI destroys the error event used to send the error
    from the consumer loop to the main loop.

- Deregister the telemetry data for the HHD.

- Clean up the graph buffer.

- Deregister event references.

![HMIExitActions.vi\label{figureonehundredninety63e5cd1c918bdedc5ec74ce777b52bec}](../Resources/figures/63e5cd1c918bdedc5ec74ce777b52bec.png)

##### Loops

There are three loops, the Main Event loop, the consumer loop and the update
graph loop. The communication between them is reviewed in the following figure:

![Window loop relation\label{figureonehundredeighty-fived56fcea25c158a2e8dcc9d06ec6d4c8d}](../Resources/figures/d56fcea25c158a2e8dcc9d06ec6d4c8d.png)

###### Main event loop

This loop responds to the events received from: the user actions in this window,
the error event from secondary loops, the events from Main.vi and Menu.vi and
the events from the PXI.

All the events are described in the next section.

####### Freeze Graph

Stop the graph update.

![Main Loop's events: Freeze graph user event.\label{figureonehundredninety5de5bcb6d980099bb1c31a56327ea24a}](../Resources/figures/5de5bcb6d980099bb1c31a56327ea24a.png)

####### Update graph

Start the graph update.

![Main Loop's events: Update graph user event.\label{figureonehundredninety-onef56ade8dbe74c456b45c80e2425a7f3c}](../Resources/figures/f56ade8dbe74c456b45c80e2425a7f3c.png)

####### Stop panel

This event stops the VI. StopVISequenceEnqueue VI enqueues all the steps needed
to stop the window.

The sequence steps are:

- EndSequence.

- Stop VI.

![Main Loop's events: Stop panel user event.\label{figureonehundredninety-two1c0d297b7cec3cc28e585a16cdaf280f}](../Resources/figures/1c0d297b7cec3cc28e585a16cdaf280f.png)

####### Timeout event

Updates the data for the monitoring area unless the graph.

![Main Loop's events: Timeout event.\label{figureonehundredninety-three0c3a4ed4187f18b812cb6f2ccca8e4d5}](../Resources/figures/0c3a4ed4187f18b812cb6f2ccca8e4d5.png)

####### Ack event out

This is executed when an ack event is received from the PXI. The next sequence
step, Wait For Done, is enqueued to the Consumer Loop queue at the opposite end,
and True constant is enqueued to the Done/Ack Sync queue to exit the wait for
ack state of the consumer loop.

![Main Loop's events: Ack event out user event.\label{figureonehundredninety-four1bab211e087c9c93a84196c0202db645}](../Resources/figures/1bab211e087c9c93a84196c0202db645.png)

####### Done event out

This is executed when a done event is received from the PXI. A true constant is
enqueued at the synchronization queue to exit the wait done case at the consumer
loop.

![Main Loop's events: Done event.\label{figureonehundredninety-fivec6b6a4387fbd346ab9621e1d8445df99}](../Resources/figures/c6b6a4387fbd346ab9621e1d8445df99.png)

####### Consumer loop error event out

It puts an error coming from other loops to the error line.

![Main Loop's events: Consumer loop error event.\label{figureonehundredninety-six2c3a257eb00d6522d9dd7dd60362dee8}](../Resources/figures/2c3a257eb00d6522d9dd7dd60362dee8.png)

####### Error event out

This is executed when an error event is received. It does the following:

- Filter the source to get the command family number:

  - 600 belongs to PowerSupply. In this case,
    - MPSSequences.lvlib:ForceTerminateSequence.vi is executed.

  - 5000 belongs to TMA OMT. In this case:

    - MPSSequences.lvlib:ForceTerminateSequence.vi is executed.

    - A pop-up is launched to display the error.

  - If the error has another origin do nothing.

The VI ForceTerminateSequence does the following:

- Flush queue.

- Enqueue EndSequence.

- Enqueue a true at the Synchronization queue.

![Main Loop's events: Error event. TMA Management Error case.\label{figureonehundredninety-seven99f25d2a864fca7c84e664c61055ca73}](../Resources/figures/99f25d2a864fca7c84e664c61055ca73.png)

![Main Loop's events: Error event. Power Supply error case.\label{figureonehundredninety-eight2c29984b14e118b9af3b1eced71ec134}](../Resources/figures/2c29984b14e118b9af3b1eced71ec134.png)

####### On button

It enqueues the steps needed to turn on the subsystem using the
OnSequenceEnqueue.

The following sequence steps are enqueued:

- InitSequence.

- On

- WaitForAck.

- EndSequence.

![Main Loop's events: On Button value change.\label{figureonehundredninety-nine547336821cedb8e73657bc2ac6a62ab3}](../Resources/figures/547336821cedb8e73657bc2ac6a62ab3.png)

####### Off button

It enqueues the steps needed to turn on the subsystem using the
OffSequenceEnqueue.vi.

The following sequence steps are enqueued:

- InitSequence.

- Off

- WaitForAck.

- EndSequence.

![Main Loop's events: Off Button value change.\label{figuretwohundred73fd72b7f65bb704376a523dbbc2f451}](../Resources/figures/73fd72b7f65bb704376a523dbbc2f451.png)

####### Reset alarm button

It enqueues the steps needed to reset the subsystem using the
ResetSequenceEnqueue.vi.

The following sequence steps are enqueued:

- InitSequence.

- Reset

- WaitForAck.

- EndSequence.

![Main Loop's events: Reset alarm button value change\label{figuretwohundredone1422f39d292b3ccc6ba7b49d2a09e7d5}](../Resources/figures/1422f39d292b3ccc6ba7b49d2a09e7d5.png)

####### HMIMain2Window Event out

It receives the event reference coming from the main, and depending on it does
the following:

- Update Commander: it updates the visibility and the enabling of the controls
    depending on the mode of operation and the device used.

- Default: it does nothing.

![Main Loop's events: HMIMain2Window event.\label{figuretwohundredtwo00c726b75dfc27ed3ed22607c6fc435d}](../Resources/figures/00c726b75dfc27ed3ed22607c6fc435d.png)

###### Consumer Loop

Here the steps enqueued by the main loop are dequeued and processed.

![The consumer loop.\label{figuretwohundredthree2d20046ae31069aff148adac8a63d894}](../Resources/figures/2d20046ae31069aff148adac8a63d894.png)

In this loop CppAppMPSCMD.lvclass, that is inherited from CppAppCommand.lvclass,
is received from the main loop. It overrides the methods of
CppAppCommand.lvclass. This class has only 2 methods as this subsystem has only
two CMDs available.

Available methods:

- Power CMD: CppAppMPSCMD.lvclass:SendPowerCMD.vi

- Reset Alarm CMD: CppAppMPSCMD.lvclass:ResetAlarm.vi

####### On

Requests to turn on the subsystem using the SendPowerCMD.vi, overridden by the
one from the CppAppMPSCMD.lvclass.

![Cases of the consumer loop's case structure: On case.\label{figuretwohundredthree86760e2705506f748c2b4a9e032b178b}](../Resources/figures/86760e2705506f748c2b4a9e032b178b.png)

####### Off

Requests to turn off the subsystem using the SendPowerCMD.vi, overridden by the
one from the CppAppMPSCMD.lvclass.

![Cases of the consumer loop's case structure: Off case.\label{figuretwohundredfive670b98064c4114a8c58474c1b91d6fac}](../Resources/figures/670b98064c4114a8c58474c1b91d6fac.png)

####### Reset

Requests to reset alarms of the subsystem using the ResetAlarm.vi, overridden by
the one from the CppAppMPSCMD.lvclass.

![Cases of the consumer loop's case structure: Reset case.\label{figuretwohundredsixda13b302d16b941fd2eeef62b5a41840}](../Resources/figures/da13b302d16b941fd2eeef62b5a41840.png)

####### Wait For Ack

It waits for the ack response of the PXI in order to synchronize. After a
timeout of 2,5 seconds throws a timeout error, the ack must be received before
this timeout.

![Cases of the consumer loop's case structure: WaitForAck case.\label{figuretwohundredseven3828350ba86b51783d12b0e8314ae5a9}](../Resources/figures/3828350ba86b51783d12b0e8314ae5a9.png)

####### Wait For Done

It waits for the done response of the PXI in order to synchronize. After the
specified timeout at the ack event a timeout error is throwed.

![Cases of the consumer loop's case structure: WaitForDone case.\label{figuretwohundredeight07fd9cb2999e02b541d0ecea67a3672a}](../Resources/figures/07fd9cb2999e02b541d0ecea67a3672a.png)

####### Init Sequence

This case puts the window in executing mode: disabling the possibility of
pushing buttons and hiding the menu.

![Cases of the consumer loop's case structure: InitSequence case.\label{figuretwohundrednine4ad159eb612954c5bc11cd4aa47c4d79}](../Resources/figures/4ad159eb612954c5bc11cd4aa47c4d79.png)

####### End Sequence

This case puts the window back into normal mode: hides the ack progress bar,
enables the controls and shows the menu.

![Cases of the consumer loop's case structure: EndSequence case.\label{figuretwohundredtenf9d38fc1903263c02c7fe1ca7f81ba9b}](../Resources/figures/f9d38fc1903263c02c7fe1ca7f81ba9b.png)

####### Stop VI

This case is used to stop the current loop. Here the Consumer Loop queue is
released.

![Cases of the consumer loop's case structure: StopVI case.\label{figuretwohundredeleven7de9017618651709ec41755226e03c12}](../Resources/figures/7de9017618651709ec41755226e03c12.png)

###### Update Graph Loop

This loop at the bottom is used uniquely for the graph management.

UpdateHMIWaveformFromDBLArrayVariables.vi takes from the telemetry task the
values to plot in the graph. The graph is updated with the
UpdategraphPlotNames.vi. The temporization of the loop is done by the
TelemetryLoggingTask.lvclass:GetNewTelemetryData.vi that waits until new data is
available.

The case structure from the loop has two possible cases:

- If the waveform array is not empty and the local variable update graph is
    true:

  - The graph time is change to relative time ToRelativeTime.vi.

  - The graph is updated.

  - Legend names are updated if first call.

- In other cases, nothing is done.

![The Updating graph loop.\label{figuretwohundredtwelve32acf94dd2a058c306f01c699b601194}](../Resources/figures/32acf94dd2a058c306f01c699b601194.png)

#### List of SubVIs

- Activatedeactivatecontrols.vi: This manages the control cover, to do so it
    checks the following global variables:

  - GBL_UserGroups

  - GBL_OperationMode

  - GBL_CurrentDevice

![Activatedeactivatecontrols.vi context help.\label{figuretwohundredthirteenc72b36307540c94a17fb6f977cb73c59}](../Resources/figures/c72b36307540c94a17fb6f977cb73c59.png)

- Buffer.lvclass:InsertSampleAndGetBuffer.vi: Insert sample in the buffer and
    get last desired elements of the buffer

![Buffer.lvclass:InsertSampleAndGetBuffer.vi icon.\label{figuretwohundredfourteenthousandonehundredeighty-six3b6d59b599b0aa9d048769e15f09952c}](../Resources/figures/3b6d59b599b0aa9d048769e15f09952c.png)

- CppAppCommand.lvclass:ResetAlarm.vi: Send Reset Alarm Command to

![CppAppCommand.lvclass:ResetAlarm.vi context help.\label{figuretwohundredfifteen94de407240f830af8d5c77ee7d0f7ba3}](../Resources/figures/94de407240f830af8d5c77ee7d0f7ba3.png)

- CppAppCommand.lvclass:SendPowerCMD.vi: Send Command to TMA Management

![CppAppCommand.lvclass:SendPowerCMD.vi context help.\label{figuretwohundredsixteen0a909c59c4c56dcc3466abad24cfc008}](../Resources/figures/0a909c59c4c56dcc3466abad24cfc008.png)

- EndSequence.vi: It disables the possibility of pushing buttons sending the
    event disable menu. Buttons are greyed out and the ack progress bar is made
    invisible.

![EndSequence.vi context help.\label{figuretwohundredseventeen62c92f012ded37f8a790de438c746070}](../Resources/figures/62c92f012ded37f8a790de438c746070.png)

- FilterSource.vi: The source will be filtered to get the command family
    number if it exist. If the number does not exist the source will be
    transmitted as it comes.

![FilterSource.vi context help.\label{figuretwohundredeighteen32ab4a1b4976ab16bd868659ef08bcb2}](../Resources/figures/32ab4a1b4976ab16bd868659ef08bcb2.png)

- GenerateErrorEvent.vi: Generates the error event when an error occurs at the
    consumer loop of the HMI windows

![GenerateErrorEvent.vi context help.\label{figuretwohundrednineteendd205ae467e5e356badd5b2a48958b6a}](../Resources/figures/dd205ae467e5e356badd5b2a48958b6a.png)

- GetLastStateChartState.vi: This vi takes the last state from the status
    string.

![GetLastStateChartState.vi context help.\label{figuretwohundredfortyd667bde7952d593c6b95209d5782c6cc}](../Resources/figures/d667bde7952d593c6b95209d5782c6cc.png)

- GetTelemetryForWindow.vi: Get telemetry data for the specified window.

![GetTelemetryForWindow.vi context help.\label{figuretwohundredforty-one7b12af820984539ff93da07b4cc1e8cc}](../Resources/figures/7b12af820984539ff93da07b4cc1e8cc.png)

- GraphUpdateLoopErrorHandling.vi: Graph update while loop error handling,
    this VI is makes the required actions to manage the errors in the graph
    update loop.

![GraphUpdateLoopErrorHandling.vi context help.\label{figuretwohundredforty-two4754ca1ec1893a03a2c64d7704bf8d37}](../Resources/figures/4754ca1ec1893a03a2c64d7704bf8d37.png)

- HMIExitActions.vi: This SubVI does the following:

  - Destroy the consumer loop error event.

  - Deregister telemetry for the HHD.

![HMIExitActions.vi context help.\label{figuretwohundredforty-threefa06b6a7cbd051d44bae6f08eeede080}](../Resources/figures/fa06b6a7cbd051d44bae6f08eeede080.png)

- HMITimeoutChoice.vi: Define the timeout for refreshing the HMI depending on
    the current device GBL

![HMITimeoutChoice.vi context help.\label{figuretwohundredforty-fourc37aa9a19a92493d676519fa9d27bdb8}](../Resources/figures/c37aa9a19a92493d676519fa9d27bdb8.png)

- InitSequence.vi: It disables the possibility of pushing buttons sending the
    event disable menu. Buttons are greyed out.

![InitSequence.vi context help.\label{figuretwohundredforty-fivef99cad48d796825cf3e97374839b73da}](../Resources/figures/f99cad48d796825cf3e97374839b73da.png)

- LoopInitializationActions.vi: Here the initialization actions for the
    PowerSupply window are executed. The actions are:

  - Get the telemetrry URLs for this window.

  - Obtain the queue for consumer loop.

  - Obtain the queue for the main loop.

  - Register events for the window

  - Ack Event: This one is taken from the
        GetEVENTFromTMA.lvclass:GetEventRefs. - Done Event: This one is taken
        from the GetEVENTFromTMA.lvclass:GetEventRefs. - Error Event: This one
        is taken from the GetEVENTFromTMA.lvclass:GetEventRefs. - Warning Event:
        This one is taken from the GetEVENTFromTMA.lvclass:GetEventRefs.

  - Consumer Loop Error Event: This one is created here and then registered.

  - HMIMain2Window Event: This one is taken from the VI Data In.

![LoopInitializationActions.vi context help.\label{figuretwohundredforty-sixdc86ba28673df8370ebc9fac6a50b428}](../Resources/figures/dc86ba28673df8370ebc9fac6a50b428.png)

- MPSSecuences.lvlib:ForceTerminateSequence.vi: Forces to end the sequence.
    Error in the input is only transmitted to the output.

![MPSSecuences.lvlib:ForceTerminateSequence.vi context help.\label{figuretwohundredforty-seven50e9056ae8808b66f439fe18a7cf09ca}](../Resources/figures/50e9056ae8808b66f439fe18a7cf09ca.png)

- MPSSecuences.lvlib:NoAckOperations.vi: Do the operations for an ack event:

  - Ack: Do nothing.

  - No Ack: Restore window and show a pop-up.

![MPSSecuences.lvlib:NoAckOperations.vi context help.\label{figuretwohundredforty-eight16e91ff8f5f6a57170bc7a81bbcce4a9}](../Resources/figures/16e91ff8f5f6a57170bc7a81bbcce4a9.png)

- MPSSecuences.lvlib:OffSecuenceEnqueue.vi: Enqueues the sequences needed to
    turn off the system..

![MPSSecuences.lvlib:OffSecuenceEnqueue.vi context help.\label{figuretwohundredforty-nine03231b3c5ae2a0586f8611cd74def969}](../Resources/figures/03231b3c5ae2a0586f8611cd74def969.png)

- MPSSecuences.lvlib:OnSecuenceEnqueue.vi: Enqueues the sequences needed to
    turn on the system..

![MPSSecuences.lvlib:OnSecuenceEnqueue.vi context help.\label{figuretwohundredfifty6f9d86e6324b9ac3fb857a07c2a57ec1}](../Resources/figures/6f9d86e6324b9ac3fb857a07c2a57ec1.png)

- MPSSecuences.lvlib:ResetSecuenceEnqueue.vi: : Enqueues the sequences needed
    to reset the system.

![MPSSecuences.lvlib:ResetSecuenceEnqueue.vi context help.\label{figuretwohundredfifty-one2c11f35c54ea7625e125a5d8d08cbc24}](../Resources/figures/2c11f35c54ea7625e125a5d8d08cbc24.png)

- MPSSecuences.lvlib:StopVISecuenceEnqueue.vi: : Enqueues the sequences needed
    to stop the VI.

![MPSSecuences.lvlib:StopVISecuenceEnqueue.vi context help.\label{figuretwohundredfifty-twod002a2d3e2880efd088e151117cb9fad}](../Resources/figures/d002a2d3e2880efd088e151117cb9fad.png)

- PanelErrorHandling.vi: Display the error using a "Simple error handler" if
    the error is new.

![PanelErrorHandling.vi context help.\label{figuretwohundredfifty-three4b29ad24388a2d754fb0c08366b05a40}](../Resources/figures/4b29ad24388a2d754fb0c08366b05a40.png)

- ProgressVarUpdate.vi: This VI makes the progress var keep restarting it self
    for ever.

![ProgressVarUpdate.vi context help.\label{figuretwohundredfifty-foure45d6f7e8a47e6f6a4ab82c6688b6ab4}](../Resources/figures/e45d6f7e8a47e6f6a4ab82c6688b6ab4.png)

- StringStatus2ColorBoxStatus.vi: Gets the color for the status indicator from
    the status string. The status and arrive with substates (case insetive) This
    will put a green color in the output if the status starts with a "On" word
    This will put a red color if "Fault" or "alarm" is the status string or if
    starts with "internal errors" Warning is not implemented yet

![StringStatus2ColorBoxStatus.vi context help.\label{figuretwohundredfifty-fivebf82482babf8ed2a822c35877ffd1f55}](../Resources/figures/bf82482babf8ed2a822c35877ffd1f55.png)

- TimeDataBuffer.lvclass:InsertSampleAndGetBufferTimeData.vi: Insert sample in
    the buffer and get last desired elements of the buffer

![TimeDataBuffer.lvclass:InsertSampleAndGetBufferTimeData.vi contexthelp.\label{figuretwohundredfifty-six07bc00655076c3659d0461ffd30c734b}](../Resources/figures/07bc00655076c3659d0461ffd30c734b.png)

- TMAOMT_ErrorDialog.vi: Shows a pop up window when an error occurs at the TMA
    Management Operation task

![TMAOMT_ErrorDialog.vi context help.\label{figuretwohundredfifty-sevenbd564dac0c12d28e4d47bc632f24b340}](../Resources/figures/bd564dac0c12d28e4d47bc632f24b340.png)

- ToRelativeTime.vi: Converts full time to relative.

![ToRelativeTime.vi context help.\label{figuretwohundredfifty-eight31a1b886e8826a1ca283e8fe3af6048c}](../Resources/figures/31a1b886e8826a1ca283e8fe3af6048c.png)

- UpdateGraphPlotNames.vi: Update legend names using the variable names. The
    legend row number is changed according to the number of waveforms

![UpdateGraphPlotNames.vi context help.\label{figuretwohundredfifty-nine5469c31b487fb41db1b74b1296b9de8c}](../Resources/figures/5469c31b487fb41db1b74b1296b9de8c.png)

- UpdateHMIWaveformFromDBLArrayVariables.vi: Generates the waveform to display
    by the HMI

![UpdateHMIWaveformFromDBLArrayVariables.vi context help.\label{figuretwohundredsixty7a7aa13444b98408bef57ddcf72e7574}](../Resources/figures/7a7aa13444b98408bef57ddcf72e7574.png)

- WaitForAck.vi: Sets timeout with a fixed time of 2500 ms as it should
    respond very fast.

![WaitForAck.vi context help.\label{figuretwohundredsixty-one269c21999087c54579bf4b5d2d1ad83c}](../Resources/figures/269c21999087c54579bf4b5d2d1ad83c.png)

- WaitForDone.vi: Add 2500ms to received timeout. The TMA OMT will add 2000 ms
    to the received timeout so this 500 ms more.

![WaitForDone.vi context help.\label{figuretwohundredsixty-twoaad243c441b5ba389330ce8dd2d4633a}](../Resources/figures/aad243c441b5ba389330ce8dd2d4633a.png)
