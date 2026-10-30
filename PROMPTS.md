# Prompty dla zespołu IT
Uruchamiać w uprawnionym środowisku z repozytorium aplikacji. Każdy prompt to oddzielne zadanie. Nie przekazywać modelowi sekretów ani danych studentów. Dokumenty opisują wymagania; technologie i przyczyny trzeba sprawdzić.

## 1. Rozpoznanie stosu
<zadanie>Ustal stos modułu zapisów i miejsca odpowiedzialne za sortowanie, sesje i wysyłanie preferencji. Gotowe, gdy każde ustalenie ma dowód w pliku lub konfiguracji.</zadanie>
<kontekst>README.md i docs/01-autoscroll.md tego opracowania; manifesty, lockfile i kod aplikacji. Nie odczytuj sekretów. Nie utożsamiaj stosu logowania ze stosem aplikacji.</kontekst>
<wyjscie>Tabela: element, technologia/wersja, plik:linia, pewność; lista brakujących informacji. Bez zmian kodu.</wyjscie>

## 2. Naprawa autoscroll
<zadanie>Napraw brak przewijania listy podczas przeciągania preferencji do krawędzi widoku. Gotowe, gdy pozycję 150 można przenieść na 1 jednym gestem.</zadanie>
<kontekst>docs/01-autoscroll.md; użyj wyniku rozpoznania stosu. Najpierw odtwórz błąd. Preferuj konfigurację istniejącej biblioteki, bez migracji frameworka.</kontekst>
<wyjscie>Minimalny patch, test regresji przed/po, dowody z przeglądarki dla góry/dołu, nieruchomego wskaźnika, anulowania i telefonu. Opisz ograniczenia i wycofanie. Bez deployu.</wyjscie>

## 3. Diagnoza wydajności
<zadanie>Znajdź wąskie gardło początku zapisów i zaproponuj poprawkę uzasadnioną metrykami.</zadanie>
<kontekst>docs/02-wydajnosc.md. Korzystaj z zanonimizowanych metryk i logów udostępnionych przez administratora. Oddziel logowanie, aplikację, bazę i proxy. Nie uruchamiaj obciążenia produkcji.</kontekst>
<wyjscie>Przyczyna z dowodem, alternatywne hipotezy, plan pomiaru przed/po, zmiana, koszt, ryzyko i rollback. Brak danych oznacz jako brak danych. Bez zmian infrastruktury.</wyjscie>

## 4. Plan skalowania
<zadanie>Dobierz pojemność dla potwierdzonej architektury na otwarcie zapisów i publikację wyników.</zadanie>
<kontekst>docs/02-wydajnosc.md i wyniki diagnozy. Kubernetes tylko jeśli istnieje i aplikacja wspiera wiele replik. Uwzględnij sesje, czas startu, węzły i sumę pul połączeń.</kontekst>
<wyjscie>Plan konfiguracji z uzasadnieniem każdej liczby, harmonogram wcześniejszego zwiększenia pojemności, metryki HPA jeśli dotyczy, limity bazy, koszt, test i rollback. Nie stosuj zmian.</wyjscie>

## 5. Przydział i integralność
<zadanie>Zweryfikuj wpływ czasu zgłoszenia na przydział i odporność na ponowienia oraz konkurencję.</zadanie>
<kontekst>docs/03-przydzial.md; kod i zatwierdzony regulamin. Uczelnia decyduje o zasadach. Nie zmieniaj ich samodzielnie.</kontekst>
<wyjscie>Opis obecnej reguły z dowodem, przypadki testowe remisu i ostatniego miejsca, rekomendacja zbierania preferencji w oknie oraz plan idempotencji i trwałych potwierdzeń. Bez zmian regulaminu.</wyjscie>

## 6. Odbiór
<zadanie>Zweryfikuj poprawkę i pełny przepływ zapisów według docs/04-odbior.md. Gotowe, gdy każde kryterium ma wynik i dowód albo jawny brak weryfikacji.</zadanie>
<kontekst>Środowisko testowe i konta syntetyczne; obciążenie oraz progi zatwierdzone przez administratora. Bez testu obciążeniowego produkcji.</kontekst>
<wyjscie>Tabela PASS/FAIL/NIE SPRAWDZONO z dowodem, p95/p99, błędy, integralność danych, ograniczenia i rekomendacja wdrożenia. Nie raportuj planu testu jako wykonanego testu.</wyjscie>
