---
id: 418
title: 'Sprawdzanie wydajności w Xamarin.Forms'
date: '2017-12-17T12:08:15+01:00'
author: 'Tomasz Wiśniewski'
layout: post
guid: 'https://lazyloading.pl/?p=418'
permalink: /2017/12/17/sprawdzanie-wydajnosci-xamarin-forms/
dsq_thread_id:
    - '6354308807'
categories:
    - Xamarin
---

Kiedyś zdarzyło mi się przeprowadzić testy wydajnościowe Xamarina. Porównywałem Xamarin.Forms do Xamarin.Android i do aplikacji w Javie. Testy te robiłem wtedy tylko dla Androida. Wyniki możecie obejrzeć [tutaj](https://amellsoftware.com/2016/07/28/android-performance-java-vs-xamarin-vs-xamarin-forms/) oraz [tutaj](https://amellsoftware.com/2016/08/04/android-performance-java-vs-xamarin-vs-xamarin-forms-part-2/). Ogólnie rzecz biorąc, nie były one dobre dla Xamarin.Forms.

#### Xamarin.Forms – czy następna wersja będzie wydajniejsza?

Jakiś czas temu, przeglądając forum Xamarina, natknąłem się na pomysł stworzenia aplikacji, która mierzyłaby wydajność Xamarin.Forms pomiędzy kolejnymi wersjami. Chodzi głównie o to, żeby zobaczyć, czy są wykonywane jakieś optymalizacje, czy może wydajność spada i w jakich obszarach się to dzieje. Był to pomysł podsunięty zespołowi Xamarina, ale znając życie, niewiele się w tym temacie u nich dzieje 🙂 Dlatego ostatnio zacząłem sam pracować nad taką aplikacją (nazwa robocza XFP) i posiadam jakiś pierwszy prototyp. Obecnie aplikacja mierzy 7 rzeczy:

- Czas wykonywania metody `Forms.Init()`
- Czas wykonywania metody `LoadApplication()`
- Czas do pojawienia się pierwszego widoku (początek – przed metodą `Forms.Init()`, koniec – w `OnAppearing()` pierwszej strony)
- Czas tworzenia i renderowania kontrolki `BoxView`
- Czas tworzenia i renderowania kontrolki `Button`
- Czas tworzenia i renderowania kontrolki `Label`
- Czas tworzenia i renderowania kontrolki `Image`

Na razie tylko tyle, chociaż jeśli będzie to dobrze wyglądać, to planuję dodać sporo więcej testów sprawdzających także inne kontrolki, layouty, różne ich kombinacje, itp.

Obecnie, aplikację można pobrać z Githuba: <https://github.com/tomwis/XamFormsPerf>

Działa dla Androida, iOSa i UWP. Gdy uruchomimy aplikację testy zaczynają się automatycznie i po momencie dostaniemy podsumowanie. Możemy również wykonać kilka pętli testów i dostaniemy uśredniony wynik.

#### Automatyzacja

Jednak takie testy najlepiej jeszcze bardziej zautomatyzować – za pomocą testów UI. Co robi taki test:

- Otwiera aplikację – tu następnie testy w aplikacji wykonują się automatycznie
- Pobiera podsumowanie testów i zapisuje je w pliku .csv
- Powtarza powyższe w ilu pętlach chcemy

Dodałem wykonywanie testów w pętlach, ponieważ dobrze jest wykonać takie testy przynajmniej kilka razy i uśrednić wynik. Taki uśredniony wynik również otrzymamy na koniec w oddzielnym pliku csv.

Na razie stworzyłem takie testy tylko dla Androida, ponieważ Xamarin.UITest nie wspiera UWP, a testów dla iOS nie da się pisać na Windowsie 🙂 Resztę postaram się dopisać w miarę możliwości nieco później.

Testy najlepiej wykonywać albo zawsze na tym samym urządzeniu albo od nowa dla wszystkich interesujących nas wersji Xamarin.Forms. Druga metoda być może jest nawet lepsza, bo wtedy wiemy, że testujemy na urządzeniu w podobnym stanie, z tą samą wersję systemu, itp. Wyniki powinny być w ten sposób bardziej miarodajne.

#### Podsumowanie

Zobaczymy, czy pomysł się sprawdzi i czy dostanę z tego jakieś użyteczne dane. Najlepiej byłoby prezentować te dane gdzieś online, z ładnymi wykresami, tabelkami i porównaniami między wersjami. Zobaczymy.