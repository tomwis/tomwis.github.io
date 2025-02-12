---
id: 225
title: 'Nowości w Visual Studio 2017 i 15.3 z perspektywy Xamarina'
date: '2017-09-10T14:09:55+01:00'
author: 'Tomasz Wiśniewski'
layout: post
guid: 'https://lazyloading.pl/?p=225'
permalink: /2017/09/10/nowosci-visual-studio-2017-15-3-perspektywy-xamarina/
dsq_thread_id:
    - '6132920962'
categories:
    - 'Visual Studio'
---

Visual Studio 2017 jest dostępny od marca tego roku. Ostatnio wyszła nowa, większa aktualizacja, 15.3, która mogła być interesująca. A jak wyszło? Zobaczmy. Przejdziemy przez ciekawsze nowości zarówno wersji podstawowej, jak i aktualizacji, które są ważne z punktu widzenia dewelopera Xamarina. Uściślę tylko, że mam tu na myśli normalnego Visual Studio, a nie Xamarin Studio na Maca, któremu zmieniono nazwę na Visual Studio for Mac. Jest to tylko zabieg marketingowy i program ten nie ma nic wspólnego z prawdziwym Visualem. Niestety wprowadza to trochę w błąd, zwłaszcza nowych użytkowników.

#### Szybkość uruchamiania solucji

Szybkość uruchamiania zarówno Visuala jak i solucji miały ulec znacznej poprawie. Szczerze to za bardzo tego nie odczułem. Być może wiąże to się z jakimiś specjalnymi przypadkami. Nie robiłem dokładnych pomiarów, ale **znacznego, odczuwalnego przyspieszenia raczej nie ma**.

Jest jednak jedna ciekawa opcja, która nazywa się **Lightweight solutions**. Jeśli jest ona aktywna, to projekt z solucji ładuje się dopiero, gdy chcemy go użyć. Czyli np. gdy go rozwiniemy, żeby podejrzeć/edytować jakiś plik.

#### Intellisense w XAMLu dla Xamarin.Forms

W Visual Studio 2015 podpowiedzi w xamlu w projektach Xamarin.Forms nigdy mi nie chciały działać. W VS2017 zostało to w końcu poprawione i działają tak jak powinny. Zdawałoby się, że to podstawa, ale jednak musieliśmy na to trochę poczekać. Jednak ważne, że już jest 🙂

![](/wp-content/uploads/2017/08/vs2017_xaml_intellisense.png)

#### Zintegrowane aktualizacje dla Xamarina

Ktoś w Microsofcie wpadł na pomysł, że dobrze będzie aktualizować Xamarina razem z Visual Studio. Teoretycznie nie powinno nam to robić zbyt wielkiej różnicy. Mi jednak robi, ponieważ od którejś aktualizacji Visual odmówił współpracy i aktualizacja zawsze kończyła się niepowodzeniem. Wtedy musiałem odinstalowywać Visuala i instalować go na nowo. I tak już od ładnych kilku aktualizacji.

#### Run to click

Od teraz podczas debugowania możemy wykonać kod do wskazanego miejsca i debugger zatrzyma się w nim bez stawiania breakpointa. Wystarczy, że klikniemy na zieloną strzałkę, która pojawia się na początku linii.

![](/wp-content/uploads/2017/08/vs2017_run_to_click.png)

Całkiem fajna opcja, jednak wydaje mi się, że musimy zbyt długo czekać, aż strzałka się pojawi. Jeśli ktoś działa szybko, to pewnie wygodniej będzie mu postawić breakpointa.

#### Live unit testing

Live unit testing to wykonywanie wszystkich testów w czasie rzeczywistym.

![](/wp-content/uploads/2017/08/vs2017_live_unit_testing.png)

[Źródło: [docs.microsoft.com](https://docs.microsoft.com/en-us/visualstudio/ide/whats-new-in-visual-studio)]

Super sprawa, widzimy, która ścieżka przechodzi, która nie, a która nie jest objęta żadnymi testami. Chciałem tu dodać własny zrzut ekranu, jednak gdy po 20 minutach Live Unit Testing nadal nie chciał mi działać, to dałem sobie spokój i dodałem zrzut z dokumentacji. Nie wiem czy popsuli to w aktualizacji, czy potrzebna jest tu inna magia, ale jakiś czas temu używałem tej funkcji bez problemu.

#### Rozwiązywane konfliktów z gita

Mamy trochę lepszą obsługę gita. Gdy podczas merge’a pojawi się konflikt możemy go w łatwiejszy sposób obsłużyć w Visualu.

![](/wp-content/uploads/2017/08/vs2017_merge_conflict.png)

[Źródło: [The Future of C# (Build 2017)](https://channel9.msdn.com/Events/Build/2017/B8104)]

Mamy teraz dedykowane opcje do wybrania wersji naszej lub tej drugiej (lub obu). Być może kiedyś doczekamy się tak fajnego rozwiązania jakie mamy np. w PyCharm:

![](/wp-content/uploads/2017/08/pycharm_merge_conflict-1024x527.png)

[Źródło: [Dokumentacja PyCharm](https://www.jetbrains.com/help/pycharm/resolving-conflicts.html)]

#### Structure Visualizer

Visual od teraz rysuje linie pomiędzy nawisami pomagające łatwiej odnaleźć się w kodzie.

![](/wp-content/uploads/2017/08/vs2017_structure_visualizer1.png)

Dodatkowo, gdy najedziemy na nawias kończący, zobaczymy co jest na jego początku, czyli np. definicję funkcji. Natomiast jeśli najedziemy na przerywaną linię, to zobaczymy pełną ścieżkę od początku namespace’a:

![](/wp-content/uploads/2017/08/vs2017_structure_visualizer2.png)

#### Find All References

Find All References to opcja znajdująca się pod prawym przyciskiem myszy i w Visualu 2017 została odświeżona:

![](/wp-content/uploads/2017/08/vs2017_references.png)

Mamy tu teraz kolorowanie składni, które bardzo pomaga. Wcześniej opcji tej używało się raczej ciężko. Dodatkowo doszły też różnego rodzaju grupowania.

#### Edytor uprawnień

Zmiana, która pojawiła się w aktualizacji 15.3. Plik entitlements.plist, który jest używany przy projekcie Xamarin.iOS w końcu dostał swój edytor. Zawsze to wygodniej niż ręczne edytowanie xmla.

![](/wp-content/uploads/2017/08/vs2017_entitlements_editor.png)

#### Xamarin Live Player – brak

Miałem nadzieję, że w aktualizacji 15.3 pojawi się Xamarin Live Player zapowiadany na Build 2017. Do tej pory znajdował się w wersji preview Visuala. Zespół jednak podjął decyzję, że dalej pozostanie on w wersji preview. Trochę szkoda, ale z drugiej strony pewnie chcą go dopracować. Także czekamy do następnej większej aktualizacji.

#### Ogólne wrażenia

Visual Studio 2017 dostarczył nam kilku fajnych i usprawniających pracę nowości. Większość z wymienionych wyżej rzeczy jest na pewno na plus, z Intellisense dla xamla w Xamarin.Forms na czele. Aktualizacja 15.3 w kwestii Xamarina zmieniła raczej niewiele, chyba że są jakieś poprawki stabilności, których na pierwszy rzut oka nie widać. Czekałem na tę wersję, głównie ze względu na Xamarin Live Player. A czekać trzeba było znacznie dłużej niż na wcześniejsze aktualizacje (3 miesiące od 15.2, w porównaniu do 1 miesiąca pomiędzy 15.0 – 15.1 – 15.2). Dlatego też liczyłem na trochę więcej. Nawet jeśli nie nowe funkcje, to poprawki błędów.

I skoro przy błędach jesteśmy. Martwi trochę jakość tego wydania. Jak wyżej wspominałem, przy każdej aktualizacji muszę reinstalować Visuala. Wczoraj przy debugowaniu projektu UWP, Visual zacinał się co pewien czas i odwieszał dopiero po kilku minutach. Deploy dla Androida nie zawsze wgrywa nową wersję aplikacji lub po prostu trwa w nieskończoność i konieczny jest rebuild lub usuwanie folderów bin/obj. Jest kilka takich mniejszych problemów z najróżniejszymi sprawami, które po pewnym czasie zaczynają mocniej przeszkadzać. Być może wraz ze startem wersji preview Microsoft zwolnił dział QA i liczą na zgłoszenia od ludzi 😉