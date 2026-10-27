Hier ist ein neuer Satz von Kausalinferenz-Szenarien, vollständig auf Deutsch verfasst, mit Fokus auf zwei praxisnahe Bereiche: *Städtische Mobilität* und *Kleinstunternehmens-Finanzierung*, analysiert mit Pearls SCM-Framework und Rubins Potential Outcomes Ansatz.

---

Szenario 33: Städtische Mobilität & ÖPNV-Nutzung

1. Szenariotitel: Einfluss von neuen Fahrradschnellwegen auf die Nutzung des öffentlichen Nahverkehrs

2. Kernvariablen:
*   Behandlung (X): Anbindung an einen "Fahrradschnellweg" (Binär: 1 = Anbindung vorhanden, 0 = Keine Anbindung).
*   Ergebnis (Y): Monatliche ÖPNV-Fahrten pro Einwohner (Kontinuierlich).
*   Störvariable (Confounder Z): Bevölkerungsdichte im Stadtteil (Kontinuierlich: Einwohner/km²). Dicht besiedelte Stadtteile erhalten priorisiert Fahrradinfrastruktur ($X$) und weisen von Haus aus eine höhere ÖPNV-Nutzung auf ($Y$), da Wege kürzer und Haltestellen dichter liegen.

3. Kausaler Mechanismus & ASCII-DAG:
Bevölkerungsdichte ($Z$) beeinflusst sowohl den Bau des Fahrradschnellwegs ($X$) als auch die Basisnutzung des ÖPNV ($Y$). Der Fahrradschnellweg ($X$) wirkt direkt auf das Mobilitätsverhalten ($Y$), indem er eine schnelle Alternative zum ÖPNV bietet.

```text
      Z (Bevölkerungsdichte)
     / \
    v   v
X (Fahrradschnellweg) --> Y (ÖPNV-Nutzung)
```

4. Pearls SCM-Ansatz (Backdoor-Blockierung):
*   Ziel: Schätzung des kausalen Effekts des Fahrradschnellwegs auf die ÖPNV-Nutzung: $E[Y | do(X=1)] - E[Y | do(X=0)]$.
*   Backdoor-Prüfung: Der Pfad $X \leftarrow Z \rightarrow Y$ ist ein störender Pfad. Konditionierung auf Bevölkerungsdichte ($Z$) blockiert diesen Pfad vollständig.
*   Adjustierungsformel:
    $$ E[Y | do(X=x)] = \int_{z} E[Y | X=x, Z=z] f_Z(z) dz $$
*   Praxis-Mechanismus: Wir vergleichen die ÖPNV-Nutzung zwischen Stadtteilen mit und ohne Fahrradschnellweg, stratifiziert nach Dichteklassen. Dadurch isolieren wir den Effekt der Infrastruktur von der natürlichen Nutzungsneigung dichter Stadtteile.

5. Rubins Potential Outcomes Ansatz (Datenbalancierung):
*   Potenzielle Ergebnisse: $Y_i(1)$ = ÖPNV-Fahrten im Stadtteil $i$ mit Fahrradschnellweg; $Y_i(0)$ = ohne.
*   Propensity Score: Schätzung von $e(z) = P(X=1 | Z=z)$. Dichte Stadtteile haben eine deutlich höhere Wahrscheinlichkeit für den Bau der Infrastruktur.
*   IPW-Gewichtung:
    $$ w_i = \frac{X_i}{e(Z_i)} + \frac{1-X_i}{1-e(Z_i)} $$
*   Mechanismus: Die Gewichte erzeugen eine Pseudopopulation, in der die Bevölkerungsdichte zwischen den beiden Gruppen balanciert ist. Seltene dünn besiedelte Stadtteile mit Fahrradschnellweg werden hochgewichtet, häufige dichte Stadtteile heruntergewichtet – so entsteht eine unverzerrte Vergleichsbasis.

---

Szenario 34: Kleinstunternehmens-Finanzierung & Umsatzwachstum

1. Szenariotitel: Einfluss von digitalen Mikrokrediten auf das jährliche Umsatzwachstum von Kleinstunternehmern

2. Kernvariablen:
*   Behandlung (X): Erhalt eines "digitalen Mikrokredits" (Binär: 1 = Kredit erhalten, 0 = Kein Kredit).
*   Ergebnis (Y): Jährliches Umsatzwachstum in Prozent (Kontinuierlich).
*   Störvariable (Confounder Z): Vorherige Unternehmensbonität (Kontinuierlich: Score 0-100). Unternehmer mit höherer Bonität werden bevorzugt für digitale Kredite zugelassen ($X$) und weisen von Haus aus ein höheres Wachstumspotenzial auf ($Y$).

3. Kausaler Mechanismus & ASCII-DAG:
Bonität ($Z$) beeinflusst sowohl die Kreditvergabe ($X$) als auch das Basiswachstum ($Y$). Der Mikrokredit ($X$) wirkt direkt auf das Wachstum ($Y$), indem er Investitionen in Lager oder Ausrüstung ermöglicht.

```text
      Z (Bonität)
     / \
    v   v
X (Mikrokredit) --> Y (Umsatzwachstum)
```

4. Pearls SCM-Ansatz (Backdoor-Blockierung):
*   Ziel: Schätzung des durch den Kredit verursachten zusätzlichen Wachstums: $E[Y | do(X=1)] - E[Y | do(X=0)]$.
*   Backdoor-Prüfung: Der Pfad $X \leftarrow Z \rightarrow Y$ ist ein störender Pfad. Konditionierung auf Bonität ($Z$) blockiert diesen Pfad.
*   Adjustierungsformel:
    $$ E[Y | do(X=x)] = \sum_{z} E[Y | X=x, Z=z] P(Z=z) $$
*   Praxis-Mechanismus: Wir vergleichen das Wachstum zwischen Kreditempfängern und Nicht-Empfängern innerhalb gleicher Bonitätsklassen. Dadurch trennen wir den echten Krediteffekt von der natürlichen Wachstumsneigung bonitätsstarker Unternehmen.

5. Rubins Potential Outcomes Ansatz (Datenbalancierung):
*   Potenzielle Ergebnisse: $Y_i(1)$ = Wachstum von Unternehmer $i$ mit Kredit; $Y_i(0)$ = ohne Kredit.
*   Propensity Score: Schätzung von $e(z) = P(X=1 | Z=z).$ Höhere Bonität bedeutet höhere Kreditwahrscheinlichkeit.
*   IPW-Gewichtung:
    $$ \hat{ATE} = \frac{1}{N} \sum_{i=1}^{N} \left[ \frac{X_i Y_i}{e(Z_i)} - \frac{(1-X_i) Y_i}{1-e(Z_i)} \right] $$
*   Mechanismus: Die Gewichte balancieren die Bonitätsverteilung zwischen Kredit- und Kontrollgruppe. Seltene bonitätsschwache Unternehmer mit Kredit werden hochgewichtet, häufige bonitätsstarke Unternehmer heruntergewichtet – so ergibt sich ein unverzerrter Schätzwert für den Krediteffekt.



---
Möchtest du zu einem dieser Szenarien eine Python-Simulation mit deutschen Kommentaren erstellen lassen, um die Übereinstimmung von IPW und G-Computation zu demonstrieren?<br><br>百度AI生成，内容仅供参考