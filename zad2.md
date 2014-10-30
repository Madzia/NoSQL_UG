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

Dane zostały ściągnięte ze (strony)[http://archive.ics.uci.edu/ml/datasets/Individual+household+electric+power+consumption], link to pliku z danymi: (link)[http://archive.ics.uci.edu/ml/machine-learning-databases/00235/household_power_consumption.zip] plik zip zawierający 2075259 rekordów z 9 atrybutami każdy. Plik zawiera informacje dotyczące zużycia prądu.

Przykładowy rekord:
```sh
Date;Time;Global_active_power;Global_reactive_power;Voltage;Global_intensity;Sub_metering_1;Sub_metering_2;Sub_metering_3
16/12/2006;17:24:00;4.216;0.418;234.840;18.400;0.000;1.000;17.000
```

======

#zad 2. b)

