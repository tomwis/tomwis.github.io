---
id: 83
title: 'Jak rozwiązać błąd "Payload contains two or more files with the same destination path"?'
date: '2017-09-17T15:45:40+01:00'
author: 'Tomasz Wiśniewski'
layout: post
guid: 'https://lazyloading.pl/?p=83'
permalink: /2017/09/17/rozwiazac-blad-payload-contains-two-or-more-files-with-the-same-destination-path/
dsq_thread_id:
    - '6150077013'
categories:
    - Xamarin
---

#### Problem

Ostatnio pisząc aplikację UWP trafiłem na taki błąd:

> Error: Payload contains two or more files with the same destination path ‚Plugin.InAppBilling.pdb’. Source files:  
> C:\\Users\\user\\.nuget\\packages\\Plugin.InAppBilling\\1.2.2\\lib\\UAP10\\Plugin.InAppBilling.pdb  
> project\_path\\pcl\\bin\\Debug\\Plugin.InAppBilling.pdb  
> File: vs\_path\\MSBuild\\Microsoft\\VisualStudio\\v15.0\\AppxPackage\\Microsoft.AppXPackage.Targets  
> Line: 1740

Błąd ten pojawił się po raz kolejny. Pamiętam, że musiałem się z nim uporać już wcześniej, niestety zapomniałem jak… Dlatego teraz postanowiłem to sobie zapisać, żeby znowu nie zapomnieć i nie tracić czasu na szukanie rozwiązania. Zwłaszcza, że jego znalezienie nie było takie proste (chociaż samo rozwiązanie jest).

Była to aplikacja Xamarinowa pisana na Windowsa, Androida i iOS. Jak widzimy w ścieżce błędu dotyczy on pewnego pluginu, a konkretnie ma problem z dwoma identycznymi plikami pdb.

#### Rozwiązanie

Po pierwsze musimy zwrócić uwagę na ścieżki – w którym projekcie występuje problem. Nie jest to wcale projekt UWP, tylko PCL ze wspólnym kodem (plugin był dodany do obu projektów). Tam więc trzeba szukać rozwiązania problemu. W tym przypadku wystarczyło:

1. Rozwinąć referencje i wybrać tę której nazwa widnieje w błędzie (tutaj Plugin.InAppBilling).
2. Klikamy prawym przyciskiem myszy i przechodzimy do Properties.
3. Tutaj zmieniamy Copy Local na False.
4. Usuwamy kłopotliwy plik (Plugin.InAppBilling.pdb) z folderu bin lub usuwamy po prostu cały folder – tak jak wspomniane wyżej, chodzi o bin z projektu PCL, a nie UWP.
5. Kompilujemy i cieszymy się, że aplikacja buduje się poprawnie.