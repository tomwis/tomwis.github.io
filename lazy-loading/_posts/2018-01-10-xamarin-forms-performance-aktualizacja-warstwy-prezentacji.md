---
id: 440
title: 'Xamarin.Forms Performance - aktualizacja warstwy prezentacji'
date: '2018-01-10T21:21:41+01:00'
author: 'Tomasz Wiśniewski'
layout: post
guid: 'https://lazyloading.pl/?p=440'
permalink: /2018/01/10/xamarin-forms-performance-aktualizacja-warstwy-prezentacji/
dsq_thread_id:
    - '6405103923'
categories:
    - Xamarin
---

Już wcześniej wspominałem o swoim projekcie odnośnie testowania wydajności kolejnych wersji Xamarin.Forms. Wpisy temu poświęcone znajdziecie [tutaj]/2017/12/17/sprawdzanie-wydajnosci-xamarin-forms/) oraz [tutaj]/2017/12/24/xamarin-forms-performance-warstwa-prezentacji/).

W ostatnich dniach znalazłem trochę czasu, żeby zaktualizować ten projekt. Zrobiłem 3 rzeczy, na których na chwilę obecną najbardziej mi zależało:

1. Zaktualizowałem prezentację wyników. Teraz **zamiast tabelek widzimy ładne wykresy**. Do każdego testu mamy jeden wykres, który pokazuje czas wykonywania testu dla każdej wersji oraz procentową różnicę. Kolejne kolumny są czerwone lub zielone w zależności od tego czy wynik się pogorszył, czy poprawił od poprzedniej wersji.
2. Dodałem zakładkę dla iOSa na stronie.
3. **Wykonałem testy w trybie Release na prawdziwych urządzeniach** w kilku wersjach Xamarin.Forms. Chciałem wykonać te testy dla jeszcze wcześniejszych wersji Xamarin.Forms, ale niestety miałem problemy ze zbudowaniem aplikacji. Dałem sobie więc na razie spokój. Obecnie ważniejsze są aktualne wersje, a testy dla starszych chciałem wykonać raczej z ciekawości.

Dla przypomnienia, obecny adres strony z wynikami to: <http://xamformsperf.azurewebsites.net>

#### Komentarz do wyników

Są to już wyniki z trybu Release, z urządzeń, więc można się pokusić o pewien komentarz. Na androidzie widać, że są pewne różnice w wynikach, raz na plus, raz na minus. Widać, że **wersja 2.4.0.91020 im się udała** i był tam pewien wzrost wydajności. W wersji 2.5.0.91635 jednak nie było już tak dobrze. Jeśli chodzi o iOS to wyniki są wszędzie niemal takie same. Jest to aż trochę ciężkie do uwierzenia. Myślę, że będę się musiał temu jeszcze przyjrzeć.Testy były wykonywane jako testy ui po 10 razy, a na koniec obliczana była średnia i ten wynik jest wyświetlany na stronie.

#### Przyszły rozwój

Jak na razie wybrałem tylko kilka wersji Xamarin.Forms. Chciałem już opublikować stronę w miarę używalnej wersji, tak żeby przynosiła pewną wartość. Od tego miejsca można ten projekt dalej rozwijać. To co chciałbym zrobić w następnej kolejności to:

- Napisanie większej ilości testów, np. dla różnych konfiguracji layoutów i podwidoków
- Większa automatyzacja. Obecnie muszę zmieniać wersję Xamarin.Forms ręcznie, podobnie budować aplikację, deployować ją i odpalać testy. Gdyby udało się choć trochę zautomatyzować ten proces, aktualizacja wyników dla nowych wersji i nowych testów byłaby o wiele wygodniejsza.

To wszystko na chwilę obecną. Jeśli macie jakieś uwagi, czy propozycje, chętnie ich wysłucham. Zapraszam do zostawiania komentarzy.