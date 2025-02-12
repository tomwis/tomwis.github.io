---
id: 298
title: 'MvvmCross - rozbudowany framework MVVM dla Xamarin i Xamarin.Forms'
date: '2017-10-01T12:42:48+01:00'
author: 'Tomasz Wiśniewski'
layout: post
guid: 'https://lazyloading.pl/?p=298'
permalink: /2017/10/01/mvvmcross-rozbudowany-framework-mvvm-dla-xamarin-xamarin-forms/
dsq_thread_id:
    - '6183321666'
categories:
    - Xamarin
---

Niedawno pisałem o [FreshMvvm](/2017/09/03/freshmvvm-lekki-framework-mvvm-dla-xamarin-forms/), który był bardzo lekki i prosty w użyciu. Dzisiaj pora na inny framework, z zupełnie odmiennym podejściem. MvvmCross jest bardziej kompleksowym rozwiązaniem, posiadającym większą ilość różnych funkcji. **Możemy go stosować zarówno w projektach z natywnym Xamarinem, jak i w Xamarin.Forms.**

Przy okazji opisywania FreshMvvm stworzyłem prostą aplikację do budżetu. Teraz przepisałem ją na MvvmCross i na jej podstawie przedstawię poniżej ważniejsze funkcje tego frameworka. Jest to aplikacja Xamarin.Forms, a więc będę się skupiać na aspektach MvvmCross dotyczących Formsów. Wspierane przez moją aplikację platformy to Android i iOS. UWP niestety nie dałem rady uruchomić.

#### StarterPack

MvvmCross posiada pakiet nuget o nazwie [MvvmCross.StarterPack](https://www.nuget.org/packages/MvvmCross.StarterPack/). **Jest to bardzo pomocna paczka, która dodaje nam do projektów wszystkie pliki potrzebne do początkowej konfiguracji** i odpalenia aplikacji przez ten framework. Bez tej paczki byłoby to raczej trudne, ponieważ nigdzie nie widziałem dobrego poradnika jak to zrobić. Ostatnio [pojawił się post](https://medium.com/@martijn00/using-mvvmcross-with-xamarin-forms-part-1-eaee5815bb8c) o rozpoczynaniu pracy z Xamarin.Forms przy użyciu MvvmCross, więc jest to pewna pomoc. Ale jeśli chodzi o natywnego Xamarina, to niestety nic podobnego nie znalazłem. Dlatego bez StarterPacka rozpoczęcie pracy z tym frameworkiem byłoby ciężkie.

Jest też dostępnych kilka szablonów do Visual Studio dla MvvmCross – [tutaj](https://www.mvvmcross.com/documentation/getting-started/getting-started.html#mvvmcross-templates). Nie są to jednak szablony oficjalne.

#### Podstawowa konfiguracja

Minimalna konfiguracja frameworka odbywa się w dwóch miejscach. We wspólnym kodzie, w projekcie PCL dodajemy klasę `CoreApp` dziedziczącą po `MvxApplication`. Tam możemy np. załadować serwisy, czy zadeklarować początkowy ViewModel:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/62cbfce127794ea680235faa86916294.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/62cbfce127794ea680235faa86916294).</noscript></div>Druga część konfiguracji odbywa się w projekcie danej platformy. Tam tworzymy klasę `Setup`, która np. na androidzie dziedziczy po `MvxFormsAndroidSetup` (a na innych platformach po podobnych klasach). Tam odbywa się np. rejestracja implementacji interfejsów, których zachowanie jest zależne od platformy, tworzenie wyżej wspomnianej klasy `CoreApp`, ustalanie zachowania IoC (więcej o tym niżej), rejestracja konwerterów i parę innych rzeczy. Mamy tu prawie całą konfigurację dla danej platformy (oprócz pluginów) – [więcej w dokumentacji](https://www.mvvmcross.com/documentation/fundamentals/Customizing-using-App-and-Setup).

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/09a8c0c20f4fc763c3af5e1463cc6629.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/09a8c0c20f4fc763c3af5e1463cc6629).</noscript></div>#### Bindings

Wiązanie widoku i ViewModelu jest w zasadzie automatycznie, trzeba jednak spełnić 1 warunek – nazewnictwo. **Widok musi kończyć się na View, a view model na ViewModel.** Tak więc, np. `DashboardView` i `DashboardViewModel` zostaną ze sobą powiązane. Nazewnictwo wiązań może zostać przez nas nadpisane w klasie `Setup`. Dodatkowo do widoku możemy dodać typ view modeli, np. `MvxContentPage<DashboardViewModel>`. Wtedy z poziomu widoku będziemy mieli dostępną właściwość `ViewModel` o podanym typie.

Możemy używać wbudowanych w Xamarin.Forms wiązań, czyli np. `Text="{Binding MyProperty}"`. **Są jednak również dostępne specjalne wiązania z MvvmCross.**

1. Pierwszy typ wiązania to `mvx:Bi.nd="Text MyProperty; TextColor MyColorProperty"`. Zalety: 
    1. Mamy wiele wiązań w jednym miejscu. Składnia jest tu trochę krótsza.
    2. MvvmCross ma kilka wbudowanych konwerterów, np. dla Visibility, czy koloru (sam ich nie używałem – niby wierzę na słowo, ale jak zaraz się okaże, różnie to bywa).
    3. Mamy tu łączenie warunków za pomocą `And`, `Or`. Czasami może to zaoszczędzić trochę kodu.
2. Drugi typ wiązania to `mvx:La.ng="Text TranslatedStringId"`. Powinno to umożliwiać łatwe tłumaczenie tekstów z plików resx lub json. Powinno, ale niestety tego nie robi, ponieważ nie działa. Zmarnowałem na to sporo więcej czasu niż powinienem. Zadałem nawet na ten temat pytania na Stackoverflow i repozytorium MvvmCross – niestety odpowiedziała mi cisza. W przykładzie z oficjalnego repozytorium ta funkcja także nie działa, a więc nie jest to mój błąd w używaniu. Trzeba po prostu poczekać aż to naprawią. Byłoby miło, bo jest to całkiem przydatna rzecz.

Wiązania te wyglądają tak samo jak przy natywnym Xamarinie. Jest to więc plus jeśli kiedyś planowalibyśmy przepisanie aplikacji lub rozpoczęcie nowej już w natywnym Xamarinie. Sam tych wiązań nie używałem. Jeden powód to taki, że `mvx:La.ng` nie działało, a drugi taki, że niektóre wiązania z `mvx:Bi.nd` również miały pewne problemy – być może rozwiązywalne, ale po przygodach z `mvx:La.ng` już nie miałem za bardzo czasu i ochoty tego sprawdzać.

#### Inversion of Control i Dependency Injection

Jak można zobaczyć we fragmencie kodu powyżej, w linijce 10, dostępny jest kontener IoC. Rejestracja do niego może również odbywać się tak jak w pierwszym fragmencie w linijkach 5-8 (dla klas zaimplementowanych w bibliotece, z której to wywołujemy).

Zależności możemy wstrzykiwać do ViewModeli poprzez konstruktor lub poprzez właściwość. Aby użyć drugiego sposobu, należy dodać do właściwość atrybut `[MvxInject]` lub w klasie `Setup` nadpisać metodę `CreateIocOptions` i ustawić `PropertyInjectorOptions` na `MvxPropertyInjectorOptions.All` (jak w przykładzie wyżej – linijka 28).

#### Nawigacja i komunikacja między ViewModelami

Nawigacja odbywa się tutaj pomiędzy ViewModelami, a nie widokami. Mamy dostępną metodę `Navigate` w `IMvxNavigationService`, która nam w tym pomaga. Jako że widoki są powiązane z ViewModelami, to nawigując do nowego view modelu, framework wyszukuje powiązaną z nim stronę i ją wczytuje. Możliwe jest napisanie własnych prezenterów, czyli sekcji odpowiedzialnych za wczytywanie widoku, jeśli chcemy obsłużyć jakieś niestandardowe sytuacje.

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/b5c7eff29773a9ab38d77de47b0b4661.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/b5c7eff29773a9ab38d77de47b0b4661).</noscript></div>Jak widzimy wyżej, mamy kilka wariantów nawigacji. Możemy po prostu przejść do nowej strony, ale **możemy też przekazać jakiś parametr**. Będzie on dostępny w nowym view modelu w metodzie Initialize:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/b3aee83798f80a0eb08de83be3337d7c.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/b3aee83798f80a0eb08de83be3337d7c).</noscript></div>**Trzeci wariant metody pozwala nam natomiast na poczekanie aż nowy view model zwróci wartość.** Aby to zrobić, w nowym view modelu (u nas `MonthEditViewModel`) należy wywołać `await Close(MonthItem);`. Metoda ta zamyka obecny view model (a więc także i połączony z nim widok) i zwraca `MonthItem` do poprzedniego view modelu. Jest to bardzo wygodne rozwiązanie.

#### Zdarzenia w ViewModelach

W view modelach mamy dostępne metody powiązane ze zdarzeniami widoków – `ViewAppearing` oraz `ViewDisappearing`. Również bardzo przydatna rzecz.

#### Pluginy

MvvmCross posiada wiele różnych pluginów. Przykładowo – messenger do wzorca publish/subscribe, Accelerometer, DownloadCache, PhoneCall, Share i wiele innych. [Tutaj](https://www.mvvmcross.com/documentation/plugins/3rd-party-plugins?scroll=898) link do pluginów 3rd party z dokumentacji. W menu po lewej znajduje się też lista oficjalnych pluginów (niestety każdy na oddzielnej stronie, więc nie mogę zalinkować).

W Xamarin.Forms nie wszystkie pluginy się przydadzą. Mamy tu już wbudowanego messengera, czy `Device.OpenUri`, które poradzi sobie z przekierowaniem do telefonu.

**Każdy plugin trzeba załadować poprzez stworzenie odpowiedniej klasy** (nigdzie jej nie wywołujemy – framework ją sobie znajdzie i załaduje plugin).

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/2970594247dad358c1e836bcef409e1c.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/2970594247dad358c1e836bcef409e1c).</noscript></div>Na szczęście dzieje się to automatycznie podczas instalowania pakietu danego pluginu z nugeta.

#### Społeczność i dokumentacja

Społeczność na pewno jest czymś co trzeba wspomnieć przy MvvmCross. Jest to duży framework, rozwijany przez grupę osób. Można więc zakładać, że jego wsparcie szybko się nie zakończy. **Jak na razie ciągle jest aktywnie rozwijany.**

Dokumentacja jest obszerna. Co więcej, twórca MvvmCross tworzył kilka lat temu wiele postów na temat tego frameworka, nagrywał również sporo pomocnych filmików tłumaczących jak zaimplementować wiele rzeczy. Wszystkie te filmiki dostępne są [tutaj](http://mvvmcross.blogspot.com).

Społeczność również jest rozbudowana, na [StackOverflow](https://stackoverflow.com/questions/tagged/mvvmcross) znajdziemy masę pytań związanych z MvvmCross – na które w dużej mierze odpowiadali twórcy.

Jest tu jednak pewien problem. Wcześniej wspominałem o trudnych początkach, jeśli nie mielibyśmy StarterPacka, czy o nie działającym mvx:La.ng i braku pomocy. Jak widać – nie zawsze jest różowo i nie zawsze będziemy w stanie dostać pomoc, mimo rozbudowanej społeczności. Ale ten problem dotyczy raczej każdej technologii 🙂 Drugim zgrzytem jest dokumentacja – jest niepełna i nieaktualna. Framework jest ciągle rozwijany, a dokumentacja nie do końca. Na wiele pytań znajdziemy jednak odpowiedzi, jeśli nie na oficjalnej stronie, to na StackOverflow.

#### Podsumowanie

MvvmCross to rozbudowany framework i opisałem tu jedynie część jego możliwości. To dobre narzędzie do natywnego Xamarina, gdzie dodaje sporo wartości i ciężko byłoby się bez niego obejść. **A czy użyłbym go w Xamarin.Forms? Raczej nie.** Tutaj według mnie lepiej sprawdzi się coś mniejszego jak [FreshMvvm](/2017/09/03/freshmvvm-lekki-framework-mvvm-dla-xamarin-forms/), czy MvvmLight Toolkit. Będę one wystarczające. Są prostsze, a więc także próg wejścia będzie niższy – co może być ważne dla programistów dopiero zaczynających pracę z Xamarin.Forms. Framework ten stawia na prostotę tworzenia aplikacji, a więc łatwiejsze w użyciu biblioteki świetnie się tu nadadzą.

Link do przykładowej aplikacji na githubie: <https://github.com/tomwis/SimpleBudgetMvvmCross>

Oraz do oficjalnej strony MvvmCross: <https://www.mvvmcross.com>