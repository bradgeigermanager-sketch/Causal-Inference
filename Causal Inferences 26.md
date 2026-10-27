以下是一套全新的因果推断场景，完全使用波兰语撰写，聚焦于两个东欧本土特色领域：苹果种植可持续农业和社区二手图书交换项目，通过珀尔结构因果模型和鲁宾潜在结果框架完成完整分析。

---

Scenariusz 51: Zrównoważone Sadownictwo & Nawadnianie Kropelkowe

1. Tytuł Scenariusza: Wpływ instalacji systemu nawadniania kropelkowego na roczną plonowanie jabłoni w małych rodzinnych sadach

2. Zmienne Kluczowe:
*   Interwencja (X): Użycie "certyfikowanego systemu nawadniania kropelkowego" (Binarna: 1 = System zainstalowany, 0 = Tradycyjne podlewanie).
*   Wynik (Y): Roczna plonowanie jabłek (Ciągła: tony na hektar).
*   Zmienna Myląca (Z): Poziom nawodnienia gleby (Ciągła: wskaźnik 0-100). Sady na glebach o niskim naturalnym nawodnieniu mają większą szansę na otrzymanie dotacji na system nawadniania ($X$), a te same gleby mają naturalnie niższy bazowy plon jabłek bez dodatkowej wody ($Y$).

3. Mechanizm Przyczynowy & DAG ASCII:
Poziom nawodnienia gleby ($Z$) wpływa zarówno na decyzję o instalacji nawadniania ($X$), jak i na bazowy plon sadu ($Y$). System nawadniania kropelkowego ($X$) bezpośrednio zwiększa plon ($Y$) poprzez stabilne dostarczanie wody w krytycznych fazach wzrostu drzew.

```text
      Z (Nawodnienie gleby)
     / \
    v   v
X (Nawadnianie kropelkowe) --> Y (Plon jabłek)
```

4. Podejście SCM Pearl'a (Blokada ścieżki backdoor):
*   Cel: Oszacowanie rzeczywistego wzrostu plonu spowodowanego wyłącznie systemem nawadniania: $E[Y | do(X=1)] - E[Y | do(X=0)]$.
*   Kontrola backdoor: Ścieżka $X \leftarrow Z \rightarrow Y$ jest ścieżką mylącą. Warunkowanie na poziomie nawodnienia gleby ($Z$) całkowicie ją blokuje.
*   Wzór dostosowania:
    $$ E[Y | do(X=x)] = \int_{z} E[Y | X=x, Z=z] f_Z(z) dz $$
*   Mechanizm praktyczny: Porównujemy plony jabłek między sadami z nawadnianiem kropelkowym a tymi z tradycyjnym podlewaniem, stratyfikowane według klas nawodnienia gleby. Oddziela to rzeczywisty efekt systemu od naturalnej różnicy w żyzności gleb.

5. Podejście Potencjalnych Wyników Rubin'a (Równoważenie danych):
*   Potencjalne wyniki: $Y_i(1)$ to plon sadu $i$ z nawadnianiem kropelkowym; $Y_i(0)$ bez systemu.
*   Wynik skłonności (Propensity Score): Oszacowanie $e(z) = P(X=1 | Z=z)$. Sady na glebach suchych mają znacznie wyższe prawdopodobieństwo otrzymania systemu nawadniania.
*   Wagi IPW:
    $$ w_i = \frac{X_i}{e(Z_i)} + \frac{1-X_i}{1-e(Z_i)} $$
*   Mechanizm: Wagi tworzą pseudo-populację, w której rozkład nawodnienia gleby jest zrównoważony między obiema grupami. Rzadkie sady na bardzo nawodnionych glebach, które mimo to zainstalowały nawadnianie, otrzymują wyższą wagę, podczas gdy częste sady na suchych glebach dostają niższą wagę, eliminując błąd selekcji.

---

Scenariusz 52: Społeczna Wymiana Książek & Częstość Czytania

1. Tytuł Scenariusza: Wpływ członkostwa w społecznej wymianie książek na liczbę przeczytanych książek rocznie u mieszkańców małego miasta

2. Zmienne Kluczowe:
*   Interwencja (X): Aktywne członkostwo w "lokalnej sieci wymiany książek" (Binarna: 1 = Członek, 0 = Nieczłonek).
*   Wynik (Y): Liczba książek przeczytanych w ciągu roku (Ciągła: sztuki na osobę).
*   Zmienna Myląca (Z): Liczba godzin wolnego czasu tygodniowo (Ciągła: godziny/tydzień). Mieszkańcy z większą ilością wolnego czasu mają większą szansę dołączyć do sieci wymiany książek ($X$), a te osoby mają naturalnie wyższą bazową liczbę przeczytanych książek nawet bez wymiany ($Y$).

3. Mechanizm Przyczynowy & DAG ASCII:
Czas wolny ($Z$) wpływa zarówno na decyzję o dołączeniu do wymiany książek ($X$), jak i na bazową częstość czytania ($Y$). Sieć wymiany książek ($X$) bezpośrednio zwiększa liczbę przeczytanych książek ($Y$), zapewniając darmowy dostęp do różnorodnych tytułów.

```text
      Z (Czas wolny)
     / \
    v   v
X (Wymiana książek) --> Y (Liczba przeczytanych książek)
```

4. Podejście SCM Pearl'a (Blokada ścieżki backdoor):
*   Cel: Oszacowanie rzeczywistego wzrostu liczby przeczytanych książek spowodowanego siecią wymiany: $E[Y | do(X=1)] - E[Y | do(X=0)]$.
*   Kontrola backdoor: Ścieżka $X \leftarrow Z \rightarrow Y$ jest ścieżką mylącą. Warunkowanie na ilość wolnego czasu ($Z$) całkowicie ją blokuje.
*   Wzór dostosowania:
    $$ E[Y | do(X=x)] = \sum_{z} E[Y | X=x, Z=z] P(Z=z) $$
*   Mechanizm praktyczny: Porównujemy liczbę przeczytanych książek między członkami a nieczłonkami sieci, stratyfikowane według przedziałów czasu wolnego. Oddziela to efekt wymiany książek od naturalnej przewagi osób z dużą ilością wolnego czasu.

5. Podejście Potencjalnych Wyników Rubin'a (Równoważenie danych):
*   Potencjalne wyniki: $Y_i(1)$ to liczba przeczytanych książek mieszkańca $i$ jako członka sieci; $Y_i(0)$ bez członkostwa.
*   Wynik skłonności (Propensity Score): Oszacowanie $e(z) = P(X=1 | Z=z)$. Osoby z dużą ilością wolnego czasu mają wyższe prawdopodobieństwo dołączenia do wymiany.
*   Wagi IPW:
    $$ \hat{ATE} = \frac{1}{N} \sum_{i=1}^{N} \left[ \frac{X_i Y_i}{e(Z_i)} - \frac{(1-X_i) Y_i}{1-e(Z_i)} \right] $$
*   Mechanizm: Wagi równoważą rozkład ilości wolnego czasu między obiema grupami. Rzadkie osoby z małą ilością wolnego czasu, które są członkami sieci, otrzymują wyższą wagę, podczas gdy częste osoby z dużą ilością wolnego czasu dostają niższą wagę, co zapewnia bezstronne porównanie.

---
Czy chcesz, abym stworzył pełną symulację w Pythonie dla jednego z tych scenariuszy z komentarzami po polsku, aby pokazać, jak obliczenie ATE za pomocą IPW i G-computation daje zgodne wyniki?<br><br>百度AI生成，内容仅供参考