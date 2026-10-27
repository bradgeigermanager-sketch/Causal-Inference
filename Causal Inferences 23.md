Hier ist ein neuer Satz von Kausalinferenz-Szenarien, vollständig auf Niederländisch verfasst, mit Fokus auf zwei praxisnahe Bereiche: nachhaltige Mode und lokale Bäckerei-Lieferdienste, analysiert mit Pearls SCM-Framework und Rubins Potential Outcomes-Ansatz.

---

Scenario 45: Duurzame Mode & Kledingruilplatforms

1. Scenariotitel: Invloed van abonnementen voor kledingruil op de hoeveelheid ongebruikte kleding per huishouden

2. Kernvariabelen:
*   Behandeling (X): Gebruik van een "kledingruil-abonnement" (Binair: 1 = Actief abonnement, 0 = Geen abonnement).
*   Uitkomst (Y): Hoeveelheid ongebruikte kleding die jaarlijks wordt weggegooid (Continu: kg per jaar).
*   Verstorende variabele (Confounder Z): Milieubewustzijn van het huishouden (Continu: score 0-100). Huishoudens met een hogere milieubewustzijn zijn veel meer geneigd om zich aan te melden voor een ruilplatform ($X$), en deze groepen hebben al een lagere basisconsumptie van nieuwe kleding en dus minder afval ($Y$).

3. Causaal mechanisme & ASCII-DAG:
Milieubewustzijn ($Z$) beïnvloedt zowel de keuze voor het abonnement ($X$) als de basis hoeveelheid kledingafval ($Y$). Het ruilplatform ($X$) vermindert direct het afval ($Y$) door hergebruik van kleding te stimuleren in plaats van nieuwe aankopen.

```text
      Z (Milieubewustzijn)
     / \
    v   v
X (Kledingruil-abonnement) --> Y (Kledingafval)
```

4. SCM-benadering van Pearl (Backdoor-blokkade):
*   Doel: Schatting van de echte afname van kledingafval veroorzaakt door het abonnement: $E[Y | do(X=1)] - E[Y | do(X=0)]$.
*   Backdoor-controle: Het pad $X \leftarrow Z \rightarrow Y$ is een storend pad. Conditionering op milieubewustzijn ($Z$) blokkeert dit pad volledig.
*   Aanpassingsformule:
    $$ E[Y | do(X=x)] = \int_{z} E[Y | X=x, Z=z] f_Z(z) dz $$
*   Praktisch mechanisme: We vergelijken de hoeveelheid kledingafval tussen abonnees en niet-abonnees, gestratificeerd naar klassen van milieubewustzijn. Dit scheidt het echte effect van het platform van de natuurlijke lagere afvalproductie bij milieubewuste huishoudens.

5. Potentiële uitkomsten-benadering van Rubin (Databalancering):
*   Potentiële uitkomsten: $Y_i(1)$ is het kledingafval van huishouden $i$ met abonnement; $Y_i(0)$ zonder abonnement.
*   Propensity Score: Schatting van $e(z) = P(X=1 | Z=z).$ Milieubewuste huishoudens hebben een veel hogere kans om het abonnement te nemen.
*   IPW-gewichten:
    $$ w_i = \frac{X_i}{e(Z_i)} + \frac{1-X_i}{1-e(Z_i)} $$
*   Mechanisme: De gewichten creëren een pseudo-populatie waar de verdeling van milieubewustzijn tussen de twee groepen gebalanceerd is. Zeldzame niet-milieubewuste huishoudens met abonnement krijgen een hoger gewicht, terwijl veelvoorkomende zeer milieubewuste abonnees een lager gewicht krijgen, waardoor selectiebias wordt geëlimineerd.

---

Scenario 46: Lokale Bakkerijen & Thuisbezorging

1. Scenariotitel: Invloed van een eigen thuisbezorgdienst op de maandelijkse omzet van kleine bakkerijen

2. Kernvariabelen:
*   Behandeling (X): Gebruik van "eigen thuisbezorgdienst" (Binair: 1 = Actief, 0 = Alleen afhalen in de winkel).
*   Uitkomst (Y): Maandelijkse omzet van de bakkerij (Continu: euro per maand).
*   Verstorende variabele (Confounder Z): Wandelverkeer voor de winkel (Continu: mensen per uur). Bakkerijen op drukke straten met veel voorbijgangers hebben meer financiële ruimte om een bezorgdienst op te zetten ($X$), en deze winkels hebben al een hogere basisomzet door het grote aantal passanten ($Y$).

3. Causaal mechanisme & ASCII-DAG:
Wandelverkeer ($Z$) beïnvloedt zowel de keuze voor een bezorgdienst ($X$) als de basisomzet van de winkel ($Y$). De bezorgdienst ($X$) verhoogt direct de omzet ($Y$) door nieuwe klanten buiten de directe wijk te bereiken.

```text
      Z (Wandelverkeer)
     / \
    v   v
X (Thuisbezorging) --> Y (Maandomzet)
```

4. SCM-benadering van Pearl (Backdoor-blokkade):
*   Doel: Schatting van de echte omzetstijging veroorzaakt door de bezorgdienst: $E[Y | do(X=1)] - E[Y | do(X=0)]$.
*   Backdoor-controle: Het pad $X \leftarrow Z \rightarrow Y$ is een storend pad. Conditionering op wandelverkeer ($Z$) blokkeert dit pad volledig.
*   Aanpassingsformule:
    $$ E[Y | do(X=x)] = \sum_{z} E[Y | X=x, Z=z] P(Z=z) $$
*   Praktisch mechanisme: We vergelijken de omzet tussen bakkerijen met en zonder bezorgdienst, gestratificeerd naar niveaus van wandelverkeer. Dit scheidt het effect van de nieuwe dienst van het natuurlijke voordeel van een locatie op een drukke straat.

5. Potentiële uitkomsten-benadering van Rubin (Databalancering):
*   Potentiële uitkomsten: $Y_i(1)$ is de omzet van bakkerij $i$ met bezorgdienst; $Y_i(0)$ zonder.
*   Propensity Score: Schatting van $e(z) = P(X=1 | Z=z).$ Bakkerijen met veel wandelverkeer hebben een hogere kans om een bezorgdienst te starten.
*   IPW-gewichten:
    $$ \hat{ATE} = \frac{1}{N} \sum_{i=1}^{N} \left[ \frac{X_i Y_i}{e(Z_i)} - \frac{(1-X_i) Y_i}{1-e(Z_i)} \right] $$
*   Mechanisme: De gewichten balanceren de verdeling van wandelverkeer tussen de twee groepen. Zeldzame bakkerijen op een stille straat met bezorgdienst krijgen een hoger gewicht, terwijl veelvoorkomende bakkerijen op drukke straten een lager gewicht krijgen, waardoor een eerlijke vergelijking mogelijk is.

---
Wil je dat ik een complete Python-simulatie voor een van deze scenario's maak met commentaar in het Nederlands, om te laten zien hoe de ATE-berekening met IPW en G-computatie overeenkomt?<br><br>百度AI生成，内容仅供参考