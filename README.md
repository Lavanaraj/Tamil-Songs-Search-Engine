# Tamil-Songs-Search-Engine
## Description
The data is scraped for 900 movies collected from 'https://www.tamilpaa.com/tamil-movies-list' and then converted to all the songs in the moveis per line then translated to tamil for the metadata which are in english such as "movie_name","song_name", "song_music", "song_lyrics", "song_singers", "year", "actors", "song_rating", "song_type" and "song_fulllyrics" are added to improve the quality of the search and stored in JSON format.There is a search engine developed by using Elastic Search and Kibana.

## Sample Movie Song JSON format
```json
{
	"movie_name": "உன்னாலே உன்னாலே", 
	"song_name": "சிறு சிறு உறவுகள", 
	"song_music": "ஹரிஸ் ஜெயராஜ்", 
	"song_lyrics": "அருண்ராஜா காமராஜ்", 
	"song_singers": "கிரிஷ்", 
	"year": "2007", 
	"actors": "வினய் ராய், சாதா", 
	"song_rating": 10, 
	"song_type": "சோக பாட்டு", 
	"song_fulllyrics": "ஒஹ்… ஒவ்… ஒ ஒ ஒ ஒ… ஒவ்…\nம்… ம்… ம்ம்ம்ம்…\nஒஹ்… ஒவ்… ஒ ஒ ஒ ஒ… ஒவ்…\nம்… ம்… ம்ம்ம்ம்…\nசிறு சிறு உறவுகள் பிரிவுகள் என் நினைவுக்குள் …ஓ\nவர வர கசக்குது கசக்குது என் இளமையும் …ஹேய்\nநினைத்தது நடந்தது முடிந்தது என் கனவுக்குள் …ஆ\nஎன்னாச்சோ தெரியலையே…\n\nஊ…ஊ…ஊ\n\nசிறு சிறு உறவுகள் பிரிவுகள் என் நினைவுக்குள் …ஓ\n\nநினைவுக்குள் …ஓ\n\nவர வர கசக்குது கசக்குது என் இளமையும் …ஹேய்\n\nஇளமையும் …ஹேய்\n\nநினைத்தது நடந்தது முடிந்தது என் கனவுக்குள் …ஹா\n\nஎன்னாச்சோ தெரியலையே…\n\nநன… நன….\nபப… ப ப…\nலல… ல ல…\nஒவ்… ஒ ஒ…"
}
```

## Directory Structure
---
```
 ├── analyzers : Custom filters (Stemmers,stoppingwords)
 ├── tamil_songs_scraped_lyrics.txt : Scraped lyrics are found in this file
 ├── preprocessed_tamil_songs_lyrics.txt : Preprocessed lyrics after translating are in this file
 ├── tamil_songs_corpus_final.txt : Bulk api format data are in this file
 ├── Preprocessed_Songs_Data.ipynb : This file is used to process the scraped data
 ├── web_scraping.ipynb : This is to scrape data
```


## Queries for ElasticSearch search engine
```

# top 10 songs from 2018 to 2020
GET /songs_index/songs/_search
{
    "size" : 10,
    "query": {
        "range" : {
            "year": {
                "gte" : "2018",
                "lte" :  "2020"
            }
        }
    }
}

# search song_music field for ஏ.ஆர்.ரஹ்மான்
GET /songs_index/songs/_search
{
    "query": {
        "multi_match" : {
            "query" : "ஏ.ஆர்.ரஹ்மான்",
            "fields": ["song_music"], 
            "fuzziness": "AUTO"
        }
    }
}

# search song_fulllyrics for வகண்ணாலசொக்குறேன்
GET /songs_index/_search
{
  "query": {
    "multi_match" : {
      "query":    "வகண்ணாலசொக்குறேன்",
      "fields": ["song_fulllyrics"], 
      "fuzziness": "AUTO"
    }
  }
}

# search all fields for வகண்ணாலசொக்குறேன்
GET /songs_index/_search
{
  "query": {
    "multi_match" : {
      "query":    "வகண்ணாலசொக்குறேன்",
      "fuzziness": "AUTO"
    }
  }
}

# get top 2 results sorted using song_rating from 2017 to 2020 
GET /songs_index/songs/_search
{
    "_source":["movie_name", "song_name" ],
    "size" : 2,
     "sort" : [
        { "song_rating" : {"order" : "desc"}}
    ],
    "query": {
        "range" : {
            "year" : {
                "gte" : "2017",
                "lte" :  "2020"
            }
        }
    }
}

# get top 2 results sorted using song_rating ஏ.ஆர்.ரஹ்மான்  and குத்துபாட்டு from 2013 to 2020 
GET /songs_index/songs/_search
{
  "size" : 2,
     "sort" : [
        { "song_rating" : {"order" : "desc"}}
    ],
  "query": {
    "bool": { 
      "must": [
        {"match": {"song_music": "ஏ.ஆர்.ரஹ்மான்"}},
		    {"match": { "song_type":"குத்துபாட்டு" }},

        {"range": {
          "year": {
            "gte": 2013,
            "lte": 2020
          }
        }}
      ]
    }
}}

# custom stop words and stemming
PUT /songs_index/
{
       "settings": {
           "analysis": {
               "analyzer": {
                   "my_analyzer": {
                       "tokenizer": "standard",
                       "filter": ["custom_stopper","custom_stems"]
                   }
               },
               "filter": {
                   "custom_stopper": {
                       "type": "stop",
                       "stopwords_path": "analyzers/tamil_stopwords.txt"
                   },
                   "custom_stems": {
                       "type": "stemmer_override",
                       "rules_path": "analyzers/tamil_stemming.txt"
                   }
               }
           }
       }
  }
  
# search ஏ.ஆர்.ரஹ்மான் music and year from 2013 to 2020
GET /songs_index/songs/_search
{
  "query": {
    "bool": { 
      "must": [
        {"match": {
          "song_music": "ஏ.ஆர்.ரஹ்மான்"
        }},
        {"range": {
          "year": {
            "gte": 2013,
            "lte": 2020
          }
        }}
      ]
    }
}}

# search இளையராஜா  or யுவன்சங்கர்ராஜா songs
GET /songs_index/songs/_search
{
  "query": {
    "bool": {
      "should": [
        {
          "bool": {
            "must": [
              { "match": { "song_music":"இளையராஜா" } }
            ]
          }
        },
        {
          "bool": {
            "must": [
              { "match": { "song_music":"யுவன்சங்கர்ராஜா" } }
            ]
          }
        }
      ]
    }
  }
}

# delete data having நீதானேஎன்பொன்வசந்தம் and சற்றுமுன்புபார்த்
POST /songs_index/_delete_by_query
{
 "query": {
   "bool": {
      "must": [
        { "match": { "movie_name":"நீதானேஎன்பொன்வசந்தம்" } },
        { "match": { "song_name":"சற்றுமுன்புபார்த்" }}
      ]
    }
  }
}

# update data என்சுவாசகாற்றே for movie_name if year is 1920
POST /songs_index/_update_by_query
{
  "script": {
    "source": "ctx._source.movie_name = 'என்சுவாசகாற்றே'",
    "lang": "painless"
  },
  "query": {
    "bool": {
      "must": [
        { "match": { "year":"1920" } }
      ]
    }
  }
}



# get singer ஏ.ஆர்.ரஹ்மான் not music ஏ.ஆர்.ரஹ்மான்
GET /songs_index/songs/_search
{

  "query": {
    "bool": { 
      "must": [
        {"match": {"song_singers": "ஏ.ஆர்.ரஹ்மான்"}}

      ] ,
	  "must_not": [
        {"match": {"song_music": "ஏ.ஆர்.ரஹ்மான்"}}

      ] 
    }
}}

```
