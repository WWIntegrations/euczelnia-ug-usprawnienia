# Diagnostyka i skalowanie
## Ustalenie stosu i ograniczenia
Administrator powinien potwierdzić: framework i runtime, model sesji, bazę danych, pulę połączeń, reverse proxy, VM/kontenery, ewentualny klaster i sposób logowania. Publiczne przekierowanie do centralnego logowania nie ujawnia architektury aplikacji zapisów.

Najpierw porównać okres przed otwarciem i pierwsze minuty zapisów. Zebrać RPS, współbieżność, p50/p95/p99 pełnych operacji, 5xx/429, timeouty, CPU, RAM, restarty/OOM, opóźnienia I/O, czas logowania, zajętość puli połączeń, wolne zapytania i oczekiwania na blokady. Używać identyfikatorów korelacji i metryk bez danych studentów. Oddzielić czas przyjęcia zgłoszenia od przeliczenia przydziału.

Potencjalne wąskie gardła: logowanie, aplikacja, proxy, baza, blokada wspólnego rekordu, sieć. Więcej replik aplikacji nie usuwa automatycznie pozostałych ograniczeń.

## Wariant A: istniejący Kubernetes
Warunki: aplikacja może pracować w wielu instancjach, sesje i zadania cykliczne są do tego przygotowane. Nie zakładamy, że UG używa Kubernetes.
1. Przed OTWARCIEM zapisów zwiększyć minReplicas lub liczbę replik Deployment, zapewnić zasoby węzłów i poczekać na gotowość. Liczbę wyznaczyć testem. Powtórzyć przygotowanie przed publikacją WYNIKÓW, jeżeli wtedy występuje osobny szczyt.
2. Skonfigurować requests/limits z pomiarów, startupProbe i readinessProbe. Liveness nie może powodować lawiny restartów pod obciążeniem ani traktować każdej krótkiej awarii zależności jako uszkodzenia procesu.
3. HPA dobrać do właściwego sygnału: CPU, współbieżność lub metryka ruchu. Dla CPU utilization potrzebne są resource requests. Uwzględnić opóźnienie metryk, start aplikacji i uruchamianie węzłów. Autoskalowanie reaktywne uzupełnia wcześniejsze przygotowanie.
4. Zapewnić sesje współdzielone lub inny wspierany model. Sticky sessions mogą ograniczyć problem routingu, ale nie zapewniają zachowania sesji po awarii instancji.
5. Kontrolować: liczba replik * maksymalna pula połączeń + pozostali klienci < dostępny budżet bazy. Skalowanie bez tej kontroli może nasilić awarię.
6. Sprawdzić graceful shutdown, drain ruchu i ochronę przed równoległym uruchomieniem zadań, które powinny być pojedyncze. Przed zmianą przygotować odwrócenie konfiguracji i progi wycofania.

## Wariant B: VM lub serwer aplikacyjny
Dodatkowe instancje za load balancerem, jeżeli aplikacja to wspiera; albo zwiększenie CPU/RAM/IOPS przy potwierdzonym nasyceniu. Zachować kontrolę sesji, połączeń i zadań jak wyżej.

Lepszy hosting rozważyć przy udokumentowanych limitach obecnego środowiska. Porównać pojemność podczas szczytu, niezawodność, integrację z logowaniem, lokalizację danych, kopie, monitoring, koszt i odpowiedzialność za utrzymanie. Migracja wymaga testu i planu powrotu. Kubernetes nie jest warunkiem naprawy.

## Baza i ruch
- Optymalizować zapytania oraz indeksy na podstawie planów wykonania właściwego silnika. Sprawdzać blokady i transakcje, a nie tylko CPU.
- Buforować katalog zajęć z określoną polityką unieważniania. Nie traktować cache jako źródła potwierdzeń przyjęcia i dostępności ostatniego miejsca.
- Wprowadzić idempotencję i trwałe przyjęcie zgłoszenia. Transakcja powinna chronić limit miejsc, jeżeli przydział odbywa się od razu.
- Kolejkę zastosować, gdy potrzebne jest oddzielenie przyjmowania od przetwarzania; potwierdzać dopiero trwałe przyjęcie. Kolejka potrzebuje limitów, monitorowania zaległości, deduplikacji i obsługi awarii.
- Przy przeciążeniu pokazywać kontrolowany status i retry z opóźnieniem oraz losowym rozrzutem. Nie uruchamiać agresywnego odpytywania przez wszystkich studentów.
- Nie nakładać niesprawiedliwych limitów wyłącznie per IP: wiele osób korzysta z jednej sieci akademickiej. Dobrać zabezpieczenia również do konta i operacji.

## Wynik pracy administratora
Raport: potwierdzony stos, przyczyna z dowodami, pomiary przed/po, plan pojemności, zmiany, koszt i rollback. Wydajność i reguły sprawiedliwego przydziału oceniać osobno. Źródła: [SOURCES.md](../SOURCES.md).
