---
title: Miglioramenti della sicurezza SMB
description: Spiegazione della funzionalità di crittografia SMB in Windows Server 2012 R2, Windows Server 2012 e Windows Server 2016.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 7221d3ea94ff9f2d7fca8e95cee66597e2dc6270
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402063"
---
# <a name="smb-security-enhancements"></a>Miglioramenti della sicurezza SMB

>Si applica a: Windows Server 2012 R2, Windows Server 2012, Windows Server 2016

In questo argomento vengono illustrati i miglioramenti apportati alla sicurezza di SMB in Windows Server 2012 R2, Windows Server 2012 e Windows Server 2016.

## <a name="smb-encryption"></a>Crittografia SMB

La crittografia SMB fornisce la crittografia end-to-end dei dati SMB e protegge i dati dalle occorrenze di intercettazione su reti non attendibili. È possibile distribuire la crittografia SMB con il minimo sforzo, ma potrebbero essere necessari piccoli costi aggiuntivi per hardware o software specializzato. Non sono previsti requisiti per Internet Protocol Security (IPsec) o acceleratori WAN. La crittografia SMB può essere configurata in base alle singole condivisioni o per l'intera file server e può essere abilitata per un'ampia gamma di scenari in cui i dati attraversano reti non attendibili.

>[!NOTE]
>La crittografia SMB non copre la sicurezza dei inattivi, che in genere viene gestita da Crittografia unità BitLocker.

È consigliabile prendere in considerazione la crittografia SMB per qualsiasi scenario in cui i dati sensibili devono essere protetti da attacchi man-in-the-Middle. Di seguito sono illustrati alcuni degli scenari possibili:

- I dati sensibili di un Information Worker vengono spostati usando il protocollo SMB. La crittografia SMB offre una garanzia per la privacy e l'integrità end-to-end tra il file server e il client, indipendentemente dalle reti attraversate, ad esempio le connessioni Wide Area Network (WAN) gestite da provider non Microsoft.
- SMB 3,0 consente ai file server di fornire spazio di archiviazione continuamente disponibile per le applicazioni server, ad esempio SQL Server o Hyper-V. L'abilitazione della crittografia SMB offre la possibilità di proteggere tali informazioni da attacchi snooping. La crittografia SMB è più semplice da utilizzare rispetto alle soluzioni hardware dedicate necessarie per la maggior parte delle reti di archiviazione (San).

>[!IMPORTANT]
>Si noti che esiste un notevole costo operativo per le prestazioni con qualsiasi protezione della crittografia end-to-end rispetto a quella non crittografata.

## <a name="enable-smb-encryption"></a>Abilitare la crittografia SMB

È possibile abilitare la crittografia SMB per l'intera file server o solo per specifiche condivisioni file. Per abilitare la crittografia SMB, utilizzare una delle procedure seguenti:

### <a name="enable-smb-encryption-with-windows-powershell"></a>Abilitare la crittografia SMB con Windows PowerShell

1. Per abilitare la crittografia SMB per una singola condivisione file, digitare lo script seguente sul server:
    
    ```PowerShell
    Set-SmbShare –Name <sharename> -EncryptData $true
    ```
2. Per abilitare la crittografia SMB per l'intera file server, digitare lo script seguente sul server:
    
    ```PowerShell
    Set-SmbServerConfiguration –EncryptData $true
    ```
3. Per creare una nuova condivisione file SMB con la crittografia SMB abilitata, digitare lo script seguente:
    
    ```PowerShell
    New-SmbShare –Name <sharename> -Path <pathname> –EncryptData $true
    ```

### <a name="enable-smb-encryption-with-server-manager"></a>Abilitare la crittografia SMB con Server Manager

1. In Server Manager aprire **Servizi file e archiviazione**.
2. Selezionare **condivisioni** per aprire la pagina Gestione condivisioni.
3. Fare clic con il pulsante destro del mouse sulla condivisione in cui si desidera abilitare la crittografia SMB, quindi scegliere **Proprietà**.
4. Nella pagina **Impostazioni** della condivisione selezionare **Crittografa accesso ai dati**. L'accesso al file remoto a questa condivisione è crittografato.

### <a name="considerations-for-deploying-smb-encryption"></a>Considerazioni sulla distribuzione della crittografia SMB

Per impostazione predefinita, quando è abilitata la crittografia SMB per una condivisione file o un server, solo i client SMB 3,0 sono autorizzati ad accedere alle condivisioni file specificate. Questa operazione impone all'amministratore lo scopo di proteggere i dati per tutti i client che accedono alle condivisioni. Tuttavia, in alcuni casi, un amministratore potrebbe voler consentire l'accesso non crittografato per i client che non supportano SMB 3,0, ad esempio durante un periodo di transizione quando si utilizzano versioni del sistema operativo client miste. Per consentire l'accesso non crittografato per i client che non supportano SMB 3,0, digitare lo script seguente in Windows PowerShell:

```PowerShell
Set-SmbServerConfiguration –RejectUnencryptedAccess $false
```

La funzionalità di negoziazione del dialetto sicuro descritta nella sezione successiva impedisce a un attacco man-in-the-Middle di effettuare il downgrade di una connessione da SMB 3,0 a SMB 2,0 (che utilizzerebbe l'accesso non crittografato). Tuttavia, non impedisce un downgrade a SMB 1,0, che comporta anche l'accesso non crittografato. Per garantire che i client SMB 3,0 usino sempre la crittografia SMB per accedere alle condivisioni crittografate, è necessario disabilitare il server SMB 1,0. Per istruzioni, vedere la sezione [disabilitazione di SMB 1,0](#disabling-smb-10). Se l'impostazione **-RejectUnencryptedAccess** viene lasciata con l'impostazione predefinita di **$true**, solo i client SMB 3,0 con supporto per la crittografia possono accedere alle condivisioni file (anche i client SMB 1,0 verranno rifiutati).

>[!NOTE]
>* La crittografia SMB usa l'algoritmo Advanced Encryption Standard (AES)-CCM per crittografare e decrittografare i dati. AES-CCM fornisce inoltre la convalida dell'integrità dei dati (firma) per le condivisioni file crittografate, indipendentemente dalle impostazioni di firma SMB. Se si vuole abilitare la firma SMB senza crittografia, è possibile continuare a eseguire questa operazione. Per ulteriori informazioni, vedere [le nozioni di base sulla firma SMB](https://blogs.technet.microsoft.com/josebda/2010/12/01/the-basics-of-smb-signing-covering-both-smb1-and-smb2/).
>* È possibile che si verifichino problemi quando si tenta di accedere alla condivisione file o al server se l'organizzazione usa le appliance di accelerazione Wide Area Network (WAN).
>* Con una configurazione predefinita, in cui non è consentito l'accesso non crittografato alle condivisioni file crittografate, se i client che non supportano SMB 3,0 tentano di accedere a una condivisione file crittografata, l'ID evento 1003 viene registrato nel registro eventi Microsoft-Windows-SmbServer/Operational e il client riceverà un messaggio di errore di **accesso negato** .
>* La crittografia SMB e la Encrypting File System (EFS) nel file system NTFS non sono correlate e la crittografia SMB non richiede o dipende dall'uso di EFS.
>* La crittografia SMB e la Crittografia unità BitLocker non sono correlate e la crittografia SMB non richiede o dipende dall'uso di Crittografia unità BitLocker.

## <a name="secure-dialect-negotiation"></a>Negoziazione sicura del dialetto

SMB 3,0 è in grado di rilevare attacchi man-in-the-Middle che tentano di effettuare il downgrade del protocollo SMB 2,0 o SMB 3,0 o delle funzionalità negoziate dal client e dal server. Quando il client o il server rileva un attacco di questo tipo, la connessione viene disconnessa e viene registrato l'ID evento 1005 nel registro eventi Microsoft-Windows-SmbServer/Operational. La negoziazione del dialetto sicuro non è in grado di rilevare o impedire il downgrade da SMB 2,0 o 3,0 a SMB 1,0. Per questo motivo, e per sfruttare le funzionalità complete della crittografia SMB, è consigliabile disabilitare il server SMB 1,0. Per ulteriori informazioni, vedere la pagina relativa alla [disabilitazione di SMB 1,0](#disabling-smb-10).

La funzionalità di negoziazione del dialetto sicuro descritta nella sezione successiva impedisce a un attacco man-in-the-Middle di effettuare il downgrade di una connessione da SMB 3 a SMB 2 (che utilizzerebbe l'accesso non crittografato); Tuttavia, non impedisce il downgrade a SMB 1, che comporta anche l'accesso non crittografato. Per ulteriori informazioni sui potenziali problemi con le precedenti implementazioni non Windows di SMB, vedere la [Microsoft Knowledge base](http://support.microsoft.com/kb/2686098).

## <a name="new-signing-algorithm"></a>Nuovo algoritmo di firma

SMB 3,0 usa un algoritmo di crittografia più recente per la firma: Advanced Encryption Standard (AES)-codice di autenticazione messaggi basato su crittografia (CMAC). SMB 2,0 ha utilizzato l'algoritmo di crittografia HMAC-SHA256 meno recente. AES-CMAC e AES-CCM possono accelerare significativamente la crittografia dei dati sulle CPU più moderne con supporto per le istruzioni AES. Per ulteriori informazioni, vedere [le nozioni di base sulla firma SMB](https://blogs.technet.microsoft.com/josebda/2010/12/01/the-basics-of-smb-signing-covering-both-smb1-and-smb2/).

## <a name="disabling-smb-10"></a>Disabilitazione di SMB 1,0

Il servizio browser del computer legacy e le funzionalità del protocollo di amministrazione remota in SMB 1,0 sono ora separate e possono essere eliminate. Queste funzionalità sono ancora abilitate per impostazione predefinita, ma se non si dispone di client SMB obsoleti, ad esempio computer che eseguono Windows Server 2003 o Windows XP, è possibile rimuovere le funzionalità di SMB 1,0 per aumentare la sicurezza e ridurre potenzialmente l'applicazione di patch.

>[!NOTE]
>SMB 2,0 è stato introdotto in Windows Server 2008 e Windows Vista. I client meno recenti, ad esempio i computer che eseguono Windows Server 2003 o Windows XP, non supportano SMB 2,0; e pertanto non saranno in grado di accedere alle condivisioni file o alle condivisioni di stampa se il server SMB 1,0 è disabilitato. Inoltre, alcuni client SMB non Microsoft potrebbero non essere in grado di accedere alle condivisioni file SMB 2,0 o alle condivisioni di stampa, ad esempio le stampanti con funzionalità di analisi della condivisione.

Prima di iniziare a disabilitare SMB 1,0, è necessario sapere se i client SMB sono attualmente connessi al server che esegue SMB 1,0. A tale scopo, immettere il cmdlet seguente in Windows PowerShell:

```PowerShell
Get-SmbSession | Select Dialect,ClientComputerName,ClientUserName | ? Dialect -lt 2
```

>[!NOTE]
>È consigliabile eseguire questo script ripetutamente nel corso di una settimana (più volte al giorno) per creare un audit trail. È anche possibile eseguire questa operazione come attività pianificata.

Per disabilitare SMB 1,0, immettere lo script seguente in Windows PowerShell:

```PowerShell
Set-SmbServerConfiguration –EnableSMB1Protocol $false
```

>[!NOTE]
>Se una connessione client SMB viene negata perché il server che esegue SMB 1,0 è stato disabilitato, l'ID evento 1001 verrà registrato nel registro eventi Microsoft-Windows-SmbServer/Operational.

## <a name="more-information"></a>Altre informazioni

Di seguito sono riportate alcune risorse aggiuntive relative a SMB e alle tecnologie correlate in Windows Server 2012.

- [Server Message Block](file-server-smb-overview.md)
- [Archiviazione in Windows Server](../storage.md)
- [File server di scalabilità orizzontale per i dati delle applicazioni](../../failover-clustering/sofs-overview.md)