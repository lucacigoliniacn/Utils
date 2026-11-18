---
name: lc-skill
description: Gestisce la sincronizzazione e l'aggiornamento delle skill leggendole da GitHub (https://github.com/lucacigoliniacn/Utils.git) o tramite l'archivio ZIP caricato dall'utente. Si attiva quando l'utente chiede di aggiornare, modificare, caricare, sincronizzare o scaricare le skill, sia in italiano, sia in inglese.
---
# Skill: lc-skill

## Obiettivo

Sincronizzare le skill locali con la cartella **`AI_Skill`** del repository GitHub `https://github.com/lucacigoliniacn/Utils.git`.
Le skill vanno installate nel percorso di default, se non lo conosci chiedilo all'utente.

## Procedura

1.  **Tentativo primario (GitHub):** ispeziona la cartella `AI_Skill` del repository.
2.  **Fallback:** se il repository non è accessibile o leggibile direttamente, mostra il link del repository all'utente e chiedigli di scaricare lo ZIP e caricarlo in chat.
3.  **Analisi (senza modificare nulla):** confronta le skill del repository (o dello ZIP decompresso) con quelle locali:
    * individua le skill mancanti da installare;
    * individua le skill esistenti modificate da aggiornare. Per capire se una skill è stata modificata, confronta il contenuto dei file (non date o dimensioni);
    * individua eventuali skill da rimuovere.
4.  **Recap e conferma (obbligatorio):** PRIMA di applicare qualsiasi modifica, mostra all'utente un recap chiaro delle operazioni che intendi eseguire (skill da installare, da aggiornare, da rimuovere, con percorso di destinazione). Poi chiedi esplicitamente conferma e **attendi la risposta**. Non installare, aggiornare o rimuovere nulla senza un'esplicita approvazione. Se l'utente rifiuta o chiede cambiamenti, adatta il recap e richiedi conferma. Se non ci sono differenze, comunicalo e termina.
5.  **Sincronizzazione:** solo dopo la conferma, esegui le operazioni approvate (installa le mancanti, aggiorna le modificate, rimuovi quelle approvate).
6.  **Pulizia (obbligatoria):** al termine, anche se non ci sono differenze o l'utente rifiuta le modifiche, elimina tutte le cartelle e i file temporanei creati durante la procedura (clone del repository, ZIP scaricato/caricato, cartelle di estrazione, file di appoggio). Verifica che siano stati rimossi e non toccare le skill installate.
7.  **Report finale** delle operazioni eseguite (installate, aggiornate, rimosse) e conferma dell'avvenuta pulizia dei file temporanei.
