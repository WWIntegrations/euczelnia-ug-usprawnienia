# Testy i kryteria odbioru
Testować na środowisku testowym z danymi syntetycznymi, w uzgodnionym oknie. To plan testów; testów systemu UG nie wykonano.

## Interfejs
| Scenariusz | Wynik wymagany |
| --- | --- |
| Lista 200 pozycji, przeciągnięcie 150 -> 1 | Możliwe jednym gestem, z automatycznym przewijaniem do góry |
| Pozycja 1 -> 200 | Automatyczne przewijanie w dół, poprawny wynik |
| Nieruchomy wskaźnik przy krawędzi | Lista nadal się przewija |
| Wskaźnik w środku | Brak automatycznego przewijania |
| Nagłówek sticky, zagnieżdżony scroll | Ruch właściwego kontenera, cel niezasłonięty |
| Szybka zmiana kierunku | Brak skoków i utraty przeciąganej pozycji |
| Wirtualizacja, jeśli występuje | Poprawne ID i cel także po zmianie renderowanych wierszy |
| Escape/pointercancel/utrata fokusu | Zakończenie gestu; anulowanie nie zatwierdza przypadkowej kolejności |
| Powtarzane gesty i opuszczenie widoku | Brak pozostawionych pętli i listenerów |
| iOS Safari i Android Chrome | Poprawny gest; zwykły scroll działa poza uchwytem |
| Klawiatura i przyciski bez przeciągania | Ten sam efekt zmiany kolejności, zachowany fokus |
| Błąd zapisu, odświeżenie, dwie karty | Czytelny status, brak cichego nadpisania i pozornego sukcesu |

## Zapisy i obciążenie
Przeprowadzić baseline, test narastania, spike na otwarcie i publikację wyników oraz dłuższy przebieg. Uwzględnić logowanie, katalog, zapis i wysłanie preferencji, odczyt statusu i wyniki. Sam GET strony głównej nie odtwarza zapisów. Dla szczytu użyć modelu napływu, który nie zmniejsza automatycznie ruchu tylko dlatego, że serwer zwolnił.

Liczbę użytkowników, tempo napływu, zapas i czas testu wyznaczyć z liczby uprawnionych i historycznych metryk. Propozycja do uzgodnienia: p95 przyjęcia zgłoszenia <= 2 s i p99 <= 5 s przy ustalonym szczycie. Są to cele testowe, nie pomiary obecnego systemu; osobno ustalić czas końcowego przydziału.

Wymagania integralności:
- zero utraconych trwale potwierdzonych zgłoszeń;
- zero duplikatów skutku po ponowieniu;
- brak przekroczenia limitu przy konkurencji o ostatnie miejsce;
- awaria odpowiedzi po zapisie pozwala odczytać rzeczywisty status;
- brak utraty przyjętych zgłoszeń po restarcie instancji;
- jeśli jest kolejka, zbadane przetwarzanie powtórne i zaległość;
- poprawny wynik przydziału zgodny z zatwierdzonymi przykładami i regułami.

## Dowody i wdrożenie
Raport zawiera konfigurację i wersję testowanego systemu, scenariusz ruchu, faktyczne wyniki, błędy oraz ograniczenia. Zebrać nagranie zachowania autoscroll, raporty przeglądarek i metryki całego przepływu.

Przed wdrożeniem: kopia konfiguracji, plan odwrócenia, test sesji i integralności, właściciel dyżuru. Po wdrożeniu: krótki kontrolowany test w przeglądarce i monitoring. Testów masowego ruchu nie wykonywać na produkcji bez zgody administratora. HTTP 200 nie dowodzi poprawnego zapisu i przydziału.

Dokumentacja narzędzi: [SOURCES.md](../SOURCES.md).
