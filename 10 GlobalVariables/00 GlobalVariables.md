# Global Variables

## GBL_AmbientTemperature.vi

Global variable for storing the ambient temperature value globally.

Path: HMIComputers\AmbientTemperatureUpdate\GBL_AmbientTemperature.vi

VI callers:

- Main Cabinet_EUI.vi
- HMIMain_EUI.vi

## AutomaticTestingReceiver.lvclass:GBL_ATSmodeOn.vi

Global variable used for globally having the information if the ATS is on or not.

Path: HMIComputers\AutomaticTesting\AutomaticTestingReceiver_class\GBL_ATSmodeOn.vi

VI callers:

- CallOneBottondinamically.vi
- AutomaticTestingReceiver.lvclass:ParseReceivedData.vi
- checkTimeToSendForceTerminateSequence.vi
- ATF_ConsumerLoopError.vi
- Axes Transfer Function_EUI.vi
- ACWConsumerLoopError.vi
- Azimuth Cable Wrap_EUI.vi
- AzimuthDrivesThermalConsumerLoopError.vi
- Azimuth Drives Thermal_EUI.vi
- AzimuthDrivesConsumerLoopError.vi
- Azimuth Drives_EUI.vi
- AzimuthConsumerLoopError.vi
- Azimuth General View_EUI.vi
- BalancingConsumerLoopError.vi
- Balancing General View_EUI.vi
- CCWConsumerLoopError.vi
- Camera Cable Wrap_EUI.vi
- Change Operation Mode_EUI.vi
- ChangeOperationModeConsumerLoopError.vi
- DPConsumerLoopError.vi
- Deployable Platform_EUI.vi
- ElevationDrivesThermalConsumerLoopError.vi
- Elevation Drives Thermal_EUI.vi
- ElevationDrivesConsumerLoopError.vi
- Elevation Drives_EUI.vi
- ElevationConsumerLoopError.vi
- Elevation General View_EUI.vi
- GeneralCommandsConsumerLoopError.vi
- General Commands_EUI.vi
- LPConsumerLoopError.vi
- Locking Pins_EUI.vi
- MainAxesConsumerLoopError.vi
- ElectricalCabinetConsumerLoopError.vi
- Main Cabinet_EUI.vi
- CargaVentanaEnSubpanel.vi
- MCConsumerLoopError.vi
- Mirror Cover General View_EUI.vi
- MCLConsumerLoopError.vi
- Mirror Cover Locks_EUI.vi
- OSS General View_EUI.vi
- OSSConsumerLoopError.vi
- MPSConsumerLoopError.vi
- Power Supply_EUI.vi
- SafetyConsumerLoopError.vi
- Safety System_EUI.vi
- Top End Chiller Cabinets_EUI.vi
- TECConsumerLoopError.vi
- Top End Chiller General View_EUI.vi
- HMIMain_EUI.vi

## GBL_Commander.vi

Global variable used for globally having the actual commander.

Path: HMIComputers\HMI\GeneralHMIControls\GBL_Commander.vi

VI callers:

- AmbientTemperatureUpdate.lvclass:Task.vi
- Clock.lvclass:Task.vi
- Activatedeactivatecontrols.vi
- EUI_Initialization.vi
- HHD_Initialization.vi
- HMIMain_EUI.vi
- HMIMain_HHD.vi

## GBL_CurrentDevice.vi

Global variable used for globally having the current device where the application is running, EUI or HHD.

Path: HMIComputers\HMI\GeneralHMIControls\GBL_CurrentDevice.vi

VI callers:

- GetTelemetryForWindow.vi
- TelemetryLogingTask.lvclass:AddVariableMethodChoice.vi
- InitTelemetryLoggingTask.vi
- TelemetryFilesManagement.lvlib:Task.vi
- HHDTelemetri MonitoringManage.vi
- Clock.lvclass:Task.vi
- Alarm History_EUI.vi
- GetObjectForCurrentDevice.vi
- HMITimeoutChoice.vi
- All Data View_EUI.vi
- Auxiliary Boxes_EUI.vi
- Activatedeactivatecontrols.vi
- Axes Transfer Function Results_EUI.vi
- WindowTelemetryOnlyEUI.lvlib:WindowTelemetryCleanUpOnlyEUI.vi
- WindowTelemetryOnlyEUI.lvlib:WindowTelemetryStopSaveOnlyEUI.vi
- WindowTelemetryOnlyEUI.lvlib:WindowTelemetryStartSaveOnlyEUI.vi
- WindowTelemetryOnlyEUI.lvlib:WindowTelemetryReadTDMSFileOnlyEUI.vi
- XYGraphTypes.lvlib:DBLArray_SendData.vi
- WindowTelemetryOnlyEUI.lvlib:WindowTelemetrySendDataOnlyEUI.vi
- XYGraphTypes.lvlib:SetGraphIterations.vi
- XYGraphTypes.lvlib:GetEventForTelemetry.vi
- WindowTelemetryOnlyEUI.lvlib:WindowTelemetryInitialize.vi
- Axes Transfer Function_EUI.vi
- XYGraphTypes.lvlib:DBL_SendData.vi
- LaunchHHDKeyboard.vi
- Azimuth Cable Wrap_EUI.vi
- Azimuth Drives Thermal_EUI.vi
- Azimuth Drives_EUI.vi
- Azimuth General View_EUI.vi
- Balancing General View_EUI.vi
- Cabinet 0101_EUI.vi
- Camera Cable Wrap_EUI.vi
- Capacitor Banks_EUI.vi
- CCWAux Safety System_EUI.vi
- Change Operation Mode_EUI.vi
- Deployable Platform_EUI.vi
- Dynalene Cooling Dist Syst 1_EUI.vi
- Dynalene Cooling Dist Syst 2_EUI.vi
- Elevation Drives Thermal_EUI.vi
- Elevation Drives_EUI.vi
- Elevation General View_EUI.vi
- Encoder_EUI.vi
- General Commands_EUI.vi
- General Purpose GW Dist Syst_EUI.vi
- General View_EUI.vi
- Locking Pins_EUI.vi
- Log In_EUI.vi
- Main Axis General View_EUI.vi
- Main Cabinet_EUI.vi
- HHDorEUIFolderChoice.vi
- DisableWindowsHHD.vi
- Mirror Cover General View_EUI.vi
- Mirror Cover Locks_EUI.vi
- Operation Manager_EUI.vi
- OSS Accumulators_EUI.vi
- OSS Azimuth Bearings 1_EUI.vi
- OSS Azimuth Bearings 2_EUI.vi
- OSS Elevation Bearings_EUI.vi
- OSS General View_EUI.vi
- OSS Main Pumps_EUI.vi
- OSS Oil Circulation_EUI.vi
- OSS Oil Cooling_EUI.vi
- Power Supply_EUI.vi
- Safety System_EUI.vi
- Setting History_EUI.vi
- Telemetry Reading_EUI.vi
- Thermal General View_EUI.vi
- Top End Chiller Cabinets_EUI.vi
- Top End Chiller General View_EUI.vi
- MainHMIAlarmIndicatorManagement.vi
- LoadKeyboardIfNeededSpeed_rad_s.vi
- XYGraphTypes.lvlib:TimeHHDGraphUpdate.vi
- EUI_Exit.vi
- EUI_Initialization.vi
- HHD_Exit.vi
- HHD_Initialization.vi
- HMIMain_EUI.vi
- HMIMain_HHD.vi

## GBL_SystemSTO.vi

Global variable used for globally having the information of the STO from the loaded window, this is then displayed in the main window in the safety section in the top left part of the window.

Path: HMIComputers\HMI\GeneralHMISubVIs\GBL_SystemSTO.vi

VI callers:

- PublishSystemSTO.vi
- HHDSafetyData.vi
- HMIMain_EUI.vi

## InterlocksLibrary.lvlib:GBL_ActiveInterlocks.vi

Global variable used for globally having the interlock information of the active window, to manage the interlock display in the windows that have it.

Path: HMIComputers\HMI\GeneralHMISubVIs\GBL_ActiveInterlocks.vi

VI callers:

- InterlocksLibrary.lvlib:InterlockSwitchManageIndex.vi
- InterlocksLibrary.lvlib:TelemetryEmptyInterlockArrayCheckStatusLed.vi

## CCWSequences.lvlib:GBL_TrackingData.vi

Global variable used for globally having the information used for CCW tracking commands when done from the EUI/HHD, this option is just for simple testing.

Path: HMIComputers\HMI\Subsystem\Camera Cable Wrap\SecuencesSubVIs\GBL_TrackingData.vi

VI callers:

- CCWSendTrack.vi
- CCWInitTrack.vi
- PopUpCCWLoadTrack_EUI.vi
- CCWSequences.lvlib:ReadTrackingFile.vi
- CCWSequences.lvlib:EmptyGBLData.vi

## GBL_UserGourps.vi

Global variable used for globally having the information of the current users group, to enable or disable certain features within the application.

Path: HMIComputers\HMI\Subsystem\Log In\GBL_UserGourps.vi

VI callers:

- Activatedeactivatecontrols.vi
- ShowJog.vi
- CheckAccessScope.vi
- ShowInvisibleSettings.vi
- Settings Modifications_EUI.vi
- Settings Set Management_EUI.vi
- EUI_Initialization.vi
- HHD_Initialization.vi

## GBL_PopUpGreyStop.vi

Global variable used for stopping the grey panel loaded when a pop up window is shown.

Path: HMIComputers\HMI\Subsystem\MainAxis\PopUp\GBL_PopUpGreyStop.vi

VI callers:

- PopUpCCWLoadTrack_EUI.vi
- PanelGris.vi
- PopUpMainAxisLoadTrack_EUI.vi
- PopUpLoadBalancingPosition_EUI.vi
- PopUpLoadSettingSetChangedSettings_EUI.vi

## MainAxesSequences.lvlib:GBL_TrackingData.vi

Global variable used for globally having the information used for main axes tracking commands when done from the EUI/HHD, this option is just for simple testing.

Path: HMIComputers\HMI\Subsystem\MainAxis\SecuencesSubVIs\GBL_TrackingData.vi

VI callers:

- Main Axis General View_EUI.vi
- PopUpMainAxisLoadTrack_EUI.vi
- MainAxesSequences.lvlib:ReadTrackingFile.vi
- MainAxesInitTrackActions.vi
