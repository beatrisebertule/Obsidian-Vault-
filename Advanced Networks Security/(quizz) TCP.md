|                            |     |             |
| -------------------------- | --- | ----------- |
| **Question 1** (Mandatory) |     | 1 / 1 point |

When making an HTTP request and analyzing the traffic in Wireshark, which display filter should be used to isolate only the initial "SYN" packets that begin the TCP connection process?

Question options:

|                                                                                                                            |                       |
| -------------------------------------------------------------------------------------------------------------------------- | --------------------- |
| ![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.2.23096 "Unselected") | tcp.analysis.flags    |
| ![Selected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioChecked.svg?v=20.26.2.23096 "Selected")       | tcp.flags.syn == True |
| ![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.2.23096 "Unselected") | ip.src == 127.0.0.1   |
| ![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.2.23096 "Unselected") | tcp.port == 80        |

|   |   |   |
|---|---|---|
|**Question 2** (Mandatory)||1 / 1 point|

When observing sequence numbers in Wireshark, what feature does the tool provide to make the analysis easier for humans?

Question options:

|                                                                                                                            |                                                                |
| -------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------- |
| ![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.2.23096 "Unselected") | It hides sequence numbers to simplify the UI.                  |
| ![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.2.23096 "Unselected") | It automatically converts all sequence numbers to hexadecimal. |
| ![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.2.23096 "Unselected") | It encrypts the sequence numbers for security.                 |
| ![Selected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioChecked.svg?v=20.26.2.23096 "Selected")       | It provides both relative numbers and next expected numbers.   |

|   |   |   |
|---|---|---|
|**Question 3** (Mandatory)||1 / 1 point|

When running a script that makes repeated HTTP connections, what behavior is typically observed regarding the source ports?

Question options:

|   |   |
|---|---|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.2.23096 "Unselected")|The source port remains identical for every single request.|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.2.23096 "Unselected")|The source port is determined by the target domain's IP address.|
|![Selected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioChecked.svg?v=20.26.2.23096 "Selected")|The OS assigns a new, usually incrementing or random ephemeral port for each connection.|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.2.23096 "Unselected")|The source port is always set to 80.|

|   |   |   |
|---|---|---|
|**Question 4** (Mandatory)||1 / 1 point|

During the TCP three-way handshake, if the client sends a SYN packet with an Initial Sequence Number (ISN) of **X**, what is the expected Acknowledgment Number in the server's SYN/ACK response?

Question options:

|                                                                                                                            |                                                               |
| -------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------- |
| ![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.2.23096 "Unselected") | **X** + 65535 (The number is incremented by the window size). |
| ![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.2.23096 "Unselected") | **X** (The number remains the same until data is sent).       |
| ![Selected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioChecked.svg?v=20.26.2.23096 "Selected")       | **X + 1** (The SYN flag consumes one sequence number) C.      |
| ![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.2.23096 "Unselected") | 0 (The sequence starts over at the server).                   |

|   |   |   |
|---|---|---|
|**Question 5** (Mandatory)||1 / 1 point|

What are the "ephemeral source ports" used in establishing TCP connection?

Question options:

|   |   |
|---|---|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.2.23096 "Unselected")|Ports used exclusively for Wireshark's internal communication.|
|![Selected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioChecked.svg?v=20.26.2.23096 "Selected")|Temporary ports assigned by the OS to the client for the duration of a connection.|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.2.23096 "Unselected")|Fixed ports used by the server to receive requests (e.g., Port 80).|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.2.23096 "Unselected")|High-security ports that cannot be tracked by packet sniffers.|

|                            |     |             |
| -------------------------- | --- | ----------- |
| **Question 6** (Mandatory) |     | 1 / 1 point |

When crafting a manual TCP SYN packet in Scapy, what is the significance of setting flags="S"?

Question options:

|                                                                                                                            |                                                                    |
| -------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------ |
| ![Selected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioChecked.svg?v=20.26.2.23096 "Selected")       | It indicates the "Synchronize" flag, used to initiate a handshake. |
| ![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.2.23096 "Unselected") | It resets the sequence numbers to zero.                            |
| ![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.2.23096 "Unselected") | It stands for "Secure" and enables TLS encryption.                 |
| ![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.2.23096 "Unselected") | It tells the server to stop the connection.                        |

|   |   |   |
|---|---|---|
|**Question 7** (Mandatory)||1 / 1 point|

Which Scapy function should be used if you want to send a packet and receive only a single response packet?

Question options:

|   |   |
|---|---|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.2.23096 "Unselected")|sr()|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.2.23096 "Unselected")|sendp()|
|![Selected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioChecked.svg?v=20.26.2.23096 "Selected")|sr1()|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.2.23096 "Unselected")|send()|

|   |   |   |
|---|---|---|
|**Question 8** (Mandatory)||1 / 1 point|

What is the purpose of sending a packet with the RST flag after injecting a request?

Question options:

|                                                                                                                            |                                                                         |
| -------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| ![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.2.23096 "Unselected") | To change the source port of the current connection.                    |
| ![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.2.23096 "Unselected") | To signal the server to send more data.                                 |
| ![Selected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioChecked.svg?v=20.26.2.23096 "Selected")       | To stop the server from retransmitting data after the injected request. |
| ![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.2.23096 "Unselected") | To clear the Wireshark display filter.                                  |

|   |   |   |
|---|---|---|
|**Question 9** (Mandatory)||1 / 1 point|

When performing a packet injection to send an HTTP GET request over an already established TCP connection, which TCP flag must be set to ensure the packet is accepted by the server as part of the ongoing session?

Question options:

|   |   |
|---|---|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.2.23096 "Unselected")|FIN (Finish)|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.2.23096 "Unselected")|SYN (Synchronize)|
|![Selected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioChecked.svg?v=20.26.2.23096 "Selected")|ACK (Acknowledgment)|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.2.23096 "Unselected")|RST (Reset)|

|   |   |   |
|---|---|---|
|**Question 10** (Mandatory)||1 / 1 point|

When manually crafting a packet to inject data into an existing TCP stream (as seen in the Exercise 2 Scapy tasks), you must provide a seq (Sequence Number). If the last packet received by the server from the legitimate client had a seq of **1000** and a payload size of **140** bytes, what is the exact seq value your spoofed packet must use to be accepted as the "next" segment in the stream?

Question options:

|   |   |
|---|---|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.2.23096 "Unselected")|1141|
|![Selected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioChecked.svg?v=20.26.2.23096 "Selected")|1140|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.2.23096 "Unselected")|1000|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.2.23096 "Unselected")|Any value between 1024 and 65535, as long as the ACK flag is set.|

all correct
10/10