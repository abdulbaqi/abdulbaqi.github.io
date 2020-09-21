---
layout: post
title: Finding Unique Words in a Quranic Chapter
subtitle: 
bigimg: /img/code.jpeg
image: /img/quran.jpeg
share-img: /img/quran.jpeg
tags: quran python technology data
---
A Quranic chapter is a world by itself. Quranic chapters in Arabic are called *surah*, and one of the two meanings of this word means a boundary wall. That is so because each surah is like a wall that has its own meaning and theme kept inside. 

This gave me the idea to write a small python code that takes a number of sura as input and returns the unique words that appeared only in this surah. 

Let's get started. 

First, I have downloaded the simple text from the Tanzil project (http://tanzil.net/download/) as a txt file. The file does not have a header, and is of the format:

`surah no|aya no| ayah text`

I will manipulate this data using python `DataFrame`, so let us load the `pandas` module.

```python
import pandas as pd
```

I am using `read_csv` to read a text file, and I have used the appropriate separator to identify the three fields. 

```python
quran = pd.read_csv('quran-simple-clean.txt', sep="|", header=None)
```

Check first few lines to make sure everything looks OK.

```python
quran.head()
```




<div>
<style>
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
<table class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0</th>
      <th>1</th>
      <th>2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>1.0</td>
      <td>بسم الله الرحمن الرحيم</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>2.0</td>
      <td>الحمد لله رب العالمين</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>3.0</td>
      <td>الرحمن الرحيم</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>4.0</td>
      <td>مالك يوم الدين</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>5.0</td>
      <td>إياك نعبد وإياك نستعين</td>
    </tr>
  </tbody>
</table>
</div>


Also, check from the end of file. 

```python
quran.tail()
```




<div>
<style>
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
<table class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0</th>
      <th>1</th>
      <th>2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>6259</th>
      <td>#    derived from or containing substantial po...</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6260</th>
      <td>#</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6261</th>
      <td>#  Please check updates at: http://tanzil.net/...</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6262</th>
      <td>#</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6263</th>
      <td>#=============================================...</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



We noticed that there are some trailing text at the end that starts at index no 6236 until the end of file, so we need to drop those lines.


```python
quran.drop(quran.index[6236:], inplace=True)
```
The above code drops those lines and updates the dataframe (hence the option `inplace` is True)

```python
quran.tail()
```


<div>
<style>
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
<table class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0</th>
      <th>1</th>
      <th>2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>6231</th>
      <td>114</td>
      <td>2.0</td>
      <td>ملك الناس</td>
    </tr>
    <tr>
      <th>6232</th>
      <td>114</td>
      <td>3.0</td>
      <td>إله الناس</td>
    </tr>
    <tr>
      <th>6233</th>
      <td>114</td>
      <td>4.0</td>
      <td>من شر الوسواس الخناس</td>
    </tr>
    <tr>
      <th>6234</th>
      <td>114</td>
      <td>5.0</td>
      <td>الذي يوسوس في صدور الناس</td>
    </tr>
    <tr>
      <th>6235</th>
      <td>114</td>
      <td>6.0</td>
      <td>من الجنة والناس</td>
    </tr>
  </tbody>
</table>
</div>


The original files had no headers, so we need to give some meaningful column names to our dataframe. 


```python
quran.columns = ['sura_no', 'aya_no', 'text']
```

Let us check the data types.


```python
quran.dtypes
```




    sura_no     object
    aya_no     float64
    text        object
    dtype: object



We noticed `sura_no` as object, so need to convert it to `.to_numeric()` and 


```python
quran['sura_no'] = pd.to_numeric(quran['sura_no'], downcast = 'integer')
```


```python
quran['aya_no'] = pd.to_numeric(quran['aya_no'], downcast = 'integer')
```


```python
quran.dtypes
```




    sura_no      int8
    aya_no      int16
    text       object
    dtype: object


A final check again. 

```python
quran.head()
```




<div>
<style>
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
<table class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sura_no</th>
      <th>aya_no</th>
      <th>text</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>1</td>
      <td>بسم الله الرحمن الرحيم</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>2</td>
      <td>الحمد لله رب العالمين</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>3</td>
      <td>الرحمن الرحيم</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>4</td>
      <td>مالك يوم الدين</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>5</td>
      <td>إياك نعبد وإياك نستعين</td>
    </tr>
  </tbody>
</table>
</div>


Now, let us get to the core of our topic. 

Here is a function definition that takes a particular surah and then returns a set containing words in that surah (i.e., `set` means it will remove duplicates).


```python
# a function to find unique words
def unique_words(sura, neg=0):
    if neg==0:
        selection = quran[quran['sura_no']==sura].text.str.split().tolist()
    else:
        selection = quran[quran['sura_no']!=sura].text.str.split().tolist()
    flat_list = [item for aya in selection for item in aya]
    return set(flat_list)
```

Notice in the definition above, I have made the optional parameter of `neg` to return a unique words in the Quran that are **NOT** in that surah.

So, `unique_words(1)` means all unique words in Surah no. 1 (i.e., Fatihah), and `unique_words(1,1)` means all unique words not in Fatihah.

Now, using the above function we can easily define a function that takes a surah number and then returns the unique words that can be found only in this surah in the entire Quran. This can be done using set functions as below. Also note the returned list is sorted. 


```python
def unique(sura):
    return (sorted(list(set(unique_words(sura))-set(unique_words(sura,1)))))
```

Let us put it to test. Here are the unique words in surah Fatihah.


```python
unique(1)
```


    ['إياك', 'المغضوب', 'اهدنا', 'نستعين', 'وإياك']




```python
unique(111)
```




    ['الحطب', 'جيدها', 'حمالة', 'سيصلى', 'لهب', 'مسد', 'يدا']


Go ahead and test it in the code snippet below. (First, press the run button.)

<iframe height="400px" width="100%" src="https://repl.it/@baqi/UniqueWordsQuran?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>

This function can be enhanced by working at root-words level instead of unique words. That will give more concise list. Nevertheless, try out small chapters and you will notice that the unique word list of a surah truly captures some of the key concepts and themes of that surah. 

Also, here is the jupyter [notebook](https://notebooks.azure.com/asharaf/projects/quran) for you to try. 

I have also other posts to investigate this question using [Linux](http://abdulbaqi.io/2019/01/15/quranic_roots/) commands or python [pandas](http://abdulbaqi.io/2019/01/19/quranic-roots-pandas/). 
