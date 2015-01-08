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
time ./mongoimport -d imdb -c imdb --type json --file ../../getglue_sample.json
```

Wynik:
```json
...
2015-01-08T00:01:17.973+0100    Progress: 19602500 documents inserted...
2015-01-08T00:01:18.203+0100    Progress: 19663900 documents inserted...
2015-01-08T00:01:18.424+0100    Progress: 19779800 documents inserted...
2015-01-08T00:01:21.605+0100    imported 19831300 documents
```

Czas:
```json
real	15m12.158s
user	6m12.119s
sys	0m32.654s
```

Sprawdzenie:
```json

```

##Wczytanie danych do bazy Mongo 2.4.12

```json
time mongoimport -d imdb2 -c imdb2 --type json --file getglue_sample.json
```

Wynik:
```json
Thu Jan  8 00:15:54.007 			19602700	18050/second
Thu Jan  8 00:15:57.005 		Progress: 11367988192/11454208342	99%
Thu Jan  8 00:15:57.005 			19679900	18071/second
Thu Jan  8 00:16:00.057 		Progress: 11425068413/11454208342	99%
Thu Jan  8 00:16:00.057 			19779700	18113/second
Thu Jan  8 00:16:02.365 check 9 19831301
Thu Jan  8 00:16:02.792 imported 19831300 objects
```

Czas:
```json
real	18m14.128s
user	7m18.132s
sys	0m36.657s
```

Sprawdzenie:
```json
mongo
MongoDB shell version: 2.4.12
connecting to: test
> use imdb2
switched to db imdb2
> db.imdb2.count()
19831300
```

| Baza Danych                             |             Czas          | Objętość bazy danych |
|-----------------------------------------|---------------------------|----------------------|
| MongoDB v 2.4.12                        |      real: 18m14.128s     |       15.946GB       |
| MongoDB v 2.8.0-rc0 WiredTiger (zlib)   |      real: 15m12.158      |       12.531GB       |


=====

#suma kontrolna oraz ilość kategorii(ModelName):
Skrypt
```js
baza = db.imdb;

map = function(){
  var x = this.modelName;
  emit(x,1);
};

reduce = function(key,values){
  return Array.sum(values);
};


var a = baza.mapReduce(map, reduce, {out: "wynik"});
printjson(a);
``` 

Wynik:
```js
connecting to: imdb
{
	"result" : "wynik",
	"timeMillis" : 282043,
	"counts" : {
		"input" : 19831300,
		"emit" : 19831300,
		"reduce" : 324535,
		"output" : 5
	},
	"ok" : 1,
}
```

Rodajów kategorii było 5.

Czas:
```js
real	4m42.119s
user	0m0.039s
sys	0m0.016s
```
#najczęśćiej używane słowa w tytułach

Skrypt
```js
baza = db.imdb;

map = function(){
  var x = this.title;
  emit(x,1);
};

reduce = function(key,values){
  var result = 0;
  values.forEach(function(item){
     result += item;
  });
  return result;
};


var a = baza.mapReduce(map, reduce, {out: "wynik"});
printjson(a);
``` 

Wynik:
```js
{
	"result" : "wynik",
	"timeMillis" : 828431,
	"counts" : {
		"input" : 19831300,
		"emit" : 19831300,
		"reduce" : 2600667,
		"output" : 52618
	},
	"ok" : 1,
}
```

Czas:
```js
real	13m48.508s
user	0m0.045s
sys	0m0.008s
```

#powtarzające się inicjały w zakładce director dla wszystkich filmów

Skrypt
```js
baza = db.imdb;

map = function(key,values){
  if (this.modelName == "movies") {
  this.director.match(/\b\w/g).forEach(function(word) {
     emit(word,1);
  });}
};

reduce = function(key,values){
return Array.sum(values);
};

var a = baza.mapReduce(map, reduce, {out: "wynik"});
printjson(a);
``` 

Wynik:
```js
connecting to: imdb
{
	"result" : "wynik",
	"timeMillis" : 429832,
	"counts" : {
		"input" : 19831300,
		"emit" : 13756198,
		"reduce" : 1017539,
		"output" : 56
	},
	"ok" : 1,
}
```

Rodajów kategorii było 5.

Czas:
```js
real	7m9.886s
user	0m0.036s
sys	0m0.018s
```

#Porównanie wyników:

| Baza Danych                           | suma kontrolna   | najczestrze slowa | inicjały       |
|---------------------------------------|------------------|-------------------|----------------|
| MongoDB v 2.4.12                      |  real 4m42.119s  |  real 13m48.508s  | real 7m9.886s  |
| MongoDB v 2.8.0-rc0 WiredTiger (zlib) |  real 4m02.421s  |  real 11m64.542s  | real 6m43.553s |

Jak widać dla każdego z przypadków MongoDB w wersji 2.8.0-rc0 używając WiredTiger z zlib wypadło lepiej niż dla wersji 2.4.12. Dla bardziej skomplikowanych obliczeń (przypadek drugi) zmiana ta jest dużo bardziej zauważalna.

=====

#Optymalizacja MapReduce

#modyfikacja skryptu dla sumy kontrolnej oraz ilości kategorii(ModelName):

Należało trochę przerobić skrypt z mapReduce na runCommand:
```js
baza = db.imdb;

map = function(){
  var x = this.modelName;
  emit(x,1);
};

reduce = function(key,values){
  return Array.sum(values);
};

var b = baza.runCommand({
  mapreduce: "imdb",
  map: map,
  reduce: reduce,
  out: "wynik",
  jsMode: true
})
printjson(b);
```

Wynik jak widać jest ten sam:
```js
connecting to: imdb
{
	"result" : "wynik",
	"timeMillis" : 248266,
	"counts" : {
		"input" : 19831300,
		"emit" : 19831300,
		"reduce" : 324711,
		"output" : 5
	},
	"ok" : 1
}
```

Następnie trzeba przerobić program na wielowątkowy:
```js

```
