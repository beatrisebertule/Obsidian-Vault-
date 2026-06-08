|                |     |             |
| -------------- | --- | ----------- |
| **Question 1** |     | 0 / 1 point |

What is the version of the TLS Record layer?

Question options:

|   |   |
|---|---|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.3.24682 "Unselected")|TLS 1.0|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.3.24682 "Unselected")|TLS 1.1|
|![Selected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioChecked.svg?v=20.26.3.24682 "Selected")|TLS 1.2|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.3.24682 "Unselected")|TLS 1.3|

|   |   |
|---|---|
||   |
|||
|## Feedback<br><br>No, the record layer is different than the handshake layer|   |

|   |   |   |
|---|---|---|
|**Question 2**||1 / 1 point|

What is the version of the TLS Handshake Protocol?

Question options:

|   |   |
|---|---|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.3.24682 "Unselected")|TLS 1.0|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.3.24682 "Unselected")|TLS 1.1|
|![Selected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioChecked.svg?v=20.26.3.24682 "Selected")|TLS 1.2|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.3.24682 "Unselected")|TLS 1.3|

|   |   |
|---|---|
||   |
|||
|## Feedback<br><br>Correct, for compatibility reasons, TLS 1.2 was kept here|   |

|   |   |   |
|---|---|---|
|**Question 3**||1 / 1 point|

Why does Wireshark then list this connection as TLS v1.3?

Question options:

|   |   |
|---|---|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.3.24682 "Unselected")|Because TLS 1.2 and 1.3 are indistinguishable|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.3.24682 "Unselected")|Because the ClientHello lists TLS 1.3 in the supported_version extension|
|![Selected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioChecked.svg?v=20.26.3.24682 "Selected")|Because the ServerHello lists TLS 1.3 in the supported_version extension|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.3.24682 "Unselected")|Because the ciphersuite selected is only supported in TLS 1.3|

|   |   |
|---|---|
||   |
|||
|## Feedback<br><br>Correct, the server has to show it agrees to this version, then it means the rest is TLS 1.3|   |

|   |   |   |
|---|---|---|
|**Question 4**||1 / 1 point|

How many ciphersuites are listed in the ClientHello?

Question options:

|   |   |
|---|---|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.3.24682 "Unselected")|1-5|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.3.24682 "Unselected")|5-10|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.3.24682 "Unselected")|10-15|
|![Selected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioChecked.svg?v=20.26.3.24682 "Selected")|16+|

|   |   |   |
|---|---|---|
|**Question 5**||1 / 1 point|

How many Signature Hash Algorithms are listed in the ClientHello?

Question options:

|   |   |
|---|---|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.3.24682 "Unselected")|1-5|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.3.24682 "Unselected")|5-10|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.3.24682 "Unselected")|10-15|
|![Selected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioChecked.svg?v=20.26.3.24682 "Selected")|16+|

|   |   |   |
|---|---|---|
|**Question 6**||0 / 1 point|

How many Supported Groups are listed in the ClientHello?

Question options:

|   |   |
|---|---|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.3.24682 "Unselected")|1-5|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.3.24682 "Unselected")|5-10|
|![Selected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioChecked.svg?v=20.26.3.24682 "Selected")|10-15|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.3.24682 "Unselected")|16+|

|   |   |   |
|---|---|---|
|**Question 7**||1 / 1 point|

Do you see the server certificate?

Question options:

|   |   |
|---|---|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.3.24682 "Unselected")|Yes, it is in the Certificate message|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.3.24682 "Unselected")|Yes, it is in the ClientHello message|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.3.24682 "Unselected")|No, there is no Certificate message in this handshake|
|![Selected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioChecked.svg?v=20.26.3.24682 "Selected")|No, the Certificate message is encrypted and shows as Application Data|

|   |   |   |
|---|---|---|
|**Question 8**||0 / 1 point|

In TLS 1.2, is the negotiated ciphersuite the same as in TLS 1.3 in your tests?

Question options:

|   |   |
|---|---|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.3.24682 "Unselected")|Yes, because this is the same client and server|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.3.24682 "Unselected")|Yes, because the list of supported ciphersuite is the same|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.3.24682 "Unselected")|No, because the list of supported ciphersuite is not the same|
|![Selected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioChecked.svg?v=20.26.3.24682 "Selected")|No, because the selected ciphersuite was specific to TLS 1.3|

|   |   |   |
|---|---|---|
|**Question 9**||0 / 1 point|

How many certificates are sent by the server?

Question options:

|   |   |
|---|---|
|![Selected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioChecked.svg?v=20.26.3.24682 "Selected")|One, this is the server certificate|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.3.24682 "Unselected")|Two, the server certificate and the root certificate|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.3.24682 "Unselected")|Three, including the server certificate and intermediate certificates|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.3.24682 "Unselected")|Four, server certificate, intermediates and root|

|   |   |   |
|---|---|---|
|**Question 10**||0 / 1 point|

Which TLS 1.3 extensions are decrypted thanks to the SSLKEYLOGFILE that you couldn't see before?

Question options:

|   |   |
|---|---|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.3.24682 "Unselected")|Encrypted Extensions|
|![Selected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioChecked.svg?v=20.26.3.24682 "Selected")|Certificate|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.3.24682 "Unselected")|Certificate Verify|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.3.24682 "Unselected")|All of the above|

|   |   |   |
|---|---|---|
|**Question 11**||1 / 1 point|

Which of the following key/secret is not part of your SSLKEYLOGFILE in Exercise 2?

Question options:

|   |   |
|---|---|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.3.24682 "Unselected")|EXPORTER_SECRET|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.3.24682 "Unselected")|CLIENT_TRAFFIC_SECRET_0|
|![Selected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioChecked.svg?v=20.26.3.24682 "Selected")|MASTER_SECRET|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.3.24682 "Unselected")|CLIENT_HANDSHAKE_TRAFFIC_SECRET|

|   |   |   |
|---|---|---|
|**Question 12**||1 / 1 point|

What is Google Trust Services WR2 certificate authority certificate SKI?

Question options:

|   |   |
|---|---|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.3.24682 "Unselected")|E4:AF:2B:26:71:1A:2B:48:27:85:2F:52:66:2C:EF:F0:89:13:71:3E|
|![Selected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioChecked.svg?v=20.26.3.24682 "Selected")|DE:1B:1E:ED:79:15:D4:3E:37:24:C3:21:BB:EC:34:39:6D:42:B2:30|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.3.24682 "Unselected")|6F:1D:35:49:10:6C:32:FA:59:A0:9E:BC:8A:E8:1F:95:BE:71:7A:0C|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.3.24682 "Unselected")|A6:73:09:27:C3:21:55:17:BB:E7:7C:38:5D:ED:05:51:25:00:54:B6|

|   |   |   |
|---|---|---|
|**Question 13**||1 / 1 point|

The leaf certificate on google.com also valid for admob-cn.com

Question options:

|                                                                                                                            |       |
| -------------------------------------------------------------------------------------------------------------------------- | ----- |
| ![Selected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioChecked.svg?v=20.26.3.24682 "Selected")       | True  |
| ![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.3.24682 "Unselected") | False |