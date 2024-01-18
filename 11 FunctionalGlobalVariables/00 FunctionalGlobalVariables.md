# Functional Global Variables

## ErrorTask.lvclass:FGV_ObjectReference.vi

This FGV contains the object of the ErrorTask.
This variable is written when the object is created and read when the object reference is needed.

Path: HMIComputers\CppAppComm\ErrorTask_class\private\FGV_ObjectReference.vi

VI callers:

- AmbientTemperatureUpdate.lvclass:ErrorHandling.vi
- AutomaticTestingReceiver.lvclass:Process.vi
- AutomaticTesting_Events.lvclass:Process.vi
- ErrorTask.lvclass:SendError.vi
- AutomaticTesting_Responses.lvclass:Process.vi
- ErrorTask.lvclass:Process.vi
- ErrorTask.lvclass:ErrorTask_Init.vi
- AmbientTemperatureUpdate.lvclass:Task.vi
- TMAOMTMonitoring.lvclass:Process.vi
- GetEVENTFromTMA.lvclass:Process.vi
- TDMSActions.lvclass:Process.vi
- TelemetryLogingTask.lvclass:Process.vi
- TelemetryFilesManagement.lvlib:Task.vi
- AlarmSavingTask.lvclass:Process.vi
- Alarm Recpetion Task.lvclass:Process.vi
- Alarm Recpetion Task HHD.lvclass:Process.vi
- CommanderCheck.lvclass:Process.vi
- Clock.lvclass:ErrorHandling.vi
- Clock.lvclass:Task.vi
- WindowTelemetrySavingTask.lvclass:Process.vi
- TCP_Telemetry.lvclass:Process.vi
- PanelErrorHandling.vi
- LaunchErrorWindow.vi
- UnloadVI.vi
- CargaVentanaEnSubpanel.vi

## FGV_ResetSelection.vi

FGV for reset selection in safety matrix. Once the reset value is read it will be force to false

Path: HMIComputers\HMI\Subsystem\Safety\FGV_ResetSelection.vi

VI callers:

- Safety System_EUI.vi
- UpdateSafetyMatrixColors.vi

## ManageTelemetryConfiguration.lvlib:FGV_TelemetryTopicsDefinitions.vi

This FGV contains the Telemetry Topics Definitions, this variable is written when the telemetry topic configuration file is read and then is its values are read when needed.
The available actions are:

- InitTopicsDefinitions: for creating the empty variable
- AddTopicDefinition: to include a new topic definition to the variable
- GetAllTopicDefinitions: returns all the topic definitions saved in the variable
- ReadTopicDefinition: returns the topic definition for the specified TopicName, if not found an error code 42 is returned with the appropiate explanation.
- ExitTopicsDefinitions: for removing the data as an empty variable

Path: HMIComputers\PXIComm\Telemetry\ManageTelemetryConfiguration\FGV_TelemetryTopicsDefinitions.vi

VI callers:

- InitTelemetryLoggingTask.vi
- ManageTelemetryConfiguration.lvlib:ReadTelemetryTopicsConfigFile.vi
- ManageTelemetryConfiguration.lvlib:InitTelemetryConfigurationFromFiles.vi
- ManageTelemetryConfiguration.lvlib:GetURLsDefinitionFromFGVs.vi

## ManageTelemetryConfiguration.lvlib:FGV_WindowTelemetryURLs.vi

This FGV contains the Window Telemetry URLs, this variable is written when the window telemetry configuration file is read and then is its values are read when needed.
The available actions are:

- InitWindowsURLs: for creating the empty variable
- AddWindowURLs: to include a new variable URL definition to the variable
- GetAllWindowNames: returns all the window names stored in the variable
- ReadWindowURLs: returns the URLs for the specified Window Name, if not found an error code 42 is returned with the appropiate explanation.
- ExitWindowsURLs: for removing the data as an empty variable

Path: HMIComputers\PXIComm\Telemetry\ManageTelemetryConfiguration\FGV_WindowTelemetryURLs.vi

VI callers:

- InitTelemetryLoggingTask.vi
- ManageTelemetryConfiguration.lvlib:ReadWindowTelemetryURLs.vi
- ManageTelemetryConfiguration.lvlib:InitTelemetryConfigurationFromFiles.vi
- GetTelemetryURLsForWindow.vi
- All Data View_EUI.vi
- ObtainDesiredTelemetry.vi

## VariableClient.lvlib:FGV_FrameQueue.vi

This is a FGV to store the references for the frame counter queues

Path: HMIComputers\teknsv_client\VariableClient\private\FGV_FrameQueue.vi

VI callers:

- VariableClient.lvlib:Method_GetBinarySubscriptionsFrameQueues.vi
- VariableClient.lvlib:SubscribedVariablesProcessingLoop.vi
- VariableClient.lvlib:Task.vi

## FGV_AlarmReceptionTask.vi

FGV for Alarm reception task objects. When init all subsystem objects will be initialized (repos and so on) and when cleanup all will be cleaned.
The FGV also allows to get all objects

Path: HMIComputers\Variables\FGVs\FGV_AlarmReceptionTask.vi

VI callers:

- GetObjectForCurrentDevice.vi
- MainHMIAlarmIndicatorManagement.vi
- ExitHMISystems.vi
- InitHMISystems.vi

## FGV_AlarmReceptionTaskHHD.vi

FGV for Alarm reception task objects for the HHD. When init all subsystem objects will be initialized (repos and so on) and when cleanup all will be cleaned.
The FGV also allows to get all objects

Path: HMIComputers\Variables\FGVs\FGV_AlarmReceptionTaskHHD.vi

VI callers:

- GetObjectForCurrentDevice.vi
- MainHMIAlarmIndicatorManagement.vi
- InitHMISystems.vi

## FGV_AmbientTemperatureUpdateTask.vi

FGV for managing the Ambient Temperature Update task class.
Available actions are:

- Init: this starts a class instance and launches the task.
- GetObject: returns the current class object
- CleanUp: cleans up the task and the object

Path: HMIComputers\Variables\FGVs\FGV_AmbientTemperatureUpdateTask.vi

VI callers:

- SetAmbientTemperatureForceStatus.vi
- GetAmbientTemperatureForceStatus.vi
- ExitHMISystems.vi
- InitHMISystems.vi

## FGV_AutomaticTestingSystem.vi

FGV to manage the ATS system. This FGV manages all the subTasks that are used for the automatic test system.
The available actions are:

- Init: this reads the configuration file for the application and depending on the configuration launches all the ATS tasks or not.
- GetObject: returns all the initialized classes objects
- CleanUp: stops and cleans up all the initialized objects, if they were not initialized it does nothing

Path: HMIComputers\Variables\FGVs\FGV_AutomaticTestingSystem.vi

VI callers:

- AutomaticTestingReceiver.lvclass:ChangeTargetVI.vi
- AutomaticTesting_Events.lvclass:SendGenericEvent.vi
- AutomaticTesting_Responses.lvclass:SendDone_ForSettingsWindow.vi
- AutomaticTesting_Responses.lvclass:SendGenericResponse.vi
- AutomaticTesting_Events.lvclass:SendDiscreteStateReportingEvent.vi
- AutomaticTestingReceiver.lvclass:SubscribeMenuRef.vi
- AutomaticTestingReceiver.lvclass:ChangeToPreviousTargetVI.vi
- AutomaticTestingReceiver.lvclass:SubscribeHMIMainRef.vi
- GetEVENTFromTMA.lvclass:PharseEvent.vi
- Alarm History_EUI.vi
- Auxiliary Boxes_EUI.vi
- Azimuth Cable Wrap_EUI.vi
- Azimuth Drives Thermal_EUI.vi
- Azimuth Drives_EUI.vi
- Azimuth General View_EUI.vi
- Balancing General View_EUI.vi
- Cabinet 0101_EUI.vi
- Camera Cable Wrap_EUI.vi
- Deployable Platform_EUI.vi
- Elevation Drives Thermal_EUI.vi
- Elevation Drives_EUI.vi
- Elevation General View_EUI.vi
- Encoder_EUI.vi
- General Commands_EUI.vi
- Locking Pins_EUI.vi
- MainAxesWarningEventActions.vi
- MainAxesErrorEventActions.vi
- Main Axis General View_EUI.vi
- Main Cabinet_EUI.vi
- Mirror Cover General View_EUI.vi
- Mirror Cover Locks_EUI.vi
- OSS General View_EUI.vi
- Power Supply_EUI.vi
- Safety System_EUI.vi
- Top End Chiller Cabinets_EUI.vi
- Top End Chiller General View_EUI.vi
- ExitHMISystems.vi
- InitHMISystems.vi

## FGV_ClockTask.vi

FGV for clock task object. This FGV manages the clock class.
The available actions are:

- Init: launches the clock task
- GetObject: returns the class object
- CleanUp: stops and cleans up the initialized task

Path: HMIComputers\Variables\FGVs\FGV_ClockTask.vi

VI callers:

- ExitHMISystems.vi
- InitHMISystems.vi

## FGV_CommsWithTMA.vi

FGV to manage the communications with the mtmount operation manager module (C++). This FGV manages all the subTasks that are used for this communication.
The available actions are:

- Init: this reads the configuration file for the application and launches all the tasks
- GetObject: returns all the initialized classes objects
- CleanUp: stops and cleans up all the initialized objects

Path: HMIComputers\Variables\FGVs\FGV_CommsWithTMA.vi

VI callers:

- InitAlarmReceptionTask.vi
- ExitHMISystems.vi
- InitHMISystems.vi

## FGV_CppAppCommand.vi

FGV to manage the CppAppCommand class. This FGV manages the class that sends the commands to the operation manager.
The available actions are:

- Init: Initialize the CppAppCommand.lvclass
- GetObject: returns the class object
- CleanUp: clean up the initialized object

Path: HMIComputers\Variables\FGVs\FGV_CppAppCommand.vi

VI callers:

- ExitHMISystems.vi
- InitHMISystems.vi

## FGV_ErrorTask.vi

FGV for the error task.

- Init: launch the error task.
- Getobject: returns the task object.
- Cleanup: stop and cleanUp the task.

Path: HMIComputers\Variables\FGVs\FGV_ErrorTask.vi

VI callers:

- ExitHMISystems.vi
- InitHMISystems.vi

## FGV_MonitorizationsTasks.vi

FGV to manage the TMAOMTMonitoring_Class class.
The available actions are:

- Init: reads the application configuration file and launches an instance of the task for the class
- GetObject: returns the class object
- CleanUp: stops the launched task and cleans up the initialized object

Path: HMIComputers\Variables\FGVs\FGV_MonitorizationsTasks.vi

VI callers:

- Operation Manager_EUI.vi
- ExitHMISystems.vi
- InitHMISystems.vi

## FGV_TCPTelemetryTask.vi

FGV to manage the TCP_TelemetryTask class. The class that sends the telemetry to the CSC.
The available actions are:

- Init: reads the application configuration file and launches an instance of the task for the class
- GetObject: returns the class object
- CleanUp: stops the launched task and cleans up the initialized object

Path: HMIComputers\Variables\FGVs\FGV_TCPTelemetryTask.vi

VI callers:

- ExitHMISystems.vi
- InitHMISystems.vi

## FGV_TelemetryLoggingTask.vi

FGV to manage the TelemetryLoggingTask class. The class that gets the telemetry from the PXIs.
The available actions are:

- Init: reads the application configuration file and launches an instance of the task for the class
- GetObject: returns the class object
- CleanUp: stops the launched task and cleans up the initialized object

Path: HMIComputers\Variables\FGVs\FGV_TelemetryLoggingTask.vi

VI callers:

- GetTelemetryForWindow.vi
- TelemetryFilesManagement.lvlib:Task.vi
- AskZipOldFiles.vi
- AskDefragmentOldFiles.vi
- AskDeleteOldFiles.vi
- HHDTelemetri MonitoringManage.vi
- XYGraphTypes.lvlib:DBLArray_SendData.vi
- XYGraphTypes.lvlib:GetEventForTelemetry.vi
- XYGraphTypes.lvlib:DBL_SendData.vi
- Main Axis General View_EUI.vi
- AuxiliaryBoxesGetTelemetry.vi
- AzimuthBearings1GetTelemetry.vi
- AzimuthBearings2GetTelemetry.vi
- ACWGetTelemetry.vi
- AzimuthDrivesGetTelemetry.vi
- AzimuthDrivesThermalGetTelemetry.vi
- BalancingGetTelemetry.vi
- CCWGetTelemetry.vi
- DeployablePlatformGetTelemetry.vi
- MainCabinetGetTelemetry.vi
- ElevationBearingsGetTelemetry.vi
- ElevationDrivesGetTelemetry.vi
- ElevationDrivesThermalGetTelemetry.vi
- GeneralViewGetTelemetry.vi
- LockingPinsGetTelemetry.vi
- PowerSupplyGetTelemetry.vi
- MainAxesGetTelemetry.vi
- MirrorCoverGetTelemetry.vi
- MirrorCoverLocksGetTelemetry.vi
- OSSGeneralViewGetTelemetry.vi
- OSSAccumulatrosGetTelemetry.vi
- OSSMainPumpsGetTelemetry.vi
- OSSOilCirculationGetTelemetry.vi
- OSSOilCoolingGetTelemetry.vi
- ThermalGeneralViewGetTelemetry.vi
- TECGetTelemetry.vi
- TECCabinetsGetTelemetry.vi
- ExitHMISystems.vi
- FGV_TCPTelemetryTask.vi
- InitHMISystems.vi
- GetQueueStatus.vi
- StartStopSavingTelemetry.vi

## FGV_TelemetryTaskConfig.vi

FGV for having the telemetry task configuration from the HMIConfig file.
The available actions are:

- Init: read the application configuration file and store the read data in the variable
- GetObject: returns the stored data from the variable
- CleanUp: cleans the data from the variable

Path: HMIComputers\Variables\FGVs\FGV_TelemetryTaskConfig.vi

VI callers:

- FGV_TelemetryLoggingTask.vi
- CheckDeleteZipAndDefragement.vi

## FGV_TelemetryTaskTekNsvRef.vi

FGV for managing the TekNSV client references identified by address.
The available actions are:

- WriteAddress: includes the provided *TelemetryTask TekNSV Ref* to the variable with the associated *Address* value
- ReadAddress: gets the *TelemetryTask TekNSV Ref* related to the specified *Address*, if the provided *Address* is not found an error with code 42 and the related description is returned
- ReadAll: returns all the available *TelemetryTask TekNSV Refs* and related *Addresses*
- CleanUp: resets the variable to empty values

Path: HMIComputers\Variables\FGVs\FGV_TelemetryTaskTekNsvRef.vi

VI callers:

- TelemetryLogingTask.lvclass:GenerateJsonStringForVariableSubscription.vi
- TelemetryLogingTask.lvclass:CleanUp.vi
- TelemetryLogingTask.lvclass:TelemetryLogingTask_Init.vi

## FGV_WindowTelemetryDirectory.vi

FGV for storing the WindowTelemetryDirectory

Path: HMIComputers\Variables\FGVs\FGV_WindowTelemetryDirectory.vi

VI callers:

- Read_FGV_WindowTelemetryDirectory.vi
- InitWindowTelemetryDirectoryFGV.vi

## FGV_WindowTelemetrySavingTask.vi

FGV to manage the WindowTelemetrySavingTask class. The class that manages storing the last movements done from the EUI.
The available actions are:

- Init: launches an instance of the task for the class
- GetObject: returns the class object
- CleanUp: stops the launched task and cleans up the initialized object

Path: HMIComputers\Variables\FGVs\FGV_WindowTelemetrySavingTask.vi

VI callers:

- WindowTelemetryOnlyEUI.lvlib:WindowTelemetryInitialize.vi
- ExitHMISystems.vi
- InitHMISystems.vi
