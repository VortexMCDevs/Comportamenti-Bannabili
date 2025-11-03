---
icon: trash-can
---

# Cancellazione di Artefatti Forensi

La cancellazione di artefatti è una delle azioni più gravi che un player possa compiere prima o durante uno screenshare. Questa pratica consiste nell'eliminare deliberatamente i file e i log che il sistema operativo Windows crea per registrare le attività.

***

#### Artefatti Monitorati e Metodi di Rilevamento

**1. Cancellazione dei File Prefetch**

* **Scopo dell'Artefatto:** I file di Prefetch (`.pf`) in `C:\Windows\Prefetch` sono la prova principale dell'esecuzione di un programma. La loro cancellazione mira a nascondere quali eseguibili sono stati avviati di recente.
* **Metodi di Rilevamento:**
  * **USN Journal:** La cancellazione di ogni singolo file `.pf` viene registrata nel USN Journal con un evento `FILE_DELETE`. L'analisi del Journal con strumenti come **JournalTrace** mostrerà una lista di file `.pf` eliminati di recente.

**2. Cancellazione del USN Journal**

* **Scopo dell'Artefatto:** Il USN Journal (`$UsnJrnl`) è il "diario" del file system; la sua cancellazione è un tentativo drastico di nascondere tutte le operazioni sui file (creazioni, cancellazioni, rinomine).

**3. Svuotamento del Cestino (Post-Freeze)**

* **Scopo dell'Azione:** Svuotare il Cestino dopo l'avvio del controllo è un tentativo di eliminare i file sospetti che erano stati cancellati in modo non definitivo.

**4. Cancellazione dei Log di Eventi (Event Viewer)**

* **Scopo dell'Azione:** I player possono tentare di cancellare i log degli eventi per nascondere prove specifiche, come la disabilitazione di servizi o i cambi di orario di sistema.

***

#### Sanzione

La cancellazione di uno o più degli artefatti forensi sopra elencati è considerata una grave infrazione, in quanto dimostra l'intenzione di nascondere attività illecite e di ostacolare il lavoro dello staff.

{% hint style="danger" %}
**Motivazione:** `TODO`

**SANZIONE:** `TODO`
{% endhint %}

> **Nota:** La presenza di software di pulizia automatica (es. CCleaner) non è una giustificazione. È responsabilità del player assicurarsi che tali programmi non vengano eseguiti durante la sessione attuale (dall'avvio del pc), in quanto la loro azione equivale a una eliminazione manuale delle prove.
