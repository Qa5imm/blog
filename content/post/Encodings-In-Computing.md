---
title: Encodings in computing
date: 2025-01-01
---

Everything around us is encoded, whether it’s representing voice using words or expressing emotions through body language and gestures. We generally rely on encodings when there’s no direct pathway between two intelligent systems or processes, requiring us to use intermediary "dumb" systems or steps that have direct route to these smart entities.

<!--more-->

Take our brain, for instance. Although it’s highly intelligent, it doesn’t have a direct way to communicate with another brain. To share something between two brains, we must go through some "dumb" intermediaries like gestures and sensory organs. For example, If you want to express that you’re angry, you might encode that emotion by making a face or cussing (I’m partial to second one, though I’ve never actually done it). This creates a chain like:


Intelligent (brain) -> dumb (gestures) -> dumb (eyes) -> Intelligent (brain)

But what if we could skip these intermiadiary steps and communicate directly with another brain? Some [folks](https://www.youtube.com/watch?v=8BrLNgKLWzs) are working on making that happen.

Second need for encodings is when an intelligent system needs to interact with a dumb system (dumb is defined relative to intelligent system), essentionaly first part of above chain. It’s similar to explaining a complex idea to someone unfamiliar with it - we simplify the complexity, encoding it into more accessible terms so they can understand the basics and build upon them later.

In computing, encodings serve a similar purpose but also include aspects like security, storage efficiency, and data integrity. However, for this article, we’ll focus only on the communication between humans’ greatest invention (computers) and God’s greatest invention (humans), since you can’t outsmart God, and His creation is more intelligent than ours.

## Encodings in computing

Digital devices require encodings as they're not intelligent enough to process and store human expressions (words, voices, etc) directly. They only understand 1's and 0's (because at the lowest level they're just a bunch of transistors with only two states: on or off), therefore, we encode human expressions into a format consisting solely of 1's and 0's. This requires a standard that can allow us to encode and store human expressions in computers and decode them later when needed. This is what people did in 1950s but humans, being close relatives of apes, tends to go crazy. That's what happened with encodings, everyone came up with their own ways to encode characters and numbers, which was fine initially but after widespread adoption of computers in 1960s it became almost impossible for two computers to communicate with each other unless they use same encoding standard. To resolve this, some wise apes got together and created an encoding scheme called ASCII. 


ASCII used seven-bit encodings for characters and numbers, both printable and non-printable, allowing for encoding upto 128 (2^7, starting from zero) symbols. Non-printable characters include beep sounds (BEL), carriage return (CR), and line feed (LF) etc., essentionally characters that one cannot see. Each symbol was asssigned a unique binary number and this mapping is commanly reffered as [ASCII table](https://en.wikipedia.org/wiki/ASCII#/media/File:USASCII_code_chart.svg), for simplicity modern ASCII tables are presented using base-10.  Some might wonder why lowercase english alphabets don't immediately follow uppercase ones. Answer: they're wise apes.

Take a look at the binary patterns below:

A : 1000001 (65)
B : 1000010 (66)
C : 1000011 (67)

a : 1100001 (97)
b : 1100010 (98)
c : 1100011 (99)


Only one bit differs between upper and lower case binary values and one can know the exact position of letters in english alphabet by removing the first two bits in both cases. One important thing to note is that ASCII was (and still is) only for english alphabet. People who didn't use english came up with their own encodings schemes.

With the advent of microprocessors, things changed because they store data in sequence of 8 bits. So, apes now had a full extra bit, they got excited!! One extra bit means 128 extra symbols (128-255). People started storing symbols from other languages, and these different encodings were given the fancy name [code pages](https://en.wikipedia.org/wiki/Code_page) (i.e., IS0-8859-1). All code pages share the same encodings up to 127 (same as ASCII); only 127 onwards differ. Because characters above 127 differed across code pages, decoding text required knowing which code page was used for encoding if it contained characters other than ASCII, otherwise text would apprear gibberish. 

This newly available space could only accommodate the alphabets of one language due to the limited number of slots, this limitation meant that supporting multiple foreign languages on a single machine was impossible. With the globalization of computers, this became a big issue because an email sent from someone in Russia will appear as gibberish in Nordic countries due to different code pages used for encoding and decoding. Moreover, some Asian languages contain thousands of characters, which couldn't be represented even with the full 8 bits, making them unencodable. To address these issues, some wise apes again sat together and created something called [Unicode](https://en.wikipedia.org/wiki/Code_page). A lot happened between ASCII and Unicode, with many encoding schemes like UCS-2, UCS-4, etc. but they were not widely adopted due to either being storage inefficient or not backwards compatible with ASCII.

In Unicode standard, every character in the world was assigned a unique number called a code point, starting with 'U+' followed by hexadecimal numbers (e.g., code point for 'A' is U+0041). The creators of Unicode didn't specify how to encode these characters, leaving that task to others, who then developed UTFs (Unicode Transformation Formats). UTFs were sets of encoding schemes to represent Unicode in binary, out of which UTF-8, UTF-16, UTF-32 are the most widely used. UTF-8 is a variable-length encoding, meaning it can store each character in 1, 2, 3, or even up to 4 bytes. UTF-16 and UTF-32 are fixed-length encodings, storing every character in 2 or 4 bytes, respectively. The good thing about UTFs was that they were all backwards compatible with ASCII (e.g., 'A' remain encoded as 65).

It's important to understand that Unicode itself is not an encoding scheme (unlike ASCII), it's just a universal standard that assigns every known symbol a unique number. You can encode these code points using whatever encoding scheme (ASCII, UTF, UCS etc) you deem fit. For example, if we encode 0041 using UTF-16, which stores everthing in two bytes, then it would be 0000 0000 0100 0000 (four bits per each hex number), which is 65 (same as ASCII) in base-10 number system. You can see from above why fixed-length encodings like UTF-16 and UTF-32 would be extremely space-inefficient if it's used to store alot of english text but some languages still use it.

Out of all encoding schemes, UTF-8 became the most widely adopted because first it was variable-length and thus highly space-efficient. Second, it was backwards compatible with ASCII, making it work with systems that only understand ASCII. Lastly, it never encodes eight consecutive zeros, which some old computers interpret as the end of a string (ignoring everything after them) making it compitable with old systems as well. You can watch this [video](https://www.youtube.com/watch?v=tbdym9ZtepQ) to understand how variable-length encoding works in UTF-8 and how it avoids encoding eight consecutive zeros.

*Commands below are only for linux and mac users (windows users - in tech - are hopeless cases (unless they're a game dev), I don't care much about them)*

Enough with the talk, let's see things in action, run `echo "cafå" > test && cat test`, it will save a file encoded in UTF-8 (default) and print out the contents, followed by `iconv -f UTF-8 -t ISO-8859-1 test` that will convert the file's encoding scheme from UTF-8 to ISO-8859-1 (code-page) and print out it's contents. You will see � in place of å, it is called a replacement character and is printed when an encoding scheme couldn't interept the byte sequence, there's so such character å defined in ISO-8859-1 so when it tries to decode bytes for å it couldn't understand it and prints �.

Before I wrap up this article, let me share some cursed knowledge with you. Length function for strings in programming languaes operate on bytes rather than characters. Open a node REPL by typing `node` in your terminal and run the following:
```
s = "👍"
s.length
```
You'll get 2 as output because JavaScript encodes strings using UTF-16. A thumbs-up emoji takes 4 bytes, but since JavaScript uses UTF-16, it interpetes 👍 as two distinct bytes. This means every English alphabet character in JavaScript will take twice as much space compared to a langauge (python) which use UTF-8 (store english alphabets in 8-bits).


