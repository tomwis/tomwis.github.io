---
id: 1
title: 'Conditional Reference w csproj na przykładzie bibliotek do reklam w Xamarin.Android'
date: '2017-08-06T18:22:52+01:00'
author: 'Tomasz Wiśniewski'
layout: post
guid: 'http://twnet_blog.x25.pl/?p=1'
permalink: /2017/08/06/conditional-reference-csproj-przykladzie-bibliotek-reklam-xamarin-android/
dsq_thread_id:
    - '6045488174'
categories:
    - 'Visual Studio'
    - Xamarin
---

Przy tworzeniu aplikacji zdarza się czasami potrzeba wywołania kodu tylko w określonej konfiguracji. Często jest to wykorzystywane, gdy np. w trybie debug chcemy mieć wartości testowe, a w trybie release wartości produkcyjne.

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/44b155616602310e8f770901e832706e.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/44b155616602310e8f770901e832706e).</noscript></div>Przy aplikacjach mobilnych możemy to wykorzystać np. w sytuacji, gdy chcemy wyświetlać testowe reklamy podczas debugowania, żeby nie nabijać wyświetleń, czy przypadkiem w nie nie kliknąć (czego reklamodawcy nie lubią). Powyższy warunek, czyli dyrektywa preprocesora jest pewnie większości znana. Jednak ciekawostką jest, że podobne warunki możemy tworzyć także w pliku csproj.

#### Warunkowe odniesienia do bibliotek

Posłużę się podobnym przykładem – z reklamami. Załóżmy, że tworzymy aplikację w Xamarinie na Androida, w której będziemy wyświetlać baner reklamowy, ale tylko w sklepie Google Play. Dodatkowo chcemy opublikować drugą wersję aplikacji w Amazon AppStore. Ta wersja będzie tylko płatna i od razu bez reklam. Skoro nie będzie w niej reklam, to możemy wyrzucić referencje do kilku bibliotek. Tutaj byłyby to Xamarin.GooglePlayServices.Ads i jej zależności. Wyrzucanie ich ręcznie za każdym razem, a potem dodawanie dla sklepu Google Play, byłoby niezwykle męczące. Dlatego też **wykorzystamy tutaj warunki w csproj tak, żeby biblioteki te były automatycznie usuwane lub dodawane przy zmianie konfiguracji.**

#### Dodatkowa konfiguracja

Stwórzmy zatem najpierw potrzebne konfiguracje. W tym celu przechodzimy do Build -&gt; Configuration Manager. Z „Active Solution Configuration” wybieramy &lt;New…&gt; i tworzymy „ReleaseAmazon”, a w „Copy settings from” wybieramy Release (czy inną naszą produkcyjną konfigurację). Dalej w okienku Configuration Manager przełączamy „Active solution configuration” na właśnie stworzoną ReleaseAmazon. Dla projektu androidowego przełączamy w kolumnie Configuration obecną konfigurację na ReleaseAmazon.

#### Edycja pliku projektu

Gdy mamy naszą konfigurację, możemy wtedy otworzyć plik .csproj w dowolnym edytorze tekstowym (lub w Visual Studio po wykonaniu „Unload project” -&gt; „Edit ProjectName.csproj”). Przechodzimy do linijki z referencją do reklam:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/7c2f9db2baa7cb9e761b224913ef5747.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/7c2f9db2baa7cb9e761b224913ef5747).</noscript></div>W tym miejscu **do znacznika Reference dodajemy atrybut Condition z odpowiednim warunkiem**:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/52ddb7e34e74aa94a3b975ad1a508068.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/52ddb7e34e74aa94a3b975ad1a508068).</noscript></div>W całości wygląda to tak:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/686862021f64d1ceb6392a507906e73a.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/686862021f64d1ceb6392a507906e73a).</noscript></div>Teraz biblioteka ta zostanie dodana do każdej konfiguracji oprócz ReleaseAmazon – u nas będę to po prostu Release i Debug, czyli konfiguracje, których używalibyśmy dla wersji aplikacji do Google Play. Podobną operację powinniśmy wykonać do wszystkich zależnych bibliotek (chyba, że używamy, którejś z nich także w innym celu). Na tę chwilę są to:

> Xamarin.GooglePlayServices.Ads.Lite
> 
> Xamarin.GooglePlayServices.Base
> 
> Xamarin.GooglePlayServices.Basement
> 
> Xamarin.GooglePlayServices.Clearcut
> 
> Xamarin.GooglePlayServices.Gass
> 
> Xamarin.GooglePlayServices.Tasks

Następnie przechodzimy do kolejnej sekcji, którą należy zmodyfikować. **Wyszukujemy tag Import, który w atrybucie Project ma ścieżkę do Xamarin.GooglePlayServices.Ads.targets.** Tutaj także musimy dodać warunek, jednak jeden warunek w tagu Import już istnieje. Swój możemy dodać z łącznikiem „And”:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/9c65c2016ea5f21282169c1ca9693903.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/9c65c2016ea5f21282169c1ca9693903).</noscript></div>To samo musimy zrobić dla Importów od bibliotek zależnych – dokładnie takich jak wymienione wyżej.

#### Podsumowanie

Po zmianach możemy zapisać plik csproj, wrócić do Visual Studio i przełączyć się na konfigurację ReleaseAmazon. Przejdźmy po tym do Solution Explorer i w projekcie Androida rozwińmy References. Zobaczymy, że biblioteki z warunkiem mają przy sobie żółty trójkąt. W niczym to jednak nie przeszkadza. Możemy zbudować projekt i sprawdzić, że wszystko działa jak należy. Oczywiście, jeśli mamy w kodzie odwołania do usuniętych bibliotek, trzeba będzie je opakować w dyrektywy preprocesora:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/42c5c0dfb7499de610639dfa4afc7059.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/42c5c0dfb7499de610639dfa4afc7059).</noscript></div>Symbol „AMAZON” użyty w kodzie powyżej należy zdefiniować w ustawieniach projektu dla konfiguracji ReleaseAmazon:

- ProjektAndroida -&gt; Properties -&gt; Build -&gt; wybrać z dropdowna Configuration „ReleaseAmazon” -&gt; do „Conditional compilation symbols” wpisać AMAZON

Podobną operację możemy wykonać z konfiguracją Debug i stworzyć konfigurację DebugAmazon. Wtedy dodalibyśmy do tagów Reference i Import dodatkowy warunek z łącznikiem „And”:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/4f28b313db486b1295102cd421d83fc0.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/4f28b313db486b1295102cd421d83fc0).</noscript></div>Taka automatyzacja dodawania/usuwania bibliotek może nam zaoszczędzić sporo czasu. Na pewno warto poświęcić chwilę na konfigurację tego mechanizmu, ponieważ jest bardzo łatwe i zajmuje dosłownie kilka minut.

Więcej informacji o opisanych warunkach można znaleźć w dokumentacji – [tutaj](https://docs.microsoft.com/pl-pl/visualstudio/msbuild/msbuild-conditions).