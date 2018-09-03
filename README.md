"# RTOS-based-pretend-Robot-Control-System" 

IDE USED: IAR Embedded Workbench,

The objective if the project was to implement a pretend robot control system on an STM32F107 microcontroller development board.
There are three major components in this project: Robot Control Center, Robot Manager, Robots(upto 13 robots).

All elements of the system are connected together in a network. They communicate over RF links. Each component on the network has a unique 8-bit address (0-255). Note that a destination address of zero means a broadcast message, which is received by both the Control Center and the Robot Manager.

The job of the control center is to accept requests from a keyboard and forward appropriate commands to the Robot Manager. The robot manager must interpret these commands and, in turn, pass commands to robots as required.

The floor on which robots travel is organized as a rectangular grid. Each cell in the grid may be occupied by only one robot at a time.
Robots have limited capabilities. A robot can only move one step at a time to an adjacent cell. There are 8 possible directions in which a robot may step. If, however a robot occupied a cell adjacent to a wall(or to another robot), a step in that direction must not be given or a collision will result. It is the responsibility of the Robot Manager not to issue dangerous commands to a robot.

"Network" communication consists of command and data packets, which are transferred via the USART2 serial port. The general packet format is as follows:
* The first three bytes of the packet constitute the packet preamble constituting of 3 "sync bytes." The preamble enables synchronization   with the start of a new packet. In case of a bad packet, you find the beginning of the next packet by searching forward in the input
  stream until encountering the correct preamble.
* Byte 3 gives the source address
* Byte 4 gives the address of the destination.
* Byte 5 gives teh packet length including the preamble, and the checksum.
* This is followed by a sequence of one or more data/command bytes.
* The last byte in the packet is a checksum byte, which is the XOR or all other bytes in the packet. Thus, XOR of the entire packet
  including the checksum, is zero.
