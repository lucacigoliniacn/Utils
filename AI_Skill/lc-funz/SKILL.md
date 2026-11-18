---
name: lc-funz
description: Linee guida ed esempi funzionali/tecnici per il modulo SAP SD e per la verifica della completezza dei requisiti di sviluppo SAP.
---

# LC Funz — SAP SD Functional Guidelines & Requirements Verification

Linee guida funzionali e requisiti per l'analisi e lo sviluppo sul modulo SAP SD e per la verifica preliminare delle richieste di sviluppo.

---

## 1. Verifiche Preliminari sui Requisiti di Sviluppo (Backend & Frontend)

Quando ricevi dei requisiti per un nuovo sviluppo in SAP (sia backend ABAP/CDS/OData che frontend Fiori/UI5):

1. **Checklist Informazioni Obbligatorie:**
   Verifica sempre la presenza delle seguenti informazioni essenziali nella richiesta dell'utente:
   - **Progetto:** Identificativo o nome del progetto di riferimento.
   - **Categoria:** Tipologia di attività (es. Bug, Change Request, Nuova funzionalità, ecc.).
   - **Priorità:** Livello di urgenza/priorità dell'attività.
   - **Data prevista:** Data / scadenza concordata per la consegna.
   - **Sistema su cui fare l'attività:** Ambiente o sistema SAP di destinazione (es. DEV, QAS, client/mandante).
   - **CD, TR o CR:** Riferimento a Change Document, Transport Request (Richiesta di Trasporto) o Change Request.
   - **Caso di test:** Dettagli, passaggi o dati per eseguire la verifica ed il collaudo del requisito.

2. **Verifica della Completezza Tecnica:**
   - Controlla che le informazioni fornite siano sufficienti per procedere con lo sviluppo sia lato **backend** che **frontend**.

3. **Gestione dei Dubbi e Requisiti Incompleti:**
   - Se manca anche solo una delle informazioni obbligatorie sopra indicate o se i requisiti tecnici risultano incompleti, ambigui o dubbi, **NON inventare nulla**.
   - Poni domande chiare e mirate all'utente per richiedere le informazioni mancanti e chiarire ogni punto prima di iniziare.

4. **Verifica della Skill `lc-dev`:**
   - Come passaggio finale prima di procedere con la fase di codice/sviluppo, verifica se esiste la skill `lc-dev`.
   - Se la skill `lc-dev` esiste, chiedi all'utente se desidera procedere con lo sviluppo del codice utilizzando le linee guida e convenzioni della skill `lc-dev`.

---

## 2. Processi e Cicli di Vendita SAP SD

- **Ciclo di Vendita Standard:** Offerta/Preventivo -> Ordine di Vendita (`VA01`) -> Consegna/Delivery (`VL01N`) -> Modifica Consegna/PGI (`VL02N`) -> Fattura (`VF01`).
- **Tipi Documento di Vendita:**
  - `OR`: Ordine Standard
  - `RE`: Reso da cliente
  - `CR`: Richiesta Nota di Credito
  - `DR`: Richiesta Nota di Debito
  - `FD`: Consegna gratuita

---

## 3. Struttura Tabelle e Flusso Dati SD

- **Testata Documento di Vendita:** `VBAK`
- **Posizioni Documento di Vendita:** `VBAP`
- **Schedulazione / Scheduline:** `VBEP`
- **Dati Partner:** `VBPA`
- **Flusso Documenti (Status/Collegamento):** `VBFA`
- **Testata Consegna:** `LIKP`
- **Posizione Consegna:** `LIPS`
- **Testata Fattura:** `VBRK`
- **Posizione Fattura:** `VBRP`

---

## 4. Logiche Funzionali e Verifiche

- **Determinazione Prezzi (PR00, ecc.):** Verificare sempre schema di calcolo e condizione prezzo attiva.
- **Controllo Credito e Blocchi:** Verificare campi blocco in `VBAK` (`LIFSK`, `FAKSK`) prima dell'elaborazione di consegne o fatture.
- **Conversioni Unità di Misura e Unità di Vendita:** Assicurarsi che le quantità siano coerenti con le UoM di vendita e UoM base (`MEINS` / `VRKME`).
