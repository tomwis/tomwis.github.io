---
id: 467
title: 'Automatyzacja procesu budowania z Cake Build w C#'
date: '2018-02-04T17:40:08+01:00'
author: 'Tomasz Wiśniewski'
layout: post
guid: 'https://lazyloading.pl/?p=467'
permalink: /2018/02/04/automatyzacja-procesu-budowania-cake-build-c/
dsq_thread_id:
    - '6458247334'
categories:
    - 'C#'
    - 'Visual Studio'
    - Xamarin
---

Ostatnio, w ramach [aktualizacji w postępach prac]/2018/02/01/xamarin-forms-performance-dalsza-automatyzacja/) nad projektem XFP, pisałem, że użyłem tam Cake’a. Niektórym być może skojarzy się to z CakePHP, ale to nie o niego chodzi 🙂 Dzisiejszy wpis będzie dotyczył [Cake Build](https://cakebuild.net).

#### Co to jest?

**Cake Build to narzędzie do automatyzacji procesu budowy aplikacji i rzeczy z tym związanych.** Możemy tutaj kopiować, czy usuwać pliki, budować projekty, przywracać pakiety z NuGeta, uruchamiać testy i wiele więcej.

**Do pisania w Cake’u używamy C#**, a więc polecam go przede wszystkim deweloperom, którzy już znają ten język i pracują z platformą .Net na co dzień. Jeśli tak nie jest, to być może znajdziesz dla siebie lepsze narzędzie, bo wybór jest spory.

Cake dodaje nam cały zestaw metod, które ułatwiają pisanie rzeczy, do których Cake został stworzony. Tak więc możemy np. uruchomić budowanie z msbuild jedną funkcją.

#### Zadania (Taski)

Cake opiera się na zadaniach. Skrypt musi posiadać co najmniej jedno takie zadanie. Posiada ono nazwę, po której później je uruchamiamy za pomocą funkcji `RunTarget`.

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/86c84e25b9f3ed5d69a6d1e9e08861d5.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/86c84e25b9f3ed5d69a6d1e9e08861d5).</noscript></div>Dodatkowo możecie zobaczyć tu obiekt `Argument`, gdzie pierwszy parametr to nazwa argumentu, a drugi to jego domyślna wartość. Są to argumenty, których możemy później używać wywołując skrypt z linii komend.

#### Przykładowe zadanie

Najprostsze zadanie mogłoby wyglądać np. tak:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/a5eb28ac01f73512b7dfcf7a03badea7.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/a5eb28ac01f73512b7dfcf7a03badea7).</noscript></div>Używamy tu funkcji `CleanDirectories`, żeby wyczyścić wszystkie foldery `bin` oraz `obj` w folderze solucji. **Do ścieżek używamy wygodnych wzorców. Kropka standardowo oznacza obecny katalog, podwójna gwiazdka to dowolne znaki z więcej niż jednego folderu (w głąb).** Gdybyśmy użyli jednej gwiazdki, oznaczałoby to nazwę pliku lub folderu – ale nie więcej niż jednego. W takim wypadku wyczyścilibyśmy foldery `bin` i `obj` tylko z najbliższych podfolderów. Z dwiema gwiazdkami wyczyścimy te foldery także z dalszych (głębszych) podfolderów. Skrypt cake zazwyczaj jest uruchamiany z głównego folderu solucji, więc jest to jego początkowy folder i to od niego podajemy ścieżkę.

Podczas prac z Xamarinem czyszczenie tych folderów odbywa się bardzo często. Polecenie `Clean` w Visual Studio nie usuwa stamtąd wszystkich plików, a Xamarin czasami zaszaleje i mimo zbudowania projektu deploy’uje na urządzenie starą wersję aplikacji. Dopiero usunięcie tych folderów pomaga.

#### Zadania zależne

Możliwe, że chcielibyśmy zbudować aplikację, ale przed tym przywrócić pakiety NuGet. Możemy to zrobić z funkcją `NuGetRestore` dla całej solucji:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/6bb5639a32ad59cc72117d2f34baba68.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/6bb5639a32ad59cc72117d2f34baba68).</noscript></div>Lub też dla pojedynczego projektu. Wtedy jednak musimy podać ścieżkę do folderu `packages`:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/6886e5547f1784369239ce3b2b50d916.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/6886e5547f1784369239ce3b2b50d916).</noscript></div>Następnie tworzymy zadanie z budowaniem. Tu jako przykład jest projekt z aplikacją na Androida w Xamarinie.

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/6d75f68ef5998e389b10dff28cf108b2.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/6d75f68ef5998e389b10dff28cf108b2).</noscript></div>Używamy funkcji `MSBuild`, ustawiamy konfigurację na `Release` i dodajemy parametr `SignAndroidPackage`, dzięki któremu zostanie stworzony także plik apk.

To na co jednak chciałem zwrócić szczególną uwagę, to druga linijka z `IsDependentOn`. **Jest to zadanie zależne. Jeśli będziemy chcieli wykonać zadanie „build-android”, to najpierw, automatycznie, wykona się zadanie „restore”.** Jest to bardzo wygodne, bo możemy dzielić zadania na mniejsze fragmenty i w razie potrzeby wywoływać je razem lub pojedynczo nie zmieniając kodu.

#### Paczki z Nugeta

Do Cake’a można również dodawać rozszerzenia (które nazywają addin i tool). Najprościej zrobić to z nowymi dyrektywami preprocesora `#addin` i `#tool`. Jeśli np. chcemy uruchomić testy z pomocą NUnita:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/3a054e26d96127abe934c70f8f476763.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/3a054e26d96127abe934c70f8f476763).</noscript></div>Dyrektywa w pierwszej linii zainstaluje nam NUnit Runnera we wskazanej wersji, a funkcja `NUnit` uruchomi wszystkie testy. Możemy też dodać obiekt `NUnitSettings` jako drugi argument i pozmieniać różne opcje.

#### Integracja z Visual Studio Code

Z Cake’a korzystałem w Visual Studio Code. Jest to bardzo wygodne, ponieważ [mamy dostępny dodatek](https://marketplace.visualstudio.com/items?itemName=cake-build.cake-vscode), który może uruchamiać Cake’owe skrypty oraz zainstalować potrzebne rzeczy w nowym projekcie. Są to skrypty sh oraz ps1 (PowerShell), które zajmują się inicjalizacją i uruchamianiem skryptów cake’owych na Windowsie/Linuxie/OSX. Dostępny jest też intellisense, ale z tego co widziałem działa raczej słabo. Pisałem akurat na Macu i natrafiłem też na jeszcze jeden, drobny problem. W skrypcie sh jest błąd. Przy uruchamianiu cake’a wskazywane jest zadanie, które ma być wykonane. Odbywa się to za pomocą argumentu target. Po argumencie powinniśmy umieścić znak równości i następnie wartość argumentu. Mamy niestety zamiast tego spację. Widziałem jednak na githubie, że błąd ten jest już poprawiony, a więc niedługo możemy się spodziewać działającej wersji. A do tej pory możemy uruchamiać skrypt ręcznie z terminala w Visual Studio Code z podaniem prawidłowego argumentu.

#### Integracja z Visual Studio

Do pełnego Visual Studio także powstał [dodatek](https://marketplace.visualstudio.com/items?itemName=vs-publisher-1392591.CakeforVisualStudio). Niestety jeszcze go nie używałem, ale z opisu wygląda na to, że ma podobne możliwości jak dodatek z Visual Studio Code (a może nawet większe). I o to właśnie chodzi. Mamy narzędzie do automatyzacji, zintegrowane ze środowiskiem i możemy wszystkiego szybko używać.

#### Dodatek do HockeyApp

Do Cake’a istnieje wiele dodatkowych paczek – możecie je przejrzeć na [tej stronie](https://cakebuild.net/addins/). Jedną z ciekawych opcji, przynajmniej dla mnie, jako że zajmuję się Xamarinem, jest integracja z HockeyApp. HockeyApp to narzędzie do raportowania błędów z aplikacji, zbierana statystyk, ale także do dystrybucji aplikacji dla testerów. Dostępny [tutaj](https://cakebuild.net/dsl/hockeyapp/) dodatek pozwala nam wgrać aplikację iOS czy Androida do HockeyApp. Jeśli mamy więc w zespole testerów, możemy w ten sposób zautomatyzować dostarczanie im nowych wersji aplikacji do testowania. Ciekawa opcja.

#### Podsumowanie

Cake Build to jedno z wielu narzędzi tego typu, ale prawdopodobnie jest to najlepsza opcja dla deweloperów używających głównie .Neta. Pozwala nam w prost sposób zautomatyzować wiele zadań, których nikt nie chce wykonywać, jest zintegrowane ze środowiskiem, może być używane zarówno z Windowsa, jak i Linuxa, czy OSX. Jeśli dążycie w swojej pracy do automatyzacji zadań i nie słyszeliście dotąd o Cake’u, to zainteresowanie się nim może być dobrym następnym krokiem.