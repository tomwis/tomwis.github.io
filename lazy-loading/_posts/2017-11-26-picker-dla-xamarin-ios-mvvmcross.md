---
id: 412
title: 'Picker dla Xamarin.iOS z MvvmCross'
date: '2017-11-26T18:12:51+01:00'
author: 'Tomasz Wiśniewski'
layout: post
guid: 'https://lazyloading.pl/?p=412'
permalink: /2017/11/26/picker-dla-xamarin-ios-mvvmcross/
dsq_thread_id:
    - '6311547088'
categories:
    - Xamarin
---

> Jeśli jesteś tu tylko po link do nugeta, proszę bardzo: <https://www.nuget.org/packages/MvxPlugins.Picker.iOS>

O dziwno, iOS nie posiada wbudowanej kontrolki typu dropdown/combobox/picker. Mamy dostępny UIPickerView, jednak zazwyczaj trzeba go trochę obudować, bo jest to tak jakby już otwarta kontrolka – a tego zazwyczaj nie potrzebujemy. W większości przypadków chcemy widzieć tylko wybrany element, a resztę dopiero po kliknięciu.

Z tego co zdołałem się zorientować, standardowym sposobem na uzyskanie w miarę klasycznego pickera jest:

- Użycie UITextField, jako pola dla zaznaczonego elementu.
- Umieszczenie UIPickerView w miejscu klawiatury.
- Pojawianie się/ukrywanie UIPickerView jest obsługiwane przez UITextField tak jak normalna klawiatura.

Aby wygodnie się z tego korzystało, trzeba tam dodać jeszcze kilka elementów, jak np.:

- Zablokowanie zmiany tekstu, czy ukrycie pozycji kursora w UITextField.
- Dodanie przycisku do potwierdzenia zmiany.
- Często też podłączamy pod picker obiekt, a nie zwykły `string`, czy `int`. W takiej sytuacji musimy znać pole, którego wartość ma być wyświetlana.

Zrobienie tego wszystkiego niby nie jest trudne, ale jednak trzeba poświęcić trochę czasu na napisanie kodu, który to obsłuży. Zwłaszcza jak robi się to pierwszy raz. Wydawało mi się to na tyle pracochłonne, że postanowiłem uprościć ten proces. Zbudowałem taką kontrolkę i umieściłem ją na nugecie:

<https://www.nuget.org/packages/MvxPlugins.Picker.iOS>

Kontrolka ta współpracuje tylko z MvvmCross.

#### Jak używać?

Bardzo prosto:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/f90223de06c1140c509fccb4272ca028.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/f90223de06c1140c509fccb4272ca028).</noscript></div>Musimy tylko stworzyć obiekt `Picker` i zbindować jego właściwości `ItemsSource` oraz `SelectedItem`. `DisplayPropertyName` nie jest wymagane, ale przydaje się, jeśli bindujemy obiekt i chcemy wyświetlić jego konkretne pole – wtedy podajemy tutaj, które pole ma się wyświetlać.

To w zasadzie tyle 🙂 Możecie też przejrzeć kod źródłowy i przykład na githubie:

<https://github.com/tomwis/MvxPlugins.Picker.iOS>