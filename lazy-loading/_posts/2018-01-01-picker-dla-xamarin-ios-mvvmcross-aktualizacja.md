---
id: 433
title: 'Picker dla Xamarin.iOS z MvvmCross - aktualizacja'
date: '2018-01-01T16:31:40+01:00'
author: 'Tomasz Wiśniewski'
layout: post
guid: 'https://lazyloading.pl/?p=433'
permalink: /2018/01/01/picker-dla-xamarin-ios-mvvmcross-aktualizacja/
dsq_thread_id:
    - '6384720119'
categories:
    - Xamarin
---

Zaktualizowałem swój plugin do MvvmCross z pickerem, o którym [niedawno pisałem]/2017/11/26/picker-dla-xamarin-ios-mvvmcross/).

Co się zmieniło?

- Najważniejsza rzecz to dodanie obsługi obrazków. Picker może teraz nie tylko wyświetlać tekst, ale też ikonki. Dla obrazków można ustawić rozmiar oraz wyrównanie w poziomie:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/9f1d6f612aca75af68bf2d0b017f89be.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/9f1d6f612aca75af68bf2d0b017f89be).</noscript></div>- Zmieniłem przycisk „Done” w toolbarze na przycisk systemowy, który jest automatycznie tłumaczony. Przy okazji usunąłem też właściwość do zmiany tekstu na przycisku „Done”, bo teraz nie jest już potrzebny.
- Dodałem metodę do dodawania przycisków do toolbara, po lewej stronie od „Done”. Metoda nazywa się `AddButtonToToolbar`
- Przetestowałem również plugin z najnowszą wersją MvvmCross (5.6.2). Wcześniej zrobiłem to tylko ze starą wersją 4.4.0. Powinien działać od wersji 4.4.0 wzwyż. Jeśli tak nie jest, dajcie znać.

To tyle na dzisiaj. Jeszcze tylko przypomnę link do nugeta:

<https://www.nuget.org/packages/MvxPlugins.Picker.iOS>