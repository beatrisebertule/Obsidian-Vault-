|   |   |   |
|---|---|---|
|**Question 1**||1 / 1 point|

When you forge Msg3, which fields besides r matter to the supplicant?

Question options:

|   |   |
|---|---|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.5.24373 "Unselected")|anonce, mic, and enc_gtk must all be correct|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.5.24373 "Unselected")|Only mic|
|![Selected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioChecked.svg?v=20.26.5.24373 "Selected")|None|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.5.24373 "Unselected")|The forged Msg3 must be encrypted|

|   |   |   |
|---|---|---|
|**Question 2**||1 / 1 point|

Where do you obtain the mic value needed to reconstruct that Msg4 plaintext?

Question options:

|   |   |
|---|---|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.5.24373 "Unselected")|Brute-force it|
|![Selected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioChecked.svg?v=20.26.5.24373 "Selected")|From the saved plaintext handshake Msg4|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.5.24373 "Unselected")|From the encrypted Msg4 itself|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.5.24373 "Unselected")|It's printed by the access point|

|   |   |   |
|---|---|---|
|**Question 3**||0 / 1 point|

What does the supplicant's nonce counter _r_ become immediately after it processes the replayed Msg3?

Question options:

|   |   |
|---|---|
|![Selected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioChecked.svg?v=20.26.5.24373 "Selected")|It keeps incrementing as normal|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.5.24373 "Unselected")|It is set to 0|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.5.24373 "Unselected")|It is set to a random number|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.5.24373 "Unselected")|It is reset back to the originally agreed nonce|
correct - back to orggina score

|   |   |   |
|---|---|---|
|**Question 4**||1 / 1 point|

You recover the keystream by XORing the intercepted (encrypted) Msg4 against the saved handshake Msg4 bytes unchanged. The secret then decrypts cleanly at the start and end but is corrupted around the middle. What is the cause?

Question options:

|                                                                                                                            |                                                             |
| -------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------- |
| ![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.5.24373 "Unselected") | The supplicant randomly skips a data packet.                |
| ![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.5.24373 "Unselected") | The access point injects an extra packet onto the uplink.   |
| ![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.5.24373 "Unselected") | A JSON length miscalculation shifts the count.              |
| ![Selected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioChecked.svg?v=20.26.5.24373 "Selected")       | The replay counter difference between the two Msg4 messages |