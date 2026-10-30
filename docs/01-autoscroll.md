# Naprawa automatycznego przewijania
## Reprodukcja
Otworzyć długą listę preferencji, przewinąć ją w dół, chwycić dowolną pozycję i przeciągnąć ją do górnej krawędzi widocznej części listy. Według zgłoszenia lista nie przewija się dalej. Oczekiwane: dalszy ruch listy przy nieprzerwanym geście. Analogicznie sprawdzić dolną krawędź.

## Najpierw rozpoznanie istniejącej aplikacji
Ustalić framework, bibliotekę sortowania i jej wersję z manifestu oraz lockfile. Odtworzyć problem w przeglądarce. Zidentyfikować rzeczywisty kontener przewijania: element z overflow i scrollHeight większym od clientHeight albo dokument. Sprawdzić zagnieżdżone kontenery, sticky header, overflow: hidden/clip, portale/overlay, wirtualizację i obsługę dotyku.

Jeżeli biblioteka ma wbudowany autoscroll, najpierw poprawić jego konfigurację i wskazanie kontenera. Nie uruchamiać dwóch mechanizmów równocześnie. Wybrać rozwiązanie zgodne z wersją używanej biblioteki; jej nazwa nie jest obecnie potwierdzona.

## Algorytm, gdy potrzebny jest własny mechanizm
1. Na początku gestu ustalić identyfikator pozycji, przewijany kontener, pozycję wskaźnika i kolejność wyjściową.
2. Wyznaczyć faktycznie widoczny prostokąt: przecięcie obszaru kontenera z viewportem i obszarami przycinających przodków, po uwzględnieniu zasłonięcia nagłówkiem. Używać współrzędnych clientY zgodnych z getBoundingClientRect.
3. Przy krawędziach ustalić strefy aktywacji. Punkt startowy do strojenia: 56 px, ale nie więcej niż 1/4 widocznej wysokości. Im bliżej krawędzi wskaźnik, tym szybszy scroll. W środku prędkość wynosi zero.
4. Podczas aktywnego gestu utrzymywać jedną pętlę requestAnimationFrame. Scroll musi trwać także wtedy, gdy wskaźnik stoi nieruchomo przy krawędzi. Sam pointermove lub dragover nie wystarczy jako zegar.
5. Liczyć przesunięcie z czasu klatki: delta = kierunek * vmax * penetracja^2 * dt. Przykładowe vmax 600 px/s jest parametrem do testów, nie ustalonym wymaganiem. Ograniczyć dt po przerwie, np. do 50 ms.
6. Przewijać natychmiast przez scrollBy({top: delta, behavior: "instant"}), bez animacji smooth konkurującej z pętlą. Uwzględnić wspierane przeglądarki.
7. Po scrollu ponownie wyznaczyć cel upuszczenia z aktualnej geometrii. Przy pointer capture event.target wskazuje element przechwytujący, więc nie może sam identyfikować celu. Korzystać z API biblioteki lub hit-testu nieuwzględniającego przeciąganego overlay.
8. Na krańcu kontenera nie zapętlać bezcelowych przesunięć. Przy zagnieżdżeniu określić jawnie, czy po dojściu do granicy wolno przewijać rodzica; nie przewijać wszystkich kontenerów jednocześnie.
9. Na drop zakończyć pętlę i zatwierdzić kolejność. Przy anulowaniu przywrócić poprzednią kolejność. Posprzątać na pointerup, pointercancel, lostpointercapture, Escape, ukryciu karty i usunięciu komponentu. Dla natywnego HTML DnD uwzględnić dragend oraz właściwy model zdarzeń.

## Dotyk, dostępność i zapis
Dla Pointer Events rozważyć setPointerCapture. touch-action: none ograniczyć do uchwytu przeciągania, aby pozostała część listy nadal przewijała się palcem. Nie blokować gestów na całej stronie. Sprawdzić Safari iOS i Chrome Android.

Pozostawić alternatywę bez przeciągania: przyciski i pole „Przenieś na pozycję”. Zapewnić klawiaturę, zachowanie fokusu i komunikat o nowej pozycji dla czytnika. To odrębne wymagania: alternatywa bez przeciągania nie zastępuje naprawy samego gestu.

Operować na stabilnych ID. Nie wysyłać żądania do serwera po każdej klatce. Zapisać kolejność po zakończeniu gestu lub istniejącym przyciskiem „Zapisz”, zgodnie z obecnym modelem. Rozróżnić zapis roboczy od wysłania zgłoszenia. Po błędzie nie pokazywać fałszywego sukcesu. Dla dwóch kart użyć kontroli wersji lub jawnego komunikatu o konflikcie. Szkicu zawierającego dane nie utrwalać bez ustalenia zasad prywatności.

Dokumentacja API i dostępności: [SOURCES.md](../SOURCES.md). Testy: [odbiór](04-odbior.md).
