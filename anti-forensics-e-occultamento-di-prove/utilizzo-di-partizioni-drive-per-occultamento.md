---
icon: usb-drive
---

# Utilizzo di Partizioni/Drive per Occultamento

Un'altra categoria di tecniche di bypass consiste nello sfruttare drive esterni, partizioni del disco o volumi cifrati per isolare e nascondere file e attività sospette. L'obiettivo è spostare le prove al di fuori del raggio d'azione di un'analisi standard, che spesso si concentra sul drive principale del sistema operativo (C:).

Queste azioni sono considerate infrazioni perché rappresentano un tentativo deliberato di rendere inaccessibili o difficilmente rintracciabili le prove di cheating.

***

#### 1. Esecuzione da Drive Esterni (USB, Hard Disk Esterni)

* **Scopo della Tecnica:** Il player memorizza ed esegue i cheat direttamente da un dispositivo di archiviazione USB. Spesso, il dispositivo viene scollegato subito prima del controllo per rimuovere fisicamente le prove.
* **Il Ruolo del File System (FAT32/exFAT vs. NTFS):**
  * **FAT32/exFAT:** Questi file system, molto comuni sulle chiavette USB, **non supportano il journaling (USN Journal)**. La cancellazione di un cheat da un drive FAT32 lascia molte meno tracce ed è più difficile da provare.
  * **NTFS:** Se il drive esterno è formattato in NTFS, avrà il suo USN Journal, che può essere analizzato per tracciare le attività.

{% hint style="info" %}
**Una Regola Comune dei Server**

La semplice connessione di un drive FAT32/exFAT non è un'infrazione. Tuttavia, molti server competitivi considerano **sanzionabile l'esecuzione di qualsiasi file da un volume FAT32 o exFAT** durante la sessione di gioco.

**Motivazione:** L'analisi forense su file system non-journaled è estremamente complessa e lunga. Poiché non esistono motivi pratici legittimi per eseguire file da tali volumi durante una sessione di gioco, questa regola serve a semplificare il lavoro dello staff e a disincentivare attivamente l'uso di questa nota tecnica di bypass.
{% endhint %}

* **Metodi di Rilevamento:**
  * **Cronologia USB:** Utilizzare tool come **USBDeview** per controllare i dispositivi USB connessi di recente e verificare l'orario di ultima disconnessione.
  * **Event Viewer:** Controllare i log `Kernel-PnP` per eventi di connessione/disconnessione di dispositivi.
  * **Prefetch/BAM:** Verificare la presenza di esecuzioni da percorsi corrispondenti a drive esterni (es. `E:\cheat.exe`).

***

#### 2. Creazione e Cancellazione di Partizioni

* **Scopo della Tecnica:** Il player crea una nuova partizione sul proprio hard disk, la utilizza per i cheat, e poi la cancella prima del controllo, rimuovendo l'intera struttura del file system di quella partizione.
* **Metodi di Rilevamento:**
  * **Event Viewer:** Analizzare i log `Applicazioni e Servizi > Microsoft > Windows > VolumeSnapshot-Driver > Operational` per gli **Event ID 116 (creazione)** o **117 (cancellazione)** di una partizione.
  * **Gestione Disco:** Aprire `diskmgmt.msc` e cercare la presenza di "spazio non allocato" sul disco, che potrebbe indicare una partizione eliminata di recente.

***

#### 3. Utilizzo di Volumi Cifrati o Nascosti

* **Scopo della Tecnica:** Il player utilizza software di crittografia (come **VeraCrypt** o **BitLocker**) per creare un "contenitore" cifrato in cui nascondere i file. Senza la password, il contenuto è inaccessibile.
* **Metodi di Rilevamento:**
  * **Tool Specifici:** Utilizzare software come **Magnet EDD (Encrypted Disk Detector)** per scansionare il sistema alla ricerca di volumi cifrati noti.
  * **Processi e Servizi:** Cercare in **System Informer** processi o servizi attivi legati a software di crittografia (es. `VeraCrypt.exe`).
  * **Gestione Disco:** Volumi cifrati montati ma bloccati potrebbero apparire in `diskmgmt.msc` come partizioni "RAW" o non formattate.

***

#### Sanzione

L'utilizzo di queste tecniche per nascondere o rendere inaccessibili le prove è considerato un'infrazione grave, poiché dimostra un chiaro intento di eludere il controllo.

{% hint style="danger" %}
**Motivazione Consigliata:** `Anti-Forensics / Occultamento di Prove`

**SANZIONE:** `Da definire dal server`
{% endhint %}

{% hint style="warning" %}
**Nota sui Volumi Cifrati**

Se durante un controllo viene scoperto un volume cifrato montato e il player si rifiuta di fornire la password per l'analisi, tale rifiuto è generalmente considerato equivalente a un'interruzione del controllo o a un'ammissione di occultamento di prove, e sanzionato di conseguenza.
{% endhint %}
