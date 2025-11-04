---
icon: trash-can
---

# Cancellazione di Artefatti Forensi

La cancellazione di artefatti è una delle azioni più gravi che un player possa compiere prima di uno screenshare. Questa pratica consiste nell'eliminare deliberatamente i file e i log che il sistema operativo Windows crea per registrare le attività, nel chiaro tentativo di nascondere le proprie tracce.

***

#### Artefatti Monitorati e Metodi di Rilevamento

**1. Cancellazione dei File Prefetch**

* **Scopo dell'Artefatto:** I file di Prefetch (`.pf`) in `C:\Windows\Prefetch` sono la prova principale dell'esecuzione di un programma. La loro cancellazione mira a nascondere quali eseguibili sono stati avviati di recente.
* **Metodi di Rilevamento:**
  * **USN Journal:** La cancellazione di ogni singolo file `.pf` viene registrata nel USN Journal con un evento `FILE_DELETE`. L'analisi del Journal con strumenti come **JournalTrace** o **Echo Journal Viewer** mostrerà una lista di file `.pf` eliminati di recente.

**2. Cancellazione del USN Journal**

* **Scopo dell'Artefatto:** Il USN Journal (`$UsnJrnl`) è il "diario" del file system; la sua cancellazione è un tentativo drastico di nascondere tutte le operazioni sui file (creazioni, cancellazioni, rinomine).
* **Metodi di Rilevamento:**
  * **Event Viewer:** La cancellazione del Journal genera l'**Event ID 3079** nel log "Applicazione".
  * **Metadati del Journal:** Strumenti come **JournalTrace** mostreranno un "Oldest Entry" molto recente. L'analisi con **FTK Imager** dei file `$J` e `$MAX` in `C:\$Extend\$UsnJrnl` rivelerà timestamp di modifica recenti, coincidenti con la cancellazione.

**3. Svuotamento del Cestino (Post-Freeze)**

* **Scopo dell'Azione:** Svuotare il Cestino dopo l'avvio del controllo è un tentativo di eliminare permanentemente i file sospetti che erano stati cancellati in modo non definitivo.
* **Metodi di Rilevamento:**
  * **Timestamp della Cartella:** L'azione di svuotare il Cestino aggiorna il timestamp "Data modifica" della cartella `C:\$Recycle.bin`. Una data di modifica molto recente è un forte indicatore di questa azione.

**4. Cancellazione dei Log di Eventi (Event Viewer)**

* **Scopo dell'Azione:** I player possono tentare di cancellare i log degli eventi per nascondere prove specifiche, come la disabilitazione di servizi o i cambi di orario di sistema.
* **Metodi di Rilevamento:**
  * Windows registra la cancellazione dei log stessi. La cancellazione del log "Sicurezza" genera l'**Event ID 1102** (nel log Sicurezza stesso). La cancellazione di altri log (come "Applicazione" o "Sistema") genera l'**Event ID 104** (nel log Sistema).

***

#### Sanzione

La cancellazione di uno o più degli artefatti forensi sopra elencati è considerata una grave infrazione, in quanto dimostra l'intenzione di nascondere attività illecite e di ostacolare il lavoro dello staff.

{% hint style="danger" %}
**Motivazione Consigliata:** `Anti-Forensics / Distruzione di Prove`

**SANZIONE:** `Da definire dal server`
{% endhint %}

{% hint style="warning" %}
**Nota sui Software di Pulizia (Cleaner)**

La presenza di software di pulizia automatica (es. CCleaner, BleachBit) non è una giustificazione. È responsabilità del player assicurarsi che tali programmi non vengano eseguiti durante la sessione di gioco soggetta a controllo (dall'avvio del PC), in quanto la loro azione equivale a una eliminazione manuale delle prove.
{% endhint %}
