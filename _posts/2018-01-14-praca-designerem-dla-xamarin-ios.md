---
id: 444
title: 'Praca z designerem dla Xamarin.iOS'
date: '2018-01-14T19:11:22+01:00'
author: 'Tomasz Wiśniewski'
layout: post
guid: 'https://lazyloading.pl/?p=444'
permalink: /2018/01/14/praca-designerem-dla-xamarin-ios/
dsq_thread_id:
    - '6413301898'
categories:
    - Xamarin
---

W dzisiejszym wpisie chciałbym opowiedzieć o swoich doświadczeniach z pracy z designerem, gdy tworzymy aplikacje na iOS. Doświadczenia te są różne i prawdę mówiąc gorsze niż oczekiwałem, gdy dopiero zaczynałem .

#### Jak tworzymy widoki dla iOS?

Widoki na iOS możemy tworzyć na kilka sposobów. Podstawowy podział to tworzenie ich w kodzie lub w designerze. W tym drugiej opcji do dyspozycji mamy 2 sposoby:

- Xib – starszy sposób tworzenia widoków. Jeden plik to jeden widok. Ma pewne ograniczenia w stosunku do storyboardów, o których niżej.
- Storyboard – nowszy sposób tworzenia widoków. Jest bardzo podobny do xibów, ale ma kilka ulepszeń. **W jednym pliku storyboard możemy mieć kilka widoków**, dodajemy je razem z ich kontrolerami. Widoki możemy łączyć nawigacją (segue) bezpośrednio w designerze. **Dodatkową zaletą storyboardów jest obecność LayoutGuides**, do których można dodawać ograniczenia Autolayoutu (o Autolayoucie trochę niżej). Przykładowo mamy TopLayoutGuide. Jeśli dodamy do niego ograniczenie Autolayout zamiast po prostu do góry widoku, wtedy nasza kontrolka będzie wyświetlać się pod status barem (ten z godziną). Co prawda w iOS 11 zostało to zastąpione przez SafeAreaLayoutGuide ze względu na iPhone X.

#### Rozmiary i Auto Layout

Kiedyś widoki w iOS tworzyło się po prostu określając rozmiary kontrolek. Określaliśmy jej pozycję oraz wysokość i szerokość. Wydaje mi się, że był pewien mechanizm, który to ułatwiał, ale było to dawno temu i nie miałem okazji go używać. Później pojawił się Auto Layout, który działa podobnie jak RelativeLayout z Androida, czyli określamy jak kontrolki są położone względem siebie. **AutoLayout jest jednak bardziej elastyczny i ma więcej możliwości.** Tworzenie widoków z nim jest całkiem wygodne… dopóki nie zaczniemy tworzyć czegoś skomplikowanego. Wtedy często musimy się męczyć z ustaleniem odpowiednich zależności, czy poszukać innego rozwiązania problemu. Jednak o tym pewnie będę pisał innym razem.

#### Jak jest z Xamarinem?

Tu również możemy tworzyć widoki w kodzie i w designerze. Ale możemy ten podział jeszcze powiększyć. Co mamy dostępne

- Designer w Visual Studio
- Designer w Visual Studio for Mac
- xCode
- Kod

Omówmy każdy z tych sposobów po kolei.

#### Designer w Visual Studio na Windowsie

Jest on kopią designera z Visual Studio for Mac. I co ciekawe designer ten jest zupełnie inny niż tego w xCode. Zastanawiam się skąd ta inwencja twórcza w zespole Xamarina. Możemy używać tu Auto Layoutu, możemy robić właściwie wszystko to co na Macu. **Ale jeśli chcecie zachować zdrowie psychiczne, to proszę, nie używajcie tego narzędzia.** Część rzeczy tu po prostu nie działa. Klikasz coś i nic się nie dzieje. Na dłuższą metę nie da się tu poważnie pracować. Na krótszą metę też jest bardzo ciężko.

#### Designer w Visual Studio for Mac

Ten designer radzi już sobie lepiej. Widać, że Xamarin włożył w niego więcej pracy. **Da się tu pracować, jeśli byśmy musieli, ale nigdy nie powiedziałbym, że jest to wygodna praca.** Tutaj też zdarzają się błędy albo po prostu niektóre rzeczy nie są rozwiązane zbyt wygodnie. Na plus jednak trzeba zaliczyć własne podwidoki. Jeśli stworzymy sobie jakąś kontrolkę w kodzie, powiedzmy proste pole do wpisywania z nagłówkiem, to taką kontrolkę możemy dodać przez designer i będziemy widzieli jej podgląd. To samo powinno dać się zrobić na Windowsie, ale nigdy nie dotarłem tam aż tak daleko. Minusem z kolei jest to, że podgląd dla naszych customowych podwidoków stworzonych w oddzielnych plikach xib nie działa. Takie podwidoki będą się wyświetlać po prostu jako biały prostokąt.

#### xCode

Czyli natywne narzędzie do pracy z aplikacjami iOS przeznaczone dla Swifta i Objective-C. Tutejszy designer nazywa się Interface Builder. Możemy w nim pracować także z aplikacjami Xamarina. W Visual Studio for Mac wystarczy kliknąć prawym przyciskiem myszy na plik xib, czy storyboard i wybrać Otwórz w -&gt; Interface Builder. Ten designer działa poprawnie. Zdarzają się jakieś zgrzyty, czasami widok nie wyświetli się poprawnie, ale **jest to na pewno najbardziej rozwinięte i najbardziej dopracowane narzędzie z trzech wymienionych**. Tutaj przez większość czasu można po prostu pracować (komfort, który wydawałoby się, powinno zapewniać każde narzędzie). Minusem będzie to, że customowe podwidoki, stworzone przez nas w C# (a nie w xibie), będą się tu wyświetlać jako białe prostokąty. Dodatkowo, xCode nie widzi naszych customowych podwidoków i nie możemy ich dodawać do obecnie tworzonego widoku. Musimy taki podwidok dodać w designerze w Visual Studio for Mac.

#### Kod

Cóż, tu nie ma właściwie problemów. Nie ma żadnego podglądu i nie trzeba się niczym martwić. Dodatkowym plusem jest to, że przy niestandardowych rzeczach i tak czasami trzeba użyć kodu. Jeśli jesteśmy z nim obeznani, to pójdzie nam szybciej. Dla niektórych może przeszkadzać brak podglądu, ale w designerach i tak zazwyczaj nie jest on idealny.

#### Czcionki i napisy

W kodzie możemy bez problemu ustawić czcionkę, jej typ i rozmiar dla wszystkich tekstów. W designerach też. Jeśli jest to jakaś niestandardowa czcionka, to dodajemy ją do projektu oraz do Info.plist i będzie ona widoczna z poziomu designera. **Wyjątkiem będzie tu sytuacja, gdy chcemy stworzyć AttribuedText i zbindować do niego wartość.** AttributedText to tekst gdzie używamy różnych czcionek, rozmiarów, kolorów w jednym tekście. Jeśli jest on ustalany dynamicznie, to musimy stworzyć go kodem. Ogólnie rzecz biorąc, jeśli tekst jest dynamiczny, nawet jeśli chodzi tylko o tłumaczenie, to trzeba go ustawiać kodem.

#### Kolory

Kolejną sprawą, która jest ważna są kolory. Często ustawiamy kolory dla tła, czy napisów. Możemy to zrobić w każdym designerze. Ale co jeśli będziemy chcieli zmienić kolor wszystkich napisów z czarnego na ciemny szary? Musimy otwierać każdy widok i po kolei wszystko zmieniać? To strasznie dużo pracy! Z jednej strony da się zapisać kolory w palecie kolorów i używać ich w różnych miejscach. **Jednak najprawdopodobniej chcemy dzielić kolory w kodzie i designerze.** Czasami trzeba ich użyć tu, czasami tu, a nie będziemy przecież deklarować ich po kilka razy i potem zmieniać w paru miejscach.

Dlatego **nowszym rozwiązaniem jest dodawanie ich do Asset Catalog**. Jest to miejsce gdzie można też dodawać ikonki, czy obrazki w różnych rozmiarach (lub wektory). Stamtąd możemy ich używać i w designerze (a przynajmniej w xCode, reszty nie pamiętam, bo później już za bardzo ich nie używałem) i w kodzie. Jedyne ale jest takie, że… **działa to tylko od iOS 11**. Jeśli chcemy wspierać wcześniejsze wersje (a na razie pewnie chcemy) i mieć jedno miejsce do deklaracji kolorów, to pozostaje nam tylko robić to w kodzie, a następnie przypisywać wszystkie kolory do kontrolek w kodzie. Jest to więcej pracy, ale na chwilę obecną jest to chyba najlepsze wyjście, bo zyskujemy tu na przejrzystości i projekt jest łatwiejszy w utrzymaniu.

Dodatkowo, jeśli dodamy do Asset Catalog kolor (co robimy przez xCode), to nie możemy go przeglądać w Visual Studio for Mac. Po prostu przestaje się uruchamiać. Najwidoczniej funkcja ta nie jest jeszcze wspierana.

#### Co polecam?

Jeśli pracujemy na Windowsie a Maca mamy tylko jako build server, to właściwie nie mamy wyjścia – **pozostaje tylko kod**, bo designer do niczego się nie nadaje. Jednak jeśli pracujemy na Macu, to **również skłaniałbym się ku kodowi**. Wydaje mi się, że praca z kodem sprawia najmniej problemów i idzie sporo szybciej. Łatwiej jest ten kod współdzielić. Co prawda, kodu tworzącego widoki będzie sporo, ale w miarę łatwo i szybko można stworzyć sobie pomocnicze metody dla najważniejszych kontrolek, tak żeby skrócić powtarzające się instrukcje. **Jest to w miarę ważne i warto zadbać o to na początku, bo po pewnym czasie zauważmy, że klepiemy ciągle to samo.** W designerze nie mamy szans iść na skróty. Dodatkowo, czasami napotykamy wyjątki, które dotyczą tylko widoków z designera. Spędziłem nad nimi trochę czasu i wydaje mi się, że po prostu nie warto.