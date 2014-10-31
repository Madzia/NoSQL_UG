NoSQL_UG - Magdalena Sarzyńska
========

#Wstęp

Wszystkie zadania zostały wykonane na Ubuntu 14.04 LTS.
Sprzęt: Procesor Intel Core i5-M 450 @ 2.40GHz × 4 Dysk HDD 500 GB. Pamięć RAM 4GB.

======

##Zadanie 2.

a) Wyszukać w sieci dane zawierające co najmniej 1_000_000 rekordów/jsonów.

b) Dane zapisać w bazie MongoDB.

c) Przygotować w JavaScript co najmniej cztery agregacje korzystające z Aggregation Pipeline.

d) Zaprogramować i wykonać wszystkie agregacje z pkt. 3 w_innym niż JavaScript języku programowania. Skorzystać z jednego z driverów wymienionych na stronie MongoDB Drivers.

e) Wyniki przedstawić w postaci tabelek, graficznej (wykresów, itp.).

=======

#zad 2. a)

Dane zostały ściągnięte ze [strony](http://archive.ics.uci.edu/ml/datasets/Individual+household+electric+power+consumption), link to pliku z danymi: [link](http://archive.ics.uci.edu/ml/machine-learning-databases/00235/household_power_consumption.zip) plik zip zawierający 2075259 rekordów z 9 atrybutami każdy. Plik zawiera informacje dotyczące zużycia prądu.

Przykładowy rekord:
```sh
Date;Time;Global_active_power;Global_reactive_power;Voltage;Global_intensity;Sub_metering_1;Sub_metering_2;Sub_metering_3
16/12/2006;17:24:00;4.216;0.418;234.840;18.400;0.000;1.000;17.000
```

Oraz poprawiam dane poleceniem:
```sh
cat household_power_consumption.txt | tr ";" "," > power.txt 
```

Przykładowy wynik po poprawie:
```sh
>head -n 3 power.txt 
Date,Time,Global_active_power,Global_reactive_power,Voltage,Global_intensity,Sub_metering_1,Sub_metering_2,Sub_metering_3
16/12/2006,17:24:00,4.216,0.418,234.840,18.400,0.000,1.000,17.000
16/12/2006,17:25:00,5.360,0.436,233.630,23.000,0.000,1.000,16.000
```

======

#zad 2. b)

Wczytywanie danych do bazy Mongo:
```sh
time mongoimport -d power -c power --type csv --headerline --file power.txt
```

Wynik operacji:
```sh
Fri Oct 31 21:28:06.022 		Progress: 131383460/132960755	98%
Fri Oct 31 21:28:06.022 			2051200	34186/second
Fri Oct 31 21:28:06.598 check 9 2075260
Fri Oct 31 21:28:07.310 imported 2075259 objects
```

Czasy:
```sh
real	1m1.264s
user	0m6.460s
sys	0m1.388s
```

![image](screens/z2zdj1.png)

Sprawdzenie poprawnego wprowadzenia wyników:
```sh
> db.power.find().limit(1)
{	
	"_id" : ObjectId("5453f425c520b3a1d86d3b80"), 
	"Date" : "16/12/2006", 
	"Time" : "17:24:00", 
	"Global_active_power" : 4.216, 
	"Global_reactive_power" : 0.418, 
	"Voltage" : 234.84, 
	"Global_intensity" : 18.4, 
	"Sub_metering_1" : 0, 
	"Sub_metering_2" : 1, 
	"Sub_metering_3" : 17 
}
```

Oraz przeliczenie wprowadzonych danych:
```sh
db.power.count()
2075259
```

#zad 2. c)


