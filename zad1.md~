NoSQL_UG
========

#Wstęp

Wszystkie zadania zostały wykonane na Ubuntu 14.04 LTS.
Sprzęt: Procesor Intel Core i5-M 450 @ 2.40GHz × 4 Dysk HDD 500 GB. Pamięć RAM 4GB.

======

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
========

##Zadanie 1.
a) Zaimportować, do systemów baz danych uruchomionych na swoim komputerze, danych z pliku Train.csv bazy:
- MongoDB
- PostgreSQL

b) Zliczyć liczbę zaimportowanych rekordów (Odpowiedź: powinno ich być 6_034_195).

c) (Zamiana formatu danych.) Zamienić string zawierający tagi na tablicę napisów z tagami następnie zliczyć wszystkie tagi i wszystkie różne tagi.

d) Brak polecenia.

e) Wyszukać w sieci dane zawierające obiekty GeoJSON. Następnie dane zapisać w bazie MongoDB.

========
#MongoBD
#Zad1 a) b)
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
=======
#c)

Zadanie zostało wykonane przy pomocy PyMongo. Skrypt wykorzystany do zadania:
```sh

```

Otrzymane wyniki:
```sh

```

Wykorzystanie zasobów podczas przetwarzania pliku:
![image](zdj3.png)

========
#d)
brak polecenia na stronie

========
#e)



========
#PostgreSQL
#Zad1 a) b)
początkowo trzeba stworzyć tabelę:
```sh
>psql magda
>>CREATE TABLE train (Id serial PRIMARY KEY, Title VARCHAR, Body VARCHAR, Tags VARCHAR);
```

PostgreSQL umożliwia mieżenie czasu komendą:
```sh
>\timing
```

w wyniku czego otrzymany został czas oraz automatycznie wynik zliczenia wszystkich wierszy:
```sh
magda=# COPY train (Id, Title, Body, Tags) FROM '/home/magda/Pobrane/Train_changed2.csv' WITH DELIMITER ',' CSV HEADER;
COPY 6034195
Time: 889515,013 ms
```
czyli w przeliczeniu otrzymany został czas 14m49.515s

![image](zdj2.png)

Przeliczenie ilości wystąpień w bazie:
```sh
>SELECT COUNT(*) FROM train;
  count
`---------
 6034195
(1 row)

Time: 394345,551 ms
```

===

#c)



===

