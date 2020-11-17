# Commands and Events Management

This section contains the different components related to the commands and
events from the TMA that are:

- TCP Client: this component is the one that connects to the TMA over TCP to
  send and receive the TCP messages. The message to send is specified to the
  task by a public method of the TCP client object, and the received messages
  are published in a user event created when the object is initialized.

- TMA Commanding: this component is the one sending commands to the TMA and
  monitoring the events received from it. This is done using the TCP Client
  component.

- Alarm Management: this component gets the alarm and warning events from the
  TCP client, logging the received alarms and warnings to have a record of the
  different events occurred while operating the system.
