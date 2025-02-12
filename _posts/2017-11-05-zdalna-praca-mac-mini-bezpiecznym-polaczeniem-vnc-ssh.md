---
id: 282
title: 'Zdalna praca z Mac Mini z bezpiecznym połączeniem VNC przez SSH'
date: '2017-11-05T13:27:01+01:00'
author: 'Tomasz Wiśniewski'
layout: post
guid: 'https://lazyloading.pl/?p=282'
permalink: /2017/11/05/zdalna-praca-mac-mini-bezpiecznym-polaczeniem-vnc-ssh/
dsq_thread_id:
    - '6264138210'
categories:
    - Xamarin
---

Jako programista Xamarina, który tworzy aplikacje na iOS muszę pracować z MacOS. Jest mi potrzebny do budowania aplikacji na iOS oraz do odpalania symulatora iPhone’a i iPada. Jednak moim głównym narzędziem jest sprzęt z Windowsem i na co dzień przesiaduję w Visual Studio. Jeśli mamy wersję Enterprise to możemy cieszyć się symulatorem bezpośrednio w Windowsie. Jeśli nie, to symulator odpali się na Macu i tam musimy go obsługiwać. Dodatkowo, co jakiś czas trzeba zajrzeć na Maca, żeby np. po aktualizacji Xcode zaakceptować regulamin albo żeby coś skonfigurować. Potrzebujemy więc wygodnego dostępu do Maca.

Jeśli możemy mieć Maca pod ręką, to super – jest to prawdopodobnie najwygodniejsze rozwiązanie (być może oprócz pracy bezpośrednio na Macu – [polecam wpis Damiana Antonowicza na ten temat](https://blog.damianantonowicz.pl/2017/08/16/dlaczego-przesiadlem-sie-na-mac-os-jako-programista-xamarin/)). Ale co jeśli chcemy pracować z Mac’iem zdalnie? Bo np. jest daleko od nas lub chcemy go schować w szafie, bo nie mamy już miejsca na biurku na kolejny sprzęt.

#### Zdalne logowanie

Po pierwsze musimy skonfigurować zdalne logowanie (remote login), zgodnie z [instrukcjami w dokumentacji Xamarina](https://developer.xamarin.com/guides/ios/getting_started/installation/windows/connecting-to-mac/). Jest to potrzebne, aby Visual Studio mógł połączyć się z Mac’iem do budowania aplikacji.

#### Udostępnianie ekranu

Jeśli chcemy widzieć pulpit Maca, to najprostszym sposobem jest skonfigurowanie udostępniania ekranu (screen sharing) – [instrukcje tutaj](https://support.apple.com/kb/PH25554?locale=pl_PL). Opiera się to na VNC. Jest to trochę inna technologia niż zdalny pulpit (RDP/remote desktop) w Windowsie. W skrócie, RDP przesyła przez sieć tylko informacje o obiektach (np. pozycja, rozmiar, kolor), a VNC przesyła całe zrzuty ekranu. Dlatego też **wydajność VNC jest raczej słaba** i jeśli mielibyśmy pracować tak cały dzień – nie dałoby rady. Do prostych zadań jednak w zupełności wystarczy.

#### Klient VNC

Gdy mamy skonfigurowane udostępnianie ekranu na Macu, wtedy możemy zainstalować klienta VNC na Windowsie. Jest dostępnych kilka, m. in. [TightVNC](http://www.tightvnc.com), [RealVNC](https://www.realvnc.com/en/), [UltraVNC](http://www.uvnc.com). Osobiście używam RealVNC – najmilszy dla oka, a opcje podobne jak w innych (zresztą i tak za dużo pewnie nie będzie się ich używać). Do tego dostępne są aplikacje na Androida i iOSa, a więc możemy sterować Mac’iem nawet z telefonu. Mogłoby się wydawać, że jest to bardzo niewygodne, ale spróbowałem i nie było tak źle – pewnie sporo w tym zasługi dobrze zaplanowanego interfejsu/gestów.

#### Bezpieczeństwo

**Połączenie przez VNC nie jest szyfrowane.** Przesyłamy więc przez sieć wszystkie dane w otwartej formie, a są tutaj też dane poufne, takie jak chociażby hasło. W sieci lokalnej to jeszcze pół biedy, ale jeśli Mac znajduje kompletnie gdzie indziej i przesyłamy dane przez sieć publiczną, to warto zadbać o bezpieczeństwo. RealVNC mógłby być tu najłatwiejszym rozwiązaniem, ponieważ po założeniu konta, nawet w darmowym planie, mamy szyfrowanie. Prawdopodobnie jest to dobre rozwiązanie, ja jednak byłem nastawiony do niego raczej niechętnie. Postanowiłem spróbować czegoś innego – ssh. Protokół ten szyfruje wszystkie dane. **Możemy stworzyć tunel przez ssh do naszego Maca, a następnie połączyć się przez VNC do tunelu.**

Aby to zrobić potrzebujemy klienta ssh do Windowsa. Najpopularniejszym jest [PuTTY](http://www.putty.org), ale mamy też inne, jak [mRemoteNG](https://mremoteng.org), czy [MobaXterm](http://mobaxterm.mobatek.net) – dwa ostatnie są bardziej rozbudowane. Ja pokażę jak stworzyć tunel na podstawie Putty.

1. Otwieramy Putty i w pierwszym oknie (`Session`) w miejsce `xxx.xxx.xxx.xxx` wpisujemy IP naszego Maca. Port zostawiamy na 22 i typ na SSH.![]/wp-content/uploads/2017/09/putty1.png)
2. Przechodzimy w drzewku po lewej do `Connection -> SSH -> Tunnels`.![]/wp-content/uploads/2017/09/putty2.png)
3. Jako `Destination` wpisujemy IP Maca z portem dla VNC, domyślnie jest to 5900.
4. W `Source port` wpisujemy port lokalny pod jakim widoczne będzie połączenie.
5. Resztę zostawiamy jak jest, chyba że widzimy, że w naszym przypadku coś powinno być inaczej, jak np. `Remote` zamiast `Local`.
6. Możemy wrócić do zakładki `Session` i zapisać sesję pod dowolną nazwą (wpisujemy nazwę pod `Saved Sessions` i klikamy `Save`).
7. Uruchamiamy połączenie przyciskiem `Open`.

Wyskoczy nam nowe okno, w którym będziemy musieli się zalogować do sesji ssh użytkownikiem i hasłem z naszego Maca. Wtedy w kliencie VNC zamiast podawać host/ip i port Maca, wpisujemy `localhost:5901` (lub inny port, jeśli podaliśmy inny w `Source port`). Takie połączenie będzie przechodzić przez ssh i dzięki temu będzie w całości szyfrowane.

#### Automatyzacja

Rozwiązanie takie możemy również zautomatyzować skryptem. Możemy do tego użyć np. aplikacji konsolowej dołączanej do `Putty`, czyli `plink`. Jest to Putty działające z linii poleceń. Przyjmuje różne argumenty, jak np. nazwę zapisanej sesji (`load`), login (`l`), czy hasło (`pw`).

`plink -load mac_session_name -l mac_login -pw mac_password`

W taki sposób możemy szybciej ustanowić bezpieczne połączenie.

Klientów VNC również można używać z linii poleceń. Przykładowo:

UltraVNC: `vncviewer localhost:5901 -password my_password`

TightVNC: `vncviewer -optionsfile="my_config_file.vnc"`

RealVNC:

- `vncviewer localhost:5901`
- `vncviewer -config my_config_file.vnc`

Z tego co zauważyłem TightVNC i RealVNC nie pozwalają na podanie hasła jako parametru (jest to hasło do połączenia VNC, a nie do profilu użytkownika na Macu), można je jednak zapisać w konfiguracji sesji – w pliku z rozszerzeniem vnc. Niestety nie znalazłem opcji w RealVNC do stworzenia takiego pliku, ale przeglądając sieć wydaje mi się, że taka opcja była dostępna w poprzednich wersjach. TightVNC natomiast taką opcję posiada. Możemy zapisać sesję w pliku .vnc (z hasłem lub bez) po ustanowieniu połączenia. Taki plik jest również rozpoznawany przez RealVNC.

#### Podsumowanie

Zdalna praca z Mac’iem jest dosyć prosta – przynajmniej na pierwszy rzut oka. Nie jest to bardzo wygodne rozwiązanie, ale jeśli potrzebujemy go tylko do budowania i od czasu do czasu jakieś konfiguracji – powinno w pełni wystarczyć. Dodanie dodatkowej warstwy bezpieczeństwa również nie wymaga dużo pracy, a do tego łatwo to zautomatyzować.

Jeśli używacie innego sposobu na pracę z Mac’iem, czy macie podobne, ale wygodniejsze rozwiązanie – chętnie o tym przeczytam w komentarzach.