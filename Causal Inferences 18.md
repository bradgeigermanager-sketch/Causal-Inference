Ecco un nuovo set di scenari di inferenza causale, interamente in italiano, focalizzati su due settori reali: *Agricoltura Biologica* e *Formazione Professionale Digitale*, analizzati con il modello causale strutturale di Pearl e il framework dei risultati potenziali di Rubin.

---

Scenario 35: Agricoltura Biologica e Resa delle Coltivazioni

1. Titolo dello Scenario: Impatto della conversione all’agricoltura biologica sulla resa del grano duro

2. Variabili Chiave:
*   Trattamento (X): Conversione a "Metodo Biologico Certificato" (Binario: 1 = Convertito, 0 = Agricoltura Convenzionale).
*   Risultato (Y): Resa annuale di grano duro (Continua: quintali per ettaro).
*   Fattore Confondente (Z): Qualità iniziale del suolo (Continua: indice di fertilità 0-100). Le aziende con terreni più fertili sono più propense a investire nella certificazione biologica ($X$), e questi terreni producono naturalmente rese più elevate anche senza trattamenti ($Y$).

3. Meccanismo Causale e DAG ASCII:
La fertilità del suolo ($Z$) influenza sia la decisione di conversione ($X$) che la resa base ($Y$). Il metodo biologico ($X$) agisce direttamente sulla resa ($Y$) modificando l’equilibrio nutritivo e la gestione delle infestanti.

```text
      Z (Fertilità del Suolo)
     / \
    v   v
X (Agricoltura Biologica) --> Y (Resa del Grano)
```

4. Approccio SCM di Pearl (Blocco Backdoor):
*   Obiettivo: Stimare la differenza di resa causata esclusivamente dalla conversione biologica: $E[Y | do(X=1)] - E[Y | do(X=0)]$.
*   Verifica Backdoor: Il percorso $X \leftarrow Z \rightarrow Y$ è una via confondente. Condizionare sulla fertilità del suolo ($Z$) blocca completamente questo percorso.
*   Formula di Aggiustamento:
    $$ E[Y | do(X=x)] = \int_{z} E[Y | X=x, Z=z] f_Z(z) dz $$
*   Meccanismo Pratico: Confrontiamo le rese tra aziende convertite e convenzionali, stratificate per classi di fertilità del suolo. Questo isola l’effetto del metodo biologico dalla produttività intrinseca del terreno.

5. Approccio Risultati Potenziali di Rubin (Bilanciamento Dati):
*   Risultati Potenziali: $Y_i(1)$ = resa dell’azienda $i$ se convertita al biologico; $Y_i(0)$ = se rimane convenzionale.
*   Punteggio di Propensione: Stima di $e(z) = P(X=1 | Z=z)$. Aziende con suoli fertili hanno una probabilità maggiore di conversione.
*   Pesi IPW:
    $$ w_i = \frac{X_i}{e(Z_i)} + \frac{1-X_i}{1-e(Z_i)} $$
*   Meccanismo: I pesi creano una pseudo-popolazione dove la distribuzione di fertilità è bilanciata tra i due gruppi. Le rare aziende con suolo povero ma convertite vengono sovrappesate, mentre le comuni aziende fertili convertite vengono sottopesate, eliminando il bias di selezione.

---

Scenario 36: Formazione Professionale Digitale e Tasso di Occupazione

1. Titolo dello Scenario: Impatto dei corsi di programmazione online sul tasso di assunzione nel settore tech

2. Variabili Chiave:
*   Trattamento (X): Partecipazione a "Corso di Programmazione Certificato" (Binario: 1 = Partecipato, 0 = Nessun corso).
*   Risultato (Y): Assunzione in un ruolo tech entro 6 mesi (Binario: 1 = Assunto, 0 = Non assunto).
*   Fattore Confondente (Z): Livello di istruzione pregressa (Ordinale: Diploma, Laurea Triennale, Laurea Magistrale). Persone con titoli di studio più alti sono più motivate e più facilmente ammesse ai corsi avanzati ($X$), e hanno già una probabilità maggiore di essere assunte nel settore tech anche senza corso ($Y$).

3. Meccanismo Causale e DAG ASCII:
Il titolo di studio ($Z$) influenza sia l’accesso al corso ($X$) che la probabilità di assunzione base ($Y$). Il corso di programmazione ($X$) aumenta direttamente la probabilità di assunzione ($Y$) tramite competenze pratiche e progetti nel portfolio.

```text
      Z (Titolo di Studio)
     / \
    v   v
X (Corso di Programmazione) --> Y (Assunzione Tech)
```

4. Approccio SCM di Pearl (Blocco Backdoor):
*   Obiettivo: Stimare l’aumento della probabilità di assunzione causato esclusivamente dal corso: $P(Y=1 | do(X=1)) - P(Y=1 | do(X=0))$.
*   Verifica Backdoor: Il percorso $X \leftarrow Z \rightarrow Y$ è una via confondente. Condizionare sul titolo di studio ($Z$) blocca questa confusione.
*   Formula di Aggiustamento:
    $$ P(Y=1 | do(X=1)) = \sum_{z} P(Y=1 | X=1, Z=z) P(Z=z) $$
*   Meccanismo Pratico: Confrontiamo i tassi di assunzione tra partecipanti e non partecipanti, divisi per gruppo di titolo di studio. Questo separa l’effetto formativo del corso dal vantaggio iniziale del titolo di studio.

5. Approccio Risultati Potenziali di Rubin (Bilanciamento Dati):
*   Risultati Potenziali: $Y_i(1)$ = stato di assunzione della persona $i$ se partecipa al corso; $Y_i(0)$ = se non partecipa.
*   Punteggio di Propensione: Stima di $e(z) = P(X=1 | Z=z).$ Laureati hanno una propensione maggiore di accedere al corso.
*   Pesi IPW:
    $$ \hat{ATE} = \frac{1}{N} \sum_{i=1}^{N} \left[ \frac{X_i Y_i}{e(Z_i)} - \frac{(1-X_i) Y_i}{1-e(Z_i)} \right] $$
*   Meccanismo: I pesi bilanciano la distribuzione dei titoli di studio tra i due gruppi. Le rare persone con solo diploma che hanno frequentato il corso vengono sovrappesate, mentre le comuni laureate vengono sottopesate, rendendo il confronto imparziale.



---
Vuoi che generi una simulazione Python completa per uno di questi due scenari, con commenti in italiano per mostrare come calcolare l’ATE con IPW e G-computation?<br><br>百度AI生成，内容仅供参考