---
id: 331
title: 'Asynchroniczność z Reactive Extensions jest bardzo prosta'
date: '2017-11-12T11:48:39+01:00'
author: 'Tomasz Wiśniewski'
layout: post
guid: 'https://lazyloading.pl/?p=331'
permalink: /2017/11/12/asynchronicznosc-reactive-extensions-prosta/
dsq_thread_id:
    - '6278942297'
categories:
    - Xamarin
---

Dzisiaj chciałbym zrobić krótki wstęp do [Reactive Extensions](http://reactivex.io). Sam dopiero zapoznaję się z tym tematem, dlatego też znam tylko podstawowy tej biblioteki. Wygląda ona jednak bardzo zachęcająco. W pewnych okolicznościach potrafi bardzo uprościć kod, a więc i skrócić czas potrzebny na jego pisanie oraz późniejsze utrzymywanie.

#### Czym są Reactive Extensions?

Najprościej mówiąc, Reactive Extensions (czy w skrócie – Rx) to biblioteka, która umożliwia nam pisanie reaktywnych aplikacji, czyli takich, które nasłuchują na zmiany i same odświeżają np. UI, a nie odpytują o zmiany co pewien czas (np. bazę danych, czy jakąś usługę). Mamy tu 3 składniki:

- Dane wejściowe z jakiegoś zdarzenia, które są traktowane jako strumień danych (Observable)
- LINQ za pomocą którego możemy wykonywać różne operacje na tym strumieniu (jest to bardziej rozbudowania wersja LINQ z bardzo fajnymi możliwościami)
- Subskrypcje, w których dostajemy dane i możemy z nimi coś zrobić

Koncept tego rozwiązania jest niby prosty, ale na początku może być ciężko zrozumieć, w czym to pomaga i w jakich sytuacjach może się to przydać. Tak naprawdę opieramy się tu na zdarzeniach, dlaczego więc po prostu nie użyć zwykłych zdarzeń?

#### W czym nam to pomaga?

1. Przeznaczeniem Rx jest pracowanie ze strumieniami danych, więc jeśli mamy taki scenariusz, to możemy znacznie uprościć kod. Jeśli nie, to być może niekoniecznie powinniśmy go używać.
2. Rx nie ogranicza się do prostych zdarzeń. W postaci strumienia można przedstawić też inne rzeczy, np. możemy nasłuchiwać na wyniki z usługi.
3. W prosty sposób możemy filtrować przychodzące wartości za pomocą LINQ.
4. Możemy wykonywać różne operacje na strumieniach, jak łączenie kilku strumieni w jeden (na różne sposoby).

#### Przykład

Dobrym przykładem będzie praca z danymi wprowadzanymi przez użytkownika, np. przy wyszukiwaniu. Użytkownik wpisuje w wyszukiwarce nazwę filmu, my nasłuchujemy na zdarzenie zmiany zawartości pola tekstowego, odpytujemy pewną usługę o nazwy filmów i prezentujemy wyniki dla użytkownika. Na jakie problemy się tutaj natkniemy?

- Użytkownicy często piszą szybko. Nie chcielibyśmy więc wysyłać zapytania dla każdej dodanej literki, tylko po pewnym czasie gdy użytkownik przestał już pisać.
- Nie chcemy wyszukiwać filmów od razu, po jednej literce, bo otrzymalibyśmy ogromną liczbę wyników. Lepiej jest poczekać i zacząć wyszukiwać np. gdy mamy już wpisane 3 litery.
- Jeśli działamy z usługą internetową, a w dzisiejszych czasach tak prawdopodobnie będzie, to mogą zdarzać się opóźnienia. Możliwe jest, że wyślemy zapytanie, użytkownik zmieni tekst, wyślemy kolejne zapytanie z nowym tekstem, nowy wynik zostanie zwrócony, a starsze zapytanie miało z jakiegoś powodu duże opóźnienie i przyszło jeszcze później. W takim wypadku wyświetlimy użytkownikowi złe wyniki (wyniki od poprzedniego zapytania). Chcielibyśmy więc, żeby wyniki przychodziły w takiej kolejności w jakiej wysyłaliśmy zapytania.

Napisanie kodu, który obsłuży powyższe scenariusze zajęłoby trochę czasu (i miejsca). Z Reactive Extensions taki kod zajmuje 7 linijek. Ale po kolei.

#### Trochę kodu

Użyjemy tutaj Xamarin.Forms. Żeby skorzystać z Rx musimy zainstalować paczkę z nugeta [System.Reactive](https://www.nuget.org/packages/System.Reactive/). Tworzymy sobie kontrolę `Entry` z nazwą `searchEntry`. Następnie podłączamy się do zdarzenia `TextChanged` i nasłuchujemy na nie za pomocą `Observable.FromEventPattern`. Stworzy nam to kolekcję `IObservable`, na których to kolekcjach opiera się całe Rx. Na takiej kolekcji możemy używać LINQ jak w tym fragmencie:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/0a15835206b4c9e60c31528287a5ec90.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/0a15835206b4c9e60c31528287a5ec90).</noscript></div>Używamy tutaj `Select`, gdzie wywołujemy metodę `GetMovies`, która jest asynchroniczna i przekazujemy do niej tekst z kontrolki. Następnie subskrybujemy do tej kolekcji, a gdy dostaniemy wyniki, to wyświetlamy je w kontrolce `Label`, z nazwą `searchResults`. To chyba najprostszy przykład. Jak na razie nie ma tu nic niezwykłego. Dodajmy tu kilka elementów, o których wspominałem wyżej:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/c8e8d2ac7e45d5d55449d6f3dcb5f2fc.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/c8e8d2ac7e45d5d55449d6f3dcb5f2fc).</noscript></div>Dodaliśmy tutaj `Where`, żeby wyszukiwanie zaczynało się od 3 znaków. Następnie mamy `Throttle`. Jest to specjalna metoda LINQ z Reactive Extensions. Dzięki niej możemy ograniczyć ilość zapytań. Jeśli użytkownik wpisuje nowe litery szybko, to tylko ostatni wynik w danych 500 milisekundach zostanie przekazany dalej (do usługi, która wyszukuje filmy). Po `Select` korzystamy z metody `Concat`. Zapewnia nam ona to, że wyniki przyjdą nam w takiej kolejności, w jakiej wysyłane były zapytania. I ostatnia metoda, `ObserveOn`, jest potrzebna do ustalenia wątku, tak żebyśmy w `Subscribe` mogli dopisać tekst do kontroli na wątku UI.

Ściągnijcie sobie aplikację, do której link jest na końcu posta i przetestujcie to sami. W konfiguracji `Debug` możecie zobaczyć gotowe rozwiązanie zgodne z powyższymi wytycznymi. Metoda `GetMovies` ma wbudowane losowe, małe opóźnienie, więc wyniki nie będą natychmiastowe.

Możecie też przestawić konfigurację na `Debug2`. Tutaj ładujemy rozwiązanie bez metody `Concat`. Gdy będziecie wpisywać znaki do pola tekstowego możecie podejrzeć w `Outpucie` w jakiej kolejności przychodzą wyniki – wyniki tutaj to ten sam tekst, który wpisujecie. Zauważycie, że czasami nie będą w poprawnej kolejności (gdy wpisujecie coś bardzo szybko). Możecie przejść do metody `InitRxNoConcat` w pliku MainPage.xaml.cs w projekcie PCL. Tam można zakomentować metodę `SelectMany` i odkomentować `Select` i `Concat` – po tej operacji wyniki powinny zawsze przychodzić w poprawnej kolejności. Zauważycie pewnie jednak w `Outpucie`, że trwa to dłużej – w końcu czasami musimy poczekać, tak żeby przestawić wyniki w dobrej kolejności.

#### Podsumowanie

Reactive Extensions nie jest nowością, jest już z nami od wielu lat i niektórzy na pewno dobrze je znają. Mam jednak wrażenie, że biblioteka ta nie jest znana wystarczająco dobrze i nie jest zbyt chętnie używana. Wydaje mi się, że może to być spowodowane dużym progiem wejścia. Trzeba tu bowiem trochę przestawić swój sposób myślenia, tak żeby zrozumieć jak działają strumienie w Rx i jakie jest ich zadanie. Po początkowym zapoznaniu się z tą biblioteką uważam, że warto jej się bliżej przyjrzeć. Może ona znacznie uprościć kod w pewnych sytuacjach. Dodatkowo, dla .neta, w tym i Xamarina, dostępny jest framework MVVM [ReactiveUI](https://reactiveui.net), który opiera się na Rx. Co więcej, można go łączyć z innymi framworkami MVVM, jak np. MvvmCross. Dzięki temu, możemy wykorzystywać go tylko w niektórych sytuacjach, tam gdzie jest potrzebny. A w innych dalej opieramy się na zwykłym MVVM.

Przykładowa aplikacji: <https://github.com/tomwis/RxExample>

Przykłady dla Rx: <http://rxwiki.wikidot.com/101samples>