# Global Variables

## GBL_AmbientTemperature.vi

Global variable for storing the ambient temperature value globally.

Path: HMIComputers\\AmbientTemperatureUpdate\\GBL_AmbientTemperature.vi

VI callers (reader):

- Main Cabinet_EUI.vi

VI callers (writer):

- HMIMain_EUI.vi

## AutomaticTestingReceiver.lvclass:GBL_ATSmodeOn.vi

Global variable used for globally having the information if the ATS is on or not.

Path: HMIComputers\\AutomaticTesting\\AutomaticTestingReceiver_class\\GBL_ATSmodeOn.vi

VI callers (reader):

- CallOneBottondinamically.vi
- CargaVentanaEnSubpanel.vi
- checkTimeToSendForceTerminateSequence.vi

VI callers (writer):

- AutomaticTestingReceiver.lvclass:ParseReceivedData.vi
- HMIMain_EUI.vi

## GBL_Commander.vi

Global variable used for globally having the actual commander.

Path: HMIComputers\\HMI\\GeneralHMIControls\\GBL_Commander.vi

VI callers (reader):

- AmbientTemperatureUpdate.lvclass:Task.vi
- Clock.lvclass:Task.vi
- Activatedeactivatecontrols.vi

VI callers (writer):

- EUI_Initialization.vi
- HHD_Initialization.vi
- HMIMain_EUI.vi
- HMIMain_HHD.vi

## GBL_CurrentDevice.vi

Global variable used for globally having the current device where the application is running, EUI or HHD.

Path: HMIComputers\\HMI\\GeneralHMIControls\\GBL_CurrentDevice.vi

VI callers (reader):

- GetTelemetryForWindow.vi
- TelemetryLogingTask.lvclass:AddVariableMethodChoice.vi
- TelemetryFilesManagement.lvlib:Task.vi
- HHDTelemetri MonitoringManage.vi
- Clock.lvclass:Task.vi
- HHDorEUIFolderChoice.vi
- DisableWindowsHHD.vi
- GetObjectForCurrentDevice.vi
- HMITimeoutChoice.vi
- Activatedeactivatecontrols.vi
- WindowTelemetryOnlyEUI.lvlib:WindowTelemetryCleanUpOnlyEUI.vi
- WindowTelemetryOnlyEUI.lvlib:WindowTelemetryStopSaveOnlyEUI.vi
- WindowTelemetryOnlyEUI.lvlib:WindowTelemetryStartSaveOnlyEUI.vi
- WindowTelemetryOnlyEUI.lvlib:WindowTelemetryReadTDMSFileOnlyEUI.vi
- WindowTelemetryOnlyEUI.lvlib:WindowTelemetrySendDataOnlyEUI.vi
- XYGraphTypes.lvlib:SetGraphIterations.vi
- XYGraphTypes.lvlib:GetEventForTelemetry.vi
- WindowTelemetryOnlyEUI.lvlib:WindowTelemetryInitialize.vi
- LaunchHHDKeyboard.vi
- Change Operation Mode_EUI.vi
- Log In_EUI.vi
- Main Axis General View_EUI.vi
- MainHMIAlarmIndicatorManagement.vi
- LoadKeyboardIfNeededSpeed_rad_s.vi
- XYGraphTypes.lvlib:TimeHHDGraphUpdate.vi
- EUI_Exit.vi
- HHD_Exit.vi
- HMIMain_EUI.vi
- HMIMain_HHD.vi

VI callers (writer):

- EUI_Initialization.vi
- HHD_Initialization.vi

## GBL_SystemSTO.vi

Global variable used for globally having the information of the STO from the loaded window, this is then displayed in the main window in the safety section in the top left part of the window.

Path: HMIComputers\\HMI\\GeneralHMISubVIs\\GBL_SystemSTO.vi

VI callers (reader):

- HHDSafetyData.vi
- HMIMain_EUI.vi

VI callers (writer):

- PublishSystemSTO.vi

## InterlocksLibrary.lvlib:GBL_ActiveInterlocks.vi

Global variable used for globally having the interlock information of the active window, to manage the interlock display in the windows that have it.

Path: HMIComputers\\HMI\\GeneralHMISubVIs\\GBL_ActiveInterlocks.vi

VI callers (reader):

- InterlocksLibrary.lvlib:InterlockSwitchManageIndex.vi

VI callers (writer):

- InterlocksLibrary.lvlib:TelemetryEmptyInterlockArrayCheckStatusLed.vi

## CCWSequences.lvlib:GBL_TrackingData.vi

Global variable used for globally having the information used for CCW tracking commands when done from the EUI/HHD, this option is just for simple testing.

Path: HMIComputers\\HMI\\Subsystem\\Camera Cable Wrap\\SecuencesSubVIs\\GBL_TrackingData.vi

VI callers (reader):

- CCWSendTrack.vi
- CCWInitTrack.vi
- PopUpCCWLoadTrack_EUI.vi

VI callers (writer):

- CCWSequences.lvlib:ReadTrackingFile.vi
- CCWSequences.lvlib:EmptyGBLData.vi

## GBL_UserGourps.vi

Global variable used for globally having the information of the current users group, to enable or disable certain features within the application.

Path: HMIComputers\\HMI\\Subsystem\\Log In\\GBL_UserGourps.vi

VI callers (reader):

- Activatedeactivatecontrols.vi
- ShowJog.vi
- ShowInvisibleSettings.vi
- Settings Modifications_EUI.vi
- Settings Set Management_EUI.vi

VI callers (writer):

- EUI_Initialization.vi
- CheckAccessScope.vi
- HHD_Initialization.vi

## GBL_PopUpGreyStop.vi

Global variable used for stopping the grey panel loaded when a pop up window is shown.

Path: HMIComputers\\HMI\\Subsystem\\MainAxis\\PopUp\\GBL_PopUpGreyStop.vi

VI callers (reader):

- PanelGris.vi

VI callers (writer):

- PopUpCCWLoadTrack_EUI.vi
- PopUpMainAxisLoadTrack_EUI.vi
- PopUpLoadBalancingPosition_EUI.vi
- PopUpLoadSettingSetChangedSettings_EUI.vi

## MainAxesSequences.lvlib:GBL_TrackingData.vi

Global variable used for globally having the information used for main axes tracking commands when done from the EUI/HHD, this option is just for simple testing.

Path: HMIComputers\\HMI\\Subsystem\\MainAxis\\SecuencesSubVIs\\GBL_TrackingData.vi

VI callers (reader):

- Main Axis General View_EUI.vi
- PopUpMainAxisLoadTrack_EUI.vi
- MainAxesInitTrackActions.vi

VI callers (writer):

- MainAxesSequences.lvlib:ReadTrackingFile.vi
