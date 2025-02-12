---
id: 130
title: 'Dlaczego moja strona w Xamarin.Forms nie jest niszczona?'
date: '2017-09-24T17:38:59+01:00'
author: 'Tomasz Wiśniewski'
layout: post
guid: 'https://lazyloading.pl/?p=130'
permalink: /2017/09/24/dlaczego-moja-strona-xamarin-forms-niszczona/
dsq_thread_id:
    - '6167364224'
categories:
    - Xamarin
---

Być może zdarzyło wam się śledzić, czy nie macie wycieków pamięci podczas tworzenia aplikacji. W C# teoretycznie jest całkiem łatwo, bo mamy garbage collector, który sam powinien zadbać o czyszczenie już niepotrzebnych obiektów. Niestety czasami tak się nie dzieje i najczęściej jest to nasza wina – **zwyczajnie zapomnimy usunąć referencje do naszego obiektu**. Np. rejestrujemy zdarzenie, ale już nigdzie tej rejestracji nie usuwamy. Wtedy obiekt nie zostanie zniszczony i będzie zalegał w pamięci.

Jednak jeśli chodzi o Xamarin.Forms to nie zawsze jest to takie oczywiste. Jest to nadal młody framework, a jak to z nowościami bywa – jest w nich sporo błędów. Nie jestem pewien czy sytuacja, którą teraz opiszę to błąd, czy może świadoma decyzja, ale jednak warto o niej wiedzieć.

#### Zachowanie NavigationPage

W Xamarin.Forms mamy wbudowaną nawigację. Aby jej używać musimy opakować naszą stronę w `NavigationPage`. Potem możemy używać takich metod jak `PushAsync`, czy `PopAsync`. Gdy przechodzimy do nowej strony za pomocą `PushAsync` to poprzednia pozostaje w `NagivationStack`. Gdy wracamy z nowej strony do poprzedniej za pomocą `PopAsync` to nowa strona jest usuwana z `NavigationStack`. Gdy nie trzymamy do niej żadnych dodatkowych referencji to oczekiwalibyśmy, że strona ta zostanie zniszczona. Tak się jednak nie stanie, ponieważ `NavigationPage` trzyma referencję do ostatniej strony, którą wyrzucił z `NavigationStack` za pomocą `PopAsync`. W takiej sytuacji strona ta nigdy nie zostanie zniszczona, jeśli nie wykonamy operacji nawigacji jeszcze raz – czy to `PushAsync`, czy `PopAsync`.

Aby to lepiej zrozumieć spójrzmy do kodu źródłowego Xamarin.Forms:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/6b734c39433b26e5b32cfbf76d33df40.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/6b734c39433b26e5b32cfbf76d33df40).</noscript></div>Metoda `PopAsync` działa na właściwości `CurrentNavigationTask`. W linijkach 17-18 widzimy, że jest tam zapisywany `Task<Page>`, czyli nasza usuwana strona. Właściwość ta zostanie nadpisana dopiero przy ponownym wywołaniu metody `PopAsync` lub przy wywołaniu `PushAsync`, gdzie w linijce 28 lub 36 również nadpisujemy tę właściwość.

Zachowanie to łatwiej zauważyć na Windowsie i iOS, które zazwyczaj usuwają dany obiekt w miarę szybko. Android często zwleka z tym zadaniem, bo niczego się nie boi i lubi zaglądać OutOfMemoryException w oczy 😉

#### Jak sobie z tym poradzić?

Może to nie mieć znaczenia. Jeśli nasze strony są małe i często nawigujemy, to pewnie nawet tego nie zauważymy. Ale jeśli aplikacja zbudowana jest tylko z kilku dużych stron, to może się to czasami okazać problematyczne. Nie jest to duży problem, ale warto mieć to na uwadze, używając wbudowanej nawigacji. Najczęściej nic nie trzeba z tym robić, ale w razie gdybyśmy chcieli temu zaradzić, to jednym z rozwiązań jest napisanie własnej nawigacji.