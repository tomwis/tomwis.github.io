---
id: 404
title: 'LiveXAML - podgląd UI dla Xamarin.Forms w czasie rzeczywistym'
date: '2017-12-10T12:02:05+01:00'
author: 'Tomasz Wiśniewski'
layout: post
guid: 'https://lazyloading.pl/?p=404'
permalink: /2017/12/10/livexaml-podglad-ui-dla-xamarin-forms-czasie-rzeczywistym/
dsq_thread_id:
    - '6340630310'
categories:
    - Xamarin
---

Jedną z bolączek Xamarina jest to, że czasami podczas budowania aplikacji pojawiają się dziwne problemy, które nie co końca wiadomo jak rozwiązać. Często pomocne jest usunięcie folderów bin/obj, czy restart Visual Studio. Jeśli już projekt się buduje to trochę czasu może to potrwać. Chyba zależy to od humoru Visuala 🙂 Zawsze zastanawiałem się, jak na różnych prezentacjach, czy w Xamarin University, projekty odpalają się niemal natychmiast. Prezentujący i tak mówi „trzeba chwilę poczekać”, a ja tylko sobie myślę „żebyś ty człowieku zobaczył jak to u mnie działa…”.

Niemniej jednak, jednym z obejść dla tego problemu jest podgląd UI na żywo w aplikacji. Jest to obejście częściowe, bo pomaga tylko przy projektowaniu wyglądu w xamlu, ale jest to pewna pomoc.

#### Xamarin Live Player

Jakiś czas temu Xamarin pokazał [Live Player](https://www.xamarin.com/live). Jest to aplikacja, którą instalujemy na naszym smartfonie i dzięki niej możemy widzieć na żywo zmiany wprowadzane w kodzie w Visualu. Nie używałem jeszcze tej funkcji, ponieważ nadal jest w preview i trzeba zainstalować dla niej wersję preview Visual Studio. Nie mogę się więc wypowiedzieć jak działa, ale znając jakoś stabilnych wydań Xamarina, domyślam się, że przed nami jeszcze daleka droga. Live Player nie działa także z emulatorami, trzeba korzystać z fizycznego urządzenia (sprostowanie: od czasu gdy pisałem ten post, Xamarin zdążył wydać już Live Player w stabilnej wersji Visuala, więc może niedługo coś o nim napiszę).

#### LiveXAML

[LiveXAML](http://www.livexaml.com) pozwala nam na podgląd zmian w plikach xaml w odpalonej aplikacji. Działa z i bez debugowania. Możemy z niego korzystać na emulatorach oraz urządzeniach (w tej samej sieci).

Gdy zrobimy jakąś zmianę, musimy zapisać plik i widok się odświeża. Z krótkiej zabawy tym narzędziem mogę powiedzieć, że działają wszystkie podstawowe modyfikacje. Również zmiany właściwości `Padding` czy `Margin`. Odnoszę się do nich, ponieważ Microsoft wprowadził podobną funkcję do xamla dla UWP, jednak tam nie można zmieniać marginesów, bo aplikacja się wywala 🙂 W LiveXAML nie ma tego problemu. Działają również custom renderery i efekty.

Oczywiście, zmiany działają tylko dla xamla. Jeśli więc dojdziemy do momentu, gdzie musimy stworzyć konwerter, aplikację trzeba zatrzymać, napisać konwerter w C# i wtedy można wrócić do debugowania i podglądu na żywo. Podobnie z efektami i custom rendererami. Czasami więc trzeba robić przerwy. Jednak korzyści i tak są dużo. Nie musimy po każdej małej zmianie budować i odpalać aplikacji, co zazwyczaj zajmuje masę czasu.

Nie ma tutaj co dużo pisać. Trzeba to po prostu wypróbować samemu 🙂

#### Cena

LiveXAML to produkt niepochodzący od Xamarina i jest w większości płatny. Ma okres próbny, a po nim możemy używać go za darmo dla projektów z max. trzema plikami xamla. Jeśli mamy więc jakieś bardzo małe projekty lub tylko częściowo w xamlu (a częściowo w C#), to da się używać tego narządzia za darmo. W innym wypadku trzeba zapłacić. Jednak w większości przypadków prawdopodobnie warto. Zaoszczędzimy dzięki temu masę czasu i poniesiony koszt szybko się zwróci.

#### Podsumowanie

Deployment aplikacji Xamarin.Forms potrafi trwać długo i robienie tego przy każdej małej zmianie sprawi, że zmarnujemy masę czasu. Widziałem, że w innych technologiach, jak ReactNative, zmiany widoczne są w czasie rzeczywistym. Bardzo bym chciał, żeby, na ile to możliwe, był to też standard w Xamarinie. LiveXAML mocno przybliża nas do tego celu i życzę temu projektowi jak najlepiej. Mam nadzieję, że Xamarin i Microsoft szybko nadgonią i funkcja ta będzie wbudowana w Visuala. Skoro stworzyli coś podobnego dla UWP, to Xamarin powinien być następny w kolejce.