## Topic Modelling-LSA(Latent Semantic Analysis)

Topic Modelling adalah mengelompokkan data teks berdasarkan suatu topik tertentu. Cara kerja topik modelling seperti clustering, dikatakan seperti clustering karena mengelompokkan dokumen berdasarkan kemiripannya.

## LSA(Latent Semantic Analysis)

LSA untuk menganalisa hubungan antara sebuah frase/kalimat dengan sekumpulan dokumen.

## Tahapan LSA

Berikut adalah tahapan LSA:

#### Menghitung TF-IDF

Menghitung TF-IDF dengan menggunakan m x n

dengan keterangan:

m = fitur kata pada dokumen

TF-IDF dihitung menggunakan persamaan berikut:
$$
W_{i,j}=tfi,j\times \log \dfrac{N}{df^{j}}
$$
Dengan keterangan:

Wij adalah Score TF-IDF 

tfi,j adalah term dari dokumen

N adalah total dokumen

Df j adalah Dokumen

```python
import pandas as pd
import numpy as np
#Import Library untuk Tokenisasi
import string 
import re #regex library

# import word_tokenize & FreqDist dari NLTK
from nltk.tokenize import word_tokenize 
from nltk.probability import FreqDist
from nltk.corpus import stopwords

from sklearn.feature_extraction.text import CountVectorizer
from sklearn.feature_extraction.text import TfidfVectorizer
```

```python
vect =TfidfVectorizer(stop_words=list_stopwords,max_features=1000) 
vect_text=vect.fit_transform(dataPTA['Abstrak'])
print(vect_text.shape)
print(vect_text)
```

## Menampilkan SVD

SVD adalah suatu pemfaktoran matriks dengan mengurai suatu matriks ke dalam dua matriks.

Singular value decomposition dari matrix A dinyatakan sebagai : A= USV

Untuk matriks U: Baris mewakili vektor dokumen pada topik

Untuk matriks V: Baris mewakili vector istilah yang dinyatakan dalam topik

Untuk Matriks S: matriks diagonal yang memiliki elemen- elemen diagonal sebagai nilai singular dari A.

Untuk pengimplementasian SVD pada python:

```python
from sklearn.decomposition import TruncatedSVD
lsa_model = TruncatedSVD(n_components=10, algorithm='randomized', n_iter=10, random_state=42)

lsa_top=lsa_model.fit_transform(vect_text)
```

```python
l=lsa_top[0]
print("Document 0 :")
for i,topic in enumerate(l):
  print("Topic ",i," : ",topic*100)
```

```python
print(lsa_model.components_.shape) # (no_of_topics*no_of_words)
print(lsa_model.components_)
```

## Mengekstrak Topic dan Term

Pada percobaan ini melakukan sebanyak 10 topik.

```python
# most important words for each topic
vocab = vect.get_feature_names()

for i, comp in enumerate(lsa_model.components_):
    vocab_comp = zip(vocab, comp)
    sorted_words = sorted(vocab_comp, key= lambda x:x[1], reverse=True)[:10]
    print("Topic "+str(i)+": ")
    for t in sorted_words:
        print(t[0],end=" ")
    print("\n")
```

