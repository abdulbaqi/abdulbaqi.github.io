---
layout: post
title: Standing on the shoulder of Giants
subtitle: Finding Root Words of the Quran using Linux Commands
bigimg: /img/matrix.jpg
image: /img/bash.png
share-img: /img/bash.png
tags: quran linux text
---

Linux command line is a powerful and free tool. Using just few commands one can productively do powerful text analysis. In this quick tutorial I will demonstrate few of such Linux commands.

Any data science task starts with a question. So, what is our question in this exercise? 

The Question is: **what are the top root words in the Quran?**

For this, we are going to use the [Quranic Arabic Corpus](http://corpus.quran.com/) (QAC) which contains morphological information (among them root word) for each word of the Qruan. 

Without further ado, let us get started.

# curl

`curl -s` allows us to visit a URL and display its contents. `-s` option allows that to be in silent mode. I have downloaded the original file for QAC and kept a version in my website. Let us use `curl` to visit my website and download that file.


```python
!curl -s http://textminingthequran.com/data/quranic-corpus-morphology-0.4.txt > quran_tags.txt
```

Above, `curl` brings the file and displays it into screen, but I wanted to send those lines to a file instead by using `>`.

We can get some initial information about this file using *word count* command `wc`


```python
!wc -l quran_tags.txt
```

    128276 quran_tags.txt


This file contains 128,276 lines. The `-l` option of `wc` indicates number of lines.

Let me display first few lines of this file to see what it contains. This can be done by using first `cat` command to list the entire file but instead of displaying this big file, I will `pipe` the results to another command called `head` to display only first 10 lines. This piping business is a very powerful UNIX tool and we used `|` to do that. 


```python
! cat quran_tags.txt | head
```
    # PLEASE DO NOT REMOVE OR CHANGE THIS COPYRIGHT BLOCK
    #====================================================================
    #
    #  Quranic Arabic Corpus (morphology, version 0.4)
    #  Copyright (C) 2011 Kais Dukes
    #  License: GNU General Public License
    #
    #  The Quranic Arabic Corpus includes syntactic and morphological
    #  annotation of the Quran, and builds on the verified Arabic text
    #  distributed by the Tanzil project.
    
    
We notice that the file contains some copyright block at the beginning and the actual Quranic annotation starts from line number 57. To prove this, I will take the first 60 lines and then `pipe` the results to `tail -5` to show me the *last* 5 lines. 


```python
! cat quran_tags.txt | head -60 | tail -5
```
    LOCATION  FORM  TAG FEATURES
    (1:1:1:1) bi  P PREFIX|bi+
    (1:1:1:2) somi  N STEM|POS:N|LEM:{som|ROOT:smw|M|GEN
    (1:1:2:1) {ll~ahi PN  STEM|POS:PN|LEM:{ll~ah|ROOT:Alh|GEN
    

So, we know that our file has 128,276 lines of code and 57 lines from top has some copyright notes. Using `tail` I can get a working copy of the file by chopping these few lines from top and display only (128,276 - 57 = 128,219) lines from button and save it into a handy file called `qt` (short for Quranic tags).


```python
! cat quran_tags.txt | tail -128219 > qt
```

Let us display the top of the new file to make sure things went as intended. 


```python
!cat qt | head
```
    (1:1:1:1) bi  P PREFIX|bi+
    (1:1:1:2) somi  N STEM|POS:N|LEM:{som|ROOT:smw|M|GEN
    (1:1:2:1) {ll~ahi PN  STEM|POS:PN|LEM:{ll~ah|ROOT:Alh|GEN
    (1:1:3:1) {l  DET PREFIX|Al+
    (1:1:3:2) r~aHoma`ni  ADJ STEM|POS:ADJ|LEM:r~aHoma`n|ROOT:rHm|MS|GEN
    (1:1:4:1) {l  DET PREFIX|Al+
    (1:1:4:2) r~aHiymi  ADJ STEM|POS:ADJ|LEM:r~aHiym|ROOT:rHm|MS|GEN
    (1:2:1:1) {lo DET PREFIX|Al+
    (1:2:1:2) Hamodu  N STEM|POS:N|LEM:Hamod|ROOT:Hmd|M|NOM
    (1:2:2:1) li  P PREFIX|l:P+
    


By looking into this structure, here is a brief on what each column means. 

**first column (1:1:1:1)**
(Seq No.:sura no.:verse no.:word no. (within that verse):segment no. (within that word))

**Second column**
This is word form in Buckwalter transliteration as documented in Kais Dukes work [here](http://corpus.quran.com/java/buckwalter.jsp). 

**Third Column**
The part-of-speech tagging of this word, see [here](http://corpus.quran.com/documentation/tagset.jsp) for a listing of these tags.

**Fourth column** contains a number of morphological features seperated by `|`. The one concerning us in this exercise is the feature prefixed by `ROOT:`. See this [documentation](http://corpus.quran.com/documentation/morphologicalfeatures.jsp) for more details of Qruanic morphological features.

# `cut` and `sed`

`cut` is cool. It is a handy swiss knife at the hand of a data scientist. It splits the line by a delimiter with option `-d` and then pick whatever column you specify by option `f`. In our case, the fields are separated by a tab for which I need to specify `-d'\t\'` as the delimiter, this however will throw an error, because `-d` accepts only one character as delimiter but `\t` is two characters. (I found a solution to use `-d$'\t'` but for some reason the Jupyter notebook is not allowing me to use it). If it were just comma, I would have used `-d','` instead.

This gives me an opportunity to introduce another giant called `sed`. Among other things, it will find and replace texts in our file. Let us use it to replace those tabs with comma, so later `cut` can use `cut -d','` without any error. 


```python
!cat qt | sed 's/\t/,/g'| tail
```

    (114:5:3:1),fiY,P,STEM|POS:P|LEM:fiY
    (114:5:4:1),Suduwri,N,STEM|POS:N|LEM:Sador|ROOT:Sdr|MP|GEN
    (114:5:5:1),{l,DET,PREFIX|Al+
    (114:5:5:2),n~aAsi,N,STEM|POS:N|LEM:n~aAs|ROOT:nws|MP|GEN
    (114:6:1:1),mina,P,STEM|POS:P|LEM:min
    (114:6:2:1),{lo,DET,PREFIX|Al+
    (114:6:2:2),jin~api,N,STEM|POS:N|LEM:jin~ap|ROOT:jnn|F|GEN
    (114:6:3:1),wa,CONJ,PREFIX|w:CONJ+
    (114:6:3:2),{l,DET,PREFIX|Al+
    (114:6:3:3),n~aAsi,N,STEM|POS:N|LEM:n~aAs|ROOT:nws|MP|GEN
    
Now pipe that with `cut`


```python
!cat qt | sed 's/\t/,/g'| cut -d',' -f4 | tail
```

    STEM|POS:P|LEM:fiY
    STEM|POS:N|LEM:Sador|ROOT:Sdr|MP|GEN
    PREFIX|Al+
    STEM|POS:N|LEM:n~aAs|ROOT:nws|MP|GEN
    STEM|POS:P|LEM:min
    PREFIX|Al+
    STEM|POS:N|LEM:jin~ap|ROOT:jnn|F|GEN
    PREFIX|w:CONJ+
    PREFIX|Al+
    STEM|POS:N|LEM:n~aAs|ROOT:nws|MP|GEN
    

So, above we asked to split the Quranic root files by comma `-d','` and wanted to preserve only the fourth column `-f4`, and then we are showing only the `tail`. 

(*a side note: I initially tried `head` but was getting a broken pipe error, because head hurried to show things before it is ready by the process before it, so, I switched to tail to give enough time for `cut` to finish its business*)

# grep

And now the powerful `grep`. I will use it to pick the lines that has the `ROOT:` words. I am using *regular expressions* hence the `-E` option and want to output only the captured word and not the entire lines by specifying the `-o` option. The pattern I used is `'ROOT:[^|]*'` which in plain English means: Traverse all lines and return only the lines that has the word ROOT:followed by anything except the character `|`. 

Regular Expression is a wild beast and worth investing time if you want to analyze text. You can always test various patterns on-line for example at [regexr.com](https://regexr.com/)


```python
!cat qt | sed 's/\t/,/g'| cut -d',' -f4 | grep -oE 'ROOT:[^|]*' |tail
```

    ROOT:Alh
    ROOT:nws
    ROOT:$rr
    ROOT:wsws
    ROOT:xns
    ROOT:wsws
    ROOT:Sdr
    ROOT:nws
    ROOT:jnn
    ROOT:nws


I want to the exact the word that appears after the prefix `ROOT:`. To do this I can employ a `cut` again on the `:` delimiter and take the second column. (I am sure there are better ways though).


```python
!cat qt | sed 's/\t/,/g'| cut -d',' -f4 | grep -oE 'ROOT:[^|]*' |\
cut -d':' -f2 | tail
```

    Alh
    nws
    $rr
    wsws
    xns
    wsws
    Sdr
    nws
    jnn
    nws


This way I have the list of all roots in the Quran. 

Next, I want to sort them and count them.

# sort


```python
!cat qt | sed 's/\t/,/g'| cut -d',' -f4 | grep -oE 'ROOT:[^|]*' |\
cut -d':' -f2 |\
sort | tail
```

    zyn
    zyn
    zyn
    zyt
    zyt
    zyt
    zyt
    zyt
    zyt
    zyt


All `sort` does is to alphabetically sort the list, hence all the repeated roots are stacked. What I wanted is a unique list of those roots. Here comes the `uniq` command.

# uniq


```python
!cat qt | sed 's/\t/,/g'| cut -d',' -f4 | grep -oE 'ROOT:[^|]*' |\
cut -d':' -f2 |\
sort | uniq | tail
```

    zwd
    zwj
    zwl
    zwr
    zxrf
    zyd
    zyg
    zyl
    zyn
    zyt


The above gives the unique list of all roots of the Quran without preserving how many of each roots are there.

While here, let me use `wc` with the `-l` option to tell me the number of lines, and hence number of unique root words in the Quran.


```python
!cat qt | sed 's/\t/,/g'| cut -d',' -f4 | grep -oE 'ROOT:[^|]*' |\
cut -d':' -f2 |\
sort | uniq | wc -l
```

    1651


**Wow**! with just few commands I managed to discover that the Quran contains a total of **1651** roots. This should give hope to those who intend to learn vocabulary of the Quran, which has nearly 77k words, but only 1,651 root words to memorize. 

Now, let us revisit `uniq` with the `-c` option to preserve the counts.


```python
!cat qt | sed 's/\t/,/g'| cut -d',' -f4 | grep -oE 'ROOT:[^|]*' |\
cut -d':' -f2 |\
sort | uniq -c | tail
```

          2 zwd
         81 zwj
          4 zwl
          6 zwr
          4 zxrf
         61 zyd
          9 zyg
         10 zyl
         46 zyn
          7 zyt


All that is left is to do a `sort` again but with options `-n` to make numeric sort on the counts and `-r` to make reverse sort in the descending order. Let us take the most popular 20 roots.

(*note: again with the above I run into broken pipe issue, so I resorted to tail, then removed the reverse order*)


```python
!cat qt | sed 's/\t/,/g'| cut -d',' -f4 | grep -oE 'ROOT:[^|]*' |\
cut -d':' -f2 |\
sort | uniq -c | sort -n | tail -20
```

        346 jEl
        360 Eml
        363 kll
        373 E*b
        381 smw
        382 Ayy
        405 ywm
        461 ArD
        513 rsl
        514 byn
        519 $yA
        525 kfr
        549 Aty
        660 qwm
        854 Elm
        879 Amn
        980 rbb
       1390 kwn
       1722 qwl
       2851 Alh


### Great revelations

See the power of the Linux shell. With just few piped commands, I produced a sorted list of the roots and their frequency in the Quran. Here are some winning roots.

**Alh**

Sure, no doubt that the word 'Allah' will be the winner far exceeding the runner's up.

**qwl**

The second most frequent root word in the Quran is (قول) `qwl` which are the derivatives of `saying`. After all, the Quran is the sayings of Allah. Anyone would recognize the thousands of times Allah command His prophet Muhammad - peace be upon him - to `say` whatever Allah wants him to say, and hence it is the greatest prove that the job of Prophet Muhammad is to convey whatever he is told to convey and never to author anything himself. This data analysis just comes to prove that.

**kwn**

In the third position comes all derivatives of (كون) `kwn` which refers to the verb to be. And verb to be is the mother of all actions, and it is the word through which Allah executes His orders. When He intends something, He just says 'Be' and it becomes. Shakespeare might had some glimpse of the significance of this verb when he said, "To be or not to be, that is the Question".

**rbb**

The fourth most frequent word refers to the word **Lord** which is another way to say Allah. Actually -with few exceptions- you can add this count to the counts of the word Allah.

**Amn** and **Elm** 

These includes all derivatives of faith/believe `Amn` and knowledge `Elm`, showing great emphasis on these two qualities as essential ingredients for salvation. Islam is nothing but seeking knowledge and having faith accordingly. Also note `Amn` brings in all derivatives of `peace` as well. 

One can derive much more insights from just this root frequencies. Spend some time studying the significance of each of those roots, but here I focused more on the technical bits.

## Extension: Roots of a surah

We are very close to another handy extension of finding roots of not the entire Quran, rather only a particular surah. First let us revisit the file again.


```python
!cat qt|tail
```

    (114:5:3:1) fiY P STEM|POS:P|LEM:fiY
    (114:5:4:1) Suduwri N STEM|POS:N|LEM:Sador|ROOT:Sdr|MP|GEN
    (114:5:5:1) {l  DET PREFIX|Al+
    (114:5:5:2) n~aAsi  N STEM|POS:N|LEM:n~aAs|ROOT:nws|MP|GEN
    (114:6:1:1) mina  P STEM|POS:P|LEM:min
    (114:6:2:1) {lo DET PREFIX|Al+
    (114:6:2:2) jin~api N STEM|POS:N|LEM:jin~ap|ROOT:jnn|F|GEN
    (114:6:3:1) wa  CONJ  PREFIX|w:CONJ+
    (114:6:3:2) {l  DET PREFIX|Al+
    (114:6:3:3) n~aAsi  N STEM|POS:N|LEM:n~aAs|ROOT:nws|MP|GEN
    

So, the secret of grabbing a surah is to `grep '(114:'` to get all lines for surah no. 114. With this in mind, I will just add this small addition at the beginning of my already existing pipe as follows.


```python
!cat qt| grep '(114:'|sed 's/\t/,/g'| cut -d',' -f4 | grep -oE 'ROOT:[^|]*' |\
cut -d':' -f2 |\
sort | uniq -c | sort -nr
```

          5 nws
          2 wsws
          1 xns
          1 rbb
          1 qwl
          1 mlk
          1 jnn
          1 Sdr
          1 Ew*
          1 Alh
          1 $rr


All that is left is to place this entire code inside a shell script and allow the user to run the script passing the surah no. as parameter and get the list of roots for that surah.

Open up a file using your favorite editor (I use `mcedit`), and create a file, say `qr.sh` (short for Quran Roots) with the following content:

```linux
#!/usr/bin/env bash
NUM="$1"

cat qt| grep '('$NUM':'|sed 's/\t/,/g'| cut -d',' -f4 | grep -oE 'ROOT:[^|]*' |\
cut -d':' -f2 |\
sort | uniq -c | sort -n
```

The first line is for the shell to recognize where to find the `bash` shell if needed. The second line is a variable that captures the first argument from user as we will see later. The rest is the exact code we have seen earlier, just note how I am using this variable `$NUM` in the first `grep`.

After that, you need to make this script executable by the following command.


```python
!chmod u+x qr.sh
```

Now, let us try to launch our new script asking to return all roots for sura no. 1


```python
!./qr.sh 1
```

          1 Dll
          1 Ebd
          1 Elm
          1 Ewn
          1 Hmd
          1 dyn
          1 gDb
          1 gyr
          1 hdy
          1 mlk
          1 nEm
          1 qwm
          1 rbb
          1 smw
          1 ywm
          2 Alh
          2 SrT
          4 rHm


Sure enough, the derivatives of the root word of `rHm` (رحم) (meaning mercy) is the most frequent word in this surah. 

Just out of curiosity, we know Surah No. 55 (ar-Rahman) has lots of repetition of the verse:

(فبأي آلاء ربكما تكذبان)

Let us test that


```python
!./qr.sh 55 | tail
```

          4 Hsn
          4 byn
          4 mrj
          4 wzn
          5 smw
          6 Ans
          8 jnn
         31 Alw
         32 k*b
         36 rbb


# Conclusion

This file is a gold mine that we only are scratching its surface, and starting with Linux command line. I will take this problem into more interesting one using `python`'s `pandas` tool [here](http://abdulbaqi.io/2019/01/19/quranic-roots-pandas/). 

You will find useful material in [this](https://www.datascienceatthecommandline.com) book and as well as in the [data36](https://data36.com/) blog.