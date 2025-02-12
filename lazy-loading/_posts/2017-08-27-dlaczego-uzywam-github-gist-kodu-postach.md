---
id: 171
title: 'Dlaczego używam GitHub Gist do kodu w postach?'
date: '2017-08-27T15:47:04+01:00'
author: 'Tomasz Wiśniewski'
layout: post
guid: 'https://lazyloading.pl/?p=171'
permalink: /2017/08/27/dlaczego-uzywam-github-gist-kodu-postach/
dsq_thread_id:
    - '6099617279'
categories:
    - WordPress
---

Jest to blog technologiczny, więc w postach często pojawia się jakiś kod. Nie można go wkleić po prostu do treści posta, ponieważ będzie to zupełnie nieczytelne – nie będziemy mieli ani kolorowania składni, ani wcięć (chyba, że chcemy się sporo napracować), ani kod nie będzie w żaden inny sposób wyróżniony.

public class SampleClass

{

public int SampleMethod()

{

return 1 + 2;

}

}

Odstępy między liniami też są zbyt duże. Najprostszą alternatywą są cytaty. Tutaj będziemy mieli już pewne wyróżnienie, ale dalej jest słabo. Może się to jednak czasami nadawać do kodu w jednej linijce.

> public class SampleClass
> 
> {
> 
> public int SampleMethod()
> 
> {
> 
> return 1 + 2;
> 
> }
> 
> }

Ludzie najczęściej instalują do tego zadania plugin typu [SyntaxHighlighter Evolved](https://pl.wordpress.org/plugins/syntaxhighlighter/). Ja też tak na początku zrobiłem, jednak nie podobało mi się tam kilka rzeczy. Głównym problemem było to, że **gdy przechodziłem w poście do widoku html, to znaki &lt;, &gt; zamieniały się w &amp;lt; oraz &amp;gt; i trzeba było je ręcznie naprawiać**. Tragedia. Często męczyłem się też z wcięciami, bo nie chciały działać tak jak powinny. Inną mniejszą niedogodnością było zawijanie tekstu – trzeba do tego włączyć starszą wersję pluginu, wersję 2, bowiem wersja 3 nie posiada tej opcji.

GitHub Gist rozwiązuje pierwszy oraz drugi problem i głównie dlatego zdecydowałem się na zmianę. Teoretycznie ma też opcję zawijania tekstu, ale na razie nie udało mi się zmusić jej do działania.

#### Czy to aby na pewno wygodne?

Nowe gisty tworzy się szybko. Właściwie nie trzeba mieć nawet konta na GitHubie, można tworzyć anonimowe gisty. Problem jest tylko z ich usuwaniem. W dokumentacji jest napisane, że żeby usunąć taki gist musimy napisać do pomocy GitHuba. Ja jednak przed chwilą taki gist stworzyłem i widziałem przycisk „Delete”. Co więcej, stworzyłem ten gist na karcie prywatnej, gdzie byłem wylogowany. Gdy otworzyłem go bez trybu prywatnego, gdzie byłem zalogowany, to także mogłem go usunąć. Ciekawa sprawa. **Dlatego lepiej posiadać konto na GitHubie i tworzyć gisty będą zalogowanym.**

Aby stworzyć takiego gist można przejść bezpośrednio na adres <https://gist.github.com> lub z [github.com](https://github.com) wybrać plusa z menu na górze po prawej przy profilu, a następnie „New gist”. Wtedy:

1. Dodajemy nazwę pliku – nie jest ważne jaka to nazwa. Najlepiej, żeby jakoś łączyła się z kodem, który tam umieścimy, ale nie jest to konieczne. **Ważne jest za to rozszerzenie – na jego podstawie dobierane jest kolorowanie składni.**
2. Wklejamy kod. Lub też piszemy.
3. W menu po prawej na górze możemy wybrać opcje dla wcięć (spacje lub taby oraz głębokość) i zawinięć wiersza.
4. Opcjonalnie możemy dodać opis na samej górze.
5. Zapisujemy. Gist może być publiczny lub prywatny. Nie ma znaczenia, który wybierzemy, oba można wyświetlić w poście.

**Aby wyświetlić gist w poście potrzebny jest plugin.** Ja używam [oEmbed Gist](https://pl.wordpress.org/plugins/oembed-gist/). Z tym pluginem wystarczy wkleić link do posta i gist od razu pokazuje nam się w podglądzie. Jest to całkiem wygodne.

<div class="oembed-gist"><script src="https://gist.github.com/tomwis/9e07ef8c64528a7394e016b907a7bbba.js"></script><noscript>View the code on [Gist](https://gist.github.com/tomwis/9e07ef8c64528a7394e016b907a7bbba).</noscript></div>

[![](/wp-content/uploads/2017/08/gist_w_podgladzie--1024x283.png)](/wp-content/uploads/2017/08/gist_w_podgladzie-.png)

Brakuje mi tu trochę tego zawijania wierszy. Mam nadzieję, że w końcu uda mi się zmusić tę opcję do działania. Wydaje mi się, że z tym posty będzie się zwyczajnie wygodniej czytało. Oprócz tego na razie nie mam zastrzeżeń do gistów. Pracuje mi się z nimi przyjemniej niż z SyntaxHighlighter Evolved.

#### Kod inline

Miałem tu napisać o tym, że chciałbym jakoś wyróżniać kod w zdaniu. Gdy np. omawiam coś i w zdaniu wymieniam nazwę metody, czy klasy, chciałbym żeby były one jakoś wyróżnione – inną czcionką (jak monospace), czy może tłem. Jednak niedawno odkryłem znacznik &lt;code&gt; i zaraz potem skrót Short + Atl + x, który w edytorze wordpressa opakowuje zaznaczony tekst w ten znacznik i wygląda to `o tak`. Świetna sprawa, polecam 🙂