# PowerBI-project

Narzędzia: Microsoft Power BI, Power Query, DAX

## Opis projektu 
Projekt przedstawia analizę symulowanych danych sprzedażowych fikcyjnej firmy działającej na polskim rynku. Zbiór obejmuje ponad 16 tys. rekordów w 6 powiązanych tabelach, zawierających dane dotyczące sprzedaży, klientów, produktów, kanałów sprzedaży, geografii, kalendarza oraz miesięcznych celów sprzedażowych.

Główna tabela transakcyjna zawiera 15 000 rekordów sprzedaży, natomiast pozostałe tabele pełnią funkcję tabel wymiarów oraz danych pomocniczych. Dane obejmują okres od stycznia 2025 do grudnia 2026, czyli 24 miesiące historii sprzedaży.

## Cel biznesowy
Głównym celem projektu było stworzenie interaktywnego raportu umożliwiającego analizę:

- monitorowanie wyników sprzedaży w latach 2025–2026,
- ocena rentowności i marży,
- analiza udziału kanałów sprzedaży i segmentów klientów,
- identyfikacja najlepszych i najsłabszych produktów,
- porównanie wyników regionalnych,
- sprawdzanie trendu sprzedaży w czasie oraz realizacji celów.

## Struktura danych 
Model składa się z 7 tabel:
| Tabela                 | Liczba rekordów | Zawartość                                                                                                                  |
| ---------------------- | --------------: | -------------------------------------------------------------------------------------------------------------------------- |
| **sprzedaz** |          15 000 | dane transakcyjne: data sprzedaży i wysyłki, produkt, klient, region, kanał, ilość, cena, rabat, koszt i status zamówienia |
| **klienci**            |             500 | dane klientów: nazwa, segment, region, opiekun i przypisanie geograficzne                                                  |
| **produkty**           |              30 | katalog produktów: nazwa, kategoria, podkategoria, marka i cena bazowa                                                     |
| **kanaly**             |               4 | kanały sprzedaży wraz z ich typem                                                                                          |
| **geografia**          |               4 | regiony, centrale oraz menedżerowie regionalni                                                                             |
| **kalendarz**          |             737 | tabela dat obejmująca lata 2025–2026                                                                                       |


## Przygotowanie i czyszczenie danych
Dane zostały celowo przygotowane w sposób przypominający informacje pochodzące z różnych systemów biznesowych, dlatego przed rozpoczęciem analizy wymagają transformacji i uporządkowania.

Proces ETL został wykonany w Power Query. Narzędzie zostało wykorzystane do importu, transformacji oraz przygotowania danych do dalszego modelowania i analizy.
Proces czyszczenia danych rozpoczęto od tabeli sprzedaż 

![Tabela_sprzedaż](screens/Tabela_sprzedaz.png)
![Tabela_sprzedaż_1](screens/Tabela_sprzedaz_1.png)
![Tabela_sprzedaż_1](screens/Tabela_sprzedaz_2.png)

W ramach przygotowania danych:
- ustawiono prawidłowe nagłówki i typy danych, w tym właściwe ustawienia regionalne dla dat
- usunięto błędne wartości w kolumnach z datami,
- z nieuporządkowanych pól klientów i produktów wydobyto właściwe identyfikatory,
- ustandaryzowano identyfikatory klientów poprzez dodanie odpowiedniego prefiksu,
- usunięto zbędne spacje oraz niepotrzebne znaki,
- oczyszczono i ujednolicono nazwy regionów,
- ustandaryzowano wartości statusów zamówień do postaci Done, In progress oraz Cancelled,
- brakujące wartości rabatu zastąpiono zerami,
- usunięto zbędne kolumny techniczne,
zidentyfikowano i usunięto duplikaty transakcji.


Dodatkowo przygotowano i uporządkowano osobne tabele klientów, produktów, geografii oraz kanałów sprzedaży. Na podstawie danych sprzedażowych utworzono również słownik kanałów, a tabela produktów została wzbogacona o informacje o cenach oraz średniej cenie produktu wyliczonej z wykorzystaniem grupowania.

**Tabela klienci**
![Tabela_klienci](screens/Tabela_klienci.png)

**Tabela produkty**
![Tabela_produkty](screens/Tabela_produkty.png)

**Tabela geografia**
![Tabela_geografia](screens/Tabela_geografia.png)

**Tabela kanały**

![Tabela_kanały](screens/Tabela_kanały.png)


Jednym z elementów transformacji było przygotowanie dedykowanej tabeli kalendarza. Zakres tabeli został wyznaczony automatycznie na podstawie minimalnej i maksymalnej daty sprzedaży, dzięki czemu kalendarz dostosowuje się do zakresu danych źródłowych.

Tabela została wzbogacona m.in. o rok, numer i nazwę miesiąca, numer dnia tygodnia oraz nazwę dnia tygodnia. Nazwy miesięcy i dni zostały odpowiednio sformatowane, a kolumnom tekstowym przypisano właściwy porządek sortowania.

![Tabela_kalendarz](screens/Tabela_kalendarz.png)

Kod transformacji danych języku M znajduje się w folderze ETL 

## Model danych

Po zakończeniu transformacji przygotowano relacyjny model danych oparty na rozdzieleniu tabeli faktów od tabel wymiarów. Tabela sprzedaży pełni rolę centralnej tabeli faktów, natomiast dane opisujące klientów, produkty, kanały, geografię oraz czas służą jako wymiary pozwalające filtrować i grupować wyniki.

![Model](screens/Model.png)

Relacje zostały utworzone ręcznie, z uwzględnieniem prawidłowej kardynalności oraz kierunku filtrowania. Dla tabeli sprzedaży i kalendarza przygotowano dwie relacje: aktywną dla daty sprzedaży oraz nieaktywną dla daty wysyłki. Ukryto również techniczne kolumny kluczy oraz uporządkowano strukturę modelu w celu zwiększenia jego czytelności.

W dalszym etapie do modelu została dołączona także tabela miesięcznych celów sprzedażowych poprzez powiązanie jej z tabelami kalendarza i geografii.

![Model_relacyjny](screens/Model_gwiazda_platek.png)

## Analiza danych za pomocą DAX

W ramach projektu zbudowano zestaw miar DAX, które pozwalają na obliczanie:

- sumy ilości sprzedanych produktów,
- sprzedaży brutto i netto,
- średniej ceny i średniej wartości transakcji,
- kosztu sprzedaży,
- marży brutto i marży procentowej,
- liczby klientów i różnych produktów,
- udziału kanałów i segmentów klientów,
- średniego czasu od sprzedaży do wysyłki.

Pełny słownik miar w języku DAX znajduje się w pliku measures_DAX.txt


## Wyniki 
Raport prezentuje wyniki sprzedaży firmy w okresie od stycznia 2025 do grudnia 2026. Kluczowe wskaźniki obejmują sprzedaż brutto, liczbę transakcji, marżę procentową oraz dynamikę wzrostu sprzedaży w porównaniu z rokiem poprzednim. Na dashboardzie przedstawiono kluczowe wskaźniki biznesowe:

- Sprzedaż brutto — całkowita wartość sprzedaży w analizowanym okresie,
- Liczba transakcji — całkowity wolumen operacji sprzedażowych,
- Marża brutto i marża procentowa — ocena rentowności sprzedaży,
- Średnia wartość transakcji — wartość przeciętnego zamówienia,
- Koszt sprzedaży — nakłady związane z realizacją sprzedaży,
- Liczba klientów i liczba różnych produktów — zasięg i struktura oferty,
- Udział kanałów sprzedaży — udział poszczególnych kanałów w przychodach,
- Udział segmentów klientów — udział klienta w całkowitej sprzedaży,
- Dynamika YoY — zmiana sprzedaży względem poprzedniego roku.
  
![Raport strona 1](images/Strona_1.png)
![Raport strona 2](images/Strona_2.png)
![Raport strona 3](images/Strona_3.png)

## Wnioski 
Wnioski
Najważniejsze wnioski wynikające z analizy są następujące:

- sprzedaż rośnie dynamicznie, co wskazuje na poprawę efektywności handlowej i wzrost skali działalności,
- marża procentowa utrzymuje się na wysokim poziomie,
- największy udział w przychodach mają kanały partnerskie i online, co sugeruje przewagę modelu sprzedaży cyfrowej i opartej na partnerstwach,
- najważniejszą rolę odgrywają segmenty klientów B2B i VIP,
- sprzedaż jest silnie zróżnicowana według produktów i regionów, a niektóre obszary generują zdecydowanie większą wartość niż inne,
- sezonowość ma duże znaczenie dla wyników, dlatego planowanie sprzedaży powinno uwzględniać cykle miesięczne,
- realizacja zamówień jest na wysokim poziomie, a udział zamówień anulowanych jest niski. 