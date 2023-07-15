# Roanoke Challenge Write Up

## Background
On 07/14/2023 @plasma in the Roanoke Discord (https://discordapp.com/invite/dtXeKFM) shared an image of a flyer his friend found at Sweet Donkey.

![challenge-flyer](https://github.com/dru1d-foofus/RoanokeChallengeWriteUp/assets/4245930/53304377-ba3d-4ec8-b1c4-a41730968747)


This seemed like a fun challenge and a great way to solve problems with some fellow Roanokers; so away we went.

## Part 1
The first thing you'll notice on the flyer is a QR code, a URL, and some backwards text.

Some things to note:
- The URL and QR code go to the same place - https://www.protectedtext.com/letthegamebegin
- The backwards text (in a readable format) is "DoesRoanokeHaveAnyTechNerds?"

When visiting the URL, you are presented with a protected paste.

![paste-prompt](https://github.com/dru1d-foofus/RoanokeChallengeWriteUp/assets/4245930/b061b573-7900-4c60-869f-310d5215b47f)


We ended up trying variations of the backwards text and after handjamming for a minute or so obtained access to the paste with: "sdreNhceTynAevaHekonaoRseoD"; this is exactly what was printed on the flyer, but with the "?" stripped off the front.

We are then presented with a blob of hexadecimal:
```
4953424e20393738303135363333323235350a0a0a54656368206e657264
7320646f6e2774207265616420706f657472792c20736f3a0a4c696e6520
310a4c696e6520320a20202020202020416c736f204c696e6520320a2020
4c696e6520330a4c696e6520340a0a0a6c656e28616e7377657229203d20
35360a616e737765725b3a355d203d20226573707767220a64656c696e65
61746f72203d20222f220a0a2d2d424547494e20414e535745522d2d0a32
203839203820362f3320313232203920312f33203432203220312f322031
3234203820312f322031323820313220372f3220313332203420332f3320
37203820320a2f31203139203520312f3420323031203120312f33203232
35203420342f342f3320323130203720322f3320323137203220322f3420
323520313120310a2f34203539203420332f34203838203620352f372f32
20353220332031302f33203131203720312f342039203620342f33203231
39203420330a2f3120313039203620342f3420323131203120312f312033
33203220342f3320323033203220332f3120313432203620312f33203231
37203420322f3420313936203320350a2f342f34203832203520352f3220
3235203320342f34203831203320342f3220313134203420362f322f3420
37203420360a2f312038203620362f32203334203420312f342032343320
3420332f332033203120322f3220313533203520322f3320313634203620
312f32203733203920340a2f3220313332203420332f3220383020342032
2f342f34203231203120332f32203833203620322f332031393720382037
2f3320313534203620320a2f3220313832203220322f3220353020342033
2f31203437203220312f3320313531203620352f322f3220313034203320
322f31203636203420310a2d2d454e4420414e535745522d2d0a0a7e2320
616e73776572202d2d617070656e6420222e6f6e696f6e220a0a7e232065
63686f202d6e202232663262656465376533663062373464643164662220
3e206165735f7061737377645f7074315f666f725f6c617465722e747874
0a0a0a0a0a0a502e532e202d20546869732067616d6520697320666f7220
74686f7365206c6f63616c20746f20526f616e6f6b652c2056412e205468
6520656e64206973207573656c65737320746f20616c6c206f7468657273
2e200a
```

Converting that from hex (We used cyberchef for this) nets us the following: 
```
ISBN 9780156332255


Tech nerds don't read poetry, so:
Line 1
Line 2
       Also Line 2
  Line 3
Line 4


len(answer) = 56
answer[:5] = "espwg"
delineator = "/"

--BEGIN ANSWER--
2 89 8 6/3 122 9 1/3 42 2 1/2 124 8 1/2 128 12 7/2 132 4 3/3 7 8 2
/1 19 5 1/4 201 1 1/3 225 4 4/4/3 210 7 2/3 217 2 2/4 25 11 1
/4 59 4 3/4 88 6 5/7/2 52 3 10/3 11 7 1/4 9 6 4/3 219 4 3
/1 109 6 4/4 211 1 1/1 33 2 4/3 203 2 3/1 142 6 1/3 217 4 2/4 196 3 5
/4/4 82 5 5/2 25 3 4/4 81 3 4/2 114 4 6/2/4 7 4 6
/1 8 6 6/2 34 4 1/4 243 4 3/3 3 1 2/2 153 5 2/3 164 6 1/2 73 9 4
/2 132 4 3/2 80 4 2/4/4 21 1 3/2 83 6 2/3 197 8 7/3 154 6 2
/2 182 2 2/2 50 4 3/1 47 2 1/3 151 6 5/2/2 104 3 2/1 66 4 1
--END ANSWER--

~# answer --append ".onion"

~# echo -n "2f2bede7e3f0b74dd1df" > aes_passwd_pt1_for_later.txt





P.S. - This game is for those local to Roanoke, VA. The end is useless to all others. 
```

## Part 2
We have some idea of what to do next. We're looking at a cipher that needs to be decoded in order to get us to the next step. Here are the things that really stick out about this clue:
- ISBN is the International Standard Book Number and it serves as a unique identifier for books. Plugging 9780156332255 into Google returns T.S. Eliot's "Four Quartets"; a collection of poetry. We're looking at a book cipher.
- We're also given some information about how to align text from the poems. Anything that has more than one indentation should be appended to the previous line.
- We're told the plaintext is 56 characters in length.
- We're told the first 5 characters are "espwg".
- We're given a delimiter.
- We're given a block of ciphertext.
- We're told to append a ".onion" to our answer. This means that we're looking for a valid .onion site address. These are typically 56 characters in length and only contain the following characters [a-z][2-7].
- We're also given some AES key material for later on.

The first thing we do here is source a copy of the book. There were a few options on doing this, but for sake of convenience (and ease of data processing) we look for online sources. http://www.davidgorman.com/4quartets/index.htm seems to be a solid choice for our purposes. We'll need this for reference later.

We need to tackle the ciphertext block before we can continue. We know that "/" is a delimiter and this means we can break up the text. We opted to replace any occurrence of "/" with a newline. This leaves us with:
```
2 89 8 6
3 122 9 1
3 42 2 1
2 124 8 1
2 128 12 7
2 132 4 3
3 7 8 2
1 19 5 1
4 201 1 1
3 225 4 4
4
3 210 7 2
3 217 2 2
4 25 11 1
4 59 4 3
4 88 6 5
7
2 52 3 10
3 11 7 1
4 9 6 4
3 219 4 3
1 109 6 4
4 211 1 1
1 33 2 4
3 203 2 3
1 142 6 1
3 217 4 2
4 196 3 5
4
4 82 5 5
2 25 3 4
4 81 3 4
2 114 4 6
2
4 7 4 6
1 8 6 6
2 34 4 1
4 243 4 3
3 3 1 2
2 153 5 2
3 164 6 1
2 73 9 4
2 132 4 3
2 80 4 2
4
4 21 1 3
2 83 6 2
3 197 8 7
3 154 6 2
2 182 2 2
2 50 4 3
1 47 2 1
3 151 6 5
2
2 104 3 2
1 66 4 1
```

We can see that the number of items on each line are consistent - either there are 4 items on a line or there is only one. Let's see if we can determine the rules for the cipher based on our provided information. Let's analyze the first entry in our ciphertext as we already have the plaintext for it.

![cipher-breakdown](https://github.com/dru1d-foofus/RoanokeChallengeWriteUp/assets/4245930/55b6cc17-8c5f-482e-beec-6b28f7247d75)


- You can see that the first item represents the poem in the collection. This will always be 1 - 4; it is the "Four Quartets" after all.
- The second item varies pretty drastically. This is indicative of it being assigned to a line within the poem.
- The third item can be deduced as a word within the line.
- Which leaves the fourth as the letter within the word.

We can go ahead and take the text from the poems and toss them into individual text files. There are instances where formatting will need to be done in order to properly align the key to our cipher. We can refer back to:
```
Line 1
Line 2
       Also Line 2
  Line 3
Line 4
```
