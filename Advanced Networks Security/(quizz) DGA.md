|                                              |     |             |
| -------------------------------------------- | --- | ----------- |
| **Question 1**Correct on previous attempt(s) |     | 1 / 1 point |

What does the provided _classifier.py_ effectively detect?

Question options:

|   |   |
|---|---|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.5.24373 "Unselected")|Any domain produced by an algorithm, regardless of appearance|
|![Selected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioChecked.svg?v=20.26.5.24373 "Selected")|Domains that look statistically random (high entropy, many digits, long vowel-less runs)|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.5.24373 "Unselected")|Domains that were registered within the last 24 hours|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.5.24373 "Unselected")|Domains longer than 15 characters|

|   |   |   |
|---|---|---|
|**Question 2**Retaken||1 / 1 point|

Which DGA type would most easily achieve a detection rate below 10% against the classifier you used in the exercise?

Question options:

|   |   |
|---|---|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.5.24373 "Unselected")|Arithmetic-based|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.5.24373 "Unselected")|Hash-based|
|![Selected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioChecked.svg?v=20.26.5.24373 "Selected")|Wordlist-based|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.5.24373 "Unselected")|It makes no difference; all types score the same|

|   |   |   |
|---|---|---|
|**Question 3**Correct on previous attempt(s)||1 / 1 point|

Your DGA produces `3f9a1c7e0b.com`. Which classifier feature is it MOST likely to trigger?

Question options:

|   |   |
|---|---|
|![Selected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioChecked.svg?v=20.26.5.24373 "Selected")|Number of digits|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.5.24373 "Unselected")|A dictionary-word match|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.5.24373 "Unselected")|Edit distance to a popular domain|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.5.24373 "Unselected")|None; hex strings are never flagged|

|   |   |   |
|---|---|---|
|**Question 4**Correct on previous attempt(s)||1 / 1 point|

To keep entropy below the classifier’s threshold, a DGA should:

Question options:

|   |   |
|---|---|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.5.24373 "Unselected")|Draw every character uniformly at random from a large alphabet|
|![Selected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioChecked.svg?v=20.26.5.24373 "Selected")|Use natural or repeated character patterns (e.g. real words)|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.5.24373 "Unselected")|Add more digits to each domain|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.5.24373 "Unselected")|Increase the domain length|

|   |   |   |
|---|---|---|
|**Question 5**Correct on previous attempt(s)||1 / 1 point|

Having to add a method-specific feature for each DGA type best illustrates that DGA detection is:

Question options:

|   |   |
|---|---|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.5.24373 "Unselected")|A solved problem once entropy is measured|
|![Selected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioChecked.svg?v=20.26.5.24373 "Selected")|An arms race / cat-and-mouse between generation and detection|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.5.24373 "Unselected")|Purely a matter of TLD validation|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.5.24373 "Unselected")|Independent of how the domains are generated|

|   |   |   |
|---|---|---|
|**Question 6**Correct on previous attempt(s)||1 / 1 point|

A permutation-based DGA turns paypal.com into paypa1.com. The best detection feature is:

Question options:

|   |   |
|---|---|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.5.24373 "Unselected")|Number of digits|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.5.24373 "Unselected")|Shannon entropy|
|![Selected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioChecked.svg?v=20.26.5.24373 "Selected")|Edit (Levenshtein) distance to a list of popular domains|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.5.24373 "Unselected")|Consonant/vowel ratio|

|   |   |   |
|---|---|---|
|**Question 7**Correct on previous attempt(s)||1 / 1 point|

Two people run the same DGA with the same seed on different machines. The output domains will be:

Question options:

|   |   |
|---|---|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.5.24373 "Unselected")|Completely different|
|![Selected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioChecked.svg?v=20.26.5.24373 "Selected")|Identical|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.5.24373 "Unselected")|The same except for the TLD|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.5.24373 "Unselected")|Randomly different on each run|

|   |   |   |
|---|---|---|
|**Question 8**Correct on previous attempt(s)||1 / 1 point|

Raising the classifier's threshold (with default score values) will most likely:

Question options:

|   |   |
|---|---|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.5.24373 "Unselected")|Increase both true positives and false positives|
|![Selected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioChecked.svg?v=20.26.5.24373 "Selected")|Decrease both true positives and false positives|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.5.24373 "Unselected")|Increase true positives while decreasing false positives|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.5.24373 "Unselected")|Have no effect on either|

|   |   |   |
|---|---|---|
|**Question 9**Correct on previous attempt(s)||1 / 1 point|

A classifier reports 99% accuracy on a domain list where 0.1% of domains are DGA-generated. Why is this nearly meaningless? 

Question options:

|   |   |
|---|---|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.5.24373 "Unselected")|Accuracy can't be measured on a list of domain|
|![Selected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioChecked.svg?v=20.26.5.24373 "Selected")|Under extreme class imbalance, a trivial always-benign model scores 99%+, so accuracy hides recall/precision|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.5.24373 "Unselected")|Entropy is undefined for benign domains|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.5.24373 "Unselected")|99% is below the required detection threshold|

|   |   |   |
|---|---|---|
|**Question 10**Correct on previous attempt(s)||1 / 1 point|

Reverse-engineering the algorithm and seed from a malware sample gives defenders which decisive advantage over purely statistical detection?

Question options:

|   |   |
|---|---|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.5.24373 "Unselected")|A lower false-positive rate only|
|![Selected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioChecked.svg?v=20.26.5.24373 "Selected")|The ability to pre-compute and block future domains before the botnet uses them|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.5.24373 "Unselected")|Higher entropy in detection|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.5.24373 "Unselected")|It removes the need for DNS monitoring|

|   |   |   |
|---|---|---|
|**Question 11**Correct on previous attempt(s)||1 / 1 point|

Why generate thousands of domains daily but register only one or a few?

Question options:

|   |   |
|---|---|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.5.24373 "Unselected")|Registration is free, so they register all|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.5.24373 "Unselected")|To exhaust DNS servers resources with domains that are never used|
|![Selected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioChecked.svg?v=20.26.5.24373 "Selected")|Defenders must register every candidate, while the operator only needs one|
|![Unselected](https://brightspace.ru.nl/d2l/img/0/QuestionCollection.Main.radioUnchecked.svg?v=20.26.5.24373 "Unselected")|Hashing inevitably produces duplicates|