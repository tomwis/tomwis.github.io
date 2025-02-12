---
id: 372
title: 'Wprowadzenie do ReactiveUI'
date: '2017-11-19T16:03:12+01:00'
author: 'Tomasz Wiśniewski'
layout: post
guid: 'https://lazyloading.pl/?p=372'
permalink: /2017/11/19/wprowadzenie-do-reactiveui/
dsq_thread_id:
    - '6295033211'
categories:
    - Xamarin
---

[Ostatnio pisałem o Reactive Extensions]/2017/11/12/asynchronicznosc-reactive-extensions-prosta/), które bardzo mi się spodobały. Dzisiaj chciałbym zrobić krótki wstęp do ReactiveUI.

#### Co to jest?

ReactiveUI to framework MVVM, który wykorzystuje Reactive Extensions i pozwala nam z nich korzystać ze wzorcem MVVM. Może być wykorzystywany razem z innymi frameworkami MVVM, tak więc możemy używać np. MvvmCross, a do ReactiveUI odwoływać się tylko, gdy będziemy widzieli taką potrzebę.

Wykorzystam tu taki sam przykład, jak w poście o Reactive Extensions, czyli wyszukiwarka filmów.

#### Widok i bindowania

Sam widok jest bardzo prosty:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/84269ea15f06cbfacbf89d92a6c67fb0.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/84269ea15f06cbfacbf89d92a6c67fb0).</noscript></div>Mamy tutaj tylko pole do wpisywania, ActivityIndicator i listę z wynikami. Może rzuciło wam się w oczy, że nie ma tu bindowań. Mogliśmy używać zwykłych bindowań z Xamarin.Forms, jednak ReactiveUI oferuje także swój mechanizm i z niego skorzystamy. Deklaruje się go w code behind:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/d554d74dc5079202e5d5b7052cff95da.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/d554d74dc5079202e5d5b7052cff95da).</noscript></div>Po kolei, co musimy zrobić:

1. Strona musi implementować interfejs `IViewFor`. W nim znajdują się właściwości `ViewModel` o podanym typie generycznym oraz o typie `object`.
2. Do właściwości `ViewModel` przypisujemy nasz view model (linia 6).
3. Teraz możemy tworzyć bindowania w kodzie. Nie ma tutaj wersji dla xamla. Do bindowań używamy extension methods `Bind` (tworzy two-way binding) i `OneWayBind`.

Mam pewne wątpliwości, czy jest to dobre rozwiązanie, ale zrobiłem to tutaj do przetestowania frameworka. W porównaniu do innych rozwiązań trzeba się tu trochę więcej napisać. Nie wiem dlaczego tak to rozwiązano. Jedyne co przychodzi mi do głowy to to, że dzięki użyciu interfejsu możemy to rozwiązanie połączyć z innymi frameworkami, które często mają swoją klasę bazową dla stron. Tylko czy jest sens to robić po stronie widoku? Przykładowo, MvvmCross miałby swoją właściwość ViewModel i swój system bindowań, więc ReactiveUI jest tu już raczej niepotrzebne.

#### ViewModel

Nasz view model powinien dziedziczyć po `ReactiveObject`. Do wyszukiwania będziemy potrzebowali tekstu do wyszukania, komendy, która wyszuka filmy na podstawie podanego tekstu oraz listy wyników:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/5c3a58174396bc505e208ad44096b0ce.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/5c3a58174396bc505e208ad44096b0ce).</noscript></div>Używamy tutaj `ReactiveCommand` jako komendy. Pierwszy parametr generyczny (`string`) to parametr przekazywany do komendy, a drugi (`IEnumerable<string>`) to zwracana wartość, czyli lista wyników. `ReactiveList<string>` to reaktywna lista do wyświetlania wyników – jest ona zbindowana bezpośrednio do `ListView.ItemsSource` w naszym widoku. `SearchTerm` to zwykła właściwość przechowująca wartość, którą wpiszemy w polu tekstowym. `RaiseAndSetIfChanged` to metoda z ReactiveUI, która, jak sama nazwa wskazuje, zajmuje się już `INotifyPropertyChanged`.

Następnie musimy stworzyć `ReactiveCommand` za pomocą metody `ReactiveCommand.CreateFromTask`, która wywoła usługę zwracającą listę pasujących filmów. Komenda zostanie wywołana jeśli warunek `canSearch` jest spełniony. Definiujemy go wyżej z pomocą `WhenAny`, które obserwuje, czy jakaś właściwość uległa zmianie. U nas nasłuchujemy na zmiany w `SearchTerm` i dodatkowo sprawdzamy, czy mamy wpisane przynajmniej 3 znaki.

Do komendy należy później dodać subskrypcję (inaczej się nie wywoła), w której robimy coś z wynikami. Tutaj przypisujemy je do listy `Results`, aby wyświetlić je użytkownikowi.

Na koniec używamy `WhenAnyValue`, żeby nasłuchiwać na zmiany w `SearchTerm` i wywołać wtedy naszą komendę – ale nie częściej niż 500 ms.

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/42dd6b4a7acc7fa01ce4e65a3bbc788d.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/42dd6b4a7acc7fa01ce4e65a3bbc788d).</noscript></div>Fajnie byłoby też widzieć, że wyszukiwanie jest w toku – dlatego w widoku dodaliśmy ActivityIndicator. W view modelu musimy stworzyć specjalne pole typu `ObservableAsPropertyHelper<bool>`:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/abb1ee864dbef12eef649c21f5758bf8.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/abb1ee864dbef12eef649c21f5758bf8).</noscript></div>Wartość z tego pola zwracam później w publicznej właściwości `IsSearching`, która jest zbindowana do `ActivityIndicator.IsVisible` w widoku. Chcemy, żeby wartość tego pola zmieniała się, gdy rozpoczynamy i kończymy wyszukiwanie. Musimy więc nasłuchiwać, czy komenda się wykonuje z metodą `ToProperty`:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/3f8dd7c893d3c7aa374ef9d4f1873f0e.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/3f8dd7c893d3c7aa374ef9d4f1873f0e).</noscript></div>Powyższe rozwiązanie dostępne jest w przykładowym kodzie w konfiguracji `Debug`.

#### Problemy

Powyższa wersja działa niestety trochę inaczej niż przykład z Reactive Extensions. Gdy nasza komenda się wywoła, a my dalej coś wpisujemy, to drugi raz się nie odpali. Uruchomi się dopiero, gdy zaczniemy coś wpisywać, gdy poprzednia komenda już się zakończy. W pewnym scenariuszach to pewnie pożądane rozwiązanie, jednak nie u nas – możemy przez to dostać wyniki do nieaktualnej wartości.

Możemy trochę zmodyfikować kod inicjalizujący komendę, tak żeby kolejne komendy wywoływały się bez czekania na skończenie poprzedniej:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/6d9690c5152f4849eaebcc94ff6f6bb8.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/6d9690c5152f4849eaebcc94ff6f6bb8).</noscript></div>Tworzymy teraz komendę z `Observable`. Używamy też `TakeUntil`, które działa w ten sposób, że nasze `Observable` ze `StartAsync` będzie zwracać elementy, aż `canSearch` (także `Observable`) zwróci jakiś element – a stanie się to, gdy wpiszemy coś do pola tekstowego (na które w `canSearch` nasłuchujemy). To rozwiązanie można przetestować przestawiając konfigurację w testowym projekcie na `Debug2`.

Powyższy kod jest już trochę bardziej skomplikowany, a pozostaje jeszcze jeden problem – kolejność przychodzenia wyników. Jeśli odpowiedź do wcześniej wpisanego tekstu przyjdzie później (przez opóźnienia), to wyświetlą nam się złe wyniki. W przykładzie z Reactive Extensions rozwiązaliśmy to metodą `Concat`. W ReactiveUI jeszcze nie do końca wiem gdzie i co dokładnie trzeba wpisać, żeby działało to tak jak powinno.

Natknąłem się na jeszcze jeden problem – `ReactiveList`, której używam w tym przykładzie nie działa na iOS. Lista się nie odświeża. Na Androidzie i UWP wszystko działa poprawnie. Fix jest w miarę prosty – musimy po prostu użyć zwykłego `ObservableCollection` z `INotifyPropertyChanged`. Dyskusja o tym błędzie toczyła się na repo ReactiveUI przez… 2,5 roku. Nie czytałem całego wątku, ale issue został już zamknięty i z tego co się zorientowałem nie został poprawiony. Chyba uważają, że to błąd Xamarin.Forms 🙂 Cóż, być może tak jest, kto wie. Poniżej link do dyskusji:

<https://github.com/reactiveui/ReactiveUI/issues/806>

#### Dokumentacja

Problem z ReactiveUI jest jeszcze jeden – dokumentacja. Na początku wydawało mi się, że jest całkiem dobrze. Na oficjalnej stronie są opisane główne założenia, metody, jest jakiś przykład. Znajdą się nawet jakieś nagrania z konferencji. Jednak im dalej w las, tym jest gorzej. Okazuje się, że w ReactiveUI z wersji na wersję często robione są duże zmiany. A dokumentacja i przykłady nie zawsze są aktualizowane. Dlatego często czyta się, czy ogląda materiał, próbuje się go zastosować i okazuje się, że coś nie działa – bo w obecnej wersji robi się to już inaczej. Wydaje mi się, że przez to próg wejścia jest dosyć wysoki.

#### Podsumowanie

ReactiveUI wydaje się być ciekawym rozwiązaniem, jednak posiada pewne wady. Nie jest tak łatwy w użyciu jakbym tego oczekiwał. Co prawda, dopiero zaczyna go poznawać i może dalej będzie lepiej, ale to się dopiero okaże.

Kod przykładowej aplikacji: <https://github.com/tomwis/ReactiveUIExample>

Oficjalna strona: <https://reactiveui.net>