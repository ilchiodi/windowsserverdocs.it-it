---
title: Domande frequenti (FAQ) il servizio di migrazione archiviazione
description: Domande frequenti sul servizio di migrazione di archiviazione, ad esempio i file che sono esclusi dal trasferimento durante la migrazione da un server a un altro.
author: nedpyle
ms.author: nedpyle
manager: siroy
ms.date: 11/06/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage
ms.openlocfilehash: df03f722b7b36a163693f675a2eaade2fabeb82f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860912"
---
# <a name="storage-migration-service-frequently-asked-questions-faq"></a>Domande frequenti (FAQ) il servizio di migrazione archiviazione

In questo argomento è fornite le risposte alle domande frequenti (FAQ) sull'uso [servizio di migrazione archiviazione](overview.md) la migrazione di server.

## <a name="excluded-files"></a> Quali file e cartelle vengono esclusi dal trasferimento?

Il servizio di migrazione di archiviazione non trasferire file o cartelle che sappiamo che potrebbero interferire con l'operazione di Windows. In particolare, ecco cosa abbiamo non trasferire o spostare nella cartella PreExistingData nella destinazione:

- Windows, file, i file di programma (x86), i dati del programma, gli utenti di programma
- $Recycle.bin, recycler, Recycled, System Volume Information, $UpgDrv$, $SysReset, $Windows. ~ BT, $Windows. ~ LS, Windows. old, avvio, ripristino, documenti e impostazioni
- pagefile.sys, hiberfil.sys, swapfile.sys, winpepge.sys, config.sys, bootsect.bak, bootmgr, bootnxt
- Qualsiasi file o cartelle nel server di origine che è in conflitto con le cartelle escluse nella destinazione. <br>Ad esempio, se è presente una cartella N:\Windows nell'origine e viene eseguito il mapping da C:\ volume nella destinazione, non venga trasferito, indipendentemente dal relativo contenuto, perché potrebbe interferire con la cartella di sistema C:\Windows nella destinazione.

## <a name="domain-migration"></a> Sono supportate le migrazioni di dominio?

Il servizio di migrazione di archiviazione non consente di eseguire la migrazione tra domini di Active Directory. Le migrazioni tra server sempre verranno aggiunto il server di destinazione allo stesso dominio. È possibile usare le credenziali di migrazione da altri domini nella foresta Active Directory. Il servizio di migrazione di archiviazione supporta la migrazione tra gruppi di lavoro.  

## <a name="cluster-support"></a> I cluster sono supportati come origini o destinazioni?

Il servizio di migrazione di archiviazione attualmente non eseguire la migrazione tra cluster in Windows Server 2019. Si prevede di aggiungere il supporto di cluster in una versione futura del servizio di migrazione di archiviazione.

## <a name="local-principals"></a> Gruppi locali e locale agli utenti di eseguire la migrazione?

Il servizio di migrazione di archiviazione attualmente non eseguire la migrazione gli utenti locali o i gruppi locali in Windows Server 2019. Si prevede di aggiungere supporto utente locale e migrazione dei gruppi locali in una versione futura del servizio di migrazione di archiviazione.

## <a name="domain-controller"></a> È supportata la migrazione del controller di dominio?

Il servizio di migrazione di archiviazione attualmente non eseguire la migrazione di controller di dominio in Windows Server 2019. In alternativa, purché si dispone di più di un controller di dominio nel dominio di Active Directory, abbassare di livello il controller di dominio prima di eseguire la migrazione, quindi alzare di livello la destinazione dopo il completamento di trasferimento. Si prevede di aggiungere il supporto di migrazione del controller di dominio in una versione futura del servizio di migrazione di archiviazione.

## <a name="share-attributes"></a> Quali attributi vengono migrati dal servizio di migrazione di archiviazione?

Il servizio di migrazione di archiviazione esegue la migrazione di tutti i flag, le impostazioni e sicurezza delle condivisioni SMB. Tale elenco di flag che viene eseguita la migrazione di servizio di migrazione di archiviazione include:

    - Stato di condivisione
    - Tipo di disponibilità
    - Tipo di condivisione
    - Modalità di enumerazione cartella *(noto anche come basata sull'accesso enumerazione o ABE)*
    - Modalità di memorizzazione nella cache
    - Leasing di modalità
    - Istanza di SMB
    - Timeout di autorità di certificazione
    - Limite di utenti simultanei
    - Sempre disponibile
    - Descrizione           
    - Crittografare i dati
    - Comunicazione remota di identità
    - Infrastruttura
    - Nome
    - Percorso
    - Con ambito
    - Nome dell'ambito
    - Descrittore di sicurezza
    - Copia shadow
    - Speciale
    - Temporaneo

## <a name="move-db"></a> È possibile spostare il database del servizio di migrazione di archiviazione?

Il servizio di migrazione di archiviazione Usa un database di extensible storage engine (ESE) viene installato per impostazione predefinita nella cartella c:\programdata\microsoft\storagemigrationservice nascosti. Questo database aumenterà man mano che vengono aggiunti i processi e i trasferimenti vengono completati e possono usare spazio su disco significativo dopo la migrazione di milioni di file se non si elimina i processi. Se il database deve essere spostato, procedere come segue:

1. Arrestare il servizio "Servizio di migrazione di archiviazione" sul computer dell'agente di orchestrazione.
2. Acquisire la proprietà del `%programdata%/Microsoft/StorageMigrationService` cartella
3. Aggiungere l'account utente hanno pieno controllo sulle che condividono e tutti i relativi file e sottocartelle.
4. Spostare la cartella in un'altra unità nel computer dell'agente di orchestrazione.
5. Impostare il Registro di sistema valore REG_SZ seguente:

    DatabasePath HKEY_Local_Machine\Software\Microsoft\SMS = *percorso della cartella di database nuovo in un volume diverso* . 
6. Assicurarsi che il sistema abbia il controllo completo a tutti i file e sottocartelle della cartella
7. Rimuovere le autorizzazioni il proprio account.
8. Avviare il servizio "Servizio di migrazione di archiviazione".

## <a name="transfer-threads"></a> È possibile aumentare il numero di file che copiare contemporaneamente?

Il servizio Proxy di servizio di migrazione di archiviazione copia 8 file contemporaneamente in un processo specifico. Questo servizio viene eseguito sull'agente di orchestrazione durante il trasferimento se il computer di destinazione è Windows Server 2012 R2 o Windows Server 2016, ma viene eseguito anche in tutti i nodi di destinazione Windows Server 2019. È possibile aumentare il numero di thread di copia simultanee modificando il nome del valore REG_DWORD del Registro di sistema seguente in formato decimale in ogni nodo che esegue il Proxy di SMS:

    HKEY_Local_Machine\Software\Microsoft\SMSProxy
    FileTransferThreadCount

 L'intervallo valido è 1 e 128 in Windows Server 2019. 

 Dopo la modifica è necessario riavviare il servizio Proxy del servizio migrazione archiviazione in tutti i computer partipating in una migrazione. Si prevede di aumentare questo numero in una versione futura la migrazione del servizio di archiviazione.

## <a name="non-windows"></a> È possibile migrare da origini diverse da Windows Server?

La versione del servizio di migrazione di archiviazione fornita in Windows Server 2019 supporta la migrazione da Windows Server 2003 e sistemi operativi successivi. Attualmente non è possibile eseguire la migrazione da Linux, Samba, NetApp, EMC o altri dispositivi di archiviazione SAN e NAS. Si prevede di consentire questa operazione in una versione futura la migrazione del servizio di archiviazione, a partire con il supporto Linux Samba.

## <a name="previous-versions"></a> È possibile eseguire la migrazione versioni precedenti dei file?

La versione del servizio di migrazione di archiviazione fornita in Windows Server 2019 non supporta versioni precedenti la migrazione (eseguito con il servizio Copia shadow del volume) dei file. Verrà eseguita la migrazione solo la versione corrente. 

## <a name="ntfs-refs"></a> È possibile migrare da NTFS a REFS?

La versione del servizio di migrazione di archiviazione fornita in Windows Server 2019 non supporta la migrazione da NTFS per i file System (Refs). È possibile eseguire la migrazione da NTFS per NTFS e REFS in ReFS. Si tratta per impostazione predefinita, a causa di molte delle differenze nelle funzionalità, i metadati e altri aspetti che ReFS non duplichi da NTFS. ReFS è da intendersi come un sistema di file del carico di lavoro dell'applicazione, non è un sistema di file generici. Per altre informazioni, vedere [Panoramica Resilient File System (ReFS)](../refs/refs-overview.md)

## <a name="consolidate-servers"></a> È possibile consolidare più server in un singolo server?

La versione del servizio di migrazione di archiviazione fornita in Windows Server 2019 non supporta il consolidamento di più server in un unico server. Un esempio di consolidamento consiste nella migrazione tre server di origine separato - che può avere gli stessi nomi di condivisione e percorsi di file locali, in un unico server nuovo virtualizzato questi percorsi e le condivisioni per evitare eventuali collisioni, o si sovrappongono quindi ha risposto a tutte e tre i nomi dei server precedente e l'indirizzo IP. È possibile aggiungere questa funzionalità in una versione futura del servizio di migrazione di archiviazione.  


## <a name="give-feedback"></a> Quali sono le opzioni per fornire commenti e suggerimenti, segnalazione di bug, o ottenere supporto?

Per inviare commenti e suggerimenti sul servizio di migrazione di archiviazione:

- Usare lo strumento di Hub di Feedback incluso in Windows 10, fare clic su "Suggerire una funzionalità" e specificando la categoria di "Windows Server" e una sottocategoria di "Migrazione dell'archiviazione"
- Usare la [Windows Server UserVoice](https://windowsserver.uservoice.com) sito
- Email smsfeed@microsoft.com

Per segnalare bug:

- Usare lo strumento di Hub di Feedback incluso in Windows 10, fare clic su "Segnala un problema" e specificando la categoria di "Windows Server" e una sottocategoria di "Migrazione dell'archiviazione"
- Aprire un caso di supporto tramite [il supporto tecnico Microsoft](https://support.microsoft.com)

Per ottenere supporto:

 - Pubblicare una domanda sul [Tech Community su Windows Server](https://techcommunity.microsoft.com/t5/Windows-Server/ct-p/Windows-Server)
 - Inserire un post nel [Forum Technet di Windows Server 2019](https://social.technet.microsoft.com/Forums/en-US/home?forum=ws2019&filter=alltypes&sort=lastpostdesc) 
 - Aprire un caso di supporto tramite [il supporto tecnico Microsoft](https://support.microsoft.com)


## <a name="see-also"></a>Vedere anche

- [Panoramica del servizio di migrazione archiviazione](overview.md)
