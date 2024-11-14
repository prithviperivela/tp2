# Spelling Quiz

### encrpyt.py
```python

import random
import os

files = [
    os.path.join(path, file)
    for path, dirs, files in os.walk('.')
    for file in files
    if file.split('.')[-1] == 'txt'
]

alphabet = list('abcdefghijklmnopqrstuvwxyz')
random.shuffle(shuffled := alphabet[:])
dictionary = dict(zip(alphabet, shuffled))

for filename in files:
    text = open(filename, 'r').read()
    encrypted = ''.join([
        dictionary[c]
        if c in dictionary else c
        for c in text
    ])
    open(filename, 'w').write(encrypted)
```
### flag.txt
brcfxba_vfr_mid_hosbrm_iprc_exa_hoav_vwcrm

**FLAG: picoCTF{perhaps_the_dog_jumped_over_was_just_tired}**

## My Approach to solve the challenge

The encrypt.py encrypts the flag.txt and study-guide.txt using mono-alphabetic substitution cipher. Using the study-guide.txt we can crack the substitution key.
Using [substitution solver](https://www.guballa.de/substitution-solver), we can crack the cipher by giving it a large amount of text from study guide. It runs frequency analysis on the text and cracks the cipher.

Using this cipher we can use [decoder](https://www.dcode.fr/monoalphabetic-substitution) to decode the flag by using the substitution sequence `sprgwhkjoqzldcuvyemnbtiafx` as the reciprocal alphabet sequence.

This gives the flag as `perhaps_the_dog_jumped_over_was_just_tired`
