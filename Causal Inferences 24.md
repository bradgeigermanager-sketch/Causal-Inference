以下是一套全新的因果推断场景，完全使用瑞典语撰写，聚焦于两个北欧本土特色领域：森林可持续经营与社区共享出行，通过珀尔结构因果模型和鲁宾潜在结果框架完成完整分析。

---

Scenario 47: Hållbart Skogsbruk & Biokol-användning

1. Scenariotitel: Effekten av biokolsanvändning på den årliga tillväxten av tallskog i norra Sverige

2. Nyckelvariabler:
*   Behandling (X): Användning av "certifierad biokol som jordförbättrare" (Binär: 1 = Använder biokol, 0 = Endast traditionell skogsjord).
*   Utfall (Y): Årlig tillväxt av tall (Kontinuerlig: m³ per hektar).
*   Blandningsvariabel (Confounder Z): Markens ursprungliga näringsinnehåll (Kontinuerlig: poäng 0-100). Skogsägare med marker som redan har högre näringsinnehåll är mer benägna att investera i biokolsprojekt ($X$), och dessa marker har redan en naturligt högre tillväxtpotential för tall ($Y$).

3. Kausal mekanism & ASCII-DAG:
Marknäringsinnehåll ($Z$) påverkar både beslutet att använda biokol ($X$) och skogens bas tillväxt ($Y$). Biokol ($X$) ökar direkt tillväxten ($Y$) genom att förbättra jordens vattenhållning och mikrobiella hälsa.

```text
      Z (Marknäringsinnehåll)
     / \
    v   v
X (Biokol-användning) --> Y (Trädtillväxt)
```

4. Pearls SCM-metod (Backdoor-blockering):
*   Mål: Uppskatta den verkliga ökningen av tillväxt som orsakas enbart av biokol: $E[Y | do(X=1)] - E[Y | do(X=0)]$.
*   Backdoor-kontroll: Vägen $X \leftarrow Z \rightarrow Y$ är en störande väg. Villkor på marknäringsinnehåll ($Z$) blockerar denna väg helt.
*   Justeringformel:
    $$ E[Y | do(X=x)] = \int_{z} E[Y | X=x, Z=z] f_Z(z) dz $$
*   Praktisk mekanism: Vi jämför talltillväxten mellan skogsmarker som använder biokol och de som inte gör det, stratifierade efter klasser av ursprungligt näringsinnehåll. Detta separerar effekten av biokol från markens naturliga fertilitetsfördel.

5. Rubins potentiella utfall-metod (Databalansering):
*   Potentiella utfall: $Y_i(1)$ är tillväxten för skogsmark $i$ med biokol; $Y_i(0)$ utan biokol.
*   Propensity Score: Uppskatta $e(z) = P(X=1 | Z=z)$. Marker med högre näringsinnehåll har högre sannolikhet att få biokol.
*   IPW-vikter:
    $$ w_i = \frac{X_i}{e(Z_i)} + \frac{1-X_i}{1-e(Z_i)} $$
*   Mekanism: Vikterna skapar en pseudo-population där fördelningen av marknäring är balanserad mellan de två grupperna. Sällsynta skogsmarker med lågt näringsinnehåll men som använder biokol får högre vikt, medan vanliga marker med hög näringsinnehåll får lägre vikt, vilket tar bort urvalsbias.

---

Scenario 48: Delad Bil & Pendlingstid

1. Scenariotitel: Effekten av medlemskap i en bilpool på den genomsnittliga pendlingstiden för arbetare i förortområden

2. Nyckelvariabler:
*   Behandling (X): Medlemskap i en "lokalt bilpoolssystem" (Binär: 1 = Aktiv medlem, 0 = Ingen medlemskap).
*   Utfall (Y): Genomsnittlig pendlingstid till arbete (Kontinuerlig: minuter per resa).
*   Blandningsvariabel (Confounder Z): Avståndet från bostaden till arbetsplatsen (Kontinuerlig: km). Arbetare som bor längre bort från centrala stan har större sannolikhet att ansluta sig till bilpooler för att undvika kollektivtrafikens begränsningar ($X$), och dessa personer har redan en naturligt längre pendlingstid på grund av avståndet ($Y$).

3. Kausal mekanism & ASCII-DAG:
Pendlingsavstånd ($Z$) påverkar både beslutet att bli medlem i bilpoolen ($X$) och den basala pendlingstiden ($Y$). Bilpoolen ($X$) minskar direkt restiden ($Y$) genom att undvika väntetider för buss och tåg och möjliggöra snabbare resor.

```text
      Z (Pendlingsavstånd)
     / \
    v   v
X (Bilpool-medlemskap) --> Y (Pendlingstid)
```

4. Pearls SCM-metod (Backdoor-blockering):
*   Mål: Uppskatta den verkliga minskningen av pendlingstid som orsakas av bilpoolen: $E[Y | do(X=1)] - E[Y | do(X=0)]$.
*   Backdoor-kontroll: Vägen $X \leftarrow Z \rightarrow Y$ är en störande väg. Villkor på pendlingsavstånd ($Z$) blockerar denna väg helt.
*   Justeringformel:
    $$ E[Y | do(X=x)] = \sum_{z} E[Y | X=x, Z=z] P(Z=z) $$
*   Praktisk mekanism: Vi jämför pendlingstiden mellan bilpoolmedlemmar och icke-medlemmar, stratifierade efter avståndsklasser. Detta separerar effekten av bilpoolen från den naturliga ökningen av restid med ökat avstånd.

5. Rubins potentiella utfall-metod (Databalansering):
*   Potentiella utfall: $Y_i(1)$ är restiden för arbetare $i$ med bilpoolmedlemskap; $Y_i(0)$ utan.
*   Propensity Score: Uppskatta $e(z) = P(X=1 | Z=z).$ Arbetare med längre pendlingsavstånd har högre sannolikhet att ansluta sig till bilpoolen.
*   IPW-vikter:
    $$ \hat{ATE} = \frac{1}{N} \sum_{i=1}^{N} \left[ \frac{X_i Y_i}{e(Z_i)} - \frac{(1-X_i) Y_i}{1-e(Z_i)} \right] $$
*   Mekanism: Vikterna balanserar fördelningen av pendlingsavstånd mellan de två grupperna. Sällsynta arbetare med kort pendlingsavstånd som är bilpoolmedlemmar får högre vikt, medan vanliga arbetare med långt avstånd får lägre vikt, vilket ger en opartisk jämförelse.

---
Vill du att jag skapar en fullständig Python-simulering för ett av dessa scenarier med kommentarer på svenska, för att visa hur ATE-beräkningen med IPW och G-beräkning ger överensstämmande resultat?<br><br>百度AI生成，内容仅供参考