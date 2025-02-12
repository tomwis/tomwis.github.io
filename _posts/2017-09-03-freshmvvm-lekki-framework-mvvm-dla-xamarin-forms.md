---
id: 137
title: 'FreshMvvm - lekki framework do MVVM dla Xamarin.Forms'
date: '2017-09-03T14:57:36+01:00'
author: 'Tomasz Wiśniewski'
layout: post
guid: 'https://lazyloading.pl/?p=137'
permalink: /2017/09/03/freshmvvm-lekki-framework-mvvm-dla-xamarin-forms/
dsq_thread_id:
    - '6116549985'
categories:
    - Xamarin
---

#### FreshMvvm z Xamarin.Forms

Czym jest MVVM pewnie wielu już wie. Jest to standardowy wybór przy pracy z aplikacjami UWP czy Xamarina. Tutaj nie będę się o tym rozpisywał, bowiem MVVM zostało już przedstawione w wielu poradnikach w sieci. W tekście tym chciałbym omówić jeden z frameworków do MVVM – FreshMvvm.

**FreshMvvm został stworzony specjalnie pod Xamarin.Forms.** Dlaczego? Jak sam autor mówi, tworzył projekt w oparciu o inny framework (MvvmCross), ale było tam sporo rzeczy, których nie potrzebował. Postanowił więc stworzyć coś mniejszego i lżejszego. Po zapoznaniu się z jego pracą, wydaje mi się, że udało mu się zbudować naprawdę fajne narzędzie. Jest ono też bardzo dobre dla początkujących, ponieważ jest proste do nauki. **Paczka jest oczywiście dostępna na nugecie pod nazwą FreshMvvm.**

#### Części składowe

FreshMvvm posiada kilka podstawowych funkcji:

- Automatyczne wiązanie widoku z ViewModelem
- Nawigacja pomiędzy ViewModelami
- Metody do inicjalizacji i przekazywania informacji między ViewModelami
- Podstawowe zdarzenia ze stron dostępne w ViewModelach (pokazywanie i znikanie)
- Dialogi dostępne z ViewModeli
- Wbudowany kontener IOC
- Wstrzykiwanie przez konstruktor do ViewModeli

Opiszę teraz każdą z nich trochę dokładniej. Stworzyłem przykładową aplikację do budżetu domowego i będę się do niej odwoływał przedstawiając powyższe funkcje.

![](/wp-content/uploads/2017/08/budget_s4-576x1024.png)

#### Binding strony i ViewModelu

Binding, czy też **wiązanie jest tutaj automatyczne**. Warunkiem jest jednak odpowiednie nazewnictwo. W Xamarin.Forms mamy Page (strona) oraz View (widok). Page to pełna strona, a View może pełnić rolę widoku częściowego, jak UserControl w WPF, czy w Windows Phone. Dlatego też autor na początku wymagał nazewnictwa DashboardPage i DashboardPageModel. Później na szczęście zmienił zdanie i teraz możemy nazywać ViewModel tak, jak większość jest do tego przyzwyczajona – DashboardViewModel. **Framework rozpozna te 2 nazwy i ustawi BindingContext dla DashboardPage na DashboardViewModel**. Początkowa inicjalizacja jest bardzo prosta:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/9035522303ded540014b54ce4668b21c.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/9035522303ded540014b54ce4668b21c).</noscript></div>Pobieramy naszą stronę z FreshPageModelResolver, przypisujemy do standardowej nawigacji, a następne do MainPage.

#### Nawigacja i przekazywanie informacji pomiędzy ViewModelami

FreshMvvm ma wbudowanych kilka podstawowych modeli nawigacji: Basic Navigation, Master Detail i Tabbed Navigation. Można też zaimplementować własną nawigację za pomocą interfejsu IFreshNavigationService. Jak nietrudno się domyślić, Basic Navigation to normalna nawigacja między stronami, Master Detail obsługuje layout MasterDetailPage – [tutaj](https://developer.xamarin.com/guides/xamarin-forms/application-fundamentals/navigation/master-detail-page/) więcej informacji o nim z dokumentacji Xamarina, a z Tabbed Navigation możemy przechodzić pomiędzy zakładkami – tak jak [tutaj](https://developer.xamarin.com/guides/xamarin-forms/application-fundamentals/navigation/tabbed-page/). W swojej aplikacji używałem Basic Navigation – jej inicjalizację widać we fragmencie powyżej. Później możemy przejść z jednego ViewModelu do drugiego:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/b6514a8a0af584c32485837bbc387edf.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/b6514a8a0af584c32485837bbc387edf).</noscript></div>CoreMethods to właściwość dostępna w ViewModelu dziedziczącym po FreshBasePageModel. **Możemy wywołać PushPageModel z argumentem – jest to łatwy sposób na przekazanie informacji do nowego ViewModelu.** Tam wywoła się metoda Init, gdzie możemy odczytać przekazaną wartość:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/321fab635d738572d6d4472b873cec89.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/321fab635d738572d6d4472b873cec89).</noscript></div>Jak widzimy jest to typ object, więc możemy przekazać dowolny obiekt i go rzutować. W ten sposób w aplikacji mogłem przekazywać miesiąc, który chciałem edytować. W przypadku braku tej informacji po prostu tworzony był nowy.

![](/wp-content/uploads/2017/08/budget_s1-576x1024.png)

Działa to też w drugą stronę. **Gdy wychodzimy ze strony, do metody PopPageModel możemy przekazać dane** (lub nie, jeśli nie chcemy):

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/3bf3352331177dd294cd5ad7dddf9443.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/3bf3352331177dd294cd5ad7dddf9443).</noscript></div>Wtedy strona się zamyka i wracamy do poprzedniej, a w jej ViewModelu wywoła się metoda ReverseInit:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/36e903550f3d8c0099f0b3b520427254.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/36e903550f3d8c0099f0b3b520427254).</noscript></div>Tak więc w wygodny sposób możemy przekazywać dane pomiędzy ViewModelami.

#### Zdarzenia w ViewModelach

W ViewModelach mamy dostępne eventy takie jak na stronach (Page), czyli ViewIsAppearing oraz ViewIsDisappearing (to w sumie ciekawe, że autor nazwał te metody w taki sposób, a nie **Page**Is…). Wywołują się one wtedy, kiedy tego byśmy oczekiwali (co nietrudno zgadnąć po nazwach) – czyli jak strona się pojawia lub znika.

#### Dialogi dostępne z ViewModeli

W ViewModelach często chcemy pokazać jakiś dialog dla użytkownika. Popularnym rozwiązaniem w Xamarin.Forms jest użycie pluginu [Acr.UserDialogs](https://github.com/aritchie/userdialogs) – są to bardzo rozbudowane dialogi. Jednak jeśli potrzebujemy tylko czegoś prostego, to nie ma sensu instalować dodatkowej biblioteki. W FreshMvvm możemy znowu odwołać się do CoreMethods:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/7f5343574af0a1d4ecb3dce798f150d2.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/7f5343574af0a1d4ecb3dce798f150d2).</noscript></div>W aplikacji wykorzystałem to do wyświetlenia informacji dla użytkownika, gdy chciał stworzyć nowy miesiąc, ale stworzył go już wcześniej.

![](/wp-content/uploads/2017/08/budget_s3-576x1024.png)

W CoreMethods mamy też dostępne DisplayActionSheet, czyli dialog z listą opcji do wyboru.

#### Wbudowany kontener IOC i wstrzykiwanie przez konstruktor

FreshMvvm posiada także wbudowany kontener IOC (Inversion of Control – odwrócenie sterowania) – jeśli nie znasz jeszcze tego terminu, to także odsyłam do licznych artykułów na innych stronach. Jest to coś co właściwie zawsze się przydaje. Pod spodem FreshMvvm używa tutaj TinyIOC. Używanie jest bardzo proste:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/74964a4d8e81fc9e549a33c48b607ba2.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/74964a4d8e81fc9e549a33c48b607ba2).</noscript></div>**Odwołujemy się do FreshIOC. Container i możemy tak zarejestrować nasz interfejs, czy klasę.** W powyższym fragmencie rejestrujemy IFileService oraz IDbService. Ten drugi ma podaną konkretną implementację, która posiada konstruktor z parametrem typu IFileService:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/52b8939d4e94617f1b39891a831b98c6.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/52b8939d4e94617f1b39891a831b98c6).</noscript></div>**Jeśli IFileService jest już zarejestrowany w naszym kontenerze, to jego wartość zostanie przekazana do konstruktora** (wstrzyknięcie przez konstruktor). Podobnie działa to dla ViewModeli. Tam przekazuję IDbService i jego wartość jest wstrzykiwana.

#### Bonus – Fody

Chciałbym powiedzieć o jeszcze jednej ciekawej rzeczy. Pakiet z nugeta PropertyChanged.Fody dodaje nam bardzo fajną funkcjonalność do naszego projektu. Nie jest to zależność FreshMvvm, ale autor sam używa tego w poradnikach i dokumentacji, więc ja też spróbowałem.

**Pakiet ten automatycznie implementuje nam INotifyPropertyChanged.** Co to oznacza? Przykładowo, porównajmy jak wygląda właściwość w Mvvm Light, a jak z Fody:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/691886a8c67536d9f08917cd384b364e.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/691886a8c67536d9f08917cd384b364e).</noscript></div>Bez Fody musimy sami zaimplementować INotifyPropertyChanged lub użyć frameworku, który już to robi (jak np. Mvvm Light z RaisePropertyChanged), a następnie wywołać dla każdej właściwości przy zmianie metodę informującą o zmianie wartości. **Fody robi to automatycznie, dlatego nasze właściwości są o wiele krótsze.** Czyni to nasze ViewModele dużo prostszymi i łatwiejszymi w utrzymaniu. Oczywiście, jest to na pewno mniej wydajne, ale czy jest to spadek wydajności, którym należałoby się przejmować? Żeby tego się dowiedzieć, trzeba by przeprowadzić testy. Jednak w małych projektach na pewno nie będzie to nikomu przeszkadzać.

#### Podsumowanie

FreshMvvm okazał się być całkiem przyjemnym frameworkiem. Ma sporo ułatwień, których i tak prawie zawsze potrzebujemy, a bez niego musielibyśmy je pisać samemu. Warto wspomnieć, że Xamrin.Forms posiada już podstawowy MVVM. Jeśli by się uprzeć to nie trzeba stosować żadnego frameworka. Ale jak wspomniałem – wtedy część rzeczy, które występują w praktycznie każdym projekcie, trzeba by pisać od zera. Framework taki jak FreshMvvm nam tego oszczędza.

Kod przykładowej aplikacji dostępny jest na githubie: <https://github.com/tomwis/simplebudgetfreshmvvm>

Odsyłam też na githuba FreshMvvm, gdzie dostępna jest obszerniejsza dokumentacja, a także kilka instruktażowych filmików: <https://github.com/rid00z/FreshMvvm>