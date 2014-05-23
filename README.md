Groovy lang Plugin for Elasticsearch
==================================

The Groovy language plugin allows to have `groovy` as the language of scripts to execute.

In order to install the plugin, simply run: `bin/plugin -install elasticsearch/elasticsearch-lang-groovy/2.0.0`.

* For master elasticsearch versions, look at [master branch](https://github.com/elasticsearch/elasticsearch-lang-groovy/tree/master).
* For 1.3.x elasticsearch versions, look at [es-1.3 branch](https://github.com/elasticsearch/elasticsearch-lang-groovy/tree/es-1.3).
* For 1.2.x elasticsearch versions, look at [es-1.2 branch](https://github.com/elasticsearch/elasticsearch-lang-groovy/tree/es-1.2).
* For 1.1.x elasticsearch versions, look at [es-1.1 branch](https://github.com/elasticsearch/elasticsearch-lang-groovy/tree/es-1.1).
* For 1.0.x elasticsearch versions, look at [es-1.0 branch](https://github.com/elasticsearch/elasticsearch-lang-groovy/tree/es-1.0).
* For 0.90.x elasticsearch versions, look at [es-0.90 branch](https://github.com/elasticsearch/elasticsearch-lang-groovy/tree/es-0.90)

|     Groovy Lang Plugin      |    elasticsearch    |  groovy  | Release date |
|-----------------------------|---------------------|----------|:------------:|
| 2.3.0-SNAPSHOT              | 1.3.0 -> 1.3        |  2.2.1   |  XXXX-XX-XX  |

Please read documentation relative to the version you are using:

* [2.3.0-SNAPSHOT](https://github.com/elasticsearch/elasticsearch-lang-groovy/blob/es-1.3/README.md)

Using groovy with function_score
--------------------------------

Let's say you want to use `function_score` API using `groovy`. Here is
a way of doing it:

```sh
curl -XDELETE "http://localhost:9200/test"

curl -XPUT "http://localhost:9200/test/doc/1" -d '{
  "num": 1.0
}'

curl -XPUT "http://localhost:9200/test/doc/2?refresh" -d '{
  "num": 2.0
}'

curl -XGET "http://localhost:9200/test/_search" -d'
{
  "query": {
    "function_score": {
      "script_score": {
        "script": "Math.pow(doc[\"num\"].value, 2)",
        "lang": "groovy"
      }
    }
  }
}'
```

gives

```javascript
{
   // ...
   "hits": {
      "total": 2,
      "max_score": 4,
      "hits": [
         {
            // ...
            "_score": 4
         },
         {
            // ...
            "_score": 1
         }
      ]
   }
}
```

Using groovy with script_fields
-------------------------------

```sh
curl -XDELETE "http://localhost:9200/test"

curl -XPUT "http://localhost:9200/test/doc/1?refresh" -d'
{
  "obj1": {
   "test": "something"
  },
  "obj2": {
    "arr2": [ "arr_value1", "arr_value2" ]
  }
}'

curl -XGET "http://localhost:9200/test/_search" -d'
{
  "script_fields": {
    "s_obj1": {
      "script": "_source.obj1", "lang": "groovy"
    },
    "s_obj1_test": {
      "script": "_source.obj1.test", "lang": "groovy"
    },
    "s_obj2": {
      "script": "_source.obj2", "lang": "groovy"
    },
    "s_obj2_arr2": {
      "script": "_source.obj2.arr2", "lang": "groovy"
    }
  }
}'
```

gives

```javascript
{
  // ...
  "hits": [
     {
        // ...
        "fields": {
           "s_obj2_arr2": [
              [
                 "arr_value1",
                 "arr_value2"
              ]
           ],
           "s_obj1_test": [
              "something"
           ],
           "s_obj2": [
              {
                 "arr2": [
                    "arr_value1",
                    "arr_value2"
                 ]
              }
           ],
           "s_obj1": [
              {
                 "test": "something"
              }
           ]
        }
     }
  ]
}
```

License
-------

    This software is licensed under the Apache 2 license, quoted below.

    Copyright 2009-2014 Elasticsearch <http://www.elasticsearch.org>

    Licensed under the Apache License, Version 2.0 (the "License"); you may not
    use this file except in compliance with the License. You may obtain a copy of
    the License at

        http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS, WITHOUT
    WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the
    License for the specific language governing permissions and limitations under
    the License.
