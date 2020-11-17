# Introduction

This document explains the structure for the Human Machine Interface (HMI)
software located in the HMIComputers repository,
[here](https://gitlab.tekniker.es/aut/projects/3151-LSST/LabVIEWCode/HMIComputers).
The purpose of this documentation is to explain the architecture design of the
HMI system. To do so, the main components of the HMI code are explained. The
develop tools and tips are also included as part of the documentation.

Important notes:

- In the documentation there could be some misunderstandings due to the naming of the hardware devices and software. Mainly when using the abbreviations EUI and HHD.
  - EUI stands for Engineering User Interface, but sometimes it is used to
    refer to the PC that runs the EUI itself. This PC is named MCC (Main
    Control Computer).
  - HHD stands for Handheld Device, but the EUI that runs on the HHD it is
    also called HHD.

- HMI stands for Human Machine Interface, but sometimes it is referred as EUI.
