NoSQL_UG - Magdalena Sarzyńska
========

#Wstęp

Wszystkie zadania zostały wykonane na Ubuntu 14.04 LTS.
Sprzęt: Procesor Intel Core i5-M 450 @ 2.40GHz × 4 Dysk HDD 500 GB. Pamięć RAM 4GB.

======

##Zadanie 3.

[GetGlue and Timestamped Event Data](http://getglue-data.s3.amazonaws.com/getglue_sample.tar.gz) (ok. `11 GB`, `19 831 300` json-ów, próbka 100 jsonów [getglue101](https://github.com/nosql/aggregations-2/blob/master/data/wbzyl/getglue101.json)). Są to dane z [IMDB](http://www.imdb.com/) z lat 2007–2012, tylko filmy i przedstawienia TV. 

Przykładowy dokument `json`:

```json
{
  "_id": ObjectId("5374418642gf2a2b34540421"),
  "comment": "", 
  "modelName": "movies", 
  "displayName": "", 
  "title": "Short Circuit", 
  "timestamp": "2008-10-29T04:50:41Z", 
  "image": "http://ia.media-imdb.com/images/M/MV5BMTM3NzIwNjE4OV5BMl5BanBnXkFtZTcwNjk0MTUxMQ@@.jpg", 
  "userId": "brianhaddad", 
  "private": "false", 
  "director": "john badham", 
  "source": "", 
  "version": "1", 
  "link": "http://www.imdb.com/title/tt0091949/", 
  "lastModified": "2011-12-16T19:39:33Z", 
  "action": "Liked", 
  "lctitle": "short circuit", 
  "objectKey": "movies/short_circuit/john_badham"
}
```

Po rozpakowaniu ściągniętego pliku z danymi poleceniem:
```json
tar -xf getglue_sample.tar.gz
```

Należy dane wczytać do bazy danych. Wykorzystana w tym celu będzie wersja Mongo 2.8.0.rc0 + wiredtiger +zlib, a następnie Mongo 2.4.12. Następnie w tabeli przedstawione będą czasy działania.

======

##Wczytanie danych do bazy Mongo 2.8.0.rc0 + wiredtiger + zlib

```json
./mongod --storageEngine=wiredtiger --wiredTigerCollectionConfig=zlib
time mongoimport -d imdb -c imdb --type json --file getglue_sample.json
```

Wynik:
```json

```

Czas:
```json

```

Sprawdzenie:
```json

```

##Wczytanie danych do bazy Mongo 2.4.12

```json
time mongoimport -d imdb -c imdb --type json --file getglue_sample.json
```

Wynik:
```json

```

Czas:
```json

```

Sprawdzenie:
```json

```

| Baza Danych                             |             Czas          | Objętość bazy danych |
|-----------------------------------------|---------------------------|----------------------|
| MongoDB v 2.4.12                        |      real:       |              |
| MongoDB v 2.8.0-rc0 WiredTiger (zlib)   |      real:       |              |

