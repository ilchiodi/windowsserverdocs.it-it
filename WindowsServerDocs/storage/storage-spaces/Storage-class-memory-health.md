---
ms.assetid: 2bab6bf6-90e7-46a7-b917-14a7a8f55366
title: Gestione dell'integrità della memoria della classe di archiviazione (NVDIMM-N) in Windows
ms.prod: windows-server-threshold
ms.author: jgerend
ms.manager: dongill
ms.technology: storage-spaces
ms.topic: article
author: JasonGerend
ms.date: 08/24/2016
ms.localizationpriority: medium
ms.openlocfilehash: 0c39d704056c4ae6935f3be9c521c12ca1014820
ms.sourcegitcommit: 9db0d8b328a286b9a40fe3ab1890703ab025a0f3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/18/2018
ms.locfileid: "5014064"
---
# Gestione dell'integrità della memoria della classe di archiviazione (NVDIMM-N) in Windows
> Si applica a: Windows Server 2016, Windows 10 (versione 1607)

Questo articolo fornisce agli amministratori di sistema e ai professionisti IT le informazioni sulla gestione degli errori e dell'integrità specifica dei dispositivi di memoria della classe di archiviazione (NVDIMM-N) in Windows, illustrando le differenze tra la memoria della classe di archiviazione e i dispositivi di archiviazione tradizionale.

Se non si ha familiarità con il supporto di Windows per i dispositivi di memoria della classe di archiviazione, questi brevi video offrono tutte le informazioni generali utili:
- [Using Non-volatile Memory (NVDIMM-N) as Block Storage in Windows Server 2016 (Usare la memoria non volatile (NVDIMM-N) come archiviazione a blocchi in Windows Server 2016)](https://channel9.msdn.com/Events/Build/2016/P466)
- [Using Non-volatile Memory (NVDIMM-N) as Byte-Addressable Storage in Windows Server 2016 (Usare la memoria non volatile (NVDIMM-N) come archiviazione indirizzabile tramite byte in Windows Server 2016)](https://channel9.msdn.com/Events/Build/2016/P470)
- [Accelerazione delle prestazioni di SQL Server 2016 con memoria persistente in Windows Server 2016](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-Windows-Server-2016-SCM--FAST)

I dispositivi di memoria della classe di archiviazione NVDIMM-N conformi a JEDEC sono supportati in Windows con driver nativi, a partire da Windows Server 2016 e Windows 10 (versione 1607). Sebbene questi dispositivi abbiano un comportamento simile ad altri dischi (dischi rigidi e unità SSD), esistono alcune differenze.

Tutte le condizioni elencate di seguito devono essere occorrenze molto rare, ma dipendono dalle condizioni in cui viene usato l'hardware.

I vari casi riportati di seguito possono fare riferimento alle configurazioni di Spazi di archiviazione. Nella configurazione di interesse specifica vengono utilizzati due dispositivi NVDIMM-N come una cache write-back con mirroring in uno spazio di archiviazione. Per impostare questa configurazione, vedere [Configurazione di Spazi di archiviazione con una cache write-back NVDIMM-N](https://msdn.microsoft.com/library/mt650885.aspx).

In Windows Server 2016, l'interfaccia utente grafica di Spazi di archiviazione mostra il tipo di bus NVDIMM-N come SCONOSCIUTO. Non vi sono perdite di funzionalità o problemi di creazione di desktop virtuali pool, archiviazione. Per verificare il tipo di bus, è possibile eseguire il comando seguente:

```powershell
PS C:\>Get-PhysicalDisk | fl
```
Il parametro BusType nell'output del cmdlet mostrerà correttamente il tipo di bus come "SCM"

## Verifica dell'integrità della memoria della classe di archiviazione
Per eseguire una query sull'integrità della memoria della classe di archiviazione, usare i comandi seguenti in una sessione di Windows PowerShell.

```powershell
PS C:\> Get-PhysicalDisk | where BusType -eq "SCM" | select SerialNumber, HealthStatus, OperationalStatus, OperationalDetails
```

In questo modo viene restituito questo output di esempio:

|SerialNumber|HealthStatus|OperationalStatus|OperationalDetails|
|---|---|---|---|
|802c-01-1602-117cb5fc|Healthy|OK||
|802c-01-1602-117cb64f|Avviso|Errore prevedibile|{Soglia superata, errore NVDIMM\_N}|

> [!NOTE]
> Per trovare la posizione fisica di un dispositivo NVDIMM-N specificato in un evento, nella scheda **Dettagli** dell'evento nel Visualizzatore eventi andare a **EventData** > **Posizione**. In Windows Server 2016 è indicata la posizione errata dei dispositivi NVDIMM-N, ma questo problema è stato corretto in Windows Server, versione 1709.

Per comprendere le diverse condizioni di integrità, vedere le sezioni seguenti.

## Stato di integrità "avviso"

Questa condizione si verifica quando si controlla l'integrità di un dispositivo di memoria della classe di archiviazione e si nota che lo stato di integrità è elencato come **Avviso**, come illustrato in questo output di esempio:

|SerialNumber|HealthStatus|OperationalStatus|OperationalDetails|
|---|---|---|---|
|802c-01-1602-117cb5fc|Healthy|OK||
|802c-01-1602-117cb64f|Avviso|Errore prevedibile|{Soglia superata, errore NVDIMM\_N}|

Nella tabella seguente sono elencate alcune informazioni su questa condizione.

||Descrizione|
|---|---|
|Condizione probabile|Soglia di avviso NVDIMM-N violata|
|Causa principale|I dispositivi NVDIMM-N tengono traccia di diversi valori di soglia, ad esempio la temperatura, la durata NVM e/o la durata della risorsa di energia. Quando uno di questi valori soglia viene superato, il sistema operativo viene notificato.|
|Comportamento generale|Il dispositivo rimane completamente operativo. Si tratta di un avviso, non di un errore.|
|Comportamento di Spazi di archiviazione|Il dispositivo rimane completamente operativo. Si tratta di un avviso, non di un errore.|
|Altre informazioni|Campo OperationalStatus dell'oggetto PhysicalDisk. EventLog – Microsoft-Windows-ScmDisk0101/Operational|
|Operazione da eseguire|A seconda della soglia di avviso violata, potrebbe essere opportuno prendere in considerazione la sostituzione di tutta la memoria NVDIMM-N o di una parte di essa. Ad esempio, se viene violata la soglia di durata NVM, la sostituzione di NVDIMM-N potrebbe essere una buona soluzione.|

## Errore durante la scrittura in una memoria NVDIMM-N

Questa condizione si verifica quando si controlla l'integrità di un dispositivo di memoria della classe di archiviazione e si nota che lo stato di integrità è elencato come **Danneggiato** mentre lo stato operativo come **Errore I/O**, come illustrato in questo output di esempio:

|SerialNumber|HealthStatus|OperationalStatus|OperationalDetails|
|---|---|---|---|
|802c-01-1602-117cb5fc|Healthy|OK||
|802c-01-1602-117cb64f|Unhealthy|{Metadati obsoleti, errore I/O, errore temporaneo}|{Perdita della persistenza dei dati, perdita di dati, NV...}|

Nella tabella seguente sono elencate alcune informazioni su questa condizione.

||Descrizione|
|---|---|
|Condizione probabile|Perdita di persistenza/Alimentazione di backup|
|Causa principale|I dispositivi NVDIMM-N si basano su una fonte di alimentazione di backup per la persistenza, in genere una batteria o un supercondensatore. Se questa fonte di alimentazione di backup non è disponibile o il dispositivo non può eseguire un backup per qualsiasi motivo (errore controller/flash), i dati sono a rischio e Windows impedisce ulteriori scritture sui dispositivi interessati. Le operazioni di lettura sono comunque possibile per spostare i dati.|
|Comportamento generale|Verrà smontato il volume NTFS.<br>Nel campo dello stato di integrità PhysicalDisk verrà visualizzato lo stato "Danneggiato" per tutti i dispositivi NVDIMM-N interessati.|
|Comportamento di Spazi di archiviazione|Se è interessata una solo memoria NVDIMM-N, Spazi di archiviazione rimarrà operativo. Se sono interessati più dispositivi, le scritture sullo spazio di archiviazione avranno esito negativo. <br>Nel campo dello stato di integrità PhysicalDisk verrà visualizzato lo stato "Danneggiato" per tutti i dispositivi NVDIMM-N interessati.|
|Altre info|Campo OperationalStatus dell'oggetto PhysicalDisk.<br>EventLog – Microsoft-Windows-ScmDisk0101/Operational|
|Operazione da eseguire|Si consiglia di eseguire un backup dei dati della memoria NVDIMM-N interessata. Per ottenere l'accesso in lettura, è possibile portare online il disco manualmente (verrà visualizzato come un volume NTFS in sola lettura).<br><br>Per cancellare completamente questa condizione, deve essere risolta la causa principale (ad esempio, utilizzo di un alimentatore di servizio o sostituzione di NVDIMM-N, a seconda del problema) e il volume in NVDIMM-N deve essere portato offline e riportato online, oppure il sistema deve essere riavviato.<br><br>Per rendere NVDIMM-N di nuovo utilizzabile in Spazi di archiviazione, utilizzare il cmdlet **Reset-PhysicalDisk**, che integra nuovamente il dispositivo e avvia il processo di ripristino.|

## NVDIMM-N viene visualizzato con una capacità di "0" byte o come un "disco fisico generico"

Questa condizione si verifica quando un dispositivo di memoria della classe di archiviazione viene visualizzato con una capacità di 0 byte e non può essere inizializzato, oppure viene esposto come un oggetto "Disco fisico generico" con lo stato operativo **Comunicazione perduta**, come illustrato in questo output di esempio:

|SerialNumber|HealthStatus|OperationalStatus|OperationalDetails|
|---|---|---|---|
|802c-01-1602-117cb5fc|Healthy|OK||
||Avviso|Comunicazione perduta||

Nella tabella seguente sono elencate alcune informazioni su questa condizione.

||Descrizione|
|---|---|
|Condizione probabile|BIOS non ha esposto NVDIMM-N al sistema operativo|
|Causa principale|I dispositivi NVDIMM-N sono basati su DRAM. Quando viene fatto riferimento a un indirizzo DRAM danneggiato, la maggior parte delle CPU avvia un controllo del computer e riavvia il server. Successivamente, alcune piattaforme server annullano il mapping di NVDIMM, impedendo al sistema operativo di accedervi e causare un altro controllo potenziale del computer. Ciò può verificarsi anche se il BIOS rileva che si è verificato un errore nella memoria NVDIMM-N e deve essere sostituita.|
|Comportamento generale|NVDIMM-N viene visualizzato come non inizializzato, con una capacità di 0 byte e non può essere letto o scritto.|
|Comportamento di Spazi di archiviazione|Lo spazio di archiviazione resta operativo (purché sia interessato solo 1 NVDIMM-N).<br>L'oggetto NVDIMM-N PhysicalDisk viene visualizzato con un stato di integrità di avviso e come un "Disco fisico generico"|
|Altre informazioni|Campo OperationalStatus dell'oggetto PhysicalDisk. <br>EventLog – Microsoft-Windows-ScmDisk0101/Operational|
|Operazione da eseguire|Il dispositivo NVDIMM-N deve essere sostituito o ripulito in modo tale che la piattaforma del server lo esponga nuovamente al sistema operativo host. È consigliabile sostituire il dispositivo poiché potrebbero verificarsi altri errori non correggibili. È possibile aggiungere un dispositivo di sostituzione a una configurazione di Spazi di archiviazione con il cmdlet **Add-Physicaldisk**.|

## NVDIMM-N viene visualizzato come un disco RAW o vuoto dopo un riavvio

Questa condizione si verifica quando si controlla l'integrità di un dispositivo di memoria della classe di archiviazione e si nota che lo stato di integrità è **Danneggiato** e lo stato operativo è **Metadati non riconosciuti**, come illustrato in questo output di esempio:

|SerialNumber|HealthStatus|OperationalStatus|OperationalDetails|
|---|---|---|---|
|802c-01-1602-117cb5fc|Healthy|OK|{Sconosciuto}|
|802c-01-1602-117cb64f|Unhealthy|{Metadati non riconosciuti, metadati obsoleti}|{Sconosciuto}|

Nella tabella seguente sono elencate alcune informazioni su questa condizione.

||Descrizione|
|---|---|
|Condizione probabile|Errore di backup/ripristino|
|Causa principale|Un errore di una procedura di backup o ripristino determinerà probabilmente la perdita di tutti i dati del dispositivo NVDIMM-N. Quando viene caricato il sistema operativo, verrà visualizzato come un nuovo dispositivo NVDIMM-N senza una partizione o file system e come RAW, ovvero che non dispone di un file system.|
|Comportamento generale|Il dispositivo NVDIMM-N sarà in modalità di sola lettura. Sarà necessaria un'azione esplicita dell'utente per iniziare a usarlo nuovamente.|
|Comportamento di Spazi di archiviazione|Spazi di archiviazione rimane operativo se è interessato un solo dispositivo NVDIMM.<br>L'oggetto disco fisico NVDIMM-N viene visualizzato con lo stato di integrità "Danneggiato" e non sarà usato da Spazi di archiviazione.|
|Altre info|Campo OperationalStatus dell'oggetto PhysicalDisk.<br>EventLog – Microsoft-Windows-ScmDisk0101/Operational|
|Operazione da eseguire|Se l'utente non desidera sostituire il dispositivo interessato, è possibile usare il cmdlet **Reset-PhysicalDisk** per cancellare la condizione di sola lettura in nel dispositivo NVDIMM-N interessato. Negli ambienti di Spazi di archiviazione si tenterà di reintegrare NVDIMM-N nello spazio di archiviazione e di avviare il processo di ripristino.|

## Set interfogliati

Spesso, i set interfogliati possono essere creati nel BIOS della piattaforma per fare in modo che più dispositivi NVDIMM-N vengano visualizzati come un unico dispositivo per il sistema operativo host.

Windows Server 2016 e Windows 10 Anniversary Edition non supportano i set interfogliati dei dispositivi NVDIMM-N.

Al momento della stesura di questo articolo, non esiste alcun meccanismo che permette al sistema operativo host di identificare correttamente i dispositivi NVDIMM-N singoli in un set di questo tipo e di comunicare chiaramente all'utente quale dispositivo potrebbe aver causato un errore o deve essere sottoposto a manutenzione.


