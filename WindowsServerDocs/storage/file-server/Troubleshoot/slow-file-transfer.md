---
title: Velocità di trasferimento dei file SMB lenti
description: Viene illustrato come risolvere i problemi relativi alle prestazioni di trasferimento di file SMB.
author: Deland-Han
manager: dcscontentpm
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: af05daa164b5b2c5eca73eff51d97d4c25ba1ca3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80815394"
---
# <a name="slow-smb-files-transfer-speed"></a>Velocità di trasferimento dei file SMB lenti

Questo articolo fornisce le procedure consigliate per la risoluzione dei problemi per velocizzare il trasferimento di file tramite SMB.

## <a name="large-file-transfer-is-slow"></a>Il trasferimento di file di grandi dimensioni è lento

Se si osservano trasferimenti lenti di file di grandi dimensioni, prendere in considerazione i passaggi seguenti:

- Provare il comando copia file per i/o senza buffer (**xcopy/J** o **Robocopy/J**).

- Testare la velocità di archiviazione. Questo perché la velocità di copia dei file è limitata dalla velocità di archiviazione.

- Le copie di file talvolta iniziano rapidamente, quindi rallentano. Per verificare questa situazione, attenersi alle seguenti linee guida:
    
  - Questo problema si verifica in genere quando la copia iniziale viene memorizzata nella cache o memorizzata nel buffer (in memoria o nella cache di memoria del controller RAID) e la cache viene esaurita. In questo modo i dati verranno scritti direttamente su disco (Write-through). Si tratta di un processo più lento.
    
  - Usare i contatori di performance monitor di archiviazione per determinare se le prestazioni di archiviazione diminuiscono nel tempo. Per ulteriori informazioni, vedere [ottimizzazione delle prestazioni per i file server SMB](https://docs.microsoft.com/windows-server/administration/performance-tuning/role/file-server/smb-file-server).

- Utilizzare RAMMap (SysInternals) per determinare se l'utilizzo "file mappato" in memoria si interrompe a causa dell'esaurimento della memoria disponibile.

- Individuare la perdita di pacchetti nella traccia. Ciò può causare la limitazione del provider di congestione TCP.

- Per SMBv3 e versioni successive, verificare che SMB multicanale sia abilitato e funzionante.

- Nel client SMB, abilitare MTU di grandi dimensioni in SMB e disabilitare la limitazione della larghezza di banda. A tale scopo eseguire il comando riportato di seguito:  
  
  ```PowerShell
  Set-SmbClientConfiguration -EnableBandwidthThrottling 0 -EnableLargeMtu 1
  ```

## <a name="small-file-transfer-is-slow"></a>Il trasferimento di file di piccole dimensioni è lento

Il trasferimento lento dei file di piccole dimensioni tramite SMB si verifica in genere in presenza di molti file. Si tratta di un comportamento previsto.

Durante il trasferimento di file, la creazione di file comporta un sovraccarico del protocollo elevato e un sovraccarico file system elevato. Per i trasferimenti di file di grandi dimensioni, questi costi si verificano solo una volta. Quando viene trasferito un numero elevato di file di piccole dimensioni, il costo è ripetitivo e causa trasferimenti lenti.

Di seguito sono riportati i dettagli tecnici relativi a questo problema:

- SMB chiama un comando create per richiedere la creazione del file. Il codice verificherà se il file esiste, quindi creerà il file. O una variante del comando Create crea il file effettivo.

- Ogni comando create genera attività nell'file system.

- Una volta scritti i dati, il file viene chiuso.

- Il processo subisce molto tempo dalla latenza di rete e dalla latenza del server SMB. Ciò è dovuto al fatto che la richiesta SMB viene prima convertita in un file system comando e quindi alla latenza file system effettiva per completare l'operazione.

- Se è in esecuzione un programma antivirus, il trasferimento rallenta ancora di più. Ciò è dovuto al fatto che i dati vengono in genere analizzati una sola volta dal sniffer del pacchetto e una seconda volta quando vengono scritti su disco. In alcuni scenari, queste azioni sono ripetute per migliaia di tempo. Si osservano potenzialmente velocità minori di 1 MB/s.

## <a name="opening-office-documents-is-slow"></a>L'apertura di documenti di Office è lenta

Questo problema si verifica in genere in una connessione WAN. Si tratta di una situazione comune che in genere è causata dal modo in cui le app di Office (in particolare Microsoft Excel) accedono e leggono i dati.

È consigliabile assicurarsi che i file binari di Office e SMB siano aggiornati e quindi eseguire il test con il leasing disabilitato nel server SMB. A tale scopo, effettuare le operazioni seguenti:
   
1. Eseguire il comando di PowerShell seguente in Windows 8 e Windows Server 2012 o versioni successive di Windows:
      
   ```PowerShell
   Set-SmbServerConfiguration -EnableLeasing $false  
   ```
      
   In alternativa, eseguire il comando seguente in una finestra del prompt dei comandi con privilegi elevati:  

   ```cmd
   REG ADD HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\lanmanserver\parameters /v DisableLeasing /t REG\_DWORD /d 1 /f  
   ```
      
   > [!NOTE]
   > Dopo aver impostato questa chiave del registro di sistema, i lease SMB2 non vengono più concessi, ma oplock sono ancora disponibili. Questa impostazione viene utilizzata principalmente per la risoluzione dei problemi.
    
2. Riavviare il file server o riavviare il servizio **Server** . Per riavviare il servizio, eseguire i comandi seguenti:

   ```cmd  
   NET STOP SERVER 
   NET START SERVER
   ```

Per evitare questo problema, è anche possibile replicare il file in un file server locale. Per altre informazioni, vedere [vendo Office Documents to a Network Server is Slow when using EFS](https://docs.microsoft.com/office/troubleshoot/office/saving-file-to-network-server-slow).
