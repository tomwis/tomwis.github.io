---
id: 384
title: 'Alternatywa dla XAMLa - AmmyUI i json'
date: '2017-12-03T12:46:41+01:00'
author: 'Tomasz Wiśniewski'
layout: post
guid: 'https://lazyloading.pl/?p=384'
permalink: /2017/12/03/alternatywa-dla-xamla-ammyui-json/
dsq_thread_id:
    - '6326212545'
categories:
    - Xamarin
---

Jakiś czas temu zastanawiałem się dlaczego tak właściwie Microsoft postawił na styl xml jako język opisu warstwy UI dla aplikacji. Czy coś prostszego nie byłoby lepsze? Obecnie ciągle używa się jsona zamiast xmla i wszyscy chwalą sobie to rozwiązanie. Dlaczego więc nie spróbować by tego samego z opisem UI? Podobne pytanie można by postawić także odnośnie axml z Androida.

Okazuje się, że nie tylko ja nad tym myślałem (niemożliwe ;)). Pewien czas po tych rozważaniach trafiłem zupełnie przypadkiem na AmmyUI. Jest to nowy sposób opisu warstwy UI dla aplikacji używających XAML, a więc WPF, UWP i Xamarin.Forms (oraz NoesisGUI, ale niestety pierwszy raz o tym słyszę). Język ten bazuje na jsonie i zastępuje UI w XAMLu.

W ramach wypróbowania AmmyUI stworzyłem prostą aplikację w Xamarin.Forms (właściwie bardziej jednostronne UI):

![](/wp-content/uploads/2017/10/ammyui_example1_1-576x1024.png)

Link do niej znajduje się na końcu posta.

Zobaczmy teraz jak wygląda AmmyUI.

#### Podstawowy wygląd

Porównajmy sobie jak prezentuje się AmmyUI w stosunku do xamla:

XAML:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/fd438a966c79d08ae4aec908d809065d.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/fd438a966c79d08ae4aec908d809065d).</noscript></div>AmmyUI:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/f1292baec7de4e642e1aae958014c01b.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/f1292baec7de4e642e1aae958014c01b).</noscript></div>Github niestety nie ma kolorowania składni dla ammy, ale w Visual Studio nie ma już z tym większego problemu:

![](/wp-content/uploads/2017/10/ammyui1.png)

Mógłbym się jedynie przyczepić, że kolory nie są najlepiej dobrane pod ciemny motyw, ale to raczej drobnostka, którą łatwo poprawić. Jak widzimy, taka składnia sprawia, że plik jest krótszy niż gdybyśmy pisali go w xamlu. Wiele innych rzeczy również jest uproszczona, np. aby zdefiniować wiersze w kontrolce `Grid` wystarczy napisać:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/d4e9f2b47600f995e6993f6543c33d59.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/d4e9f2b47600f995e6993f6543c33d59).</noscript></div>W gridzie definiujemy zaledwie `#TwoRows` razem z wysokością każdego wiersza, a następne w kontrolce piszemy `#Cell` i podajemy wiersz oraz kolumnę. Przy okazji `#TwoRows` przejdziemy do kolejnej funkcjonalności – mixins.

#### Mixins

Z tym `#TwoRows` to trochę oszustwo – bowiem nie moglibyśmy zrobić tego dla dowolnej liczby wierszy. Jest to mixin, czyli coś w rodzaju funkcji, która może przyjmować parametry i zwraca właściwości obiektów. Jest on już zdefiniowany przez twórcę, więc jest to dla nas taki skrót, ale możemy bez problemu tworzyć własne mixiny. Zobaczmy jak zdefiniowany jest mixin `TwoRows`:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/b242b71d02bf3281ea113702fa696deb.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/b242b71d02bf3281ea113702fa696deb).</noscript></div>Mamy tutaj nazwy znane z xamla – kolekcję `RowDefinitions` i poszczególne definicje wierszy zawarte w nawiasie klamrowym, tak jak kolekcje w jsonie. Później mixinów używamy z hashem # na początku.

#### Bindowania, resource’y i konwertery

Bindowania mają trochę skróconą formę ze słówkiem `bind`. Statycznych resource’ów możemy używać ze słówkiem `resource`, a więc możemy np. trzymać resource’y w `App.xaml` w xamlu i używać ich na stronie z Ammy. Składnia `resource dyn` pomaga nam z dynamicznymi resource’ami. Ale możemy je też definiować w ammy. Przykładowo tak to wygląda z kolorami:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/dcc11fb5d69d5c0784c8cc0b30a3b472.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/dcc11fb5d69d5c0784c8cc0b30a3b472).</noscript></div>Trochę krócej niż z użyciem xamla, prawda?

Ciekawą sprawą są tutaj konwertery, bowiem możemy pisać je bezpośrednio na stronie z ammy, używając C# lub odwołać się do jakiejś funkcji. Przykładowo:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/ebd9822f166e4759437400e9aa7bdfb8.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/ebd9822f166e4759437400e9aa7bdfb8).</noscript></div>Używamy słówka `convert` przy bindowaniu, a dalej mamy coś w rodzaju wyrażenia lambda. Wydaje mi się to całkiem pomocne, bo czasami zdarza się pisać naprawdę proste konwertery, np. do odwrócenia boola. To trochę ułatwia zadanie.

Zwróćmy też uwagę na importowanie namespace’a za pomocą słowa `using` – dokładnie jak w C# (tylko bez średnika na końcu). Jest to sporo prostsze niż to co musimy robić w xamlu.

#### Czytelność

Obawiałem się trochę jak będzie z czytelnością przy większych plikach i bardziej zagnieżdżonych strukturach. Nie jest jednak źle. Pomaga tutaj zdecydowanie dodawanie do końcowej klamry informacji o tym, do której kontrolki dana klamra należy:

[![](/wp-content/uploads/2017/10/ammyui2_light-1024x657.png)](/wp-content/uploads/2017/10/ammyui2_light.png)

Miłym akcentem jest też pokazywanie koloru jako podkreślenia wartości hexowej jak np. w `BackgroundColor` – tutaj niestety niewiele widać, bo jest to prawie biały 🙂

#### Live coding

To jest jeden z największych plusów AmmyUI – widzimy podgląd UI na żywo w emulatorze. Nie mamy tu okienka do podglądu UI w Visual Studio, tylko widzimy wszystko w działającej aplikacji. Gdy zmieniamy tło, zapisujemy plik i tło zmienia się w aplikacji. W międzyczasie możemy sobie chodzić po aplikacji, klikać, itd. i podgląd na żywo cały czas działa. Można to robić z podłączonym debuggerem lub nie.

A jeśli zrobimy jakiś błąd w kodzie ui, to w aplikacji pojawi się błąd. W moim przypadku był dosyć czytelny, mam nadzieję, że zawsze tak jest:

![](/wp-content/uploads/2017/10/ammy_error1.png)

Cudowna sprawa. Budowanie i deploy aplikacji Xamarina czasami trwa długo i taka funkcjonalność może znacznie przyczynić się do szybkości kodowania UI.

#### Konwerter z Xamla

Jeśli mamy już napisaną aplikację, to istnieje konwerter, który przekształci nasz projekt na ammy. Możemy też przekonwertować pojedyncze pliki. Ciekawy jestem jak dobrze radzi sobie ten konwerter. Jeśli to sprawdzę, to postaram się napisać posta o problemach, które można napotkać.

#### Problemy z Xamarin.Forms

Dla Xamarin.Forms AmmyUI jest niestety na razie w becie. Szybko to odczułem, bowiem nie dałem rady uruchomić aplikacji na symulatorze iOS. Android i UWP działają w porządku. Mam nadzieję, że z czasem zostanie to poprawione, bo bez tego nie za bardzo da się używać Ammy w Formsach.

#### Podsumowanie

Nie pokazywałem tu wszystkich możliwości AmmyUI. Twórcy twierdzą, że da się tu zrobić wszystko to co w xamlu. Jest to produkt całkowicie darmowy i open-source.

Pierwsze wrażenie jakie wywarło na mnie AmmyUI jest bardzo dobre. Wygląda na to, że tworzenie UI jest w nim łatwiejsze niż w Xamlu, zwłaszcza że mamy całkiem dobre podpowiedzi – powiedziałbym, że lepsze niż w xamlu dla Xamarin.Forms. Jednak, spodziewam się, że w bardziej rozbudowanych projektach mogą pojawiać się problemy. W końcu jest to dosyć młody projekt. Mam nadzieję, że będzie dalej rozwijany i zyska trochę na popularności.

Link do mojej testowej aplikacji: <https://github.com/tomwis/AmmyUIExample>

Link do oficjalnej strony: <http://www.ammyui.com>

Zachęcam do obejrzenia filmików na oficjalnej stronie, które dobrze pokazują możliwości AmmyUI.