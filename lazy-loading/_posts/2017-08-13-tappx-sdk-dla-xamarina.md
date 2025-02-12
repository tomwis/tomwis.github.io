---
id: 84
title: 'Tappx SDK dla Xamarina'
date: '2017-08-13T20:03:04+01:00'
author: 'Tomasz Wiśniewski'
layout: post
guid: 'https://lazyloading.pl/?p=84'
permalink: /2017/08/13/tappx-sdk-dla-xamarina/
dsq_thread_id:
    - '6063478949'
categories:
    - Xamarin
---

#### Czym jest Tappx?

Tappx to platforma do promowania swoich aplikacji. Jest to tzw. „cross-promotion”. Umieszczamy w swojej aplikacji reklamy Tappx i tak promujemy inne aplikacje z tej sieci. Za to zdobywamy punkty i dzięki temu inne aplikacje, które zintegrowały Tappx, mogą wyświetlać reklamy z naszą aplikacją. Wspólnie się promujemy i jest to całkowicie darmowe. Nie musimy tu płacić za wyświetlanie reklam z naszą aplikacją, bo zapłatą jest już miejsce reklamowe, które udostępniamy w naszej aplikacji.

Jeśli używałeś AdDuplexa na Windows Phone, to powinieneś wiedzieć o co chodzi. Działa to na podobnej zasadzie.

#### Biblioteka dla Xamarina

Tappx jest dostępny na Androida i iOS, nie mają biblioteki dla UWP, czy dla Xamarina. Na szczęście w dosyć prosty sposób da się tworzyć wiązania C# do bibliotek napisach w Javie. Można to zrobić zgodnie z [tym](https://developer.xamarin.com/guides/android/advanced_topics/binding-a-java-library/) poradnikiem w dokumentacji Xamarina. Chciałem wypróbować jak Tappx będzie się spisywał, dlatego stworzyłem dla niego takie wiązanie i przy okazji opublikowałem jako paczkę nuget:

- Projekt na githubie: <https://github.com/tomwis/tappxbinding>
- Nuget: <https://www.nuget.org/packages/Xamarin.Bindings.Tappx>

#### Jak używać?

Procedura jest bardzo prosta:

1. Dodajemy nugeta do projektu androidowego.
2. Dodajemy banner Tappx.  
        <span style="color: #ffffff; display: none;">.</span>  
        W xml:  
        <span style="color: #ffffff; display: none;">.</span><div class="oembed-gist"><script src="https://gist.github.com/tomwis/36d0af81da4e94bf06eecb1dfe571768.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/36d0af81da4e94bf06eecb1dfe571768).</noscript></div><span style="color: #ffffff; display: none;">.</span>  
        Lub kodem:  
        <span style="color: #ffffff; display: none;">.</span>
        
        <div class="oembed-gist"><script src="https://gist.github.com/tomwis/54c10e7ca8a8f91922ebc36732f18b12.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/54c10e7ca8a8f91922ebc36732f18b12).</noscript></div><span style="color: #ffffff; display: none;">.</span>  
        Możemy też dodać reklamę pełnoekranową, jeśli chcemy.
3. W miejsce „/xxxxxxxxxxxxx/Pub-xxxx-Android-xxxx” wstawiamy własny klucz. Aby go wygenerować idziemy na [stronę Tappx](http://www.tappx.com/?h=d1a2c1f1248ea6cf512a5440a6d249bc) (uwaga: link afiliacyjny – dostaję punkty, jeśli się z niego zarejestrujesz, jeśli nie chcesz mi pomagać, to nie klikaj ;)), zakładamy konto i tworzymy nową aplikację.
4. ???
5. Profit. Albo i nie.

#### Integracja z AdMobem
    
Prawdopodobnie będziemy chcieli wyświetlać te reklamy na przemian z reklamami płatnymi, np. AdMob od Google. Najczęściej wyświetla się reklamy płatne, a jeśli nie są dostępne, to wtedy robimy request do sieci takiej jak Tappx. Chyba że bardziej zależy nam na promocji aplikacji niż na zarabianiu. Wtedy model ten może być inny. Przyjmując, że chcemy pokazać dla użytkownika banner Tappx, podczas gdy nie mamy dla niego płatnej reklamy, wtedy możemy podłączyć się pod listener i sprawdzić zdarzenie OnAdFailedToLoad:
    
<div class="oembed-gist"><script src="https://gist.github.com/tomwis/4e1b4a8684371c9a71b05d566424c2d3.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/4e1b4a8684371c9a71b05d566424c2d3).</noscript></div>
    
Podobnie w drugą stronę – gdy Tappx z jakiegoś powodu się nie załaduje lub przy kolejnym odświeżeniu bannera chcielibyśmy sprawdzić, czy może AdMob ma dla nas jakieś reklamy, możemy podłączyć się pod podobny listener:
    
<div class="oembed-gist"><script src="https://gist.github.com/tomwis/21362242f22610bd915a377b65a2fe7f.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/21362242f22610bd915a377b65a2fe7f).</noscript></div>

#### Podsumowanie
    
Warto zajrzeć do dokumentacji Tappx [tutaj](http://www.tappx.com/en/manual/), żeby zapoznać się ze wszystkimi możliwościami SDK.