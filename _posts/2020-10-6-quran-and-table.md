---
layout: post
title: Exploring Quran Dataset on Kaggle
subtitle: first steps using Table of datascience package
bigimg: /img/code.jpeg
image: img/kaba.jpg
share-img: img/kaba.jpg
tags: python technology data quran datascience
---
In this Tutorial, I am going to experiment with the `Table` class under the `Datascience` module for some exploratory analysis of the Quran. Let us get started.

First, I used a [Quran](https://www.kaggle.com/hammaadali/quran-clean-without-araab) database already available under Kaggle and brought it under my folder. 

below is the standard Kaggle prefix.

```python
# This Python 3 environment comes with many helpful analytics libraries installed
# It is defined by the kaggle/python Docker image: https://github.com/kaggle/docker-python
# For example, here's several helpful packages to load

import numpy as np # linear algebra
import pandas as pd # data processing, CSV file I/O (e.g. pd.read_csv)

# Input data files are available in the read-only "../input/" directory
# For example, running this (by clicking run or pressing Shift+Enter) will list all files under the input directory

import os
for dirname, _, filenames in os.walk('/kaggle/input'):
    for filename in filenames:
        print(os.path.join(dirname, filename))

# You can write up to 5GB to the current directory (/kaggle/working/) that gets preserved as output when you create a version using "Save & Run All" 
# You can also write temporary files to /kaggle/temp/, but they won't be saved outside of the current session
```

    /kaggle/input/quran-makki-madani/quran-toc.csv
    /kaggle/input/qurancsv/Quran.csv
    /kaggle/input/quran-clean-without-araab/Quran-clean-without-aarab.csv


Next, I have to install the `datascience` module using `pip`. Full documentation of this module can be found [here](http://data8.org/datascience/).

```python
!pip install datascience
```
I have ignored the long output of executing the `pip` function above. So, now let us bring the use of the `Table` module under the datascience package.

```python
from datascience import *
```
Here is the first step, reading the table using `read_table` method.

```python
q = Table.read_table('/kaggle/input/quran-clean-without-araab/Quran-clean-without-aarab.csv')
```

```
q
```


<table border="1" class="dataframe">
    <thead>
        <tr>
            <th>Unnamed: 0</th> <th>SurahNum</th> <th>AyahNum</th> <th>Ayah</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0         </td> <td>1       </td> <td>1      </td> <td>بسم الله الرحمن الرحيم                                </td>
        </tr>
        <tr>
            <td>1         </td> <td>1       </td> <td>2      </td> <td>الحمد لله رب العالمين                                 </td>
        </tr>
        <tr>
            <td>2         </td> <td>1       </td> <td>3      </td> <td>الرحمن الرحيم                                         </td>
        </tr>
        <tr>
            <td>3         </td> <td>1       </td> <td>4      </td> <td>مالك يوم الدين                                        </td>
        </tr>
        <tr>
            <td>4         </td> <td>1       </td> <td>5      </td> <td>إياك نعبد وإياك نستعين                                </td>
        </tr>
        <tr>
            <td>5         </td> <td>1       </td> <td>6      </td> <td>اهدنا الصراط المستقيم                                 </td>
        </tr>
        <tr>
            <td>6         </td> <td>1       </td> <td>7      </td> <td>صراط الذين أنعمت عليهم غير المغضوب عليهم ولا الضالين  </td>
        </tr>
        <tr>
            <td>7         </td> <td>2       </td> <td>1      </td> <td>بسم الله الرحمن الرحيم الم                            </td>
        </tr>
        <tr>
            <td>8         </td> <td>2       </td> <td>2      </td> <td>ذلك الكتاب لا ريب فيه هدى للمتقين                     </td>
        </tr>
        <tr>
            <td>9         </td> <td>2       </td> <td>3      </td> <td>الذين يؤمنون بالغيب ويقيمون الصلاة ومما رزقناهم ينفقون</td>
        </tr>
    </tbody>
</table>
<p>... (6226 rows omitted)</p>


With `Table` we can show certain number of rows from top, for example

```python
q.show(3)
```

<table border="1" class="dataframe">
    <thead>
        <tr>
            <th>Unnamed: 0</th> <th>SurahNum</th> <th>AyahNum</th> <th>Ayah</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0         </td> <td>1       </td> <td>1      </td> <td>بسم الله الرحمن الرحيم</td>
        </tr>
        <tr>
            <td>1         </td> <td>1       </td> <td>2      </td> <td>الحمد لله رب العالمين </td>
        </tr>
        <tr>
            <td>2         </td> <td>1       </td> <td>3      </td> <td>الرحمن الرحيم         </td>
        </tr>
    </tbody>
</table>
<p>... (6233 rows omitted)</p>

Similarly we can selec certain columns only

```python
q.select('Ayah')
```

<table border="1" class="dataframe">
    <thead>
        <tr>
            <th>Ayah</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>بسم الله الرحمن الرحيم                                </td>
        </tr>
        <tr>
            <td>الحمد لله رب العالمين                                 </td>
        </tr>
        <tr>
            <td>الرحمن الرحيم                                         </td>
        </tr>
        <tr>
            <td>مالك يوم الدين                                        </td>
        </tr>
        <tr>
            <td>إياك نعبد وإياك نستعين                                </td>
        </tr>
        <tr>
            <td>اهدنا الصراط المستقيم                                 </td>
        </tr>
        <tr>
            <td>صراط الذين أنعمت عليهم غير المغضوب عليهم ولا الضالين  </td>
        </tr>
        <tr>
            <td>بسم الله الرحمن الرحيم الم                            </td>
        </tr>
        <tr>
            <td>ذلك الكتاب لا ريب فيه هدى للمتقين                     </td>
        </tr>
        <tr>
            <td>الذين يؤمنون بالغيب ويقيمون الصلاة ومما رزقناهم ينفقون</td>
        </tr>
    </tbody>
</table>
<p>... (6226 rows omitted)</p>

How many rows are there in this quranic table. In other words, how many total Ayaat are there in the Quran?

```python
q.num_rows
```
    6236

let us see the last row

```python
q.row(q.num_rows-1)
```

    Row(Unnamed: 0=6235, SurahNum=114, AyahNum=6, Ayah='من الجنة والناس')


## Counting Words

Let us see how you can `apply` a function to each row of the Table.

Here is a function that given a sentence will return the number of words in that sentence.

```python
def count_words(item):
    return len(item.split())
```
Thus, we can `apply` a function on a table as follows

```python
q.apply(count_words, 'Ayah')
```
    array([4, 4, 2, ..., 4, 5, 3])

given this we can write the following handy code,and assign the result to a new table called `qwc` meaning quran with word counts.

```python
qwc = q.with_columns("words", q.apply(count_words, 'Ayah'))
qwc
```

<table border="1" class="dataframe">
    <thead>
        <tr>
            <th>Unnamed: 0</th> <th>SurahNum</th> <th>AyahNum</th> <th>Ayah</th> <th>words</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0         </td> <td>1       </td> <td>1      </td> <td>بسم الله الرحمن الرحيم                                </td> <td>4    </td>
        </tr>
        <tr>
            <td>1         </td> <td>1       </td> <td>2      </td> <td>الحمد لله رب العالمين                                 </td> <td>4    </td>
        </tr>
        <tr>
            <td>2         </td> <td>1       </td> <td>3      </td> <td>الرحمن الرحيم                                         </td> <td>2    </td>
        </tr>
        <tr>
            <td>3         </td> <td>1       </td> <td>4      </td> <td>مالك يوم الدين                                        </td> <td>3    </td>
        </tr>
        <tr>
            <td>4         </td> <td>1       </td> <td>5      </td> <td>إياك نعبد وإياك نستعين                                </td> <td>4    </td>
        </tr>
        <tr>
            <td>5         </td> <td>1       </td> <td>6      </td> <td>اهدنا الصراط المستقيم                                 </td> <td>3    </td>
        </tr>
        <tr>
            <td>6         </td> <td>1       </td> <td>7      </td> <td>صراط الذين أنعمت عليهم غير المغضوب عليهم ولا الضالين  </td> <td>9    </td>
        </tr>
        <tr>
            <td>7         </td> <td>2       </td> <td>1      </td> <td>بسم الله الرحمن الرحيم الم                            </td> <td>5    </td>
        </tr>
        <tr>
            <td>8         </td> <td>2       </td> <td>2      </td> <td>ذلك الكتاب لا ريب فيه هدى للمتقين                     </td> <td>7    </td>
        </tr>
        <tr>
            <td>9         </td> <td>2       </td> <td>3      </td> <td>الذين يؤمنون بالغيب ويقيمون الصلاة ومما رزقناهم ينفقون</td> <td>8    </td>
        </tr>
    </tbody>
</table>
<p>... (6226 rows omitted)</p>


Let me rename the first coulumn from unnamed to SrNo

```python
qwc = qwc.relabeled(0,'SrNo')
qwc
```

<table border="1" class="dataframe">
    <thead>
        <tr>
            <th>SrNo</th> <th>SurahNum</th> <th>AyahNum</th> <th>Ayah</th> <th>words</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0   </td> <td>1       </td> <td>1      </td> <td>بسم الله الرحمن الرحيم                                </td> <td>4    </td>
        </tr>
        <tr>
            <td>1   </td> <td>1       </td> <td>2      </td> <td>الحمد لله رب العالمين                                 </td> <td>4    </td>
        </tr>
        <tr>
            <td>2   </td> <td>1       </td> <td>3      </td> <td>الرحمن الرحيم                                         </td> <td>2    </td>
        </tr>
        <tr>
            <td>3   </td> <td>1       </td> <td>4      </td> <td>مالك يوم الدين                                        </td> <td>3    </td>
        </tr>
        <tr>
            <td>4   </td> <td>1       </td> <td>5      </td> <td>إياك نعبد وإياك نستعين                                </td> <td>4    </td>
        </tr>
        <tr>
            <td>5   </td> <td>1       </td> <td>6      </td> <td>اهدنا الصراط المستقيم                                 </td> <td>3    </td>
        </tr>
        <tr>
            <td>6   </td> <td>1       </td> <td>7      </td> <td>صراط الذين أنعمت عليهم غير المغضوب عليهم ولا الضالين  </td> <td>9    </td>
        </tr>
        <tr>
            <td>7   </td> <td>2       </td> <td>1      </td> <td>بسم الله الرحمن الرحيم الم                            </td> <td>5    </td>
        </tr>
        <tr>
            <td>8   </td> <td>2       </td> <td>2      </td> <td>ذلك الكتاب لا ريب فيه هدى للمتقين                     </td> <td>7    </td>
        </tr>
        <tr>
            <td>9   </td> <td>2       </td> <td>3      </td> <td>الذين يؤمنون بالغيب ويقيمون الصلاة ومما رزقناهم ينفقون</td> <td>8    </td>
        </tr>
    </tbody>
</table>
<p>... (6226 rows omitted)</p>

```python
qwc.row(qwc.num_rows-1)
```

    Row(SrNo=6235, SurahNum=114, AyahNum=6, Ayah='من الجنة والناس', words=3)


## Find English translation

Notice that the datasource we got does not have English translation. But kaggle has other [datasources](https://www.kaggle.com/mohamedwaelbishr2018/qurancsv) that has English translations. Sounds like a good use case for `join`ing two tables. Let us do that.

```python
en = Table.read_table('/kaggle/input/qurancsv/Quran.csv')
en
```

<table border="1" class="dataframe">
    <thead>
        <tr>
            <th>SrNo</th> <th>JuzNo</th> <th>SurahNo</th> <th>AyahNo</th> <th>EnglishTranslation</th> <th>OrignalArabicText</th> <th>ArabicText</th> <th>ArabicWordCount</th> <th>ArabicLetterCount</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>1   </td> <td>1    </td> <td>1      </td> <td>1     </td> <td>In the name of Allah, Most Gracious, Most Merciful.         </td> <td>?????? ??????? ???????????? ??????????                      </td> <td>??? ???? ?????? ??????                                </td> <td>4              </td> <td>19               </td>
        </tr>
        <tr>
            <td>2   </td> <td>1    </td> <td>1      </td> <td>2     </td> <td>Praise be to Allah, the Cherisher and Sustainer of the w ...</td> <td>????????? ??????? ????? ?????????????                       </td> <td>????? ??? ?? ????????                                 </td> <td>4              </td> <td>18               </td>
        </tr>
        <tr>
            <td>3   </td> <td>1    </td> <td>1      </td> <td>3     </td> <td>Most Gracious, Most Merciful;                               </td> <td>???????????? ??????????                                     </td> <td>?????? ??????                                         </td> <td>2              </td> <td>12               </td>
        </tr>
        <tr>
            <td>4   </td> <td>1    </td> <td>1      </td> <td>4     </td> <td>Master of the Day of Judgment.                              </td> <td>??????? ?????? ????????                                     </td> <td>???? ??? ?????                                        </td> <td>3              </td> <td>12               </td>
        </tr>
        <tr>
            <td>5   </td> <td>1    </td> <td>1      </td> <td>5     </td> <td>Thee do we worship, and Thine aid we seek.                  </td> <td>???????? ???????? ?????????? ???????????                    </td> <td>???? ???? ????? ??????                                </td> <td>4              </td> <td>19               </td>
        </tr>
        <tr>
            <td>6   </td> <td>1    </td> <td>1      </td> <td>6     </td> <td>Show us the straight way,                                   </td> <td>???????? ?????????? ??????????????                          </td> <td>????? ?????? ????????                                 </td> <td>3              </td> <td>19               </td>
        </tr>
        <tr>
            <td>7   </td> <td>1    </td> <td>1      </td> <td>7     </td> <td>The way of those on whom Thou hast bestowed Thy Grace, t ...</td> <td>??????? ????????? ?????????? ?????????? ?????? ????????? ...</td> <td>???? ????? ????? ????? ??? ??????? ????? ??? ???????  </td> <td>9              </td> <td>44               </td>
        </tr>
        <tr>
            <td>8   </td> <td>1    </td> <td>2      </td> <td>1     </td> <td>A. L. M.                                                    </td> <td>???                                                         </td> <td>???                                                   </td> <td>1              </td> <td>3                </td>
        </tr>
        <tr>
            <td>9   </td> <td>1    </td> <td>2      </td> <td>2     </td> <td>This is the Book; in it is guidance sure, without doubt, ...</td> <td>??????? ?????????? ??? ?????? ? ????? ? ????? ??????????????</td> <td>??? ?????? ?? ??? ? ??? ? ???? ???????                </td> <td>9              </td> <td>30               </td>
        </tr>
        <tr>
            <td>10  </td> <td>1    </td> <td>2      </td> <td>3     </td> <td>Who believe in the Unseen, are steadfast in prayer, and  ...</td> <td>????????? ??????????? ??????????? ???????????? ????????? ...</td> <td>????? ?????? ?????? ??????? ?????? ???? ??????? ??????</td> <td>8              </td> <td>47               </td>
        </tr>
    </tbody>
</table>
<p>... (6226 rows omitted)</p>

As you can see, we have the English translation under a column called "EnglishTranslation", here is how to relabel the filed (which is the fourth one if you count from zero) to a shorter column name.

```python
en = en.relabeled(4, 'en')
en
```

<table border="1" class="dataframe">
    <thead>
        <tr>
            <th>SrNo</th> <th>JuzNo</th> <th>SurahNo</th> <th>AyahNo</th> <th>en</th> <th>OrignalArabicText</th> <th>ArabicText</th> <th>ArabicWordCount</th> <th>ArabicLetterCount</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>1   </td> <td>1    </td> <td>1      </td> <td>1     </td> <td>In the name of Allah, Most Gracious, Most Merciful.         </td> <td>?????? ??????? ???????????? ??????????                      </td> <td>??? ???? ?????? ??????                                </td> <td>4              </td> <td>19               </td>
        </tr>
        <tr>
            <td>2   </td> <td>1    </td> <td>1      </td> <td>2     </td> <td>Praise be to Allah, the Cherisher and Sustainer of the w ...</td> <td>????????? ??????? ????? ?????????????                       </td> <td>????? ??? ?? ????????                                 </td> <td>4              </td> <td>18               </td>
        </tr>
        <tr>
            <td>3   </td> <td>1    </td> <td>1      </td> <td>3     </td> <td>Most Gracious, Most Merciful;                               </td> <td>???????????? ??????????                                     </td> <td>?????? ??????                                         </td> <td>2              </td> <td>12               </td>
        </tr>
        <tr>
            <td>4   </td> <td>1    </td> <td>1      </td> <td>4     </td> <td>Master of the Day of Judgment.                              </td> <td>??????? ?????? ????????                                     </td> <td>???? ??? ?????                                        </td> <td>3              </td> <td>12               </td>
        </tr>
        <tr>
            <td>5   </td> <td>1    </td> <td>1      </td> <td>5     </td> <td>Thee do we worship, and Thine aid we seek.                  </td> <td>???????? ???????? ?????????? ???????????                    </td> <td>???? ???? ????? ??????                                </td> <td>4              </td> <td>19               </td>
        </tr>
        <tr>
            <td>6   </td> <td>1    </td> <td>1      </td> <td>6     </td> <td>Show us the straight way,                                   </td> <td>???????? ?????????? ??????????????                          </td> <td>????? ?????? ????????                                 </td> <td>3              </td> <td>19               </td>
        </tr>
        <tr>
            <td>7   </td> <td>1    </td> <td>1      </td> <td>7     </td> <td>The way of those on whom Thou hast bestowed Thy Grace, t ...</td> <td>??????? ????????? ?????????? ?????????? ?????? ????????? ...</td> <td>???? ????? ????? ????? ??? ??????? ????? ??? ???????  </td> <td>9              </td> <td>44               </td>
        </tr>
        <tr>
            <td>8   </td> <td>1    </td> <td>2      </td> <td>1     </td> <td>A. L. M.                                                    </td> <td>???                                                         </td> <td>???                                                   </td> <td>1              </td> <td>3                </td>
        </tr>
        <tr>
            <td>9   </td> <td>1    </td> <td>2      </td> <td>2     </td> <td>This is the Book; in it is guidance sure, without doubt, ...</td> <td>??????? ?????????? ??? ?????? ? ????? ? ????? ??????????????</td> <td>??? ?????? ?? ??? ? ??? ? ???? ???????                </td> <td>9              </td> <td>30               </td>
        </tr>
        <tr>
            <td>10  </td> <td>1    </td> <td>2      </td> <td>3     </td> <td>Who believe in the Unseen, are steadfast in prayer, and  ...</td> <td>????????? ??????????? ??????????? ???????????? ????????? ...</td> <td>????? ?????? ?????? ??????? ?????? ???? ??????? ??????</td> <td>8              </td> <td>47               </td>
        </tr>
    </tbody>
</table>
<p>... (6226 rows omitted)</p>


```python
en = en.select(['SrNo', 'en'])
en
```

<table border="1" class="dataframe">
    <thead>
        <tr>
            <th>SrNo</th> <th>en</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>1   </td> <td>In the name of Allah, Most Gracious, Most Merciful.         </td>
        </tr>
        <tr>
            <td>2   </td> <td>Praise be to Allah, the Cherisher and Sustainer of the w ...</td>
        </tr>
        <tr>
            <td>3   </td> <td>Most Gracious, Most Merciful;                               </td>
        </tr>
        <tr>
            <td>4   </td> <td>Master of the Day of Judgment.                              </td>
        </tr>
        <tr>
            <td>5   </td> <td>Thee do we worship, and Thine aid we seek.                  </td>
        </tr>
        <tr>
            <td>6   </td> <td>Show us the straight way,                                   </td>
        </tr>
        <tr>
            <td>7   </td> <td>The way of those on whom Thou hast bestowed Thy Grace, t ...</td>
        </tr>
        <tr>
            <td>8   </td> <td>A. L. M.                                                    </td>
        </tr>
        <tr>
            <td>9   </td> <td>This is the Book; in it is guidance sure, without doubt, ...</td>
        </tr>
        <tr>
            <td>10  </td> <td>Who believe in the Unseen, are steadfast in prayer, and  ...</td>
        </tr>
    </tbody>
</table>
<p>... (6226 rows omitted)</p>


I have noticed a problem while checking the 'SrNo' of both datasets. The first dataset `qwc` has SrNo starting from Zero, while the second `en` starts from 1. So, let us create a new column `sr` in the second dataset so we have similar ID value to join both tables. 

```python
en = en.with_columns('sr', en.column('SrNo')-1).drop('SrNo')
en
```

<table border="1" class="dataframe">
    <thead>
        <tr>
            <th>en</th> <th>sr</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>In the name of Allah, Most Gracious, Most Merciful.         </td> <td>0   </td>
        </tr>
        <tr>
            <td>Praise be to Allah, the Cherisher and Sustainer of the w ...</td> <td>1   </td>
        </tr>
        <tr>
            <td>Most Gracious, Most Merciful;                               </td> <td>2   </td>
        </tr>
        <tr>
            <td>Master of the Day of Judgment.                              </td> <td>3   </td>
        </tr>
        <tr>
            <td>Thee do we worship, and Thine aid we seek.                  </td> <td>4   </td>
        </tr>
        <tr>
            <td>Show us the straight way,                                   </td> <td>5   </td>
        </tr>
        <tr>
            <td>The way of those on whom Thou hast bestowed Thy Grace, t ...</td> <td>6   </td>
        </tr>
        <tr>
            <td>A. L. M.                                                    </td> <td>7   </td>
        </tr>
        <tr>
            <td>This is the Book; in it is guidance sure, without doubt, ...</td> <td>8   </td>
        </tr>
        <tr>
            <td>Who believe in the Unseen, are steadfast in prayer, and  ...</td> <td>9   </td>
        </tr>
    </tbody>
</table>
<p>... (6226 rows omitted)</p>


```python
en.row(en.num_rows-1)
```

    Row(en='Among Jinns and among men.', sr=6235)

Everything seems in place. So, let us join. It works as follows:
Join the first table `qwc` using the `SrNo` as the ID with the corresponding table `en` which has the same ID under column `sr`. This will append all remaining columns of the second table `en` to the new table which I chose to name it as `quran`.

```python
quran = qwc.join('SrNo', en, 'sr')
quran
```

<table border="1" class="dataframe">
    <thead>
        <tr>
            <th>SrNo</th> <th>SurahNum</th> <th>AyahNum</th> <th>Ayah</th> <th>words</th> <th>en</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0   </td> <td>1       </td> <td>1      </td> <td>بسم الله الرحمن الرحيم                                </td> <td>4    </td> <td>In the name of Allah, Most Gracious, Most Merciful.         </td>
        </tr>
        <tr>
            <td>1   </td> <td>1       </td> <td>2      </td> <td>الحمد لله رب العالمين                                 </td> <td>4    </td> <td>Praise be to Allah, the Cherisher and Sustainer of the w ...</td>
        </tr>
        <tr>
            <td>2   </td> <td>1       </td> <td>3      </td> <td>الرحمن الرحيم                                         </td> <td>2    </td> <td>Most Gracious, Most Merciful;                               </td>
        </tr>
        <tr>
            <td>3   </td> <td>1       </td> <td>4      </td> <td>مالك يوم الدين                                        </td> <td>3    </td> <td>Master of the Day of Judgment.                              </td>
        </tr>
        <tr>
            <td>4   </td> <td>1       </td> <td>5      </td> <td>إياك نعبد وإياك نستعين                                </td> <td>4    </td> <td>Thee do we worship, and Thine aid we seek.                  </td>
        </tr>
        <tr>
            <td>5   </td> <td>1       </td> <td>6      </td> <td>اهدنا الصراط المستقيم                                 </td> <td>3    </td> <td>Show us the straight way,                                   </td>
        </tr>
        <tr>
            <td>6   </td> <td>1       </td> <td>7      </td> <td>صراط الذين أنعمت عليهم غير المغضوب عليهم ولا الضالين  </td> <td>9    </td> <td>The way of those on whom Thou hast bestowed Thy Grace, t ...</td>
        </tr>
        <tr>
            <td>7   </td> <td>2       </td> <td>1      </td> <td>بسم الله الرحمن الرحيم الم                            </td> <td>5    </td> <td>A. L. M.                                                    </td>
        </tr>
        <tr>
            <td>8   </td> <td>2       </td> <td>2      </td> <td>ذلك الكتاب لا ريب فيه هدى للمتقين                     </td> <td>7    </td> <td>This is the Book; in it is guidance sure, without doubt, ...</td>
        </tr>
        <tr>
            <td>9   </td> <td>2       </td> <td>3      </td> <td>الذين يؤمنون بالغيب ويقيمون الصلاة ومما رزقناهم ينفقون</td> <td>8    </td> <td>Who believe in the Unseen, are steadfast in prayer, and  ...</td>
        </tr>
    </tbody>
</table>
<p>... (6226 rows omitted)</p>

for convinience let us relable further some long fileds.

```python
quran = quran.relabeled(1, 'sno').relabeled(2,'vno')
quran
```
<table border="1" class="dataframe">
    <thead>
        <tr>
            <th>SrNo</th> <th>sno</th> <th>vno</th> <th>Ayah</th> <th>words</th> <th>en</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0   </td> <td>1   </td> <td>1   </td> <td>بسم الله الرحمن الرحيم                                </td> <td>4    </td> <td>In the name of Allah, Most Gracious, Most Merciful.         </td>
        </tr>
        <tr>
            <td>1   </td> <td>1   </td> <td>2   </td> <td>الحمد لله رب العالمين                                 </td> <td>4    </td> <td>Praise be to Allah, the Cherisher and Sustainer of the w ...</td>
        </tr>
        <tr>
            <td>2   </td> <td>1   </td> <td>3   </td> <td>الرحمن الرحيم                                         </td> <td>2    </td> <td>Most Gracious, Most Merciful;                               </td>
        </tr>
        <tr>
            <td>3   </td> <td>1   </td> <td>4   </td> <td>مالك يوم الدين                                        </td> <td>3    </td> <td>Master of the Day of Judgment.                              </td>
        </tr>
        <tr>
            <td>4   </td> <td>1   </td> <td>5   </td> <td>إياك نعبد وإياك نستعين                                </td> <td>4    </td> <td>Thee do we worship, and Thine aid we seek.                  </td>
        </tr>
        <tr>
            <td>5   </td> <td>1   </td> <td>6   </td> <td>اهدنا الصراط المستقيم                                 </td> <td>3    </td> <td>Show us the straight way,                                   </td>
        </tr>
        <tr>
            <td>6   </td> <td>1   </td> <td>7   </td> <td>صراط الذين أنعمت عليهم غير المغضوب عليهم ولا الضالين  </td> <td>9    </td> <td>The way of those on whom Thou hast bestowed Thy Grace, t ...</td>
        </tr>
        <tr>
            <td>7   </td> <td>2   </td> <td>1   </td> <td>بسم الله الرحمن الرحيم الم                            </td> <td>5    </td> <td>A. L. M.                                                    </td>
        </tr>
        <tr>
            <td>8   </td> <td>2   </td> <td>2   </td> <td>ذلك الكتاب لا ريب فيه هدى للمتقين                     </td> <td>7    </td> <td>This is the Book; in it is guidance sure, without doubt, ...</td>
        </tr>
        <tr>
            <td>9   </td> <td>2   </td> <td>3   </td> <td>الذين يؤمنون بالغيب ويقيمون الصلاة ومما رزقناهم ينفقون</td> <td>8    </td> <td>Who believe in the Unseen, are steadfast in prayer, and  ...</td>
        </tr>
    </tbody>
</table>
<p>... (6226 rows omitted)</p>

## Display a verse

So, let us see how to find verse 2:255 in the quran?

```python
quran.where('sno', 2).where('vno',255)
```

<table border="1" class="dataframe">
    <thead>
        <tr>
            <th>SrNo</th> <th>sno</th> <th>vno</th> <th>Ayah</th> <th>words</th> <th>en</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>261 </td> <td>2   </td> <td>255 </td> <td>الله لا إله إلا هو الحي القيوم لا تأخذه سنة ولا نوم له م ...</td> <td>50   </td> <td>Allah! There is no god but He,-the Living, the Self-subs ...</td>
        </tr>
    </tbody>
</table>


another example: find me first verses of all surahs.

```python
quran.where('vno', 1)
```
<table border="1" class="dataframe">
    <thead>
        <tr>
            <th>SrNo</th> <th>sno</th> <th>vno</th> <th>Ayah</th> <th>words</th> <th>en</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0   </td> <td>1   </td> <td>1   </td> <td>بسم الله الرحمن الرحيم                                      </td> <td>4    </td> <td>In the name of Allah, Most Gracious, Most Merciful.         </td>
        </tr>
        <tr>
            <td>7   </td> <td>2   </td> <td>1   </td> <td>بسم الله الرحمن الرحيم الم                                  </td> <td>5    </td> <td>A. L. M.                                                    </td>
        </tr>
        <tr>
            <td>293 </td> <td>3   </td> <td>1   </td> <td>بسم الله الرحمن الرحيم الم                                  </td> <td>5    </td> <td>A. L. M.                                                    </td>
        </tr>
        <tr>
            <td>493 </td> <td>4   </td> <td>1   </td> <td>بسم الله الرحمن الرحيم يا أيها الناس اتقوا ربكم الذي خلق ...</td> <td>33   </td> <td>O mankind! reverence your Guardian-Lord, who created you ...</td>
        </tr>
        <tr>
            <td>669 </td> <td>5   </td> <td>1   </td> <td>بسم الله الرحمن الرحيم يا أيها الذين آمنوا أوفوا بالعقود ...</td> <td>28   </td> <td>O ye who believe! fulfil (all) obligations. Lawful unto  ...</td>
        </tr>
        <tr>
            <td>789 </td> <td>6   </td> <td>1   </td> <td>بسم الله الرحمن الرحيم الحمد لله الذي خلق السماوات والأر ...</td> <td>18   </td> <td>Praise be Allah, Who created the heavens and the earth,  ...</td>
        </tr>
        <tr>
            <td>954 </td> <td>7   </td> <td>1   </td> <td>بسم الله الرحمن الرحيم المص                                 </td> <td>5    </td> <td>Alif, Lam, Mim, Sad.                                        </td>
        </tr>
        <tr>
            <td>1160</td> <td>8   </td> <td>1   </td> <td>بسم الله الرحمن الرحيم يسألونك عن الأنفال قل الأنفال لله ...</td> <td>22   </td> <td>They ask thee concerning (things taken as) spoils of war ...</td>
        </tr>
        <tr>
            <td>1235</td> <td>9   </td> <td>1   </td> <td>براءة من الله ورسوله إلى الذين عاهدتم من المشركين           </td> <td>9    </td> <td>A (declaration) of immunity from Allah and His Messenger ...</td>
        </tr>
        <tr>
            <td>1364</td> <td>10  </td> <td>1   </td> <td>بسم الله الرحمن الرحيم الر تلك آيات الكتاب الحكيم           </td> <td>9    </td> <td>A. L. R. These are the ayats of the Book of Wisdom.         </td>
        </tr>
    </tbody>
</table>
<p>... (104 rows omitted)</p>


## Conditional selectivity

With the above example, we need to be mindful of the fact that the Arabic text in this dataset includes the `bismillah` as the first part of the verse no. 1 of each surah, whereas the English translation does not do that. This have implications on the count of words. 

So, let us adjust the `words` count of the first verse of each surah by discounting 4 words (which is the count of words in Basmalah). But the last caviat is that we do not want to do this discounting on the first chapter of the Quran, because as we know the first verse of Sura Fatiha is a basmalah.

The same goes for Surah at-Tawbah which does not have the basmalah at the start, and hence it should not be discounted. 

So, let us start defining a function that does that exactly.

```python
def discount_basmalah(sura_no, verse_no, words):
    if (sura_no==1 or sura_no==9):
        return words
    if (verse_no == 1):
        return words-4
    else:
        return words
```

all it matters now is to apply that function to each row of the quran.

```python
quran = quran.with_columns('wc', quran.apply(discount_basmalah,'sno','vno', 'words'))
quran
```

<table border="1" class="dataframe">
    <thead>
        <tr>
            <th>SrNo</th> <th>sno</th> <th>vno</th> <th>Ayah</th> <th>words</th> <th>en</th> <th>wc</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0   </td> <td>1   </td> <td>1   </td> <td>بسم الله الرحمن الرحيم                                </td> <td>4    </td> <td>In the name of Allah, Most Gracious, Most Merciful.         </td> <td>4   </td>
        </tr>
        <tr>
            <td>1   </td> <td>1   </td> <td>2   </td> <td>الحمد لله رب العالمين                                 </td> <td>4    </td> <td>Praise be to Allah, the Cherisher and Sustainer of the w ...</td> <td>4   </td>
        </tr>
        <tr>
            <td>2   </td> <td>1   </td> <td>3   </td> <td>الرحمن الرحيم                                         </td> <td>2    </td> <td>Most Gracious, Most Merciful;                               </td> <td>2   </td>
        </tr>
        <tr>
            <td>3   </td> <td>1   </td> <td>4   </td> <td>مالك يوم الدين                                        </td> <td>3    </td> <td>Master of the Day of Judgment.                              </td> <td>3   </td>
        </tr>
        <tr>
            <td>4   </td> <td>1   </td> <td>5   </td> <td>إياك نعبد وإياك نستعين                                </td> <td>4    </td> <td>Thee do we worship, and Thine aid we seek.                  </td> <td>4   </td>
        </tr>
        <tr>
            <td>5   </td> <td>1   </td> <td>6   </td> <td>اهدنا الصراط المستقيم                                 </td> <td>3    </td> <td>Show us the straight way,                                   </td> <td>3   </td>
        </tr>
        <tr>
            <td>6   </td> <td>1   </td> <td>7   </td> <td>صراط الذين أنعمت عليهم غير المغضوب عليهم ولا الضالين  </td> <td>9    </td> <td>The way of those on whom Thou hast bestowed Thy Grace, t ...</td> <td>9   </td>
        </tr>
        <tr>
            <td>7   </td> <td>2   </td> <td>1   </td> <td>بسم الله الرحمن الرحيم الم                            </td> <td>5    </td> <td>A. L. M.                                                    </td> <td>1   </td>
        </tr>
        <tr>
            <td>8   </td> <td>2   </td> <td>2   </td> <td>ذلك الكتاب لا ريب فيه هدى للمتقين                     </td> <td>7    </td> <td>This is the Book; in it is guidance sure, without doubt, ...</td> <td>7   </td>
        </tr>
        <tr>
            <td>9   </td> <td>2   </td> <td>3   </td> <td>الذين يؤمنون بالغيب ويقيمون الصلاة ومما رزقناهم ينفقون</td> <td>8    </td> <td>Who believe in the Unseen, are steadfast in prayer, and  ...</td> <td>8   </td>
        </tr>
    </tbody>
</table>
<p>... (6226 rows omitted)</p>


## Makki or Madani

Let us bring in another interesting table about the place of revelation, i.e., Makki or Madani. I have brought in the [data](https://www.kaggle.com/abdulbaqi/quran-makki-madani), so let us join it with out table.

```python
q_place = Table.read_table('/kaggle/input/quran-makki-madani/quran-toc.csv')
q_place
```

<table border="1" class="dataframe">
    <thead>
        <tr>
            <th>No.</th> <th>Name Arabic</th> <th>Name</th> <th>English Meaning</th> <th>No of verses</th> <th>Place</th> <th>Chronology</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>1   </td> <td>الفاتحة    </td> <td>Al-Fatiha</td> <td>The Opening         </td> <td>7           </td> <td>Meccan </td> <td>5         </td>
        </tr>
        <tr>
            <td>2   </td> <td>البقرة     </td> <td>Al-Baqara</td> <td>The Cow             </td> <td>286         </td> <td>Medinan</td> <td>87        </td>
        </tr>
        <tr>
            <td>3   </td> <td>آل عمران   </td> <td>Al Imran </td> <td>The House of Joachim</td> <td>200         </td> <td>Medinan</td> <td>89        </td>
        </tr>
        <tr>
            <td>4   </td> <td>النساء     </td> <td>An-Nisa' </td> <td>Women               </td> <td>176         </td> <td>Medinan</td> <td>92        </td>
        </tr>
        <tr>
            <td>5   </td> <td>المائدة    </td> <td>Al-Ma'ida</td> <td>The Table Spread    </td> <td>120         </td> <td>Medinan</td> <td>112       </td>
        </tr>
        <tr>
            <td>6   </td> <td>الأنعام    </td> <td>Al-An'am </td> <td>Cattle              </td> <td>165         </td> <td>Meccan </td> <td>55        </td>
        </tr>
        <tr>
            <td>7   </td> <td>الأعراف    </td> <td>Al-A'raf </td> <td>The Heights         </td> <td>206         </td> <td>Meccan </td> <td>39        </td>
        </tr>
        <tr>
            <td>8   </td> <td>الأنفال    </td> <td>Al-Anfal </td> <td>Spoils of War       </td> <td>75          </td> <td>Medinan</td> <td>88        </td>
        </tr>
        <tr>
            <td>9   </td> <td>التوبة     </td> <td>At-Tawba </td> <td>Repentance          </td> <td>129         </td> <td>Medinan</td> <td>113       </td>
        </tr>
        <tr>
            <td>10  </td> <td>يونس       </td> <td>Yunus    </td> <td>Jonah               </td> <td>109         </td> <td>Meccan </td> <td>51        </td>
        </tr>
    </tbody>
</table>
<p>... (104 rows omitted)</p>

Let us just select the two columns that interest us when it comes to joining.

```python
q_select = q_place.select(['No.','Place'])
q_select
```

<table border="1" class="dataframe">
    <thead>
        <tr>
            <th>No.</th> <th>Place</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>1   </td> <td>Meccan </td>
        </tr>
        <tr>
            <td>2   </td> <td>Medinan</td>
        </tr>
        <tr>
            <td>3   </td> <td>Medinan</td>
        </tr>
        <tr>
            <td>4   </td> <td>Medinan</td>
        </tr>
        <tr>
            <td>5   </td> <td>Medinan</td>
        </tr>
        <tr>
            <td>6   </td> <td>Meccan </td>
        </tr>
        <tr>
            <td>7   </td> <td>Meccan </td>
        </tr>
        <tr>
            <td>8   </td> <td>Medinan</td>
        </tr>
        <tr>
            <td>9   </td> <td>Medinan</td>
        </tr>
        <tr>
            <td>10  </td> <td>Meccan </td>
        </tr>
    </tbody>
</table>
<p>... (104 rows omitted)</p>

Everything is ready, so let us do it.

```python
quran = quran.join('sno',q_select,'No.')
quran
```

<table border="1" class="dataframe">
    <thead>
        <tr>
            <th>sno</th> <th>SrNo</th> <th>vno</th> <th>Ayah</th> <th>words</th> <th>en</th> <th>wc</th> <th>Place</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>1   </td> <td>0   </td> <td>1   </td> <td>بسم الله الرحمن الرحيم                                </td> <td>4    </td> <td>In the name of Allah, Most Gracious, Most Merciful.         </td> <td>4   </td> <td>Meccan </td>
        </tr>
        <tr>
            <td>1   </td> <td>1   </td> <td>2   </td> <td>الحمد لله رب العالمين                                 </td> <td>4    </td> <td>Praise be to Allah, the Cherisher and Sustainer of the w ...</td> <td>4   </td> <td>Meccan </td>
        </tr>
        <tr>
            <td>1   </td> <td>2   </td> <td>3   </td> <td>الرحمن الرحيم                                         </td> <td>2    </td> <td>Most Gracious, Most Merciful;                               </td> <td>2   </td> <td>Meccan </td>
        </tr>
        <tr>
            <td>1   </td> <td>3   </td> <td>4   </td> <td>مالك يوم الدين                                        </td> <td>3    </td> <td>Master of the Day of Judgment.                              </td> <td>3   </td> <td>Meccan </td>
        </tr>
        <tr>
            <td>1   </td> <td>4   </td> <td>5   </td> <td>إياك نعبد وإياك نستعين                                </td> <td>4    </td> <td>Thee do we worship, and Thine aid we seek.                  </td> <td>4   </td> <td>Meccan </td>
        </tr>
        <tr>
            <td>1   </td> <td>5   </td> <td>6   </td> <td>اهدنا الصراط المستقيم                                 </td> <td>3    </td> <td>Show us the straight way,                                   </td> <td>3   </td> <td>Meccan </td>
        </tr>
        <tr>
            <td>1   </td> <td>6   </td> <td>7   </td> <td>صراط الذين أنعمت عليهم غير المغضوب عليهم ولا الضالين  </td> <td>9    </td> <td>The way of those on whom Thou hast bestowed Thy Grace, t ...</td> <td>9   </td> <td>Meccan </td>
        </tr>
        <tr>
            <td>2   </td> <td>7   </td> <td>1   </td> <td>بسم الله الرحمن الرحيم الم                            </td> <td>5    </td> <td>A. L. M.                                                    </td> <td>1   </td> <td>Medinan</td>
        </tr>
        <tr>
            <td>2   </td> <td>8   </td> <td>2   </td> <td>ذلك الكتاب لا ريب فيه هدى للمتقين                     </td> <td>7    </td> <td>This is the Book; in it is guidance sure, without doubt, ...</td> <td>7   </td> <td>Medinan</td>
        </tr>
        <tr>
            <td>2   </td> <td>9   </td> <td>3   </td> <td>الذين يؤمنون بالغيب ويقيمون الصلاة ومما رزقناهم ينفقون</td> <td>8    </td> <td>Who believe in the Unseen, are steadfast in prayer, and  ...</td> <td>8   </td> <td>Medinan</td>
        </tr>
    </tbody>
</table>
<p>... (6226 rows omitted)</p>

we can run a quick analysis of the verse lengths, by using the `sort` function.

```python
quran.sort('wc', descending=True)
```
<table border="1" class="dataframe">
    <thead>
        <tr>
            <th>sno</th> <th>SrNo</th> <th>vno</th> <th>Ayah</th> <th>words</th> <th>en</th> <th>wc</th> <th>Place</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>2   </td> <td>288 </td> <td>282 </td> <td>يا أيها الذين آمنوا إذا تداينتم بدين إلى أجل مسمى فاكتبو ...</td> <td>129  </td> <td>O ye who believe! When ye deal with each other, in trans ...</td> <td>129 </td> <td>Medinan</td>
        </tr>
        <tr>
            <td>4   </td> <td>504 </td> <td>12  </td> <td>ولكم نصف ما ترك أزواجكم إن لم يكن لهن ولد فإن كان لهن ول ...</td> <td>88   </td> <td>In what your wives leave, your share is a half, if they  ...</td> <td>88  </td> <td>Medinan</td>
        </tr>
        <tr>
            <td>24  </td> <td>2821</td> <td>31  </td> <td>وقل للمؤمنات يغضضن من أبصارهن ويحفظن فروجهن ولا يبدين زي ...</td> <td>78   </td> <td>And say to the believing women that they should lower th ...</td> <td>78  </td> <td>Medinan</td>
        </tr>
        <tr>
            <td>73  </td> <td>5494</td> <td>20  </td> <td>إن ربك يعلم أنك تقوم أدنى من ثلثي الليل ونصفه وثلثه وطائ ...</td> <td>78   </td> <td>Thy Lord doth know that thou standest forth (to prayer)  ...</td> <td>78  </td> <td>Meccan </td>
        </tr>
        <tr>
            <td>24  </td> <td>2851</td> <td>61  </td> <td>ليس على الأعمى حرج ولا على الأعرج حرج ولا على المريض حرج ...</td> <td>76   </td> <td>It is no fault in the blind nor in one born lame, nor in ...</td> <td>76  </td> <td>Medinan</td>
        </tr>
        <tr>
            <td>3   </td> <td>446 </td> <td>154 </td> <td>ثم أنزل عليكم من بعد الغم أمنة نعاسا يغشى طائفة منكم وطا ...</td> <td>75   </td> <td>After (the excitement) of the distress, He sent down cal ...</td> <td>75  </td> <td>Medinan</td>
        </tr>
        <tr>
            <td>2   </td> <td>108 </td> <td>102 </td> <td>واتبعوا ما تتلو الشياطين على ملك سليمان وما كفر سليمان و ...</td> <td>74   </td> <td>They followed what the evil ones gave out (falsely) agai ...</td> <td>74  </td> <td>Medinan</td>
        </tr>
        <tr>
            <td>2   </td> <td>202 </td> <td>196 </td> <td>وأتموا الحج والعمرة لله فإن أحصرتم فما استيسر من الهدي و ...</td> <td>73   </td> <td>And complete the Hajj or 'umra in the service of Allah.  ...</td> <td>73  </td> <td>Medinan</td>
        </tr>
        <tr>
            <td>4   </td> <td>503 </td> <td>11  </td> <td>يوصيكم الله في أولادكم للذكر مثل حظ الأنثيين فإن كن نساء ...</td> <td>71   </td> <td>Allah (thus) directs you as regards your Children's (Inh ...</td> <td>71  </td> <td>Medinan</td>
        </tr>
        <tr>
            <td>22  </td> <td>2599</td> <td>5   </td> <td>يا أيها الناس إن كنتم في ريب من البعث فإنا خلقناكم من تر ...</td> <td>70   </td> <td>O mankind! if ye have a doubt about the Resurrection, (c ...</td> <td>70  </td> <td>Medinan</td>
        </tr>
    </tbody>
</table>
<p>... (6226 rows omitted)</p>


The above tells us that Madani surahs are generally has bigger verse size, and that among the Meccan surahs the verse 20 or surah al-Muzzammil (sno. 73) is the largest among Meccan surah (and 4th largest in the Quran).

If we are interested to know the largest verses in only **Meccan** surah, then we apply the filter and then do the sort as follows.

```python
quran.where('Place', 'Meccan').sort('wc', descending=True)
```
<table border="1" class="dataframe">
    <thead>
        <tr>
            <th>sno</th> <th>SrNo</th> <th>vno</th> <th>Ayah</th> <th>words</th> <th>en</th> <th>wc</th> <th>Place</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>73  </td> <td>5494</td> <td>20  </td> <td>إن ربك يعلم أنك تقوم أدنى من ثلثي الليل ونصفه وثلثه وطائ ...</td> <td>78   </td> <td>Thy Lord doth know that thou standest forth (to prayer)  ...</td> <td>78  </td> <td>Meccan</td>
        </tr>
        <tr>
            <td>74  </td> <td>5525</td> <td>31  </td> <td>وما جعلنا أصحاب النار إلا ملائكة وما جعلنا عدتهم إلا فتن ...</td> <td>57   </td> <td>And We have set none but angels as Guardians of the Fire ...</td> <td>57  </td> <td>Meccan</td>
        </tr>
        <tr>
            <td>6   </td> <td>881 </td> <td>93  </td> <td>ومن أظلم ممن افترى على الله كذبا أو قال أوحي إلي ولم يوح ...</td> <td>49   </td> <td>Who can be more wicked than one who inventeth a lie agai ...</td> <td>49  </td> <td>Meccan</td>
        </tr>
        <tr>
            <td>12  </td> <td>1695</td> <td>100 </td> <td>ورفع أبويه على العرش وخروا له سجدا وقال يا أبت هذا تأويل ...</td> <td>47   </td> <td>And he raised his parents high on the throne (of dignity ...</td> <td>47  </td> <td>Meccan</td>
        </tr>
        <tr>
            <td>6   </td> <td>858 </td> <td>70  </td> <td>وذر الذين اتخذوا دينهم لعبا ولهوا وغرتهم الحياة الدنيا و ...</td> <td>45   </td> <td>Leave alone those who take their religion to be mere pla ...</td> <td>45  </td> <td>Meccan</td>
        </tr>
        <tr>
            <td>46  </td> <td>4524</td> <td>15  </td> <td>ووصينا الإنسان بوالديه إحسانا حملته أمه كرها ووضعته كرها ...</td> <td>45   </td> <td>We have enjoined on man kindness to his parents: In pain ...</td> <td>45  </td> <td>Meccan</td>
        </tr>
        <tr>
            <td>6   </td> <td>879 </td> <td>91  </td> <td>وما قدروا الله حق قدره إذ قالوا ما أنزل الله على بشر من  ...</td> <td>44   </td> <td>No just estimate of Allah do they make when they say: "N ...</td> <td>44  </td> <td>Meccan</td>
        </tr>
        <tr>
            <td>6   </td> <td>887 </td> <td>99  </td> <td>وهو الذي أنزل من السماء ماء فأخرجنا به نبات كل شيء فأخرج ...</td> <td>44   </td> <td>It is He Who sendeth down rain from the skies: with it W ...</td> <td>44  </td> <td>Meccan</td>
        </tr>
        <tr>
            <td>14  </td> <td>1771</td> <td>22  </td> <td>وقال الشيطان لما قضي الأمر إن الله وعدكم وعد الحق ووعدتك ...</td> <td>44   </td> <td>And Satan will say when the matter is decided: "It was A ...</td> <td>44  </td> <td>Meccan</td>
        </tr>
        <tr>
            <td>7   </td> <td>1110</td> <td>157 </td> <td>الذين يتبعون الرسول النبي الأمي الذي يجدونه مكتوبا عندهم ...</td> <td>43   </td> <td>"Those who follow the messenger, the unlettered Prophet, ...</td> <td>43  </td> <td>Meccan</td>
        </tr>
    </tbody>
</table>
<p>... (4603 rows omitted)</p>

The `words` column in the `quran` table seems redundent, so we can drop it.

```python
quran2 = quran.drop('words')
quran2
```

<table border="1" class="dataframe">
    <thead>
        <tr>
            <th>sno</th> <th>SrNo</th> <th>vno</th> <th>Ayah</th> <th>en</th> <th>wc</th> <th>Place</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>1   </td> <td>0   </td> <td>1   </td> <td>بسم الله الرحمن الرحيم                                </td> <td>In the name of Allah, Most Gracious, Most Merciful.         </td> <td>4   </td> <td>Meccan </td>
        </tr>
        <tr>
            <td>1   </td> <td>1   </td> <td>2   </td> <td>الحمد لله رب العالمين                                 </td> <td>Praise be to Allah, the Cherisher and Sustainer of the w ...</td> <td>4   </td> <td>Meccan </td>
        </tr>
        <tr>
            <td>1   </td> <td>2   </td> <td>3   </td> <td>الرحمن الرحيم                                         </td> <td>Most Gracious, Most Merciful;                               </td> <td>2   </td> <td>Meccan </td>
        </tr>
        <tr>
            <td>1   </td> <td>3   </td> <td>4   </td> <td>مالك يوم الدين                                        </td> <td>Master of the Day of Judgment.                              </td> <td>3   </td> <td>Meccan </td>
        </tr>
        <tr>
            <td>1   </td> <td>4   </td> <td>5   </td> <td>إياك نعبد وإياك نستعين                                </td> <td>Thee do we worship, and Thine aid we seek.                  </td> <td>4   </td> <td>Meccan </td>
        </tr>
        <tr>
            <td>1   </td> <td>5   </td> <td>6   </td> <td>اهدنا الصراط المستقيم                                 </td> <td>Show us the straight way,                                   </td> <td>3   </td> <td>Meccan </td>
        </tr>
        <tr>
            <td>1   </td> <td>6   </td> <td>7   </td> <td>صراط الذين أنعمت عليهم غير المغضوب عليهم ولا الضالين  </td> <td>The way of those on whom Thou hast bestowed Thy Grace, t ...</td> <td>9   </td> <td>Meccan </td>
        </tr>
        <tr>
            <td>2   </td> <td>7   </td> <td>1   </td> <td>بسم الله الرحمن الرحيم الم                            </td> <td>A. L. M.                                                    </td> <td>1   </td> <td>Medinan</td>
        </tr>
        <tr>
            <td>2   </td> <td>8   </td> <td>2   </td> <td>ذلك الكتاب لا ريب فيه هدى للمتقين                     </td> <td>This is the Book; in it is guidance sure, without doubt, ...</td> <td>7   </td> <td>Medinan</td>
        </tr>
        <tr>
            <td>2   </td> <td>9   </td> <td>3   </td> <td>الذين يؤمنون بالغيب ويقيمون الصلاة ومما رزقناهم ينفقون</td> <td>Who believe in the Unseen, are steadfast in prayer, and  ...</td> <td>8   </td> <td>Medinan</td>
        </tr>
    </tbody>
</table>
<p>... (6226 rows omitted)</p>

With a small logic, we can have another column that counts the number of characters in each verse. The logic is that we can use `len` function to count all characters in a verse which includes the **whitespace** which we can then discount by no. of words plus one. Let us do it.

```python
lc = quran2.apply(len, 'Ayah')-quran2.column('wc')+1
```

```python
quran2 = quran2.with_columns('lc', lc)
quran2
```
<table border="1" class="dataframe">
    <thead>
        <tr>
            <th>sno</th> <th>SrNo</th> <th>vno</th> <th>Ayah</th> <th>en</th> <th>wc</th> <th>Place</th> <th>lc</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>1   </td> <td>0   </td> <td>1   </td> <td>بسم الله الرحمن الرحيم                                </td> <td>In the name of Allah, Most Gracious, Most Merciful.         </td> <td>4   </td> <td>Meccan </td> <td>19  </td>
        </tr>
        <tr>
            <td>1   </td> <td>1   </td> <td>2   </td> <td>الحمد لله رب العالمين                                 </td> <td>Praise be to Allah, the Cherisher and Sustainer of the w ...</td> <td>4   </td> <td>Meccan </td> <td>18  </td>
        </tr>
        <tr>
            <td>1   </td> <td>2   </td> <td>3   </td> <td>الرحمن الرحيم                                         </td> <td>Most Gracious, Most Merciful;                               </td> <td>2   </td> <td>Meccan </td> <td>12  </td>
        </tr>
        <tr>
            <td>1   </td> <td>3   </td> <td>4   </td> <td>مالك يوم الدين                                        </td> <td>Master of the Day of Judgment.                              </td> <td>3   </td> <td>Meccan </td> <td>12  </td>
        </tr>
        <tr>
            <td>1   </td> <td>4   </td> <td>5   </td> <td>إياك نعبد وإياك نستعين                                </td> <td>Thee do we worship, and Thine aid we seek.                  </td> <td>4   </td> <td>Meccan </td> <td>19  </td>
        </tr>
        <tr>
            <td>1   </td> <td>5   </td> <td>6   </td> <td>اهدنا الصراط المستقيم                                 </td> <td>Show us the straight way,                                   </td> <td>3   </td> <td>Meccan </td> <td>19  </td>
        </tr>
        <tr>
            <td>1   </td> <td>6   </td> <td>7   </td> <td>صراط الذين أنعمت عليهم غير المغضوب عليهم ولا الضالين  </td> <td>The way of those on whom Thou hast bestowed Thy Grace, t ...</td> <td>9   </td> <td>Meccan </td> <td>44  </td>
        </tr>
        <tr>
            <td>2   </td> <td>7   </td> <td>1   </td> <td>بسم الله الرحمن الرحيم الم                            </td> <td>A. L. M.                                                    </td> <td>1   </td> <td>Medinan</td> <td>26  </td>
        </tr>
        <tr>
            <td>2   </td> <td>8   </td> <td>2   </td> <td>ذلك الكتاب لا ريب فيه هدى للمتقين                     </td> <td>This is the Book; in it is guidance sure, without doubt, ...</td> <td>7   </td> <td>Medinan</td> <td>27  </td>
        </tr>
        <tr>
            <td>2   </td> <td>9   </td> <td>3   </td> <td>الذين يؤمنون بالغيب ويقيمون الصلاة ومما رزقناهم ينفقون</td> <td>Who believe in the Unseen, are steadfast in prayer, and  ...</td> <td>8   </td> <td>Medinan</td> <td>47  </td>
        </tr>
    </tbody>
</table>
<p>... (6226 rows omitted)</p>

as previous, I am curious to know which verses has the most letters.

```python
quran2.sort('lc', descending=True)
```

<table border="1" class="dataframe">
    <thead>
        <tr>
            <th>sno</th> <th>SrNo</th> <th>vno</th> <th>Ayah</th> <th>en</th> <th>wc</th> <th>Place</th> <th>lc</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>2   </td> <td>288 </td> <td>282 </td> <td>يا أيها الذين آمنوا إذا تداينتم بدين إلى أجل مسمى فاكتبو ...</td> <td>O ye who believe! When ye deal with each other, in trans ...</td> <td>129 </td> <td>Medinan</td> <td>551 </td>
        </tr>
        <tr>
            <td>24  </td> <td>2821</td> <td>31  </td> <td>وقل للمؤمنات يغضضن من أبصارهن ويحفظن فروجهن ولا يبدين زي ...</td> <td>And say to the believing women that they should lower th ...</td> <td>78  </td> <td>Medinan</td> <td>350 </td>
        </tr>
        <tr>
            <td>73  </td> <td>5494</td> <td>20  </td> <td>إن ربك يعلم أنك تقوم أدنى من ثلثي الليل ونصفه وثلثه وطائ ...</td> <td>Thy Lord doth know that thou standest forth (to prayer)  ...</td> <td>78  </td> <td>Meccan </td> <td>329 </td>
        </tr>
        <tr>
            <td>24  </td> <td>2851</td> <td>61  </td> <td>ليس على الأعمى حرج ولا على الأعرج حرج ولا على المريض حرج ...</td> <td>It is no fault in the blind nor in one born lame, nor in ...</td> <td>76  </td> <td>Medinan</td> <td>315 </td>
        </tr>
        <tr>
            <td>2   </td> <td>108 </td> <td>102 </td> <td>واتبعوا ما تتلو الشياطين على ملك سليمان وما كفر سليمان و ...</td> <td>They followed what the evil ones gave out (falsely) agai ...</td> <td>74  </td> <td>Medinan</td> <td>308 </td>
        </tr>
        <tr>
            <td>4   </td> <td>504 </td> <td>12  </td> <td>ولكم نصف ما ترك أزواجكم إن لم يكن لهن ولد فإن كان لهن ول ...</td> <td>In what your wives leave, your share is a half, if they  ...</td> <td>88  </td> <td>Medinan</td> <td>299 </td>
        </tr>
        <tr>
            <td>33  </td> <td>3585</td> <td>53  </td> <td>يا أيها الذين آمنوا لا تدخلوا بيوت النبي إلا أن يؤذن لكم ...</td> <td>O ye who believe! Enter not the Prophet's houses,- until ...</td> <td>70  </td> <td>Medinan</td> <td>294 </td>
        </tr>
        <tr>
            <td>3   </td> <td>446 </td> <td>154 </td> <td>ثم أنزل عليكم من بعد الغم أمنة نعاسا يغشى طائفة منكم وطا ...</td> <td>After (the excitement) of the distress, He sent down cal ...</td> <td>75  </td> <td>Medinan</td> <td>291 </td>
        </tr>
        <tr>
            <td>2   </td> <td>202 </td> <td>196 </td> <td>وأتموا الحج والعمرة لله فإن أحصرتم فما استيسر من الهدي و ...</td> <td>And complete the Hajj or 'umra in the service of Allah.  ...</td> <td>73  </td> <td>Medinan</td> <td>290 </td>
        </tr>
        <tr>
            <td>2   </td> <td>239 </td> <td>233 </td> <td>والوالدات يرضعن أولادهن حولين كاملين لمن أراد أن يتم الر ...</td> <td>The mothers shall give such to their offspring for two w ...</td> <td>64  </td> <td>Medinan</td> <td>288 </td>
        </tr>
    </tbody>
</table>
<p>... (6226 rows omitted)</p>


Having the letter counts would enable lots more analysis on the stylistic properties of the Quran. For example, what is the average size of a single word in the Quran?

```python
np.array(quran2.column('lc')).sum()/np.array(quran2.column('wc')).sum()
```

    4.284034088718074

If we wanted to be a more detailed, we can repeat the same for Meccan and Medinan surahs. 

```python
qmeccan = quran2.where('Place','Meccan').select('wc','lc')
np.array(qmeccan.column('lc')).sum()/np.array(qmeccan.column('wc')).sum()
```

    4.242243587052354

```python
qmedinan = quran2.where('Place','Medinan').select('wc','lc')
np.array(qmedinan.column('lc')).sum()/np.array(qmedinan.column('wc')).sum()
```

    4.350044762757386

It shows that on average, Quranic words are around 4.3 letters and that Medinan surahs has slighly bigger word size but not that much significant. 

## Saving the File

Above was just scratching the surface of what we can do with the Quranic dataset. I will leave the rest for you. Here, I am going to save the Quran Table as a csv file. 


```python
quran.to_csv('quran-en-ar-place.csv')
```

You can play and extend with the above notebook in the [Kaggle](https://www.kaggle.com/abdulbaqi/quran-and-tables) site. 