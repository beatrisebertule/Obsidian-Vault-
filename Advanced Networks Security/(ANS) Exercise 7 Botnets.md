[[(quizz) DGA]]



1. Number of digits: counts how many characters in the domain are in [0-9]. Random  
and hash-based domains tend to have more digits than legitimate domains.  
2. Longest vowel-less sequence: the length of the longest consecutive run of characters  
with no vowels (a, e, i, o, u). Natural language words rarely produce long consonant-only  
runs; random strings do.  
3. Consonant/vowel ratio: the ratio of consonants to vowels across the whole domain. A  
high ratio indicates an unnatural character distribution.  
4. Entropy: the Shannon entropy of the character distribution. High entropy indicates that  
characters are drawn uniformly at random (as in arithmetic or hash-based DGAs). Low  
entropy, as in wordlist domains, is harder to distinguish from legitimate domains


Background: DGA Types  
DGAs are used by malware to programmatically generate large numbers of candidate domain  
names. The malware and its command-and-control (C2) server share the same algorithm and  
seed, so they independently arrive at the same domains each day. There are four main DGA  
types covered in this course, each with distinct properties that affect both how the domains look  
and how detectable they are.  

Arithmetic-based DGAs. Arithmetic-based DGAs construct domain names by generating  
sequences of ASCII characters. A seeded pseudo-random number generator (PRNG) is used to  
pick characters from a defined alphabet (typically [a-z0-9]). The resulting domains look like  
random strings of letters and digits, for example: xkq7mf2z.com, a3bc9wqp.net.  
Because the characters are drawn uniformly at random, arithmetic domains have high Shannon  
entropy and typically contain an above-average number of digits. Both of these are strong  
statistical signals that classifiers can exploit. Given the same seed, the generator will always  
produce the same sequence of domain names.  

Hash-based DGAs. Hash-based DGAs apply a hashing algorithm such as MD5 or SHA-256  
to a seed value (often combined with the current date). The hexadecimal digest is then used as  
the domain name, for example: 3f4a1bc7d9e20f8a.com.  
Hash domains are composed exclusively of hexadecimal characters: digits 0{9 and letters a{f  
(except the TLD). This severely restricted character set is a very strong detection feature: a  
1

domain where every character is in [0-9a-f] is almost certainly hash-generated. Hash-based  
DGAs are deterministic — the same seed and date always produce the same domain.  

Wordlist-based DGAs. Wordlist-based DGAs construct domain names by concatenating  
words from a dictionary. Two or three words are chosen (using a seeded PRNG) and joined,  
sometimes with a digit or separator, for example: happybluecloud.net, fastredtable7.org.  
Because real dictionary words have low entropy and natural letter patterns, wordlist domains  
closely resemble legitimate domain names. They are the hardest to detect using purely statistical  
features such as entropy or consonant/vowel ratio. Detection typically requires a dictionary or  
n-gram language model. The wordlist must be included in the malware payload.  

Permutation-based DGAs. Permutation-based DGAs start from a known seed domain (e.g.  
google.com) and apply character-level permutations: substitutions (o→0, i→1), insertions, dele-  
tions, or letter swaps. The results resemble the original domain, for example: g00gle.com,  
goggle.net.  
Permutation domains have low entropy and look very similar to legitimate domain names,  
making them the most evasive type. They are also used in typosquatting attacks. Detection  
requires comparing candidate domains against a list of known popular domains using techniques  
such as edit distance (Levenshtein distance)


<span style="color:rgb(219, 0, 0)">Raising the threshold reduces false positives but also reduces true positives;<br>lowering it has the opposite effect.</span> 

