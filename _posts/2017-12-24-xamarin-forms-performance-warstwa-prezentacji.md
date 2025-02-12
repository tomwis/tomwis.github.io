---
id: 425
title: 'Xamarin.Forms Performance - warstwa prezentacji'
date: '2017-12-24T13:18:13+01:00'
author: 'Tomasz Wiśniewski'
layout: post
guid: 'https://lazyloading.pl/?p=425'
permalink: /2017/12/24/xamarin-forms-performance-warstwa-prezentacji/
dsq_thread_id:
    - '6377123780'
categories:
    - Xamarin
---

Jest to drugi post z cyklu XFP, o sprawdzaniu wydajności Xamarin.Forms. [Ostatnio opisywałem aplikację](/2017/12/17/sprawdzanie-wydajnosci-xamarin-forms/), dzięki której można łatwo sprawdzić wydajność nowej wersji Xamarin.Forms. Trzeba ją jeszcze dopracować, ale są już podstawy. Teraz do tych podstaw dołącza kolejna warstwa – prezentacja wyników.

#### Aplikacja Webowa

Jeśli zbudowałbym tę aplikację i nigdzie nie dzielił się wynikami na bieżąco, to pewnie prawie nikt by z niej nie korzystał. Niby dużo ułatwia, ale i tak mało komu by się chciało. Dlatego myślałem nad sposobem prezentacji wyników. Najprostszym sposobem wydaje mi się Azure App Service. Można tam wrzucić aplikację webową (ASP.NET MVC) i nic za to nie płacić – jest dostępny darmowy pakiet.

Kod aplikacji webowej przechowuję na githubie, razem z aplikacją mobilną: <https://github.com/tomwis/XamFormsPerf>

Usługa na Azure jest o tyle fajna, że można do niej podłączyć githuba i sama będzie pobierać i budować projekt. Jest to spore ułatwienie.

Obecnie stronę można znaleźć pod tym adresem: [http://xamformsperf.azurewebsites.net](http://xamformsperf.azurewebsites.net/)

Jeszcze za dużo tu nie ma. Na razie tylko prezentacja wyników dla poszczególnych testów w tabelkach (i to tylko dla Androida). Każdy test pokazuje czas wykonania w milisekundach dla każdej wersji Xamarin.Forms. W ostatniej kolumnie możemy zobaczyć o ile procent szybsza/wolniejsza jest nowa wersja w stosunku do poprzedniej (poprzedniej w tabelce, dla jasności). Na razie są to tylko testowe pomiary, ale niedługo będę chciał podmienić je na prawidłowe, wykonane na prawdziwym urządzeniu. Dodatkowo, będę chciał też dodać prezentację wyników w postaci wykresów. Na pewno będzie to łatwiejsze w odbiorze niż takie tabelki.

#### Podsumowanie

Powoli klaruje się wizja tego projektu. Mam nadzieję, że wyjdzie z niego coś dobrego i przyda się społeczności Xamarin.Forms 🙂

Obecnie samo odpalanie testów i określanie wersji Xamarin.Forms dla aplikacji mobilnej odbywa się ręcznie. Fajnie byłoby to także zautomatyzować, ale musiałbym wtedy używać Test Cloud, który trochę kosztuje, co jest niezbyt dobrą wiadomością dla takiego projektu open-source. No cóż, pomyślę jeszcze nad tym.

W dalszych etapach chciałbym dodać więcej różnorodnych testów oraz przetestować testy ui dla iOSa. A co z UWP? Na razie chyba sobie daruję, bo zainteresowanie pewnie nie byłoby zbyt wielkie.

Jeśli masz jakieś uwagi, pomysły, sugestie, to bardzo chętnie o nich przeczytam w komentarzach.