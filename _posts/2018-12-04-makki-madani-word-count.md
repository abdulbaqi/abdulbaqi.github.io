---
layout: post
title: The priority of ideology over legislation
subtitle: counting Makki and Madani words in the Quran
bigimg: /img/code.jpeg
image: /img/kaba.jpg
share-img: /img/kaba.jpg
tags: quran python
---
The Quran started to reveal in Makkah for a total period of 13 years. Then, the Prophet migrated to Medina and stayed there for 10 years before he died and the Quran stopped to reveal. The 13 Meccan years were much different in nature than the 10 Medinan years. During the 13 Meccan years, prophet Muhammad struggled to build human beings that would later rule a nation. That required nurturing the tenants of the Islamic ideology in the hearts and minds of the Muslims amidst a hostile environment. Thus, the Quranic chapters and verses revealed in Makkah were suitable to address these challenges in Makkah. 

Later, and after 13 years, the Prophet and the Muslims found a supportive community who were willing to host this newly growing religion in the nearby city of Medinah. Islam founded its first state in Medinah, after the Prophet and his Maccan companions migrated there. Much of Islamic legislations and rulings governing personal, financial and foreign relations were coded in Medinah. Thus Medinan chapters and verses were different in nature. 

This article does not intend to tell the story of Meccan and Medinan verses in detail, for that you may want to refer to my [PhD report](http://textminingthequran.com/papers/firstyear.pdf).

Here, I wanted rather to ask the question: how much of the entire Quran was revealed in Makkah and how much was in Madinah? Time-wise, 13 years in Makkah out of 23 total years is equivalent to 56.5%. Does that mean also that 56% of the Quran contains Mekki verses? 

If we count number of chapters from any standard table of the contents of the Quran, we find that out of 114 chapters 86 (75.4%) are Meccan verses. However, as chapter lengths vary greatly, relying on chapter counts is not accurate. 

The most accurate method to measure the length of Mekki and Madani contents is to work at word level, and hence welcome to the world of programming. 

In what follows, is a `python` script that calls the Quran `api` which I used in a [previous](http://abdulbaqi.io/2018/10/04/random-verse-python/) article. 

The API allows retrieving a chapter of the Quran (say chapter 112) by the following link. 

```
http://api.alquran.cloud/surah/112
```

The returned `JSON` file looks like the structure below. Note, how the verse (`ayahs`) are contained in an array, and also note that `revelationType` tells us if this chapter is Mekki or Madani. 

```json
{
  "code": 200,
  "status": "OK",
  "data": {
    "number": 112,
    "name": "سورة الإخلاص",
    "englishName": "Al-Ikhlaas",
    "englishNameTranslation": "Sincerity",
    "revelationType": "Meccan",
    "numberOfAyahs": 4,
    "ayahs": [
      {
        "number": 6222,
        "text": "بِسْمِ اللَّهِ الرَّحْمَٰنِ الرَّحِيمِ قُلْ هُوَ اللَّهُ أَحَدٌ",
        "numberInSurah": 1,
        "juz": 30,
        "manzil": 7,
        "page": 604,
        "ruku": 554,
        "hizbQuarter": 240,
        "sajda": false
      },
      {
        "number": 6223,
        "text": "اللَّهُ الصَّمَدُ",
        "numberInSurah": 2,
        "juz": 30,
        "manzil": 7,
        "page": 604,
        "ruku": 554,
        "hizbQuarter": 240,
        "sajda": false
      },
      {
        "number": 6224,
        "text": "لَمْ يَلِدْ وَلَمْ يُولَدْ",
        "numberInSurah": 3,
        "juz": 30,
        "manzil": 7,
        "page": 604,
        "ruku": 554,
        "hizbQuarter": 240,
        "sajda": false
      },
      {
        "number": 6225,
        "text": "وَلَمْ يَكُنْ لَهُ كُفُوًا أَحَدٌ",
        "numberInSurah": 4,
        "juz": 30,
        "manzil": 7,
        "page": 604,
        "ruku": 554,
        "hizbQuarter": 240,
        "sajda": false
      }
    ],
    "edition": {
      "identifier": "quran-simple",
      "language": "ar",
      "name": "Simple",
      "englishName": "Simple",
      "format": "text",
      "type": "quran"
    }
  }
}

```

Studying the above structure, my idea is to loop over 114 chapters, then loop over each verse of this chapter and use `len(aya['text'].split()` to count the number of words in each verse and then maintain appropriate counters.  

Here is the `python` listing. Ask me if anything is unclear. 


<iframe height="400px" width="100%" src="https://repl.it/@baqi/count-verse-makki-madani?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>

When you run the program, it will start posting word counts for each sura sequentially, and at the end will post the following statistics.

```
Quran has 114 surah among them 86 Meccan Surah, and 28 Madani
Total words in Quran: 82823
Mekki words: 50451 [61%]
Madani words: 32372 [39%]
```

One final note: The API adopted the view that the first verse of the Quran is affixed with four Arabic words that translates into "In the name of Allah the most Merciful the Most Gracious". If you want you may subtract 452 words from the total (i.e., 113 surah which starts with Bismillah, as surah no. 9 does not start with Bismillah).