---
id: 252
title: 'Jak naprawić wyjątek "Can not perform this action after onSaveInstanceState"?'
date: '2017-10-08T13:01:51+01:00'
author: 'Tomasz Wiśniewski'
layout: post
guid: 'https://lazyloading.pl/?p=252'
permalink: /2017/10/08/jak-rozwiazac-blad-can-not-perform-this-action-after-onsaveinstancestate/
dsq_thread_id:
    - '6199374313'
categories:
    - Xamarin
---

#### Problem

Na androidzie czasami możemy natrafić na taki błąd:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/8fa1e219d0d905761f7fd01de9c49642.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/8fa1e219d0d905761f7fd01de9c49642).</noscript></div>`Can not perform this action after onSaveInstanceState` właściwie dobrze opisuje dlaczego błąd wystąpił. Nie możemy wykonać akcji, którą zamierzaliśmy wykonać po wywołaniu metody `onSaveInstanceState`. **Błąd ten może się pojawić, gdy dodajemy do widoku fragment, a akurat użytkownik pomyślał, że jednak wyjdzie z aplikacji** (przyciskiem home) lub aplikacja została przeniesiona do tła, bo np. ktoś do nas zadzwonił. Jeśli metoda `Commit` z `FragmentTransaction` zostanie wywołana po `onSaveInstanceState`, to dostaniemy powyższy błąd. Problem dokładniej jest opisany w [tym poście](http://www.androiddesignpatterns.com/2013/08/fragment-transaction-commit-state-loss.html). Stworzyłem przykładowy projekt, który prezentuje ten problem. Możecie go ściągnąć tutaj:

<https://github.com/tomwis/OnSaveInstanceStateExceptionSample>

Uruchomcie aplikację w konfiguracji `Debug`. Jeśli zaznaczycie switcha i wyjdziecie przyciskiem home, to pojawi się wyjątek – ponieważ dodajemy fragment w `OnStop`, które wywołuje się już po `onSaveInstanceState`.

#### Rozwiązanie

Rozwiązanie wydaje się proste – nie commitować fragmentów, gdy aplikacja jest zatrzymana. Jednak zazwyczaj nie robimy tego specjalnie, tylko dzieje się to asynchronicznie i wychodzi tak przez przypadek. **Rozwiązaniem może być tutaj przechwytywanie takich fragmentów, wstawianie ich do kolejki i tworzenie ich po powrocie do aplikacji.** Robimy to w następujący sposób:

1. W miejscu gdzie commitujemy fragment, sprawdzamy przed commitem czy została już wywołana metoda `onSaveInstanceState`. Jeśli nie, to kontynuujemy, a jeśli tak, to zapisujemy fragment do kolejki.
2. W aktywności dodajemy metodę `override void OnPostResume()`. W niej możemy przejść przez kolejkę zapisanych fragmentów i stworzyć je.

Taka metoda jest również zaimplementowana w przykładzie na githubie zalinkowanym wyżej. Musimy tylko uruchomić aplikację w konfiguracji `DebugCorrect` – wtedy będziemy używać aktywności `MainActivityWithQueue`. Najważniejsze części:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/285404c8a932e6517a88ab73072d1498.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/285404c8a932e6517a88ab73072d1498).</noscript></div>