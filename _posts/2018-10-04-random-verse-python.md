---
layout: post
title: Random Verse App with Python
subtitle:
bigimg: /img/code.jpeg
image: /img/quran.jpeg
share-img: /img/quran.jpeg
tags: quran python
---
Each Quranic verse is a miracle. It must be so, because it is words of God and the Wise Allah. Won't it be a wonderful idea to create an app that throws to you random verses from the Quran?

Initially I was planning to maintain a database or `csv` file containing all verses from the Quran (which are readily available). But, then I found [this](https://alquran.cloud/api) api. So, why re-invent the wheel when those guys did the job thankfully.

The idea of the `API` was that the entire Quran is 6,236 verses, and any of these verses can be retrieve with an `url` like the one below:

```
http://api.alquran.cloud/ayah/262
```

So, we can generate a random number between 1 and 6,237 in python as follows

```python
import random

aya = random.randint(1,6237)
```
The API allows many variations to get english translations. I used the following option:

```
http://api.alquran.cloud/ayah/{verse}/editions/quran-uthmani,en.pickthall
```

This brings the original Arabic text rendered in `uthmani` script and then the English translation by M. Pickthall. 

The returned `JSON` file looks like this:

```json
{
code: 200,
status: "OK",
data: [
{
number: 262,
text: "ٱللَّهُ لَآ إِلَٰهَ إِلَّا هُوَ ٱلْحَىُّ ٱلْقَيُّومُ ۚ لَا تَأْخُذُهُۥ سِنَةٌۭ وَلَا نَوْمٌۭ ۚ لَّهُۥ مَا فِى ٱلسَّمَٰوَٰتِ وَمَا فِى ٱلْأَرْضِ ۗ مَن ذَا ٱلَّذِى يَشْفَعُ عِندَهُۥٓ إِلَّا بِإِذْنِهِۦ ۚ يَعْلَمُ مَا بَيْنَ أَيْدِيهِمْ وَمَا خَلْفَهُمْ ۖ وَلَا يُحِيطُونَ بِشَىْءٍۢ مِّنْ عِلْمِهِۦٓ إِلَّا بِمَا شَآءَ ۚ وَسِعَ كُرْسِيُّهُ ٱلسَّمَٰوَٰتِ وَٱلْأَرْضَ ۖ وَلَا يَـُٔودُهُۥ حِفْظُهُمَا ۚ وَهُوَ ٱلْعَلِىُّ ٱلْعَظِيمُ",
edition: {
identifier: "quran-uthmani",
language: "ar",
name: "Uthamani",
englishName: "Uthamani",
format: "text",
type: "quran"
},
surah: {
number: 2,
name: "سورة البقرة",
englishName: "Al-Baqara",
englishNameTranslation: "The Cow",
numberOfAyahs: 286,
revelationType: "Medinan"
},
numberInSurah: 255,
juz: 3,
manzil: 1,
page: 42,
ruku: 35,
hizbQuarter: 17,
sajda: false
},
{
number: 262,
text: "Allah! There is no deity save Him, the Alive, the Eternal. Neither slumber nor sleep overtaketh Him. Unto Him belongeth whatsoever is in the heavens and whatsoever is in the earth. Who is he that intercedeth with Him save by His leave? He knoweth that which is in front of them and that which is behind them, while they encompass nothing of His knowledge save what He will. His throne includeth the heavens and the earth, and He is never weary of preserving them. He is the Sublime, the Tremendous.",
edition: {
identifier: "en.pickthall",
language: "en",
name: "Pickthall",
englishName: "Mohammed Marmaduke William Pickthall",
format: "text",
type: "translation"
},
surah: {
number: 2,
name: "سورة البقرة",
englishName: "Al-Baqara",
englishNameTranslation: "The Cow",
numberOfAyahs: 286,
revelationType: "Medinan"
},
numberInSurah: 255,
juz: 3,
manzil: 1,
page: 42,
ruku: 35,
hizbQuarter: 17,
sajda: false
}
]
}

```

Studying the above structure, I could figure out the following three key items I wanted:

```python
verse_a = json_data['data'][0]['text']
verse_en = json_data['data'][1]['text']
sura = json_data['data'][0]['surah']['englishName']+\
           '('+str(json_data['data'][0]['surah']['number'])+'):'+\
           str(json_data['data'][0]['numberInSurah'])

```
I am using `request.get(url).json()` to get the API data as json file. Then used the proper dictionary traverse notations to head for the data I wanted. 

I used a `while` loop that continues to wait for input until `q` is pressed. 

There are lots of room to improve this code, but I will leave that for you. One natural extension is to move from raw code into a web app using for example `flask` framework, which I might try at some point. 

Besides getting wisdoms from these verses, there could be many other use case for this app. For example, say there is a Quranic memorization competition, then this app can seed the questions. The competitor need to continue from whatever random verse was thrown to him/her. 

Similarly, a genius who claims to memorize sura and verse number can be tested through this app. What other innovative ideas you have? Drop them in the comment below. 

Here is my entire code, try it by pressing the green run button and pray for me!

<iframe height="600px" width="100%" src="https://repl.it/@baqi/Random-Verses-from-the-Quran?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>

