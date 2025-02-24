# Universal Asynchronous Receiver Transmitter
Universal Asynchronous Receiver Transmitter, or UART, is a fundamental communication protocol used to send and receive serial data between two devices.

It was one of the earliest communication protocols and is still relevant due to its simplicity.

Each device using UART requires three wires:
1) **Receive (RX)**: Used to receive data on that device.
2) **Transmit (TX)**: Used to transmit data from that device.
3) **Ground (GND)**: Used to provide a common reference.

**The TX line of one device must be connected to the RX line of the other device**, and vice versa. In almost all scenarios, the two devices must share a common ground to ensure proper voltage referencing.

![UART Connection](https://www.vectornav.com/inertialprimer/support-library/comh_asc.jpg)

In their idle states, the RX and TX lines are held high. This ensures easy detection of whether the device is connected, as the line will remain in a high state if the connection is intact.

Due to UART's lack of a synchronous clock, **the following features must be predetermined** and agreed upon for communication to work:
- **Baud rate:** The number of bits transmitted per second. Common baud rates include 9600, 19200, 38400, 57600, 115200.
- **Data frame length**: The number of data bits in each frame.
- **Parity check**: An optional boolean check to verify the data in the data packet. Determines whether it is enabled and which mode is used (even or odd).
	- Even parity check: Ensures the total number of 1s in the data and parity bit is even.
	- Odd parity check: Ensures the total number of 1s in the data and parity bit is odd.
	- Note that a parity check cannot properly detect errors if more than one bit is flipped.
- **Stop bits**: The number of stop bits used, with at least one required.

## UART Frames
A UART message has the following structure:
- **Start bit**: Pulls the data line low to signal incoming data.
- **Data bits**: The actual user data, typically sent starting with the least significant bit (LSB).
- **Parity bit** (optional): Used to check for data errors.
- **Stop bit(s)**: Pulls the data line high to signal the end of transmission.

![UART Frame](https://docs.arduino.cc/static/ff1d2c3971a36f4dea095a4d44fe3ce0/a6d36/message.png)

