# Dokumentacja i źródła
Sprawdzenie publicznych materiałów: 2026-10-30. Poniższe linki wspierają plan rozwiązania; nie dowodzą wdrożenia danych technologii w module zapisów UG.

## System UG
- [Instrukcja zapisów](https://historia.ug.edu.pl/sites/historia.ug.edu.pl/files/_nodes/news/112430/files/instrukcja-zapisow-na-zajecia-euczelnia.pdf): etapy procesu i przyciski kolejności.
- [Harmonogram WF 2026/2027](https://sport.ug.edu.pl/strona/115471/elektroniczne-zapisy-na-zajecia-wychowania-fizycznego-semestr-zimowy-20262027): osobne terminy układania preferencji, wysyłania i wyników.
- [Centralny Punkt Logowania](https://ug.edu.pl/o-uczelni/uslugi-it/centralny-punkt-logowania): logowanie do eUczelni.

## Interfejs
- [MDN requestAnimationFrame](https://developer.mozilla.org/en-US/docs/Web/API/Window/requestAnimationFrame): pętla przewijania i czas klatki.
- [MDN Element.scrollBy](https://developer.mozilla.org/en-US/docs/Web/API/Element/scrollBy): przewijanie kontenera.
- [MDN Pointer Events](https://developer.mozilla.org/en-US/docs/Web/API/Pointer_events): dotyk, mysz, capture i anulowanie.
- [WCAG 2.2, wyjaśnienie 2.5.7](https://www.w3.org/WAI/WCAG22/Understanding/dragging-movements.html): alternatywa dla przeciągania.
Dokumentację konkretnej biblioteki sortowania dobrać po identyfikacji pakietu i wersji. Nie potwierdzono użycia żadnej biblioteki.

## Infrastruktura i testy
- [Kubernetes HPA](https://kubernetes.io/docs/concepts/workloads/autoscaling/horizontal-pod-autoscale/): skalowanie replik na metrykach i warunki gotowości.
- [Grafana k6: spike testing](https://grafana.com/docs/k6/latest/testing-guides/test-types/spike-testing/): test nagłego wzrostu ruchu.

Biblioteki, baza i platforma wdrożenia wymagają potwierdzenia przez administratora. Przykładowe parametry w opracowaniu nie pochodzą z pomiarów UG. Opracowanie korzysta również z prywatnej bazy wiedzy autora w zakresie diagnostyki, skalowania i integralności; jej treści ani prywatnych materiałów nie opublikowano.
