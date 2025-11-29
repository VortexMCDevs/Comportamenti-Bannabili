---
icon: eraser
---

# Disabilitazione di Servizi Critici

Uno dei metodi di bypass più comuni consiste nel disabilitare o fermare intenzionalmente alcuni servizi di Windows. Questi servizi sono fondamentali per l'analisi forense perché registrano le attività del sistema e dei programmi. La loro disattivazione è considerata un'azione di **anti-forensics**, in quanto mira a nascondere le tracce e a ostacolare il controllo.

In questa pagina sono elencati i servizi critici che dovrebbero essere monitorati durante uno screenshare. La loro disattivazione o il loro stato "arrestato" costituisce un'infrazione.

***

#### Come Verificare lo Stato di un Servizio

La procedura standard, ma non la migliore, per verificare lo stato di un servizio è utilizzare il Prompt dei Comandi (CMD) con privilegi di amministratore.

1. Apri il menu Start, cerca `cmd`, clicca con il tasto destro su "Prompt dei comandi" e seleziona "Esegui come amministratore".
2.  Digita il comando seguente, sostituendo `[nome_servizio]` con il nome del servizio da controllare (es. `sysmain`).

    ```bash
    sc query [nome_servizio]
    ```
3. Analizza il campo `STATE`. Lo stato normale è `RUNNING`. Uno stato `STOPPED` è un forte indicatore di manipolazione.

***

#### Servizi Critici Monitorati

**1. SysMain (Superfetch)**

* **Scopo del Servizio:** Il servizio `SysMain` è responsabile della gestione della funzionalità **Prefetch**. Senza questo servizio attivo, Windows non crea né aggiorna i file `.pf`, che sono una delle prove principali per tracciare l'esecuzione dei programmi.
* **Nome Servizio:** `sysmain`
* **Comando di Verifica:** `sc query sysmain`

**2. DPS (Diagnostic Policy Service)**

* **Scopo del Servizio:** Il `DPS` è responsabile della diagnostica di sistema, ma in ambito forense è noto per registrare informazioni sull'esecuzione dei processi, inclusi timestamp di compilazione che possono aiutare a identificare file sostituiti (file replacement).
* **Nome Servizio:** `dps`
* **Comando di Verifica:** `sc query dps`

**3. EventLog (Windows Event Log)**

* **Scopo del Servizio:** È il servizio principale che gestisce la registrazione di tutti gli eventi di sistema nei **Log degli Eventi** (Event Viewer). Se questo servizio è fermo, azioni critiche come la cancellazione del Journal o i cambi di orario non verranno registrate.
* **Nome Servizio:** `eventlog`
* **Comando di Verifica:** `sc query eventlog`

**4. PcaSvc (Program Compatibility Assistant Service)**

* **Scopo del Servizio:** Il `PcaSvc` aiuta a garantire la compatibilità delle applicazioni, ma registra anche informazioni utili sull'esecuzione dei programmi che possono corroborare le prove trovate in altri artefatti.
* **Nome Servizio:** `PcaSvc`
* **Comando di Verifica:** `sc query PcaSvc`

**5. PlugPlay (Plug and Play)**

* **Scopo del Servizio:** Gestisce il riconoscimento dei dispositivi hardware, ma è stato osservato che la sua memoria contiene spesso tracce relative all'esecuzione di file `.jar` (Java). La sua disattivazione può nascondere l'uso di certi client basati su Java.
* **Nome Servizio:** `PlugPlay`
* **Comando di Verifica:** `sc query PlugPlay`

**6. BAM (Background Activity Moderator)**

* **Scopo del Servizio:** Il `BAM` monitora l'attività delle applicazioni in background. Il suo log nel Registro di Sistema è una prova di esecuzione estremamente affidabile, che registra percorsi e orari di attività. La sua disattivazione impedisce la registrazione di queste tracce.
* **Nome Servizio:** `bam`
* **Comando di Verifica:** `sc query bam`

***

#### Sanzione

La disabilitazione di uno o più dei servizi sopra elencati è considerata un tentativo di ostacolare l'indagine e nascondere le prove.

{% hint style="danger" %}
**Motivazione Consigliata:** `Anti-Forensics / Ostruzione del Controllo`

**SANZIONE:** `30 Days`              &#x20;
{% endhint %}

{% hint style="warning" %}
**Nota sugli "Optimizer"**

Se un player sostiene che un servizio è stato disabilitato da un software di ottimizzazione ("optimizer") o per motivi di performance, la responsabilità ricade comunque su di lui. In un ambiente di gioco competitivo, è dovere del player assicurarsi che il proprio sistema sia configurato in modo da non interferire con le procedure di controllo standard.
{% endhint %}
