---
id: 478
title: 'Szybkie wykresy z Chart.js'
date: '2018-02-11T16:44:38+01:00'
author: 'Tomasz Wiśniewski'
layout: post
guid: 'https://lazyloading.pl/?p=478'
permalink: /2018/02/11/szybkie-wykresy-chart-js/
dsq_thread_id:
    - '6482280247'
dsq_needs_sync:
    - '1'
categories:
    - JavaScript
---

<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/2.7.1/Chart.min.js"></script>

Gdy pracowałem nad projektem testowania wydajności Xamarin.Forms chciałem stworzyć prosty sposób dla ludzi do obejrzenia wyników. Mało kto będzie na tyle zdeterminowany, żeby ściągać projekt z GitHuba i go odpalać. Dlatego stworzyłem stronę, na której można przejrzeć wszystkie wyniki ([tutaj](http://xamformsperf.azurewebsites.net)). Na początku były to zwykłe tabelki, jednak tabelki, nawet z kolorami, nie są aż tak zachęcające. Dlatego postanowiłem zastąpić je wykresami. Zastanawiałem się, czego mogę używać. Myślałem o gotowych komponentach, np. od Syncfusion (strona jest napisana w ASP.NET MVC). Jednak wydawało mi się, że dla jednego wykresu będzie to za dużo zabawy. Dlatego poszukałem czegoś innego i znalazłem [chart.js](http://www.chartjs.org).

#### Wprowadzenie

Chart.js to biblioteka, jak nie trudno się domyślić, napisana w JavaScript 😉 Jest bardzo prosta w użyciu. Żeby zobaczyć pierwszy wykres na swojej stronie, wystarczy poświęcić jej kilka minut. Jednak mimo tego, że jest prosta, nadal posiada wiele opcji i wystarczy nam do różnych scenariuszy.

#### Jak zacząć?

Musimy dodać skrypt js do naszej strony. Możemy dodać go do projektu lub użyć CDN. Druga opcja jest chyba wygodniejsza. W moim projekcie wyglądało to po prostu tak:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/fb166fe88b470ec09b794a0c28a6c729.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/fb166fe88b470ec09b794a0c28a6c729).</noscript></div>Gdy skrypt jest dodany, wtedy możemy wyświetlić pierwszy wykres. W miejscu, w którym chcemy go zobaczyć, dodajemy tag `canvas` z id oraz wysokością i szerokością.

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/7142e4c97a46cf8b5eee485d29a63319.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/7142e4c97a46cf8b5eee485d29a63319).</noscript></div>Następnie umieszczamy gdzieś skrypt, który wczyta do tego canvasa wykres. Powyżej widzimy najprostszy możliwy kod – wykres kolumnowy, z dwoma wartościami. Ważne jest to aby w obiekcie `data` dodać labele (oś x), i dataset z danymi w polu `data` – ilość danych powinna się zgadzać z ilością labeli. Tyle wystarczy, `backgroundColor` można pominąć, jednak zazwyczaj jest to przydatne. Efekt będzie taki:

<canvas height="300" id="myChart" width="800"></canvas>  
<script>
var ctx = document.getElementById("myChart").getContext('2d');
var myChart = new Chart(ctx, {
    type: 'bar',
    data: {
        labels: ["Red", "Blue"],
        datasets: [{
            label: 'Which color do you like?',
            data: [12, 19],
            backgroundColor: [
                'rgba(255, 99, 132, 0.2)',
                'rgba(54, 162, 235, 0.2)'
            ]
        }]
    }
});
</script>

Jak widzimy brakuje nam pierwszej kolumny. Domyślnie oś y wyświetla się od najmniejszej wartości. Jeśli dodamy jednak do wykresu opcję `beginAtZero`, to otrzymamy oczekiwany efekt:

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/a878cf2003aed7e4cbf6230a620f8982.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/a878cf2003aed7e4cbf6230a620f8982).</noscript></div><canvas height="300" id="myChart2" width="800"></canvas>  
<script>
var ctx = document.getElementById("myChart2").getContext('2d');
var myChart = new Chart(ctx, {
    type: 'bar',
    data: {
        labels: ["Red", "Blue"],
        datasets: [{
            label: 'Which color do you like?',
            data: [12, 19],
            backgroundColor: [
                'rgba(255, 99, 132, 0.2)',
                'rgba(54, 162, 235, 0.2)'
            ],
            borderColor: [
                'rgba(255,99,132,1)',
                'rgba(54, 162, 235, 1)'
            ],
            borderWidth: 1
        }]
    },
    options: {
        scales: {
            yAxes: [{
                ticks: {
                    beginAtZero: true
                }
            }]
        }
    }
});
</script>

Dodałem także obramowanie dla kolumn. Jak widzisz, jest to bardzo proste.

Teraz zauważyłem, że w poście wykresy te wyglądają na nie do końca wyraźne (nawet jak dopasowałem rozmiar pixel do pixela). Na stronie, którą robiłem nie ma tego problemu – możecie to zobaczyć [tutaj](http://xamformsperf.azurewebsites.net). Ciekawe. Szybkie googlowanie pokazuje, że jest to znany problem 🙂 Niestety nie udało mi się na szybko znaleźć rozwiązania.

#### Opcje i pluginy

Właściwie tylko tyle potrzebujemy, żeby wyświetlać wykresy na naszej stronie. Jednak tak jak mówiłem, chart.js posiada wiele opcji, których najlepiej szukać w [dokumentacji](http://www.chartjs.org/docs/latest/). Warto też spojrzeć na [przykłady](http://www.chartjs.org/samples/latest/) – tutaj pewnie będzie nawet łatwiej znaleźć to, czego potrzebujemy. Jako że jest to js, możemy go bez problemu podejrzeć w kodzie strony.

Jeśli czegoś nie znajdziemy, to do chart.js dostępne są także pluginy. Mi osobiści przydał się plugin do wyświetlania tooltipów po najechaniu na kolumny – [tutaj link](https://github.com/chartjs/chartjs-plugin-datalabels). Jest on również dostępny przez cdn i wystarczy go wczytać tak jak chart.js. [Tutaj](http://www.chartjs.org/docs/latest/notes/extensions.html) znajdziemy listę popularnych rozszerzeń.

#### Podsumowanie

Post jest krótki, ale więcej chyba nie potrzeba. Chart.js to prosta w użyciu biblioteka do wykresów, która daje nam duże możliwości. Nie spotkałem się z większymi problemami podczas jej używania. Dopiero podczas pisania tego posta, natknąłem się na problem z lekko rozmazanymi wykresami. Jednak gdyby spędzić nad tym wystarczająco dużo czasu, pewnie da się to poprawić. Chart.js to na pewno świetna alternatywa dla innych, bardziej rozbudowanych bibliotek i warto jej użyć, gdy chcemy szybko zobaczyć efekty.