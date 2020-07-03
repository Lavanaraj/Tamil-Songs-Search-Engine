# Tamil-Songs-Search-Engine
# Tamil-Songs-Search-Engine
## Description
There are nearly about 4000 Tamil songs collected from 'https://www.tamilpaa.com/tamil-movies-list'. These collected songs are included tamil_corpus.txt. These songs have 9 meta data such as movie, song, music, lyrics, singers, year, actors, song_url and full_lyrics. Randomly selected nearly 1000 songs data are fully translated to Tamil using python translate library. There is a search engine developed by using Elastic Search and Kibana.
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
