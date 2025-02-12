---
id: 453
title: 'Xamarin.Forms Performance - dalsza automatyzacja'
date: '2018-02-01T18:51:24+01:00'
author: 'Tomasz Wiśniewski'
layout: post
guid: 'https://lazyloading.pl/?p=453'
permalink: /2018/02/01/xamarin-forms-performance-dalsza-automatyzacja/
dsq_thread_id:
    - '6452641542'
categories:
    - Xamarin
---

Od poprzedniego wpisu minęło więcej czasu niż planowałem. Zeszło mi jednak trochę z tym co robiłem, a i ten wpis nie będzie zbyt długi, bo przedstawię tylko postęp prac w moim projekcie odnośnie badania wydajności Xamarin.Forms między kolejnymi wersjami. W oddzielnych wpisach chciałbym dokładniej przestawiać narzędzia, z których miałem okazję korzystać (a przy okazji nauczyć się) podczas prac.

#### Automatyzacja wykonywania testów UI z Cake

To czemu się teraz poświęciłem, to drugi punkt z [ostatniego posta]/2018/01/10/xamarin-forms-performance-aktualizacja-warstwy-prezentacji/) na ten temat. **Do tej pory musiałem ręcznie ustawić wersję Xamarin.Forms z managera pakietów nuget, następnie dla pewności wyczyścić katalogi bin i obj, zbudować aplikację (i apk dla androida), zdeployować na urządzenie, uruchomić testy ui i przenieść wyniki do aplikacji webowej. Obecnie wszystkie te rzeczy wykonują się automatycznie!** To naprawdę niesamowite jak sobie o tym pomyślę, bo nie było to wcale tak ciężkie, jak przypuszczałem. Poszło całkiem gładko i w większości bez problemów. Z pomocą przyszedł mi [Cake Bauild](https://cakebuild.net), czyli narzędzie do automatyzacji procesu budowy aplikacji i rzeczy z tym związanych. W Cake’u piszemy przy pomocy C# i mamy do dyspozycji zestawy funkcji, które sprawiają, że używanie go jest w miarę przyjemne.

Obecnie, jeśli chcę odpalić testy dla kilku wersji Xamarin.Forms, wpisuję sobie wybrane wersje z nugeta do zmiennej, oddzielone średnikami, podłączam urządzenia (iPhone’a i Androida) do Maca, uruchamiam skrypt i czekam. Całość może potrwać długo, w zależności od tego ile wersji Formsów wybraliśmy, ale w międzyczasie możemy zająć się czymś innym. Odpalam testy na Macu, ponieważ odpalanie testów UI dla iOS nie jest dostępne z Windowsa. Dlatego wygodniej po prostu zrobić wszystko z Maca. Chyba, że zależy nam na czasie. Wtedy możemy bez problemu odpalić testy dla iOS na Macu, a dla Androida na drugim komputerze z Windowsem.

Dodałem również drobną automatyzację do kopiowania wyników testów do projektu webowego, żebym nie musiał o tym pamiętać.

#### Aktualizacja prezentacji wyników

Obecnie [na stronie z wynikami](http://xamformsperf.azurewebsites.net) możemy obejrzeć wyniki dla 10 wersji Xamarin.Forms, zarówno dla Androida jak i iOS. W międzyczasie aktualizowałem iOSa na swoim iPhone’ie, dlatego nie wszystkie testy są wykonane na tej samej wersji systemu. Co widać! Przynajmniej na pierwszych dwóch wykresach, gdzie ogólne czasy są bardzo niskie. W „App Init” na iOS 11.2.1 czas zawsze wynosił 13 ms, a na iOS 11.2.2 już 15 ms. Z powodu małych liczb, różnica wynosi aż kilkanaście procent i jest wyraźnie widoczna na wykresie.

Pierwsza myśl po spojrzeniu na wykres, byłaby taka, że to wina Xamarin.Forms! Dlatego żeby nikogo to nie zmyliło, **dodaję informację pod tytułem testu, jeśli nie wszystkie testy są wykonane na tym samym modelu urządzenia lub wersji systemu**. Dodatkowo możemy sprawdzić model urządzenia i wersję systemu po najechaniu na kolumnę na wykresie. Niestety nie posiadam jeszcze dokładnego modelu iPhone’a, co zauważyłem dopiero później, więc będę musiał to poprawić.

#### Następne kroki

Cieszę się, że udało mi się zautomatyzować tyle rzeczy w tym projekcie. Przyszłe prace powinny być o wiele bardziej przyjemne, ponieważ nie muszę już wykonywać wielu żmudnych czynności.

Kolejną rzeczą, którą chcę zrobić, jest dodanie większej ilości różnych testów. Zwłaszcza dla bardziej skomplikowanych przypadków, gdzie nie testujemy jednej funkcji, czy kontrolki. Dodatkowo warto przemyśleć aplikację webową pod kątem tego, co da się poprawić w prezentacji wyników i co będzie najważniejsze dla zaglądających tam osób.