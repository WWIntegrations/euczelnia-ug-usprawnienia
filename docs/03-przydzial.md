# Zasady przydziału i ochrona zgłoszeń
Nie potwierdzono, że obecny algorytm przydziela miejsca według czasu wysłania. Administrator powinien opisać rzeczywistą regułę, precyzję czasu, znaczenie preferencji i rozstrzyganie remisów.

## Zalecany wariant do decyzji uczelni
Zbierać i pozwalać aktualizować preferencje w ustalonym oknie. Po zamknięciu zamrozić dane i uruchomić przydział według ogłoszonych zasad, limitów oraz uprawnień. Przy identycznym priorytecie określić audytowalny sposób rozstrzygnięcia, np. losowanie, jeżeli uczelnia zaakceptuje taki regulamin. Nie zmieniać zasad po rozpoczęciu naboru.

Taki proces usuwa premiowanie sekund wysyłki tylko wtedy, gdy regulamin i algorytm faktycznie nie używają ich jako kryterium. Nadal potrzebna jest dostępność przez całe okno i procedura dla awarii.

Nie podajemy jednego algorytmu dopasowania jako gotowego bez ustalenia: jedno czy wiele miejsc na studenta, obowiązkowe przedmioty, kolizje, priorytety, limity, dopuszczalne nieprzypisanie i sposób rozstrzygania remisów. Algorytm i przykłady wyników powinny zostać zaakceptowane przez organizatora zapisów.

## Jeżeli pozostaje kolejność zgłoszeń
Zapewnić trwałe przyjęcie z identyfikatorem i czasem serwera, określić punkt ustalania kolejności i zachowanie ponowień. Poczekalnia może chronić serwer, lecz kolejka oparta na czasie wejścia nadal premiuje wcześniejsze dotarcie. Nie przedstawiać jej jako pełnego rozwiązania nierównego dostępu.

## Integralność
- Jedno zgłoszenie na konto i nabór albo jawnie wersjonowane aktualizacje.
- Idempotency key związany z użytkownikiem, naborem i treścią operacji; ten sam klucz z inną treścią oznacza konflikt. Ponowienie zwraca ten sam wynik bez duplikatu.
- Transakcyjny zapis danych i statusu, trwałe potwierdzenie oraz możliwość odczytu po utracie odpowiedzi HTTP.
- Jeśli używana jest kolejka, uniknąć luki pomiędzy zapisem w bazie i publikacją zdarzenia, np. wzorcem transactional outbox dobranym do obecnego stosu.
- Ochrona limitów miejsc transakcją/warunkowym zapisem właściwym dla silnika bazy. Sama blokada w pamięci pojedynczego procesu nie wystarcza przy wielu instancjach.
- Rozliczalna procedura awaryjna: komunikat, zakres dotkniętych zgłoszeń, decyzja o przedłużeniu lub powtórzeniu i ślad audytowy. Politykę retencji ustalić dla danych i logów.

To propozycje techniczne i organizacyjne do uzgodnienia, nie opis potwierdzonego działania obecnego systemu.
