---
icon: clock
---

# Manipolazione di Timestamp e Attributi

Oltre alla cancellazione diretta delle prove, esistono tecniche di anti-forensics più sofisticate che mirano ad **alterare** i metadati dei file per confondere l'analisi temporale e nascondere attività recenti.

Queste manipolazioni sono considerate infrazioni gravi perché dimostrano un alto livello di intenzionalità nel tentativo di eludere un controllo. Questa pagina copre le due principali forme di manipolazione dei metadati.

***

#### 1. Timestomping (Manipolazione dei Timestamp)

* **Scopo della Tecnica:** Il Timestomping consiste nel modificare manualmente i timestamp di un file (Creazione, Ultima Modifica, Ultimo Accesso). L'obiettivo è far sembrare un file recente (come un cheat scaricato o eseguito poco fa) molto più vecchio, sperando che venga ignorato durante un'analisi cronologica. Ad esempio, un file eseguito 5 minuti fa potrebbe essere alterato per mostrare come data di ultima modifica l'anno 2015.
* **Metodi di Rilevamento:**
  * **Analisi della MFT:** Il Master File Table (MFT) di NTFS memorizza due set di timestamp per ogni file: `$STANDARD_INFORMATION` ($SI) e `$FILE_NAME` ($FN). Molti tool di timestomping modificano solo i timestamp $SI. Una discrepanza tra i due set, rilevabile con tool come **MFTECmd**, è una prova quasi certa di manipolazione.
  * **USN Journal:** L'atto di modificare un timestamp genera un evento `BASIC_INFO_CHANGE` nel USN Journal. Trovare questo evento recente su un file che mostra timestamp molto vecchi è una prova inconfutabile di timestomping.

***

#### 2. Manipolazione degli Attributi dei File

* **Scopo della Tecnica:** Questa tecnica consiste nel modificare gli attributi di un file (come "Sola Lettura" o "Nascosto") per interferire con i meccanismi di logging di Windows o per nascondere il file stesso.
* **Caso Principale: Attributo "Sola Lettura" (Read-Only) su File Prefetch:**
  * **Meccanismo di Bypass:** Un bypasser può impostare l'attributo "Sola Lettura" su un file di Prefetch (`.pf`) dopo aver eseguito un programma legittimo. Successivamente, esegue un cheat dallo stesso percorso. Poiché il file `.pf` è in sola lettura, il servizio `SysMain` non può aggiornarlo con il nuovo orario di esecuzione (quello del cheat). Durante il controllo, il file Prefetch sembrerà registrare solo l'esecuzione del programma legittimo, nascondendo quella del cheat.
* **Metodi di Rilevamento:**
  * **USN Journal:** La modifica di un attributo di file, come l'impostazione "Sola Lettura", genera un evento `BASIC_INFO_CHANGE` nel USN Journal. Trovare questo evento recente su un file `.pf` è un forte indicatore di tampering.
  * **Verifica Diretta:** È possibile verificare l'attributo direttamente tramite le proprietà del file o da riga di comando (es. `dir /ar C:\Windows\Prefetch`), anche se questo non fornisce una prova temporale come il Journal.

***

#### Sanzione

La manipolazione dei metadati di un file è un'azione deliberata che richiede conoscenze tecniche e l'intento esplicito di ingannare lo staff. Per questo motivo, è considerata un'infrazione grave.

{% hint style="danger" %}
**Motivazione Consigliata:** `Anti-Forensics / Alterazione di Prove`

**SANZIONE:** `Da definire dal server`
{% endhint %}
