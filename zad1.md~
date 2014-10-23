NoSQL_UG
========

Przetworzyłam plik Train.csv następującymi komendami:
```js
>cat Train.csv | tr "\n" " " | tr "\r" "\n" > Train_changed.csv
```
Sprawdziłam ilość linii w otrzymanym pliku:
```js
>wc -l Train_changed.csv
6034196 Train_changed.csv
```
Usunęłam ostatnią (pustą) linię dodaną podczas korekty początkowej:
```js
>head -n 6034196 Train_changed.csv > Train_changed2.csv
```
Sprawdziłam ilość linii otrzymanych po przetworzeniu pliku:
```js
>wc -l Train_changed2.csv
6034195 Train_changed2.csv
```
##Zadanie 1. a) b)
a) Zaimportować, do systemów baz danych uruchomionych na swoim komputerze, danych z pliku Train.csv bazy:
*MongoDB
*PostgreSQL
b) Zliczyć liczbę zaimportowanych rekordów (Odpowiedź: powinno ich być 6_034_195).
========
Wczytywanie do bazy MongoBD:
```sh
time mongoimport -d train -c train --type csv --headerline --file Train_changed2.csv 
```
Wynik operacji:
```sh
Thu Oct 23 17:45:52.165 		Progress: 7196771848/7253917399	99%
Thu Oct 23 17:45:52.165 			5986600	10671/second
Thu Oct 23 17:45:54.693 check 9 6034196
Thu Oct 23 17:45:55.077 imported 6034195 objects
```
Czasy:
```sh
real	9m23.553s
user	1m55.715s
sys	0m14.405s
```
![image](zdj1.png)
Przeliczenie ilości wystąpień w bazie:
```js
>mongo
MongoDB shell version: 2.4.9
connecting to: test
> use train
switched to db train
> db.train.count()
6034195
```




