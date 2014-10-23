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
>head -n 6034196 Train_changed.csv > Train_changed.csv
```
Sprawdziłam ilość linii otrzymanych po przetworzeniu pliku:
```js
>wc -l Train_changed.csv
6034195 Train_changed.csv
```
#Zadanie 1.
#a) i b)


