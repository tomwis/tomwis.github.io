---
id: 319
title: 'MVVM Light Toolkit - prosty framework MVVM dla Xamarin i Xamarin.Forms'
date: '2017-10-29T15:44:53+01:00'
author: 'Tomasz Wiśniewski'
layout: post
guid: 'https://lazyloading.pl/?p=319'
permalink: /2017/10/29/mvvm-light-toolkit-prosty-framework-mvvm-dla-xamarin-xamarin-forms/
dsq_thread_id:
    - '6263122377'
categories:
    - Xamarin
---

Wcześniej pisałem już o dwóch frameworkach do MVVM – [FreshMvvm](/2017/09/03/freshmvvm-lekki-framework-mvvm-dla-xamarin-forms/) i [MvvmCross](/2017/10/01/mvvmcross-rozbudowany-framework-mvvm-dla-xamarin-xamarin-forms/). Dzisiaj przyjrzę się możliwościom Mvvm Light Toolkit. Jest to jeden ze starszych i pewnie jeden z bardziej znanych frameworków do MVVM. Przed Xamarinem był częstym wyborem (przynajmniej moim, ale pewnie też wielu innych osób) w WPF i Windows Phone’ie.

Do zaprezentowania Mvvm Light stworzyłem, podobnie jak przy poprzednich frameworkach, prostą aplikację do budżetu i na jej podstawie będę opisywał funkcjonalności (link do kodu na końcu).

#### Funkcjonalności

Mvvm Light Toolkit to dość lekka biblioteka, podobnie jak FreshMvvm. Posiada tylko podstawowe funkcjonalności:

- Zaimplementowane INotifyPropertyChanged w view modelach
- Wbudowana implementacja ICommand – RelayCommand
- Tryb designu
- Wbudowany Messenger do wzorca publish/subscribe
- Kontener IoC
- Nawigacja i dialogi

#### Inicjalizacja

Paczka Mvvm Light jest dostępna na nugecie. Mamy do wyboru 2 wersje:

- [MvvmLight](https://www.nuget.org/packages/MvvmLight/) – dodaje do naszego projektu biblioteki oraz przykładowe pliki `MainViewModel.cs` i `ViewModelLocator.cs`, a do tego modyfikuje `App.xaml.cs`
- [MvvmLightLibs](https://www.nuget.org/packages/MvvmLightLibs/) – dodaje do projektu tylko biblioteki i nic automatycznie nie zmienia (jak paczka powyżej)

Ja zawsze korzystam z drugiej paczki – ustawienie projektu jest proste i do tego w Formsach robię to trochę inaczej niż skrypt z nugeta.

Standardowo, w Mvvm Light używamy klasy nazwanej `ViewModelLocator`, w której rejestrujemy wszystkie view modele i usługi.

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/c6d4dc0cc20bbfa9a59b16d15cba9409.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/c6d4dc0cc20bbfa9a59b16d15cba9409).</noscript></div>Później, żeby mieć dostęp do `ViewModelLocatora`, tworzymy jego instancję w klasie `App.xaml.cs`:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/498cca52b21a593f0e128c723ac0a567.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/498cca52b21a593f0e128c723ac0a567).</noscript></div>Dodajemy ją również do `Resources`, żeby mieć wygodny dostęp z xamla. To tyle, jeśli chodzi o wstępną konfigurację.

#### ViewModele

W view modelach mamy zaimplementowany `INotifyPropertyChanged` i możemy informować widoki o zmianach metodą `RaisePropertyChanged`. W zasadzie poza tym jest niewiele więcej. Nie ma tu dostępnych, tak jak w innych frameworkach, metod wywoływanych przy pokazywaniu/ukrywaniu widoku, czy nawigacji między view modelami z przekazywaniem parametrów. Mamy tu tylko niezbędne minimum.

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/06366c2a74d316e37659790f329ff29a.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/06366c2a74d316e37659790f329ff29a).</noscript></div>#### RelayCommand

Do komend mamy wbudowaną implementację interfejsu ICommand – RelayCommand:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/b741d516f07099a87ef203aee8cd4728.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/b741d516f07099a87ef203aee8cd4728).</noscript></div>Możemy używać zwykłych komend lub z parametrem. Parametr dostępny jest tylko jeden, jeśli musimy przekazać coś więcej, to trzeba to opakować w jakiś obiekt.

#### Tryb designu

**Design Mode** to całkiem ciekawa funkcja, ale nie znajdziemy jej w Xamarin.Forms. Działa tylko dla WPF/WP. W view modelach mamy dostępne właściwości `IsInDesignMode` i `IsInDesignModeStatic`. Za ich pomocą można sprawdzić, czy jesteśmy w trybie designu (czyli praca w Visual Studio, podgląd xamla w designerze), czy w działającej aplikacji. Jest to pomocne, gdy chcemy wyświetlić testowe dane w designerze xamla.

#### Messenger

Mamy dostęp do wbudowanego messengera. Z poziomu view modelu możemy użyć właściwości `MessengerInstance`, a w innych miejscach `GalaSoft.MvvmLight.Messaging.Messenger.Default`.

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/8a1460c5f5acffb170fc09f25f52f935.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/8a1460c5f5acffb170fc09f25f52f935).</noscript></div>Jak widzimy, wysyłamy wiadomość metodą `Send`. Możemy wysłać wiadomość do wszystkich subskrybentów, do konkretnej klasy lub do subskrybentów z danym tokenem. Aby się zarejestrować używamy metody `Register` z podaniem typu wiadomości jakie chcemy odbierać i opcjonalnie tokenem.

#### IoC i Dependency Injection

Mvvm Light używa prostego kontenera IoC nazywanego `SimpleIoc`. W pierwszym fragmencie kodu, przy okazji inicjalizacji, widzieliśmy już jak go używać. Możemy rejestrować klasy za pomocą `SimpleIoc.Default.Register` – podając samą klasę lub także odpowiadający jej interfejs. Przy okazji `DbService` używam tam `DependencyService`, który jest częścią Xamarin.Forms. Jego metoda `Get` pozwala nam pobrać implementację interfejsu dla danej platformy. Tutaj potrzebowałem instancji `IFileService` jako parametru dla `DbService`.

W tym samym fragmencie kodu, w linijkach 13 i 14, widzimy jak odwoływać się do klas zarejestrowanych w kontenerze.

Framework zapewnia nam wstrzykiwanie przez konstruktor do view modeli obiektów, które zarejestrowaliśmy w kontenerze IoC.

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/04c1dfe7e1a1760ac8c7ff8e0617ef23.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/04c1dfe7e1a1760ac8c7ff8e0617ef23).</noscript></div>#### Bindowanie

Bindowanie nie jest tutaj automatyczne. Musimy sami przypisać view model do właściwości BindingContext danej strony. Robimy to w konstruktorze strony:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/14add90069d64100f6ca59d40b2c94d1.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/14add90069d64100f6ca59d40b2c94d1).</noscript></div>Właśnie dlatego stworzyliśmy właściwość ViewModelLocatora w klasie App – dzięki temu będzie nam wygodnie się do niej odwoływać.

#### Nawigacja i dialogi

Mvvm Light ma także wbudowaną nawigację, jednak jest to nawigacja przeznaczona dla aplikacji w natywnym Xamarinie, a nie Xamarin.Forms. Tutaj musiałem zbudować swój serwis, który wykorzystywał nawigację z Formsów, tak abym mógł używać jej w view modelach.

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/8937736669a2be796afb8b4e2e9bf1f5.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/8937736669a2be796afb8b4e2e9bf1f5).</noscript></div>Framework ma wbudowane podstawowe dialogi z metodami `ShowMessage` i `ShowError`, gdzie możemy ustawić tytuł, wiadomość, tekst przycisku (lub przycisków) i akcję po zamknięciu.

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/a260395e99d1ba1e6df341e458e37934.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/a260395e99d1ba1e6df341e458e37934).</noscript></div>Implementacja tych dialogów jest zależna od platform, więc musimy zainicjalizować je oddzielnie w każdej platformie. Niestety, wygląda na to, że dialog te nie są w pełni kompatybilne z Xamarin.Forms. Działają one bez problemu na UWP i iOS, ale z Androidem jest już problem. Lepiej pewnie będzie użyć domyślnych dialogów z Formsów lub np. [ACR User Dialogs](https://www.nuget.org/packages/Acr.UserDialogs/), które są bardzo rozbudowane.

#### Podsumowanie

Mvvm Light Toolkit to mały i prosty w obsłudze framework do MVVM. Dobrze nadaje się do małych aplikacji lub gdy dopiero zaczynamy przygodę z mvvm. Jako, że jest popularny, znajdziemy w sieci dużo materiałów na jego temat, a także dużo odpowiedzi na [StackOverflow](https://stackoverflow.com/questions/tagged/mvvm-light). Powinien być odpowiedni dla kogoś, kto chce się nauczyć na czym polega mvvm. Posiada niezbędne elementy i niewiele poza tym, a więc nie będzie niepotrzebnie mieszał w głowie i obciążał aplikacji.

Link do przykładowej aplikacji: <https://github.com/tomwis/SimpleBudgetMvvmLight>

Link do oficjalnej strony: <http://www.mvvmlight.net>