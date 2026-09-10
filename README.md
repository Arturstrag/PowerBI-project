# PowerBI-project

Narzędzia: Microsoft Power BI, Power Query, DAX

## Opis projektu 
Projekt przedstawia analizę symulowanych danych sprzedażowych fikcyjnej firmy działającej na polskim rynku. Zbiór obejmuje ponad 16 tys. rekordów w 7 powiązanych tabelach, zawierających dane dotyczące sprzedaży, klientów, produktów, kanałów sprzedaży, geografii, kalendarza oraz miesięcznych celów sprzedażowych.

Główna tabela transakcyjna zawiera 15 000 rekordów sprzedaży, natomiast pozostałe tabele pełnią funkcję tabel wymiarów oraz danych pomocniczych. Dane obejmują okres od stycznia 2025 do grudnia 2026, czyli 24 miesiące historii sprzedaży.

## Struktura danych 
Model składa się z 7 tabel:
| Tabela                 | Liczba rekordów | Zawartość                                                                                                                  |
| ---------------------- | --------------: | -------------------------------------------------------------------------------------------------------------------------- |
| **sprzedaz_surowa_v2** |          15 000 | dane transakcyjne: data sprzedaży i wysyłki, produkt, klient, region, kanał, ilość, cena, rabat, koszt i status zamówienia |
| **klienci**            |             500 | dane klientów: nazwa, segment, region, opiekun i przypisanie geograficzne                                                  |
| **produkty**           |              30 | katalog produktów: nazwa, kategoria, podkategoria, marka i cena bazowa                                                     |
| **kanaly**             |               4 | kanały sprzedaży wraz z ich typem                                                                                          |
| **geografia**          |               4 | regiony, centrale oraz menedżerowie regionalni                                                                             |
| **kalendarz**          |             737 | tabela dat obejmująca lata 2025–2026                                                                                       |
| **cele_miesieczne**    |              96 | miesięczne cele sprzedażowe dla czterech regionów                                                                          |

## Przygotowanie i czyszczenie danych
Dane zostały celowo przygotowane w sposób przypominający informacje pochodzące z różnych systemów biznesowych, dlatego przed rozpoczęciem analizy wymagają transformacji i uporządkowania.

Proces ETL został wykonany w Power Query. Narzędzie zostało wykorzystane do importu, transformacji oraz przygotowania danych do dalszego modelowania i analizy.
Proces czyszczenia danych rozpoczęto od tabeli sprzedaż 

![Tabela_sprzedaż](screens/Tabela_sprzedaż.png)
![Tabela_sprzedaż_1](screens/Tabela_sprzedaż_1.png)
![Tabela_sprzedaż_2](screens/Tabela_sprzedaż_2.png)







