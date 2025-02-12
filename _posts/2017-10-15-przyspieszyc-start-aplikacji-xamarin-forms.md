---
id: 262
title: 'Jak przyspieszyć start aplikacji w Xamarin.Forms?'
date: '2017-10-15T12:32:44+01:00'
author: 'Tomasz Wiśniewski'
layout: post
guid: 'https://lazyloading.pl/?p=262'
permalink: /2017/10/15/przyspieszyc-start-aplikacji-xamarin-forms/
dsq_thread_id:
    - '6216747709'
categories:
    - Xamarin
---

Deweloperzy, którzy mieli okazję tworzyć w Xamarin.Forms, doskonale wiedzą, że nie jest on demonem prędkości. Jest widocznie wolniejszy od natywnych aplikacji, dlatego też używając go zazwyczaj dba się o każdy szczegół związany z wydajnością. Dzisiaj przejdziemy przez parę ogólnych rad, które mogą pozytywnie wpłynąć na czasu uruchamiania aplikacji. Wiele z tych porad może mieć małe znaczenie i zastosowanie tylko jednej z nich nie da widocznego rezultatu. Jednak optymalizacja to właśnie często szukanie tych milisekund w różnych miejscach i prawdziwe efekty widać dopiero, gdy znajdziemy kilka miejsc, gdzie dało się coś poprawić.

#### Kompilacja XAMLa

**XAML Compilation (XAMLC) to najprostszy sposób na poprawę nie tylko czasu startu aplikacji, ale ogólnej jej wydajności.** Gdy opcja ta jest włączona, widoki są kompilowane do języka pośredniego (IL). Dzięki temu nie trzeba już wczytywać i parsować widoków podczas działania aplikacji, a dodatkowo nie trzeba dołączać do paczki plików xaml. Opcję tę można włączyć globalnie dla całej biblioteki:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/f55b9121c54267524824f188aa9c42e9.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/f55b9121c54267524824f188aa9c42e9).</noscript></div>Lub dla pojedynczego pliku xaml:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/f8e8863aa614dd80b71fbee53db6b5c1.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/f8e8863aa614dd80b71fbee53db6b5c1).</noscript></div>Taka opcja może się przydać, jeśli przy opcji globalnej dostajemy jakiś błąd, którego nie możemy naprawić i dotyczy on tylko jednej lub kilku stron. Wtedy dla pozostałych stron możemy włączyć kompilację pojedynczo.

#### Optymalizacja mediów

Rozmiar obrazków i dźwięków powinien być jak najmniejszy. Im większe, tym dłużej trwa ich wczytanie. Ma to znaczenie zarówno podczas działania aplikacji, gdy używany danego zasobu, jak i przy uruchamianiu, ponieważ większy rozmiar paczki oznacza dłuższy czas ładowania.

Obrazki możemy zoptymalizować np. za pomocą [TinyPNG](https://tinypng.com). Jest to strona gdzie wrzucamy pliki png lub jpg i dostajemy zoptymalizowaną wersję, najczęściej bez widocznej utraty jakości. **Czasami można w ten sposób zmniejszyć rozmiar pliku nawet o 90%, więc na pewno warto skorzystać.**

Jeśli w aplikacji używamy dźwięków, to także warto nad nimi trochę posiedzieć i np. dać tylko jeden kanał (mono) lub zmniejszyć samplowanie.

#### Kompilacja Ahead of Time

Ten punkt dotyczy Androida, ponieważ na iOS nie mamy innego wyboru. Dla Androida natomiast możemy włączyć *eksperymentalną* funkcję, która kompiluje aplikacje do kodu natywnego. Powinno to zmniejszyć czas uruchomienia aplikacji, jednak rozmiar paczki zwiększy się. Opcja ta jest eksperymentalna, więc na razie należy podchodzić do niej ostrożnie. Jest ona dostępna we `właściwościach projektu -> Android Options -> Code Generation and Runtime`.

#### Leniwe inicjowanie (lazy loading)

Rzecz niby oczywista – im mniej danych wczytujemy, tym szybciej nam to pójdzie. To na co możemy zwrócić uwagę to:

- Zasoby w App.xaml.cs – są to zasoby globalne wczytywane na start aplikacji. Warto trzymać tam rzeczy, które są potrzeba od razu. Całą resztę powinniśmy stamtąd wyrzucić i trzymać np. na poszczególnych stronach.
- Rzeczy wczytywane z Internetu. Przede wszystkim, powinny być doczytywane w tle. Nie powinniśmy na nie czekać z wczytaniem widoku (można pokazać progress bara lub inny element pasujący do sytuacji).

#### Niech pierwsza strona będzie lekka

Im mniejszy widok do wczytania, tym zobaczymy go szybciej. Przykładowo, jeśli mamy na pierwszej stronie listę, moglibyśmy przesunąć ją na inną stronę, a na stronie głównej wyświetlić tylko krótkie podsumowanie. Mamy wtedy znacznie prostszy widok i o wiele mniej danych do wczytania. **Użytkownik będzie miał wrażenie, że aplikacja uruchomiła się szybciej.** I rzeczywiście będzie mógł szybciej zacząć jej używać. Zwróćmy uwagę na takie rzeczy jak:

- Używajmy jak najmniej kontrolek
- Jeśli możemy coś uprościć stosując obrazek to prawdopodobnie dobry pomysł
- Lepiej użyć mniej bindowań i unikać converterów w xamlu. Możemy zamiast tego stworzyć odpowiednie właściwości w modelu lub viewmodelu i od razu przypisać do nich wartości w takiej formie, w jakiej chcemy je wyświetlić. Unikamy dzięki temu tworzenia instancji converterów i później odwołań do nich podczas tworzenia widoku.
- Jeśli mamy tekst z różnym formatowaniem w jednej linii, to zamiast kilku `Labeli` lepiej użyć jednego i przypisać `FormattedString` do pola `FormattedText`. [Przykład](https://forums.xamarin.com/discussion/comment/84972/#Comment_84972).

#### Nie dla SVG

Svg jest fajne i ostatnio coraz bardziej modne w aplikacjach mobilnych. Plusem niewątpliwie jest to, że mamy tylko jedną ikonkę, która będzie miała mniejszy rozmiar od kilku plików png. Do tego nie będziemy musieli spędzać czasu na skalowaniu png. Jednak gdy patrzymy na wydajność, to zrezygnowanie z svg może być dobrym pomysłem. Problem z tym formatem jest taki, że **ikonki wczytują się odrobinę dłużej**. Jest to oczywiście bardzo małe opóźnienie, jednak może być zauważalne. Kolejną rzeczą, o której warto wspomnieć jest biblioteka [FFImageLoading.Svg](https://www.nuget.org/packages/Xamarin.FFImageLoading.Svg.Forms). To prawdopodobniej z niej korzystalibyśmy w Xamarin.Forms do wyświetlania svg. Opiera się na **SkiaSharp, która to zajmuje 9MB**. Raz, że zwiększa to rozmiar paczki, a dwa, że wczytanie takiej biblioteki też chwilę potrwa.

#### Użyj natywnych widoków lub custom rendererów

Każda natywna kontrolka będzie szybsza niż kontrolka z Xamarin.Forms. Dlatego jeśli bardzo zależy nam na przyspieszeniu startu aplikacji i nie mamy już pomysłów, to możemy przepisać część lub wszystkie kontrolki na natywne za pomocą [Custom Rendererów](https://developer.xamarin.com/guides/xamarin-forms/application-fundamentals/custom-renderer/), [Native Views](https://developer.xamarin.com/guides/xamarin-forms/platform-features/native-views/) lub użyć [osadzania widoków](https://developer.xamarin.com/guides/xamarin-forms/user-interface/layouts/add-platform-controls/). Custom Renderery mają ten plus, że będzie można ich później łatwo użyć w innym miejscu, jednak ich napisanie zajmie więcej czasu. **Widoki natywne natomiast są szybsze do zrobienia, bo możemy ich używać bezpośrednio w xamlu w PCL.** Minus jest jednak taki, że trzeba dla tej strony wyłączyć kompilację xamla. Mimo wszystko, natywne widoki i tak będą szybsze niż xaml z Xamarin.Forms z kompilacją. Osadzanie widoków natywnych w Xamarin.Forms także jest ciekawą alternatywą. W miarę łatwo osiągnąć to w Shared Project z użyciem dyrektyw preprocesora. W PCL musielibyśmy tworzyć tu pewną abstrakcję i implementować ją w projektach platformowych. W naszej sytuacji, gdy chodzi nam o start aplikacji i pierwszą stronę, lepszym wyborem mogą być Native Views.

#### Zrzut ekranu pierwszej strony

To bardziej trik niż optymalizacja, ale chodzi nam głównie o to, żeby doświadczenia użytkownika były jak najlepsze. Ma mieć on wrażenie, że aplikacja uruchamia się szybko. Jeśli zobaczy pierwszą stronę szybciej, to tak właśnie pomyśli.

> „O, ale szybko.” – pomyślał użytkownik uruchamiając twoją aplikację.

Ale to nie do końca prawda, bo w tym momencie widzi tylko obrazek. Jednak zanim zdąży namyślić się, co chce nacisnąć i skierować palec w to miejsce, to jest duża szansa, że strona zdąży się już wczytać. Wtedy trzeba tylko podmienić ją z obrazkiem i gotowe. Jest to metoda, która jest [polecana przez Apple](https://developer.apple.com/ios/human-interface-guidelines/icons-and-images/launch-screen/) do stosowania we wszystkich aplikacjach na iOS. Oni idą jednak jeszcze dalej i polecają dawać taki obrazek jako splash screen (launch image) – gdzie twórcy aplikacji zazwyczaj umieszczają logo aplikacji.

#### Podsumowanie

Niektóre metody dadzą nam zauważalne rezultaty od ręki. Na pierwszym miejscu na pewno jest kompilacja xamla. Custom Renderery i Native Views dadzą nam jeszcze więcej, ale wymagają też o wiele więcej pracy, dlatego stosuje się je rzadziej. Inne, mniejsze optymalizacje również mogą się przydać, ale ich efekt nie będzie już tak mocno widoczny. Tak jak wspomniałem na początku, **często chodzi o szukanie milisekund w różnych miejscach, tak żeby sumarycznie było widać poprawę.** Czy wy macie jakieś swoje sposoby na poprawę czasu startu aplikacji? Dajcie znać w komentarzach.