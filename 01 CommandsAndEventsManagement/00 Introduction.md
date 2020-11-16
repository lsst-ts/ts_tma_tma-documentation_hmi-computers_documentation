# Introduction

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

[3. TCP Client 7](#tcp-client)

[3.1 Component Configuration 7](#component-configuration)

[3.2 Sender.lvclass 7](#sender.lvclass)

[3.2.1 Configuration file explained 7](#configuration-file-explained)

[3.2.2 Task process 8](#task-process)

[3.2.3 Task methods 8](#task-methods)

[3.2.4 CMD Reception loop 10](#cmd-reception-loop)

[4. TMA Commanding 23](#_Toc56073553)

[4.1 CppAppCommand.lvclass 24](#_Toc56073554)

[4.1.1 Methods 24](#_Toc56073555)

[4.1.2 Childs 27](#_Toc56073556)

[4.2 GetEventfromTMA.lvclass 32](#_Toc56073557)

[4.2.1 Task process 32](#_Toc56073558)

[4.2.2 Task methods 33](#_Toc56073559)

[4.2.3 CMD Reception loop 34](#_Toc56073560)

[4.3 TMAOMTMonitoring task 41](#_Toc56073561)

[4.3.1 Task process 41](#_Toc56073562)

[4.3.2 Task methods 42](#_Toc56073563)

[4.3.3 Main loop 44](#_Toc56073564)

[4.4 CommanderCheck task 59](#_Toc56073565)

[4.4.1 Task process 59](#_Toc56073566)

[4.4.2 Task methods 60](#_Toc56073567)

[4.4.3 Main loop 62](#_Toc56073568)

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
