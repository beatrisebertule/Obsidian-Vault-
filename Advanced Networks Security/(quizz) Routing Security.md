## **Question 1** (1 point)

 |   |   |   |
|---|---|---|
|**Question 1**||1 / 1 point|

![[Pasted image 20260322174102.png]]

Ex1-Q1: In the ROV++ paper, Figure 1a (no ROV), why is AS 44 not receiving the malicious subprefix announcement originated by AS 666?

Question options:

|   |   |
|---|---|
|![Selected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioChecked.svg?v=20.26.2.26192 "Selected")|AS 88 does not forward this announcement because of the valley-free principle|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.2.26192 "Unselected")|AS 88 recognizes that this hijacked prefix is not allowed and discards it|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.2.26192 "Unselected")|BGP delays announcements and AS 44 has not yet received it|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.2.26192 "Unselected")|AS 88 does not need to forward this announcement further since it makes no difference to AS 44|

|   |   |   |
|---|---|---|
|**Question 2**||1 / 1 point|

Ex1-Q1: In Figure 2a, why traffic from AS 12 destinated to 1.2.3.4 still reaches the malicious AS 666 despite AS 78 being ROV-compatible and discarding the hijacked announcement?

Question options:

|   |   |
|---|---|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.2.26192 "Unselected")|AS 78 is unable to verify the malicious route announcement because it has been validated and signed by the intermediate AS 44|
|![Selected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioChecked.svg?v=20.26.2.26192 "Selected")|AS 78 still sends traffic to AS 44 because of the legitimate announcement for 1.2/16 with the shortest path, but once it reaches AS 44, a non-ROV compatible router, it gets routed according to the hijacked subprefix route|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.2.26192 "Unselected")|AS 78 receives announcements for 1.2/16 from both AS 44 and AS 88, but AS 44 also advertises 1.2.3/24, which is a more specific prefix, and therefore prefers this route|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.2.26192 "Unselected")|AS 78 has a software bug in the routing software and does not implement ROV correctly; it should normally send the traffic to AS 88|

|   |   |   |
|---|---|---|
|**Question 3**||1 / 1 point|

![[Pasted image 20260322174129.png]]

Ex1-Q1: Why is it interesting to attempt hijacking a superprefix of a non-routed prefix that is protected by ROA?

Question options:

|   |   |
|---|---|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.2.26192 "Unselected")|Because ROAs for non-routed prefixes use AS 0, which automatically invalidates any superprefix announcements in standard ROV|
|![Selected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioChecked.svg?v=20.26.2.26192 "Selected")|Because non-routed prefixes (with AS 0 ROAs) lack legitimate BGP announcements, superprefix hijacks always succeed in redirecting traffic, perfect for repurposing in malicious activities like spam or DDoS|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.2.26192 "Unselected")|Because it requires a ROA on the superprefix, forcing attackers to forge authorizations and test RPKI security limits|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.2.26192 "Unselected")|Because superprefixes are always larger address spaces, allowing attackers to control more IP addresses than a direct hijack|

|   |   |   |
|---|---|---|
|**Question 4**||1 / 1 point|

Ex1-Q2: What's the key idea of ROV++ v1?

Question options:

|   |   |
|---|---|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.2.26192 "Unselected")|Validating the entire AS path in BGP announcements to prevent all types of hijacks|
|![Selected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioChecked.svg?v=20.26.2.26192 "Selected")|Preferring secure paths that avoid neighbor ASes sending hijacked subprefix announcements, and blackholing traffic to subprefixes known to be hijacked|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.2.26192 "Unselected")|Encrypting BGP control-plane messages to ensure confidentiality during route propagation|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.2.26192 "Unselected")|Using machine learning algorithms to predict and block potential BGP hijack attempts in real-time|

|   |   |   |
|---|---|---|
|**Question 5**||1 / 1 point|

EX1-Q3: In Figure 3a, AS 32 receives the hijacked subprefix announcement. How is traffic from AS 32 to 1.2.3.4 protected from reaching the attacker?

Question options:

|   |   |
|---|---|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.2.26192 "Unselected")|AS 32 is told by the ROV++ v2 AS 77 to ignore the path going to AS 77, which is bound to forward traffic to the attacker|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.2.26192 "Unselected")|AS 32, upon receiving the announcement with the blackhole attribute, blackholes the traffic locally even though it is not ROV++ compatible, as the attribute is transitive and triggers dropping in standard BGP routers|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.2.26192 "Unselected")|AS 32 is a ROV-capable AS, therefore it detects the prefix hijack and does not select the advertised path|
|![Selected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioChecked.svg?v=20.26.2.26192 "Selected")|AS 32 receives the prefix hijack announcement but cannot detect the blackhole attribute, so it sends the traffic anyway to AS 11 then to AS 77 due to being the shortest available path, which in turn blackholes the traffic due to no alternative routes available|

|     |
| --- |
|     |