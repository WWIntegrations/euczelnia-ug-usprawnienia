# eUczelnia UG: usprawnienie zapisów
Propozycja studentów International Business dotycząca https://zapisy.euczelnia.ug.edu.pl/.

## Problem i oczekiwany efekt
1. Według zgłoszenia podczas przeciągania dowolnej preferencji można dotrzeć tylko do górnej krawędzi aktualnie widocznego obszaru. Lista nie przewija się dalej. Student musi przerywać gest, przewijać i ponawiać przeciąganie. Oczekiwane zachowanie: przytrzymanie przeciąganej pozycji przy górnej lub dolnej krawędzi automatycznie przewija listę w odpowiednim kierunku, bez puszczania pozycji.
2. Studenci zgłaszają utrudniony dostęp przy otwarciu zapisów. W jednej relacji wysłanie preferencji było możliwe około 10:08 zamiast 10:00. To relacja użytkownika wymagająca porównania z logami, a nie potwierdzony pomiar awarii.
3. Studenci potrzebują jasnego wyjaśnienia wpływu czasu zgłoszenia na przydział. Obawa o przewagę szybszego internetu nie jest dowodem działania algorytmu.

## Materiały dla zespołu IT
- [Naprawa przeciągania i automatycznego przewijania](docs/01-autoscroll.md)
- [Diagnostyka i skalowanie infrastruktury](docs/02-wydajnosc.md)
- [Zasady przydziału i ochrona zgłoszeń](docs/03-przydzial.md)
- [Scenariusze testowe i kryteria odbioru](docs/04-odbior.md)
- [Prompty do pracy w repozytorium aplikacji](PROMPTS.md)
- [Dokumentacja źródłowa](SOURCES.md)
- [Krótka wiadomość do uczelni](WIADOMOSC.md)

## Co zostało ustalone
Oficjalna instrukcja UG opisuje układanie preferencji i przyciski „Na początek”, „W górę”, „W dół”, „Na koniec”. Ich istnienie nie rozwiązuje zgłoszonego braku automatycznego przewijania podczas przeciągania. Harmonogram WF oddziela przygotowanie preferencji od ich wysyłania. Źródła: [instrukcja](https://historia.ug.edu.pl/sites/historia.ug.edu.pl/files/_nodes/news/112430/files/instrukcja-zapisow-na-zajecia-euczelnia.pdf), [harmonogram](https://sport.ug.edu.pl/strona/115471/elektroniczne-zapisy-na-zajecia-wychowania-fizycznego-semestr-zimowy-20262027).

Publiczna strona zapisów kieruje do centralnego logowania. Nie potwierdzono frameworka modułu zapisów, silnika bazy, biblioteki przeciągania ani wykorzystania Kubernetes. Nie przenosimy informacji o technologiach innych systemów uczelnianych na ten moduł.

To specyfikacja i plan weryfikacji, bez dostępu do kodu aplikacji, logów ani infrastruktury UG. Nie wykonano testów obciążeniowych ani naprawy systemu. Parametry liczbowe w dokumentacji są propozycjami do walidacji. Nie jest to oficjalna dokumentacja UG.

Opracowanie: Wiktor Wojtarowicz. Stan: 2026-10-30.
