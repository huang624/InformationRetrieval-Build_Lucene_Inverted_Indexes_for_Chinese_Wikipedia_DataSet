# Using pyserini to build Lucene Inverted Indexes for Chinese Wikipedia DataSet to accelerating search time.  
## Requirements  
  Download dataset here:https://drive.google.com/file/d/1yyqTwl3SCMQ6P6ZWt88cPqvtzK4RdI2m/view?usp=sharing  
  ``` python
  !pip install pyserini faiss-gpu
  ```
  
## Data cleaning
We use keys of 'id' and 'contents' to build Lucene Inverted Indexes only, so we transfer keys from 'articles' to 'contents'. Then, we delete keys of 'title' and 'articles'.  
``` python
for data in datas:
  data['id'] = str(data['id'])
  data['contents'] = data['articles']
  del data['title']
  del data['articles']
```

## Data type  
{'id': '1', 'contents': '福斯家庭電影頻道（）是一個播放電影的亞洲電視頻道，由福斯國際頻道持有與出品，STAR衛視發佈。在香港、澳門、臺灣、菲律賓、新加坡、馬來西亞等地區均有業務。福斯家庭電影頻道已簽訂電影合約的單位包括20世紀福斯、迪斯尼、米高梅 - 梅耶，Studio Cana、獅門娛樂、頂峯娛樂、溫斯坦公司等。該電影頻道面向老人和兒童，主要提供家庭電影。'}  

{'id': '2', 'contents': '六羰基鎢是一種配位化合物，化學式爲W(CO)。經過它得到了第一個雙氫配合物。它是無色固體，和同族的羰基配合物六羰基鉻與六羰基鉬一樣，是揮發性的且在空氣中穩定的物質，其中鎢原子爲0價。六羰基鎢在超酸中可以被氧化：'}  

We save data as json file to bulid Lucene Inverted Indexes.  

## Building Lucene Inverted Indexes
``` python
!python -m pyserini.index.lucene \ 
  --collection JsonCollection \  # data here
  --input ./resources \
  --language zh \
  --index ./indexes \
  --generator DefaultLuceneDocumentGenerator \
  --threads 1 \
  --storePositions --storeDocvectors --storeRaw
``` 

## Result-In comparison with Linear Search Time  
__Linear Search Time: 0.4925563335418701__  
__Inverted Index Search Time: 0.014552116394042969__  

Inverted Index Search is **30** times faster than Linear Search.  
