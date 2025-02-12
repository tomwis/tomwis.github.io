---
id: 189
title: 'Nowości w C# 7, 7.1 i przedsmak C# 8'
date: '2017-08-20T16:58:37+01:00'
author: 'Tomasz Wiśniewski'
layout: post
guid: 'https://lazyloading.pl/?p=189'
permalink: /2017/08/20/nowosci-w-c-7-7-1-i-przedsmak-c-8/
dsq_thread_id:
    - '6081129888'
categories:
    - 'C#'
---

Od premiery C# 7 minęło już trochę czasu, jednak nie miałem jeszcze okazji za bardzo korzystać z wielu nowych funkcji. Ostatnio wraz z aktualizacją Visual Studio 2017 do wersji 15.3 dostaliśmy także w swoje ręce C# 7.1. Postanowiłem więc sprawdzić, co nowego dla nas przygotowano, a także co czeka nas w przyszłości.

#### C# 7 – krotki (tuples)

Wreszcie mamy używalne krotki (czyli tuples). Nie musimy już tworzyć nowych instancji przez `Tuple.Create`, teraz wystarczą nawiasy:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/e84660133aa3db4042aaeb8beede78c2.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/e84660133aa3db4042aaeb8beede78c2).</noscript></div>Możemy zwracać kilka wartości na raz, a następnie przypisać je od razu do kilku zmiennych. Typ w przypisaniu można podać na zewnątrz nawisów, tak jak tutaj `var` lub wewnątrz przy każdej zmiennej , np. `(int sum, int diff)`.

Możemy też podać jedną nazwę zmiennej:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/4f4c0e38c0f159842d02cddaa4b475e9.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/4f4c0e38c0f159842d02cddaa4b475e9).</noscript></div>Lecz wtedy dostaniemy automatycznie stworzone `calculations.Item1` oraz `calculations.Item2`, a tego raczej nikt nie chce. Lepiej więc podawać nazwę kroki i jej pól:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/9ddfd2d8f3a76aaf6325c9b497993bd2.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/9ddfd2d8f3a76aaf6325c9b497993bd2).</noscript></div>Możemy to też zrobić po prawej stronie:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/95e7847622f8dd495bcfcba6f064e3a6.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/95e7847622f8dd495bcfcba6f064e3a6).</noscript></div>Do nowych krotek potrzebujemy typu `System.ValueTuple`, który jest dostępny na nugecie. Warto też wspomnieć, że nowe krotki to pod spodem struktury i do tego zmienne. Możemy więc przypisać nową wartość do pól krotki, co było wcześniej niemożliwe.

#### C# 7 – odrzuty (discards)

Jest to coś, co głównie będzie się używać z nowymi krotkami. Teraz możemy oznaczyć zmienną podkreślnikiem `_`, jeśli nie potrzebujemy tej wartości.

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/418171f4a77d3b005be40a77f205c3e0.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/418171f4a77d3b005be40a77f205c3e0).</noscript></div>Tutaj zamieniliśmy zmienną `diff` z poprzedniego przykładu na `_` – do `_` nie można się odwołać, jest to po prostu taki odrzut, jak sama nazwa wskazuje.

Składni tej możemy też używać w innych przypadkach, jak np. z funkcjami przyjmującymi parametr `out`.

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/cb64e03a878c659492e33e5f5a1c6cbc.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/cb64e03a878c659492e33e5f5a1c6cbc).</noscript></div>#### C# 7 – zmienne `out`

Obecnie możemy deklarować zmienne `out` od razu w funkcji. Wcześniej trzeba było to robić jako oddzielna zmienna przed funkcją.

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/2429eb24100fe774c8de53301eeec02f.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/2429eb24100fe774c8de53301eeec02f).</noscript></div>Teraz wygląda to tak:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/3f5a2a03fab781a6c5659ecde378bdfe.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/3f5a2a03fab781a6c5659ecde378bdfe).</noscript></div>Jak widzicie można używać nawet `var`. Niby tylko jedna linijka mniej, ale zawsze mi to przeszkadzało.

#### C# 7 – dopasowanie do wzorca (pattern matching)

Dopasowanie do wzorca pozwala nam zaoszczędzić trochę miejsca przy wyrażeniu `is`:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/a5177c0857ffd57c36f8254df9be622e.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/a5177c0857ffd57c36f8254df9be622e).</noscript></div>Teraz od razu w warunku możemy określić nazwę zmiennej. Nie musimy w środku if’a tworzyć przypisania `var square = shape as Square`.

Możemy też użyć dopasowania w instrukcji `switch`:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/ec240d62f700106a8f04524116ba4520.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/ec240d62f700106a8f04524116ba4520).</noscript></div>Sprawdzamy tu np. czy nasz `item` jest typu `int` i od razu nadajemy mu nazwę. Możemy też dodawać warunki lub używać odrzutów (discards) jak przy `case'ach` z tablicą `object`.

#### C# 7 – funkcje lokalne (local functions)

Od teraz możemy tworzyć funkcje w funkcjach. Czasami używamy funkcji prywatnych tylko w jednym miejscu w klasie. W takiej sytuacji funkcja lokalna mogłaby zwiększyć czytelność kodu, bo od razu byłoby widać, że odnosi się ona tylko do tej jednej metody. Dodatkowo możemy też teraz w ładny sposób wydzielić długie warunki w wyrażeniach lambda. Przed zmianami:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/4f0410b70367513a58d2b5310c948a80.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/4f0410b70367513a58d2b5310c948a80).</noscript></div>I po zmianach:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/81b420a6825df0a6767f75e95c184257.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/81b420a6825df0a6767f75e95c184257).</noscript></div>#### C# 7 – więcej expression-bodied members

W C# 6 dostaliśmy tę wygodną składnię dla funkcji i właściwości read-only, teraz dochodzą do tego także konstruktory, destruktory oraz sekcja set we właściwościach i [indexerach](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/indexers/).

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/81aa45adb54d01c6ad5a4ed27e55086d.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/81aa45adb54d01c6ad5a4ed27e55086d).</noscript></div>#### C# 7 – uogólniony typ zwracany przez funkcje asynchroniczne

Do tej pory funkcje `async` mogły zwracać `void`, `Task` lub `Task<T>`. Teraz mogą być to też inne typy. Jako jeden przykładowy dodany został do .Neta `ValueTask`, który jest strukturą i w pewnych sytuacjach może być bardziej wydajny. Na tę chwilę potrzeby jest do niego pakiet z nugeta `System.Threading.Tasks.Extensions`.

#### C# 7 – inne

Jest jeszcze kilka mniejszych nowości jak np. zapis licz w postaci binarnej `0b0001`, czy separator cyfr w stałych `<span class="hljs-number">0</span>b0001_0000`. Pełną listę z większą liczbą przykładów można znaleźć w [dokumentacji](https://docs.microsoft.com/en-us/dotnet/csharp/whats-new/csharp-7).

Przejdźmy teraz do nowości w C# 7.1. Nie ma ich dużo i nie są tak znaczące jak w C# 7, jest to sporo mniejsza aktualizacja. Żeby ich użyć musimy ręcznie przełączyć projekt na C# 7.1. Możemy to zrobić w ustawieniach projektu -&gt; Build -&gt; Advanced -&gt; Language version -&gt; wybrać C# 7.1. Przy niektórych z poniższych funkcji Visual Studio sam zaproponuje tę zmianę.

#### C# 7.1 – default

Od teraz możemy napisać po prostu słówko `default`, żeby użyć domyślnej wartości danego typu. Wcześniej trzeba było podawać także typ, o który nam chodzi `default(int)`.

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/fe473b15b6dc03f5da959748c2f88593.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/fe473b15b6dc03f5da959748c2f88593).</noscript></div>#### C# 7.1 – async Task Main

Funkcja wejściowa `Main()` może teraz mieć w deklaracji `async Task`, a więc można używać w niej `awaita`.

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/8bdf195321b01e1bbaf6ca83236620f2.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/8bdf195321b01e1bbaf6ca83236620f2).</noscript></div>#### C# 7.1 – dopasowanie do wzorca (pattern matching) dla typów generycznych

W C# 7 twórcy zapomnieli o tym przypadku i jest to naprawione w kolejnej wersji języka. Gdy chcemy użyć dopasowania do wzorca na typie generycznym w C# 7, program się nie skompiluje.

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/97f632a0eceb3fda80c91053a1690739.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/97f632a0eceb3fda80c91053a1690739).</noscript></div>#### C# 7.1 – domyślne nazwy pól w krotkach

W C# 7 nazwy pól trzeba było nadawać otwarcie:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/cf4e64fa1cfa861f8d203aee99620f60.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/cf4e64fa1cfa861f8d203aee99620f60).</noscript></div>Jednak skoro mamy już zmienne, które nazywają się tak samo, to po co pisać to drugi raz. Do takiego wniosku doszli też twórcy języka i obecnie nazwy te są dodawane z automatu:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/91da94a4dc0e82ae45e7960455a228c9.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/91da94a4dc0e82ae45e7960455a228c9).</noscript></div>#### C# 8 – co nas czeka?

O C# 8 jeszcze wszystkiego nie wiadomo, ale mamy tu 2 prawdopodobne nowości, które wzbudziły moje zainteresowanie:

- Wszystkie typy referencyjne będą domyślnie nie-nullowe. Oznacza to, że `string`, czy obiekt naszej klasy nie będzie mógł być `nullem`, podobnie jak teraz typy proste (jak `int`). Oczywiście mamy nadal do dyspozycji `T?` i musielibyśmy używać takiej składni, jeśli typ nullowy byłby potrzebny.  
    <span style="display: none;">.</span><div class="oembed-gist"><script src="https://gist.github.com/tomwis/f74bba65d20293d21dbffdd6284655a7.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/f74bba65d20293d21dbffdd6284655a7).</noscript></div><span style="display: none;">.</span>
- Implementacje w interfejsach. Można by pomyśleć, że jest to niepotrzebne, bo przecież od tego są klasy abstrakcyjne. Różnica jest taka, że klasa bazowa może być jedna, a interfejsów wiele. Co więcej, jeśli dodamy metodę do interfejsu, musimy ją zaimplementować we wszystkich klasach – z domyślna implementacją w interfejsie nie trzeba będzie tego robić. Jednak z tego co słyszałem, chodzi tutaj też o Xamarina. W Javie i Swifcie taka funkcjonalność istnieje, więc byłoby łatwiej portować API z Androida i iOSa.  
    <span style="display: none;">.</span><div class="oembed-gist"><script src="https://gist.github.com/tomwis/bb6129eb22b52e840c65d6b1cac8ddf6.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/bb6129eb22b52e840c65d6b1cac8ddf6).</noscript></div><span style="display: none;">.</span>

Na koniec zostawię jeszcze kilka linków:

- [Dokumentacja](https://docs.microsoft.com/en-us/dotnet/csharp/whats-new/csharp-7)
- [The future of C#](https://channel9.msdn.com/Events/Build/2017/B8104) (wideo z Build 2017) – omawiają tutaj nowości z C# i Visual Studio