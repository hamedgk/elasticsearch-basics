### to run the elastic search docker container:
create a network for later usecases(kibana, etc)
```bash
docker network create elastic_network
```
run docker container of elasticsearch 7.17
```bash
docker run -d --name elasticsearch --net elastic_network -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" -m 1GB elasticsearch:7.17.25
```
&nbsp;
&nbsp;
&nbsp;

### create students index with below mappings
```json
{
  "mappings": {
    "properties": {
      "first_name": { 
        "type": "text", 
        "fields": { 
          "keyword": { "type": "keyword" } 
        }
      },
      "last_name": { 
        "type": "text", 
        "fields": { 
          "keyword": { "type": "keyword" } 
        }
      },
      "date_of_birth": { 
        "type": "date",
        "format" : "yyyy/MM/dd"
      },
      "age": { 
        "type": "short" 
      },
      "educational_background": {
        "type": "nested",
        "properties": {
          "degree": { 
            "type": "keyword" 
          },
          "university_name": { 
            "type": "keyword" 
          },
          "thesis": {
            "type": "nested",
            "properties": {
              "title": { 
                "type": "keyword" 
              },
              "score": { 
                "type": "short" 
              }
            }
          }
        }
      },
      "address": {
        "type": "nested",
        "properties": {
          "province": { 
            "type": "keyword" 
          },
          "city": { 
            "type": "keyword" 
          },
          "details": { 
            "type": "text" 
          }
        }
      }
    }
  }
}

```
&nbsp;
&nbsp;
&nbsp;

### populate with a few documents
```json
{ "index": {} }
{ "first_name": "حسن", "last_name": "محمدی", "date_of_birth": "1370/10/11", "age": 34, "educational_background": [{ "degree": "کارشناسی", "university_name": "دانشگاه تهران", "thesis": { "title": "هوش مصنوعی", "score": 17 } }, { "degree": "کارشناسی ارشد", "university_name": "دانشگاه صنعتی شریف", "thesis": { "title": "یادگیری ماشین", "score": 18 } }], "address": { "province": "تهران", "city": "تهران", "details": "خیابان ولیعصر، 123" } }
{ "index": {} }
{ "first_name": "زینب", "last_name": "احمدی", "date_of_birth": "1364/03/25", "age": 39, "educational_background": [{ "degree": "دکتری", "university_name": "دانشگاه علامه طباطبایی", "thesis": { "title": "عصب‌شناسی", "score": 19 } }], "address": { "province": "تهران", "city": "تهران", "details": "خیابان آزادی، 456" } }
{ "index": {} }
{ "first_name": "علی", "last_name": "اکبری", "date_of_birth": "1371/12/01", "age": 32, "educational_background": [{ "degree": "کارشناسی", "university_name": "دانشگاه تهران", "thesis": { "title": "ساختارهای داده", "score": 18 } }, { "degree": "کارشناسی ارشد", "university_name": "دانشگاه صنعتی امیرکبیر", "thesis": { "title": "تحلیل داده‌های کلان", "score": 19 } }], "address": { "province": "تهران", "city": "تهران", "details": "خیابان شریعتی، 789" } }
{ "index": {} }
{ "first_name": "فاطمه", "last_name": "رضایی", "date_of_birth": "1373/02/22", "age": 30, "educational_background": [{ "degree": "کارشناسی", "university_name": "دانشگاه صنعتی نوشیروانی بابل", "thesis": { "title": "علم محیط زیست", "score": 16 } }, { "degree": "کارشناسی ارشد", "university_name": "دانشگاه علوم پزشکی تهران", "thesis": { "title": "انرژی‌های تجدیدپذیر", "score": 17 } }], "address": { "province": "مازندران", "city": "بابلسر", "details": "خیابان دریای شمال، 789" } }
{ "index": {} }
{ "first_name": "سعید", "last_name": "خانی", "date_of_birth": "1367/06/01", "age": 36, "educational_background": [{ "degree": "دکتری", "university_name": "دانشگاه صنعتی شریف", "thesis": { "title": "محاسبات کوانتومی", "score": 19 } }], "address": { "province": "تهران", "city": "تهران", "details": "خیابان انقلاب، 123" } }
{ "index": {} }
{ "first_name": "آیدا", "last_name": "پورمحمدی", "date_of_birth": "1370/08/11", "age": 33, "educational_background": [{ "degree": "کارشناسی", "university_name": "دانشگاه اصفهان", "thesis": { "title": "علم شناختی", "score": 18 } }, { "degree": "کارشناسی ارشد", "university_name": "دانشگاه تهران", "thesis": { "title": "تحقیق در علوم اعصاب", "score": 19 } }], "address": { "province": "اصفهان", "city": "اصفهان", "details": "خیابان چهارباغ، 111" } }
{ "index": {} }
{ "first_name": "نیما", "last_name": "کریمی", "date_of_birth": "1372/11/15", "age": 31, "educational_background": [{ "degree": "کارشناسی", "university_name": "دانشگاه شیراز", "thesis": { "title": "مدیریت فناوری اطلاعات", "score": 15 } }, { "degree": "کارشناسی ارشد", "university_name": "دانشگاه صنعتی نوشیروانی بابل", "thesis": { "title": "تحلیل سیستم‌های هوشمند", "score": 17 } }], "address": { "province": "فارس", "city": "شیراز", "details": "خیابان آزادی، 222" } }
{ "index": {} }
{ "first_name": "سمیه", "last_name": "سهرابی", "date_of_birth": "1374/04/18", "age": 30, "educational_background": [{ "degree": "کارشناسی", "university_name": "دانشگاه آزاد اسلامی", "thesis": { "title": "علوم اجتماعی", "score": 14 } }, { "degree": "کارشناسی ارشد", "university_name": "دانشگاه تهران", "thesis": { "title": "پژوهش‌های اجتماعی", "score": 19 } }], "address": { "province": "تهران", "city": "تهران", "details": "خیابان تجریش، 333" } }
{ "index": {} }
{ "first_name": "علی‌رضا", "last_name": "سلیمی", "date_of_birth": "1366/09/05", "age": 37, "educational_background": [{ "degree": "کارشناسی", "university_name": "دانشگاه تبریز", "thesis": { "title": "تاریخ تمدن", "score": 18 } }, { "degree": "کارشناسی ارشد", "university_name": "دانشگاه علامه طباطبایی", "thesis": { "title": "فلسفه علم", "score": 20 } }], "address": { "province": "آذربایجان شرقی", "city": "تبریز", "details": "خیابان فردوسی، 444" } }

```
&nbsp;
&nbsp;


### 1st query
```json
{
  "query": {
    "bool": {
      "must": [
        { "match": { "first_name": "علی" } },
        { 
          "range": { 
            "date_of_birth": { 
              "gte": "1370/01/01", 
              "lte": "1379/12/31"
            } 
          }
        }
      ]
    }
  }
}
```

### 2nd query
```json
{
  "query": {
    "nested": {
      "path": "educational_background",
      "query": {
        "bool": {
          "must": [
            { "match": { "educational_background.degree": "کارشناسی" } },
            {
              "nested": {
                "path": "educational_background.thesis",
                "query": {
                  "range": { "educational_background.thesis.score": { "gt": 17 } }
                }
              }
            }
          ]
        }
      }
    }
  }
}
```

### 3rd query
```json
{
  "query": {
    "range": {
      "age": { 
        "gte": 20, 
        "lte": 50 
      }
    }
  },
  "aggs": {
    "degree_distribution": {
      "nested": {
        "path": "educational_background"
      },
      "aggs": {
        "degree_count": {
          "terms": { "field": "educational_background.degree" }
        }
      }
    }
  },
  "size": 0
}
```

### 4th query
```json
{
  "query": {
    "bool": {
      "filter": [
        {
          "nested": {
            "path": "address",
            "query": {
              "bool": {
                "must": [
                  { "match": { "address.city": "تهران" } },
                  { "match": { "address.details": "ولیعصر" } }
                ]
              }
            }
          }
        }
      ]
    }
  },
  "aggs": {
    "score_distribution": {
      "nested": {
        "path": "educational_background.thesis"
      },
      "aggs": {
        "scores": {
          "histogram": {
            "field": "educational_background.thesis.score",
            "interval": 5
          }
        }
      }
    }
  },
  "size": 0
}
```

### 1st query with scripts
```json
{
  "query": {
    "bool": {
      "must": [
        {
          "script": {
            "script": {
              "source": "doc['first_name.keyword'].value.contains(params.fname) && (doc['date_of_birth'].value.getYear() >= params.min70 && doc['date_of_birth'].value.getYear() < params.max70)",
              "params" : {
                "fname" : "علی",
                "max70" : 1380,
                "min70" : 1370
              }
            }
          }
        }
      ]
    }
  }
}
```

### 2nd query with scripts
```json
{
  "query" : {
    "bool" : {
        "must" : [
            {
                "script_score" : {
                    "query" : {
                        "match_all" : {}
                    },
                    "script" : {
                        "source" : "{{script_below}}",
                        "params" : {
                            "degree" : "کارشناسی",
                            "min_grade" : 17
                        }
                    }
                }
            }
        ]
    }
  },
  "min_score" : 1
}
```
```java
if(params._source.educational_background != null){
    for (education in params._source.educational_background){
        if(education.degree == params.degree && education.thesis.score > params.min_grade){
            return 1;
        }
    }
    return 0;
}
```

### 3rd query with script
```json
{
  "query": {
    "range": {
      "age": { 
        "gte": 20, 
        "lte": 50 
      }
    }
  },
  "aggs": {
    "degree_distribution": {
      "scripted_metric": {
        "init_script": "{{init_script}}",
        "map_script": "{{map_script}}",
        "combine_script": "{{combine_script}}",
        "reduce_script": "{{reduce_script}}"
      }
    }
  },
  "size": 0
}
```
init script
```java
state.degree_counts = [:];
```
map script
```java
if (params['_source'].containsKey('educational_background')) {
    for (edu in params['_source']['educational_background']) {
        String degree = edu.degree;
        if (degree != null) {
            if (!state.degree_counts.containsKey(degree)) {
                state.degree_counts[degree] = 0;
            }
            state.degree_counts[degree] += 1;
        }
    }
}
```
combine script
```java
return state.degree_counts;
```

reduce script
```java
Map combined = [:];
for (s in states) {
    for (entry in s.entrySet()) {
        String degree = entry.getKey();
        int count = entry.getValue();
        if (!combined.containsKey(degree)) {
            combined[degree] = 0;
        }
        combined[degree] += count;
    }
}
return combined;
```
### 4th query with script
```json
{
  "query": {
    "bool": {
      "filter": [
        {
          "nested": {
            "path": "address",
            "query": {
              "bool": {
                "must": [
                  { "match": { "address.city": "تهران" } },
                  { "match": { "address.details": "ولیعصر" } }
                ]
              }
            }
          }
        }
      ]
    }
  },
  "aggs": {
    "score_distribution_scripted": {
      "scripted_metric": {
        "init_script": "{{init_script}}",
        "map_script": "{{map_script}}",
        "combine_script": "{{combine_script}}",
        "reduce_script": "{{reduce_script}}"
      }
    }
  },
  "size": 0
}
```
init script
```java
state.scores_histogram = new HashMap();
```
map script
```java
if (params['_source'].containsKey('educational_background')) {
    for (edu in params['_source']['educational_background']) {
        if (edu.containsKey('thesis') && edu['thesis'].containsKey('score')) {
            int score = edu['thesis']['score'];
            String bucket = score.toString();
            if (!state.scores_histogram.containsKey(bucket)) {
                state.scores_histogram[bucket] = 0;
            }
            state.scores_histogram[bucket] += 1;
        }
    }
}
```
combine script
```java
return state.scores_histogram;
```

reduce script
```java
Map final_histogram = new HashMap();
for (state in states) {
    for (entry in state.entrySet()) {
        String bucket = entry.getKey();
        int count = entry.getValue();
        if (!final_histogram.containsKey(bucket)) {
            final_histogram[bucket] = 0;
        }
        final_histogram[bucket] += count;
    }
}
return final_histogram;
```
