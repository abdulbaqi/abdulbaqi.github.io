---
layout: post
title: Python and Pandas on Quranic Root Words
subtitle: Finding Root Words of the Quran using Python and Pandas
bigimg: /img/matrix.jpg
image: /img/python-icon.png
share-img: /img/matrix.jpg
tags: quran python pandas text technology
---

In a [previous](http://abdulbaqi.io/2019/01/15/quranic_roots/) post I explored some of `Linux` commands to find root words in the entire Quran or in a particular sura of the Quran.

While, `Linux` commands could be very productive at certain cases, they are not meant for data analysis. Hence, we need to resort to a proper data science programming language like R or Python. 

In this post, I will explore the power of `Python` to address a the same topic (i.e., root words in the Quran) but will extend the problem to much more interesting queries.

This post is not intended to be a beginner's tutorial to either `python` or `pandas` which is a special python package for data analysis. I expect you to have some experience with both these tools. Anyway, there are tons of resources out there. One good place is available under [kaggle](https://www.kaggle.com/learn/overview) website.

Without further ado, let us get started.

First, let us start with few setup steps, like loading the `pandas` package and rename it for ease of usage as `pd`.


```python
import pandas as pd
```

Next, read the file that contains the morphological information. Pandas has the `read_csv` function that can read directly from a URL. Note that the file contains some copyright information in the first 56 lines, and hence I am using `skiprows` option. Also, note that `read_csv` by default assumes the separator to be a comma, if not -as is the case in this file- we need to explicitly specify the delimiter and hence the `sep='\t'` option. Finally, we are displaying few lines from the top by the `head()` function.


```python
url = 'http://textminingthequran.com/data/quranic-corpus-morphology-0.4.txt'
qdforiginal = pd.read_csv(url, sep='\t',skiprows=56)
qdforiginal.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>LOCATION</th>
      <th>FORM</th>
      <th>TAG</th>
      <th>FEATURES</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>(1:1:1:1)</td>
      <td>bi</td>
      <td>P</td>
      <td>PREFIX|bi+</td>
    </tr>
    <tr>
      <th>1</th>
      <td>(1:1:1:2)</td>
      <td>somi</td>
      <td>N</td>
      <td>STEM|POS:N|LEM:{som|ROOT:smw|M|GEN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>(1:1:2:1)</td>
      <td>{ll~ahi</td>
      <td>PN</td>
      <td>STEM|POS:PN|LEM:{ll~ah|ROOT:Alh|GEN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>(1:1:3:1)</td>
      <td>{l</td>
      <td>DET</td>
      <td>PREFIX|Al+</td>
    </tr>
    <tr>
      <th>4</th>
      <td>(1:1:3:2)</td>
      <td>r~aHoma`ni</td>
      <td>ADJ</td>
      <td>STEM|POS:ADJ|LEM:r~aHoma`n|ROOT:rHm|MS|GEN</td>
    </tr>
  </tbody>
</table>
</div>



It is a good idea to save the first version locally by the `to_csv()` function. 


```python
qdforiginal.to_csv('quran-morphology-v1.csv')
```

Looking at the first few lines of the file above we see that the `LOCATION` and `FEATURES` columns need to be split further. 

Our file contains 128k lines (you can verify that by the command `qdforiginal.shape`). I prefer to take a small sample of this big file and run the experimentations of splitting. When successful, we can then run it on the entire file.

### Splitting columns

Here is my strategy: since I am interested on root words, I want to select first all rows that contain the word `ROOT:` in the `FEATURES` column. This can be done by a command like the following:


```python
qdforiginal.FEATURES.str.contains('ROOT')[:3]
```




    0    False
    1     True
    2     True
    Name: FEATURES, dtype: bool



I only took the first 3 lines of the entire 128k lines. It returns `boolean` values of `True` or `False`. So, we can pass this boolean result to filter the entire dataframe by:


```python
qdforiginal[qdforiginal.FEATURES.str.contains('ROOT:')].head(3)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>LOCATION</th>
      <th>FORM</th>
      <th>TAG</th>
      <th>FEATURES</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>(1:1:1:2)</td>
      <td>somi</td>
      <td>N</td>
      <td>STEM|POS:N|LEM:{som|ROOT:smw|M|GEN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>(1:1:2:1)</td>
      <td>{ll~ahi</td>
      <td>PN</td>
      <td>STEM|POS:PN|LEM:{ll~ah|ROOT:Alh|GEN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>(1:1:3:2)</td>
      <td>r~aHoma`ni</td>
      <td>ADJ</td>
      <td>STEM|POS:ADJ|LEM:r~aHoma`n|ROOT:rHm|MS|GEN</td>
    </tr>
  </tbody>
</table>
</div>



To ensure some random sampling, I can always use `sample()` method as follows. I will name this sample as `qsample`.


```python
qsample = qdforiginal[qdforiginal.FEATURES.str.contains('ROOT:')].sample(10); qsample
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>LOCATION</th>
      <th>FORM</th>
      <th>TAG</th>
      <th>FEATURES</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>46292</th>
      <td>(10:89:3:1)</td>
      <td>&gt;ujiybat</td>
      <td>V</td>
      <td>STEM|POS:V|PERF|PASS|(IV)|LEM:&gt;ujiybat|ROOT:jw...</td>
    </tr>
    <tr>
      <th>37295</th>
      <td>(7:193:7:1)</td>
      <td>sawaA^'N</td>
      <td>N</td>
      <td>STEM|POS:N|LEM:sawaA^'|ROOT:swy|M|INDEF|NOM</td>
    </tr>
    <tr>
      <th>30207</th>
      <td>(6:118:9:2)</td>
      <td>_#aAya`ti</td>
      <td>N</td>
      <td>STEM|POS:N|LEM:'aAyap|ROOT:Ayy|FP|GEN</td>
    </tr>
    <tr>
      <th>94595</th>
      <td>(36:43:2:1)</td>
      <td>n~a$a&gt;o</td>
      <td>V</td>
      <td>STEM|POS:V|IMPF|LEM:$aA^'a|ROOT:$yA|1P|MOOD:JUS</td>
    </tr>
    <tr>
      <th>51228</th>
      <td>(12:48:13:1)</td>
      <td>qaliylFA</td>
      <td>N</td>
      <td>STEM|POS:N|LEM:qaliyl|ROOT:qll|MS|INDEF|ACC</td>
    </tr>
    <tr>
      <th>107812</th>
      <td>(46:17:28:2)</td>
      <td>&gt;aw~aliyna</td>
      <td>N</td>
      <td>STEM|POS:N|LEM:&gt;aw~al|ROOT:Awl|MP|GEN</td>
    </tr>
    <tr>
      <th>87207</th>
      <td>(30:38:16:2)</td>
      <td>mufoliHuwna</td>
      <td>N</td>
      <td>STEM|POS:N|ACT|PCPL|(IV)|LEM:mufoliHuwn|ROOT:f...</td>
    </tr>
    <tr>
      <th>78161</th>
      <td>(25:74:8:2)</td>
      <td>*ur~iy~a`ti</td>
      <td>N</td>
      <td>STEM|POS:N|LEM:*ur~iy~a`t|ROOT:*rr|FP|GEN</td>
    </tr>
    <tr>
      <th>123443</th>
      <td>(74:41:2:2)</td>
      <td>mujorimiyna</td>
      <td>N</td>
      <td>STEM|POS:N|ACT|PCPL|(IV)|LEM:mujorim|ROOT:jrm|...</td>
    </tr>
    <tr>
      <th>68794</th>
      <td>(21:1:3:1)</td>
      <td>HisaAbu</td>
      <td>N</td>
      <td>STEM|POS:N|VN|(III)|LEM:HisaAb|ROOT:Hsb|M|NOM</td>
    </tr>
  </tbody>
</table>
</div>



My intention is to split the `LOCATION` column into four columns, and then the `FEATURES` column into a column for Root and another for Lemma.

First, I am going to split the first column `LOCATION` into four columns. This is done through the `extract` method. It takes a `regular expression` and hence the `r'...'` input. The `?P<...>` construct within the regular expression creates columns with these names. The four parenthesis within the regular expression specifies the four grouping we are interested to collect. 


```python
tmp1 = qsample.LOCATION.str.extract(r'(?P<sura>\d*):(?P<aya>\d*):(?P<word>\d*):(?P<w_seg>\d*)'); tmp1
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sura</th>
      <th>aya</th>
      <th>word</th>
      <th>w_seg</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>46292</th>
      <td>10</td>
      <td>89</td>
      <td>3</td>
      <td>1</td>
    </tr>
    <tr>
      <th>37295</th>
      <td>7</td>
      <td>193</td>
      <td>7</td>
      <td>1</td>
    </tr>
    <tr>
      <th>30207</th>
      <td>6</td>
      <td>118</td>
      <td>9</td>
      <td>2</td>
    </tr>
    <tr>
      <th>94595</th>
      <td>36</td>
      <td>43</td>
      <td>2</td>
      <td>1</td>
    </tr>
    <tr>
      <th>51228</th>
      <td>12</td>
      <td>48</td>
      <td>13</td>
      <td>1</td>
    </tr>
    <tr>
      <th>107812</th>
      <td>46</td>
      <td>17</td>
      <td>28</td>
      <td>2</td>
    </tr>
    <tr>
      <th>87207</th>
      <td>30</td>
      <td>38</td>
      <td>16</td>
      <td>2</td>
    </tr>
    <tr>
      <th>78161</th>
      <td>25</td>
      <td>74</td>
      <td>8</td>
      <td>2</td>
    </tr>
    <tr>
      <th>123443</th>
      <td>74</td>
      <td>41</td>
      <td>2</td>
      <td>2</td>
    </tr>
    <tr>
      <th>68794</th>
      <td>21</td>
      <td>1</td>
      <td>3</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



Now, let us extract the roots from the `FEATURES` column in the same way.


```python
tmp2 = qsample.FEATURES.str.extract(r'ROOT:(?P<Root>[^|]*)'); tmp2
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Root</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>46292</th>
      <td>jwb</td>
    </tr>
    <tr>
      <th>37295</th>
      <td>swy</td>
    </tr>
    <tr>
      <th>30207</th>
      <td>Ayy</td>
    </tr>
    <tr>
      <th>94595</th>
      <td>$yA</td>
    </tr>
    <tr>
      <th>51228</th>
      <td>qll</td>
    </tr>
    <tr>
      <th>107812</th>
      <td>Awl</td>
    </tr>
    <tr>
      <th>87207</th>
      <td>flH</td>
    </tr>
    <tr>
      <th>78161</th>
      <td>*rr</td>
    </tr>
    <tr>
      <th>123443</th>
      <td>jrm</td>
    </tr>
    <tr>
      <th>68794</th>
      <td>Hsb</td>
    </tr>
  </tbody>
</table>
</div>



Similarly, I can extract **Lemmas** as well. Note that Lemmas are actual words, whereas Roots are not actual words, so at times Lemmas could be more informative. 


```python
tmp3 = qsample.FEATURES.str.extract(r'LEM:(?P<Lemma>[^|]*)'); tmp3
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Lemma</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>46292</th>
      <td>&gt;ujiybat</td>
    </tr>
    <tr>
      <th>37295</th>
      <td>sawaA^'</td>
    </tr>
    <tr>
      <th>30207</th>
      <td>'aAyap</td>
    </tr>
    <tr>
      <th>94595</th>
      <td>$aA^'a</td>
    </tr>
    <tr>
      <th>51228</th>
      <td>qaliyl</td>
    </tr>
    <tr>
      <th>107812</th>
      <td>&gt;aw~al</td>
    </tr>
    <tr>
      <th>87207</th>
      <td>mufoliHuwn</td>
    </tr>
    <tr>
      <th>78161</th>
      <td>*ur~iy~a`t</td>
    </tr>
    <tr>
      <th>123443</th>
      <td>mujorim</td>
    </tr>
    <tr>
      <th>68794</th>
      <td>HisaAb</td>
    </tr>
  </tbody>
</table>
</div>



Finally, all that is left is to cancatenate the orginal sample `qsample` with these three splits `tmp1, tmp2, tmp3`, as follows. The `axis=1` option means that run the concatenation on columns (not rows).


```python
pd.concat([tmp1, qsample, tmp2,tmp3], axis=1)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
        
    }

    .dataframe tbody tr th {
        vertical-align: top;
        font-size: 10px;
    }

    .dataframe thead th {
        text-align: right;
        font-size: 10px;
    }
    .dataframe td {
      font-size: 10px;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sura</th>
      <th>aya</th>
      <th>word</th>
      <th>w_seg</th>
      <th>LOCATION</th>
      <th>FORM</th>
      <th>TAG</th>
      <th>FEATURES</th>
      <th>Root</th>
      <th>Lemma</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>46292</th>
      <td>10</td>
      <td>89</td>
      <td>3</td>
      <td>1</td>
      <td>(10:89:3:1)</td>
      <td>&gt;ujiybat</td>
      <td>V</td>
      <td>STEM|POS:V|PERF|PASS|(IV)|LEM:&gt;ujiybat|ROOT:jw...</td>
      <td>jwb</td>
      <td>&gt;ujiybat</td>
    </tr>
    <tr>
      <th>37295</th>
      <td>7</td>
      <td>193</td>
      <td>7</td>
      <td>1</td>
      <td>(7:193:7:1)</td>
      <td>sawaA^'N</td>
      <td>N</td>
      <td>STEM|POS:N|LEM:sawaA^'|ROOT:swy|M|INDEF|NOM</td>
      <td>swy</td>
      <td>sawaA^'</td>
    </tr>
    <tr>
      <th>30207</th>
      <td>6</td>
      <td>118</td>
      <td>9</td>
      <td>2</td>
      <td>(6:118:9:2)</td>
      <td>_#aAya`ti</td>
      <td>N</td>
      <td>STEM|POS:N|LEM:'aAyap|ROOT:Ayy|FP|GEN</td>
      <td>Ayy</td>
      <td>'aAyap</td>
    </tr>
    <tr>
      <th>94595</th>
      <td>36</td>
      <td>43</td>
      <td>2</td>
      <td>1</td>
      <td>(36:43:2:1)</td>
      <td>n~a$a&gt;o</td>
      <td>V</td>
      <td>STEM|POS:V|IMPF|LEM:$aA^'a|ROOT:$yA|1P|MOOD:JUS</td>
      <td>$yA</td>
      <td>$aA^'a</td>
    </tr>
    <tr>
      <th>51228</th>
      <td>12</td>
      <td>48</td>
      <td>13</td>
      <td>1</td>
      <td>(12:48:13:1)</td>
      <td>qaliylFA</td>
      <td>N</td>
      <td>STEM|POS:N|LEM:qaliyl|ROOT:qll|MS|INDEF|ACC</td>
      <td>qll</td>
      <td>qaliyl</td>
    </tr>
    <tr>
      <th>107812</th>
      <td>46</td>
      <td>17</td>
      <td>28</td>
      <td>2</td>
      <td>(46:17:28:2)</td>
      <td>&gt;aw~aliyna</td>
      <td>N</td>
      <td>STEM|POS:N|LEM:&gt;aw~al|ROOT:Awl|MP|GEN</td>
      <td>Awl</td>
      <td>&gt;aw~al</td>
    </tr>
    <tr>
      <th>87207</th>
      <td>30</td>
      <td>38</td>
      <td>16</td>
      <td>2</td>
      <td>(30:38:16:2)</td>
      <td>mufoliHuwna</td>
      <td>N</td>
      <td>STEM|POS:N|ACT|PCPL|(IV)|LEM:mufoliHuwn|ROOT:f...</td>
      <td>flH</td>
      <td>mufoliHuwn</td>
    </tr>
    <tr>
      <th>78161</th>
      <td>25</td>
      <td>74</td>
      <td>8</td>
      <td>2</td>
      <td>(25:74:8:2)</td>
      <td>*ur~iy~a`ti</td>
      <td>N</td>
      <td>STEM|POS:N|LEM:*ur~iy~a`t|ROOT:*rr|FP|GEN</td>
      <td>*rr</td>
      <td>*ur~iy~a`t</td>
    </tr>
    <tr>
      <th>123443</th>
      <td>74</td>
      <td>41</td>
      <td>2</td>
      <td>2</td>
      <td>(74:41:2:2)</td>
      <td>mujorimiyna</td>
      <td>N</td>
      <td>STEM|POS:N|ACT|PCPL|(IV)|LEM:mujorim|ROOT:jrm|...</td>
      <td>jrm</td>
      <td>mujorim</td>
    </tr>
    <tr>
      <th>68794</th>
      <td>21</td>
      <td>1</td>
      <td>3</td>
      <td>1</td>
      <td>(21:1:3:1)</td>
      <td>HisaAbu</td>
      <td>N</td>
      <td>STEM|POS:N|VN|(III)|LEM:HisaAb|ROOT:Hsb|M|NOM</td>
      <td>Hsb</td>
      <td>HisaAb</td>
    </tr>
  </tbody>
</table>
</div>



Now that we ran the experiment successfully with the sample, let us repeat it on the actual file `qdforiginal`


```python
tmp1 = qdforiginal.LOCATION.str.extract(r'(?P<sura>\d*):(?P<aya>\d*):(?P<word>\d*):(?P<w_seg>\d*)')
tmp2 = qdforiginal.FEATURES.str.extract(r'ROOT:(?P<Root>[^|]*)')
tmp3 = qdforiginal.FEATURES.str.extract(r'LEM:(?P<Lemma>[^|]*)')
df_qruan = pd.concat([tmp1, qdforiginal, tmp2,tmp3], axis=1)
```

To confirm the shape of the new dataframe `df_qruan` I can use the `shape` attribute, also I can display randomly some rows.


```python
df_qruan.shape
```




    (128219, 10)




```python
df_qruan.sample(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sura</th>
      <th>aya</th>
      <th>word</th>
      <th>w_seg</th>
      <th>LOCATION</th>
      <th>FORM</th>
      <th>TAG</th>
      <th>FEATURES</th>
      <th>Root</th>
      <th>Lemma</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>33209</th>
      <td>7</td>
      <td>50</td>
      <td>19</td>
      <td>1</td>
      <td>(7:50:19:1)</td>
      <td>EalaY</td>
      <td>P</td>
      <td>STEM|POS:P|LEM:EalaY`</td>
      <td>NaN</td>
      <td>EalaY`</td>
    </tr>
    <tr>
      <th>26277</th>
      <td>5</td>
      <td>106</td>
      <td>40</td>
      <td>1</td>
      <td>(5:106:40:1)</td>
      <td>wa</td>
      <td>REM</td>
      <td>PREFIX|w:REM+</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>61047</th>
      <td>17</td>
      <td>55</td>
      <td>4</td>
      <td>1</td>
      <td>(17:55:4:1)</td>
      <td>fiY</td>
      <td>P</td>
      <td>STEM|POS:P|LEM:fiY</td>
      <td>NaN</td>
      <td>fiY</td>
    </tr>
    <tr>
      <th>125339</th>
      <td>81</td>
      <td>14</td>
      <td>1</td>
      <td>1</td>
      <td>(81:14:1:1)</td>
      <td>Ealimato</td>
      <td>V</td>
      <td>STEM|POS:V|PERF|LEM:Ealima|ROOT:Elm|3FS</td>
      <td>Elm</td>
      <td>Ealima</td>
    </tr>
    <tr>
      <th>120502</th>
      <td>67</td>
      <td>19</td>
      <td>1</td>
      <td>1</td>
      <td>(67:19:1:1)</td>
      <td>&gt;a</td>
      <td>INTG</td>
      <td>PREFIX|A:INTG+</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



It could be possible that our newly introduced columns could have extra spaces which we can get rid of by using the `strip()` method of string as follows.


```python
quran.Root = quran.Root.str.strip()
quran.Lemma = quran.Lemma.str.strip()
```

It is good idea to save this version into a `csv` file. `index=False` avoids unncessesarity including an extra index column in the output file.


```python
df_qruan.to_csv('quran-morphology-v2.csv', index=False)
```

## join with Meccan/Medinan file

It would be very useful to augment our file with a new column that tells me if a sura is Meccan or Medinan. This will later allow to answer question like, **what are the unique root words in the Quran that appear only in Meccan sura?** for example. 

To do this, I am referring to a table of contents page I created some time back using `Angular` [here](http://textminingthequran.com/toc/)

My idea is to go that page, and use mouse to select the table, copy it in the clipboard and then perform the following operation to read the clipboard and create a dataframe `qtoc` as follows.


```python
qtoc=pd.read_clipboard()
```


```python
qtoc.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>No.</th>
      <th>Name Arabic</th>
      <th>Name</th>
      <th>English Meaning</th>
      <th>No of verses</th>
      <th>Place</th>
      <th>Chronology</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>الفاتحة</td>
      <td>Al-Fatiha</td>
      <td>The Opening</td>
      <td>7</td>
      <td>Meccan</td>
      <td>5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>البقرة</td>
      <td>Al-Baqara</td>
      <td>The Cow</td>
      <td>286</td>
      <td>Medinan</td>
      <td>87</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>آل عمران</td>
      <td>Al Imran</td>
      <td>The House of Joachim</td>
      <td>200</td>
      <td>Medinan</td>
      <td>89</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>النساء</td>
      <td>An-Nisa'</td>
      <td>Women</td>
      <td>176</td>
      <td>Medinan</td>
      <td>92</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>المائدة</td>
      <td>Al-Ma'ida</td>
      <td>The Table Spread</td>
      <td>120</td>
      <td>Medinan</td>
      <td>112</td>
    </tr>
  </tbody>
</table>
</div>



Again, let me save this dataframe locally.


```python
qtoc.to_csv('toc.csv', index=False)
```

I will now use the `merge` function to merge our original file `df_qruan` with the `qtoc` on the sura number (which is `sura` in the left `df_qruan` file and `No.` column in the right `qtoc` file. The `left` join is the one that makes sense here. The new dataframe is saved in a `quran`.


```python
quran = df_qruan.merge(qtoc.loc[:,['No.', 'Place']], how='left', left_on='sura', right_on='No.')
```

I can display few useful information through the `info()` method.


```python
quran.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 128219 entries, 0 to 128218
    Data columns (total 12 columns):
    sura        128219 non-null int64
    aya         128219 non-null object
    word        128219 non-null object
    w_seg       128219 non-null object
    LOCATION    128219 non-null object
    FORM        128011 non-null object
    TAG         128219 non-null object
    FEATURES    128219 non-null object
    Root        49968 non-null object
    Lemma       74608 non-null object
    No.         128219 non-null int64
    Place       128219 non-null object
    dtypes: int64(2), object(10)
    memory usage: 12.7+ MB


I noticed that I no longer need the `LOCATION` and `No.` column as they are now redundent. So, just drop them.


```python
quran.drop(columns=['LOCATION','No.'], inplace=True)
```

As usual, here is a local copy of the final file after doing all these setup steps.


```python
quran.to_csv('quran-morphology-final.csv', index=False)
```

## converting Buckwalter to Arabic

Our file contains Quranic words and roots in Buckwalter form, and I wanted a handy function to convert that into Arabic form. Here is how we do it.

First, referencing [this](http://corpus.quran.com/java/buckwalter.jsp) site, I can construct the following dictionary of all mappings of unicode symbols into buckwalter as follows. I will call this dictionary `abjad`.


```python
abjad = {u"\u0627":'A',
u"\u0628":'b', u"\u062A":'t', u"\u062B":'v', u"\u062C":'j',
u"\u062D":'H', u"\u062E":'x', u"\u062F":'d', u"\u0630":'*', u"\u0631":'r',
u"\u0632":'z', u"\u0633":'s', u"\u0634":'$', u"\u0635":'S', u"\u0636":'D',
u"\u0637":'T', u"\u0638":'Z', u"\u0639":'E', u"\u063A":'g', u"\u0641":'f',
u"\u0642":'q', u"\u0643":'k', u"\u0644":'l', u"\u0645":'m', u"\u0646":'n',
u"\u0647":'h', u"\u0648":'w', u"\u0649":'Y', u"\u064A":'y'}
```


```python
abjad[' ']=' '
abjad[u"\u0621"] = '\''
abjad[u"\u0623"] = '>'
abjad[u"\u0625"] = '<'
abjad[u"\u0624"] = '&'
abjad[u"\u0626"] = '}'
#abjad[u"\u0655"] = '\'' # Hamza below
abjad[u"\u0622"] = '|'
abjad[u"\u064E"] = 'a'
abjad[u"\u064F"] = 'u'
abjad[u"\u0650"] = 'i'
abjad[u"\u0651"] = '~'
abjad[u"\u0652"] = 'o'
abjad[u"\u064B"] = 'F'
abjad[u"\u064C"] = 'N'
abjad[u"\u064D"] = 'K'
abjad[u"\u0640"] = '_'
abjad[u"\u0670"] = '`'
abjad[u"\u0629"] = 'p'
abjad[u"\u0653"] = '^'
abjad[u"\u0654"] = '#'
abjad[u"\u0671"] = '{'
abjad[u"\u06DC"] = ':'
abjad[u"\u06DF"] = '@'
abjad[u"\u0653"] = '^'
abjad[u"\u06E0"] = '"'
abjad[u"\u06E2"] = '['
abjad[u"\u06E3"] = ';'
abjad[u"\u06E5"] = ','
abjad[u"\u06E6"] = '.'
abjad[u"\u06E8"] = '!'
abjad[u"\u06EA"] = '-'
abjad[u"\u06EB"] = '+'
abjad[u"\u06EC"] = '%'
abjad[u"\u06ED"] = ']'
```

Let us also construct the reverse dictionary called `alphabet` that maps the bucwalter symbols back to unicode and hence can display Arabic words. 


```python
# Create the reverse
alphabet = {}
for key in abjad:
    alphabet[abjad[key]] = key
```

Using these two dictionaries, we can always convert a string from one form to other using the following two handy functions.


```python
def arabic_to_buc(ara):
    return ''.join(map(lambda x:abjad[x], list(ara)))

def buck_to_arabic(buc):
    return ''.join(map(lambda x:alphabet[x], list(buc)))
```

Here is a small test.


```python
buck_to_arabic('EalaY`')
```




    'عَلَىٰ'




```python
arabic_to_buc('الحمد لله')
```




    'AlHmd llh'



## counting roots

Now it is time to get into the core of our query: **What are the unique root words that appear in Meccan sura, but not in the Medinan surah?**

As we saw before, we can: (1) filter a dataframe by logical checks like `quran.Place== 'Meccan'`. With that we (2) get set of all Meccan words, (3) then we select only the `Root` column, (4) then we run the `unique()` method to get an array of unique words which we can (5) then convert to list using `tolist()` function. Finally (6) we wrap the whole thing to a `set()` function, and hence we get the set of Meccan unique root words called `k` here. So, note how through chaining I could perform six operations into one. 


```python
k = set(quran[quran.Place == 'Meccan'].Root.unique().tolist())
```

With the same logic, we produce the unique list of Medinan words in a set called `d`.


```python
d = set(quran[quran.Place == 'Medinan'].Root.unique().tolist())
```

With this we can now remove the roots from Meccan list that are also in the Medinan, but the following set operation. We find out that there are 547 of such words, and 198 Medinan only words, and 898 root words appear in both.


```python
makki_words = k-d; len(makki_words)
```




    547




```python
madani_words = d - k; len(madani_words)
```




    198




```python
both = k & d
```


```python
len(both)
```




    898



We now have at our hand all nuts and bolts to define two useful functions as follows.

Our first function is `sura_words`. It takes as input a list of sura numbers (for example `[113,114]` means sura 113 and 114). It also takes which kind of unique words we want to find for this list of sura: `W` is the default word list, `R` is the Root list and `L` is the Lemma list. Note how we use the `isin()` method to filter the dataframe on only the list of sura we provide. Also note the `dropna()` function to drop the `null` values from the list. Finally note how we are returnting Arabic form of the final resuls using the `buck_to_arabic()` function we defined earlier.


```python
# function to return words given a list of sura
def sura_words(s_list, kind='W'):
    if (kind=='R'):
        result = quran[quran.sura.isin(s_list)].Root.dropna().unique().tolist()
    elif (kind=='L'):
        result = quran[quran.sura.isin(s_list)].Lemma.dropna().unique().tolist()
    else:
        result = quran[quran.sura.isin(s_list)].FORM.unique().tolist()
    return [buck_to_arabic(x) for x in result]
```

Here is a test on `Lemma` words of suran No. 111.


```python
sura_words([111],'L')
```




    ['تَبَّ',
     'يَد',
     'أَبٌ',
     'لَهَب',
     'مَا',
     'أَغْنَىٰ',
     'عَن',
     'مَال',
     'كَسَبَ',
     'يَصْلَى',
     'نَار',
     'ذُو',
     'ٱمْرَأَت',
     'حَمَّالَة',
     'حَطَب',
     'فِى',
     'جِيد',
     'حَبْل',
     'مِن',
     'مَّسَد']



The above function can have lots of utilities. Among them you may want to increase your Quranic vocabulary gradually by memorizing roots of one sura at a time. This function conviniently will give you the unique list of roots (or lemmas, or just words).

With a small variation and exploiting the set operations, we can define another function called `unique_sura_words` that again takes a list of sura and returns root (or lemma or raw words) that appears only in this list of suras. Note the `~` operator to negate a condition. So `~quran.sura.isin([113,114])` means all sura except 113 and 114. 


```python
# function to return words given a list of sura
def unique_sura_words(s_list, kind='W'):
    if (kind=='R'):
        first = quran[quran.sura.isin(s_list)].Root.dropna().unique().tolist()
        second = quran[~quran.sura.isin(s_list)].Root.dropna().unique().tolist()
        result = list(set(first)-set(second))
    elif (kind=='L'):
        first = quran[quran.sura.isin(s_list)].Lemma.dropna().unique().tolist()
        second = quran[~quran.sura.isin(s_list)].Lemma.dropna().unique().tolist()
        result = list(set(first)-set(second))
    else:
        first = quran[quran.sura.isin(s_list)].FORM.dropna().unique().tolist()
        second = quran[~quran.sura.isin(s_list)].FORM.dropna().unique().tolist()
        result = list(set(first)-set(second))
    return [buck_to_arabic(x) for x in result]
```

Using this function we know that sura 113 has these two root words that can be found no where else in the Quran.


```python
unique_sura_words([113],'R')
```




    ['نفث', 'وقب']

From here one can extend this utility to create a web app using frameworks like `flask`. 
I am leaving a [jupyter notebook](https://github.com/abdulbaqi/quranic_roots) for you to try things out yourself. 
