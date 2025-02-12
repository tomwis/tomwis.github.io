---
id: 504
title: 'Xamarin.Forms Performance - nowe testy'
date: '2018-03-07T19:46:20+01:00'
author: 'Tomasz Wiśniewski'
layout: post
guid: 'https://lazyloading.pl/?p=504'
permalink: /2018/03/07/xamarin-forms-performance-nowe-testy/
dsq_thread_id:
    - '6529483518'
categories:
    - Xamarin
---

Trochę czasu minęło od poprzedniego wpisu, a jeszcze więcej od ostatniej aktualizacji mojego projektu dotyczącego mierzenia wydajności Xamarin.Forms. Jednak mimo ciszy, trochę się w nim działo.

#### Co nowego doszło?

Dodałem do aplikacji nowe testy:

- **6 testów dla ListView**. Sprawdzam tutaj wydajność podstawowych komórek – TextCell, ImageCell, SwitchCell i EntryCell, a także wydajność ViewCell – w tym przypadku zbudował 2 layouty, podstawowy i bardziej rozbudowany. Test polega na wczytaniu listy z 200 elementami i przejechaniu do końca listy. Starałem się, żeby automatyzacja scrollowania w miarę dobrze oddawała wydajność i wydaje mi się, że w miarę dobrze to wyszło.
- **5 testów dla layoutów**. W tym: 
    - 2 dla StackLayout: 
        - Jeden to ładowanie widoku z zagnieżdżonymi StackLayoutami jeden pod drugim. Jest to struktura, która dobrze nadaje się dla StackLayouta.
        - Drugi to również zagnieżdżone StackLayouty, z tym że ich struktura przypomina bardziej coś, co zbudowalibyśmy z Gridów. Dlaczego to zrobiłem? Żeby porównać sobie wydajność właśnie z Gridami, których testy również stworzyłem.
    - 3 dla Gridów. Tutaj ładuję widok, gdzie mamy zagnieżdżone Gridy. Różnica między tymi trzema testami jest tylko jedna – typy rozmiarów w kolumnach i wierszach. W jednym teście wszystkie są jako „Star” (\*), w drugi wszystkie są jako „Auto”, a w trzecim wszystkie mają stałe wartości. Byłem ciekawy jaki rzeczywisty wpływ na wydajność mają typy rozmiarów. Jeśli też jesteście ciekawi, to możecie to sprawdzić sami w wynikach na stronie 😉

Przypominam adres strony, pod którym znajdują się aktualne wyniki: <http://xamformsperf.azurewebsites.net>

Powtórzyłem wszystkie testy, tak żeby były wykonane w jednym czasie, na tym samym urządzeniu i wersji systemu. Całość trwała jakieś 4 godziny. **Co ciekawe, jeden przebieg testów dla Androida trwa jakieś 3 razy dłużej niż dla iOS.** Może to mieć związek z szybkością samego telefonu, ale to chyba nie tylko to.

Oprócz zaktualizowania wyników na stronie, **zmieniłem również tytuły testów oraz dodałem opisy**, tak żeby było jasne, co było wykonywane w ramach danego testu.

#### Wnioski

**Obecnie mamy 18 testów. Wykonałem je na 11 najnowszych wersjach Xamarin.Forms.** Każdy test był powtarzany 10 razy i wynik był uśredniany.

Na stronie z wynikami na wykresach wyraźnie widać, że **wydajność Xamarin.Forms dla iOS w ostatnich czasach była stabilna**. Android też nie ma jakichś wielkich wahań, ale zdarzają się zmiany o kilka, czy czasami nawet kilkanaście procent. Wszystko jednak wydaje się pozostawać w normie.

Bardzo chciałem porównać starsze wersje Xamarin.Forms z nowszymi, jednak instalacja starszych wersji sprawiała dużo problemów i dlatego na razie dałem sobie z tym spokój. Jednak **fajnie byłoby zobaczyć, czy i ile Xamarin.Forms poprawił się przez ostatnie lata**. Wiemy, że brakuje mu wydajności, a z obecnych testów widać, że jakichś większych zmian nie ma.