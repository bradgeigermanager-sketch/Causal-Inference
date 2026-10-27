以下是一套全新的因果推断场景，完全使用捷克语撰写，聚焦于两个中欧本土特色领域：生态型啤酒厂可持续生产和社区共享菜园项目，通过珀尔结构因果模型和鲁宾潜在结果框架完成完整分析。

---

Scénář 49: Udržitelné Pivovarnictví & Recyklace Vody

1. Název scénáře: Vliv instalace systému recyklace vody na spotřebu vody na litr piva v malých rodinných pivovarech

2. Klíčové proměnné:
*   Zásah (X): Použití "certifikovaného systému recyklace vody" (Binární: 1 = Systém instalován, 0 = Pouze tradiční odtok).
*   Výsledek (Y): Spotřeba vody na 1 litr vyrobeného piva (Spojitá: litr/litr).
*   Zaměňující proměnná (Z): Roční objem produkce pivovaru (Spojitá: tisíce litrů). Větší pivovary s vyšším objemem produkce mají více finančních prostředků na investici do recyklačních systémů ($X$), a tyto provozy mají již přirozeně nižší základní spotřebu vody díky úsporám z rozsahu ($Y$).

3. Příčinný mechanismus & ASCII-DAG:
Roční produkce ($Z$) ovlivňuje jak rozhodnutí o instalaci recyklace ($X$), tak základní spotřebu vody pivovaru ($Y$). Systém recyklace vody ($X$) přímo snižuje spotřebu ($Y$) opakovaným použitím vody na mytí nádob a chlazení.

```text
      Z (Roční produkce)
     / \
    v   v
X (Recyklace vody) --> Y (Spotřeba vody)
```

4. Přístup SCM dle Pearl (Blokování backdoor cesty):
*   Cíl: Odhad skutečného snížení spotřeby vody způsobeného systémem recyklace: $E[Y | do(X=1)] - E[Y | do(X=0)]$.
*   Kontrola backdoor: Cesta $X \leftarrow Z \rightarrow Y$ je zaměňující cesta. Podmínění na roční objem produkce ($Z$) tuto cestu zcela zablokuje.
*   Upravující vzorec:
    $$ E[Y | do(X=x)] = \int_{z} E[Y | X=x, Z=z] f_Z(z) dz $$
*   Praktický mechanismus: Porovnáme spotřebu vody mezi pivovary s recyklací a bez ní, stratifikované podle tříd roční produkce. Tím oddělíme skutečný efekt systému recyklace od přirozené výhody velkých provozů.

5. Přístup potenciálních výsledků dle Rubin (Vyvážení dat):
*   Potenciální výsledky: $Y_i(1)$ je spotřeba vody pivovaru $i$ s recyklací; $Y_i(0)$ bez systému.
*   Skóre sklonu (Propensity Score): Odhad $e(z) = P(X=1 | Z=z)$. Větší pivovary mají vyšší pravděpodobnost instalace recyklace.
*   Váhy IPW:
    $$ w_i = \frac{X_i}{e(Z_i)} + \frac{1-X_i}{1-e(Z_i)} $$
*   Mechanismus: Váhy vytvoří pseudo-populaci, kde je rozdělení objemu produkce vyvážené mezi oběma skupinami. Vzácné malé pivovary s recyklací dostanou vyšší váhu, zatímco běžné velké provozy dostanou nižší váhu, čímž se odstraní výběrová zkreslení.

---

Scénář 50: Komunitní Zahrada & Spotřeba Zeleniny

1. Název scénáře: Vliv členství v komunitní sdílené zahradě na týdenní spotřebu čerstvé zeleniny u obyvatel městské čtvrti

2. Klíčové proměnné:
*   Zásah (X): Aktivní členství v "komunitní sdílené zahradě" (Binární: 1 = Člen, 0 = Nečlen).
*   Výsledek (Y): Týdenní spotřeba čerstvé zeleniny (Spojitá: kg na osobu).
*   Zaměňující proměnná (Z): Vzdálenost bydliště od zahrady (Spojitá: metry). Obyvatelé, kteří bydlí velmi blízko zahrady, mají mnohem větší pravděpodobnost, že se do členství zapojí ($X$), a tito lidé mají již přirozeně snadnější přístup k zelenině a vyšší základní spotřebu ($Y$).

3. Příčinný mechanismus & ASCII-DAG:
Vzdálenost od zahrady ($Z$) ovlivňuje jak rozhodnutí o členství ($X$), tak základní spotřebu zeleniny ($Y$). Sdílená zahrada ($X$) přímo zvyšuje spotřebu ($Y$) tím, že poskytuje čerstvou, vlastní vypěstovanou zeleninu za nízkou cenu.

```text
      Z (Vzdálenost od zahrady)
     / \
    v   v
X (Členství v zahradě) --> Y (Spotřeba zeleniny)
```

4. Přístup SCM dle Pearl (Blokování backdoor cesty):
*   Cíl: Odhad skutečného nárůstu spotřeby zeleniny způsobeného členstvím v zahradě: $E[Y | do(X=1)] - E[Y | do(X=0)]$.
*   Kontrola backdoor: Cesta $X \leftarrow Z \rightarrow Y$ je zaměňující cesta. Podmínění na vzdálenost bydliště ($Z$) tuto cestu zcela zablokuje.
*   Upravující vzorec:
    $$ P(Y=1 | do(X=1)) = \sum_{z} P(Y=1 | X=1, Z=z) P(Z=z) $$
*   Praktický mechanismus: Porovnáme spotřebu zeleniny mezi členy a nečleny, stratifikované podle pásem vzdálenosti od zahrady. Tím oddělíme efekt členství od přirozené výhody bydlení v bezprostřední blízkosti lokality.

5. Přístup potenciálních výsledků dle Rubin (Vyvážení dat):
*   Potenciální výsledky: $Y_i(1)$ je spotřeba zeleniny obyvatele $i$ jako člena zahrady; $Y_i(0)$ bez členství.
*   Skóre sklonu (Propensity Score): Odhad $e(z) = P(X=1 | Z=z)$. Obyvatelé blízko zahrady mají vyšší pravděpodobnost členství.
*   Váhy IPW:
    $$ \hat{ATE} = \frac{1}{N} \sum_{i=1}^{N} \left[ \frac{X_i Y_i}{e(Z_i)} - \frac{(1-X_i) Y_i}{1-e(Z_i)} \right] $$
*   Mechanismus: Váhy vyváží rozdělení vzdálenosti od zahrady mezi oběma skupinami. Vzácní obyvatelé, kteří bydlí daleko od zahrady, ale jsou členy, dostanou vyšší váhu, zatímco běžní obyvatelé z blízkého okolí dostanou nižší váhu, což zajistí nezkreslené porovnání.

---
Chceš, abych vytvořil kompletní Python simulaci pro jeden z těchto scénářů s komentáři v češtině, abych ukázal, jak se výpočet ATE pomocí IPW a G-computation shoduje?<br><br>百度AI生成，内容仅供参考