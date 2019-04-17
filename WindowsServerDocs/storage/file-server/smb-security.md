---
title: Miglioramenti della protezione SMB
description: Una spiegazione della caratteristica crittografia SMB in Windows Server 2012 R2, Windows Server 2012 e Windows Server 2016.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 831ca8266c3ec18ffb83227dcb2d39b3f953ad1a
ms.sourcegitcommit: 375e94dc70c76e7aa5679c32f0f4dbc26cf7dd83
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/10/2018
ms.locfileid: "2233527"
---
# <a name="smb-security-enhancements"></a>Miglioramenti della protezione SMB

>Si applica a: Windows Server 2012 R2, Windows Server 2012, Windows Server 2016

In questo argomento vengono illustrati i miglioramenti della protezione SMB in Windows Server 2012 R2, Windows Server 2012 e Windows Server 2016.

## <a name="smb-encryption"></a>Crittografia SMB

Crittografia SMB fornisce la crittografia dei dati SMB end-to-end e protegge i dati da eavesdropping occorrenze su reti non attendibile. È possibile distribuire crittografia SMB con il minimo sforzo, ma potrebbe essere necessario piccoli costi aggiuntivi per software o hardware specializzata. Non presenta alcun requisiti per gli acceleratori WAN o Internet Protocol security (IPsec). Crittografia SMB è possibile configurare per l'intero file server o per azione e può essere abilitato per una serie di scenari in cui dati attraversa reti non attendibili.

>[!NOTE]
>Crittografia SMB non copre sicurezza statici, in genere eseguita dal BitLocker Drive Encryption.

Crittografia SMB da considerare per tutti gli scenari in cui devono essere attivata alcuna protezione contro gli attacchi di tipo man-in-the-middle dati riservati. Gli scenari possibili includono:

- Dati riservati un information worker viene spostati utilizzando il protocollo SMB. Crittografia SMB offre la garanzia di integrità e sulla privacy end-to-end tra il file server e client, indipendentemente dal fatto reti attraversato, ad esempio étendu (WAN) connessioni di rete che vengono gestite da terze parti.
- SMB 3.0 consente ai file server fornire l'archivio sempre disponibile per le applicazioni server, ad esempio SQL Server o di Hyper-V. Abilitazione crittografia SMB offre la possibilità di proteggere tali informazioni da attacchi snooping. Crittografia SMB è più semplice da utilizzare rispetto le soluzioni hardware dedicato necessari per la maggior parte delle reti di archiviazione (SAN).

>[!IMPORTANT]
>Si noti che esiste un prestazioni notevoli costi con qualsiasi tipo di protezione confronto non crittografate end-to-end crittografia operativi.

## <a name="enable-smb-encryption"></a>Abilitare la crittografia SMB

È possibile abilitare la crittografia SMB per l'intero file server o solo per le condivisioni di file specifici. Per attivare la crittografia SMB, utilizzare una delle procedure seguenti:

### <a name="enable-smb-encryption-with-windows-powershell"></a>Attivare la crittografia SMB con Windows PowerShell

1. Per abilitare la crittografia SMB per una condivisione di file singolo, digitare il seguente script nel server:
    
    ```PowerShell
    Set-SmbShare –Name <sharename> -EncryptData $true
    ```
2. Per abilitare la crittografia SMB per l'intero file server, digitare il seguente script nel server:
    
    ```PowerShell
    Set-SmbServerConfiguration –EncryptData $true
    ```
3. Per creare una nuova condivisione file SMB con attivata la crittografia SMB, digitare lo script seguente:
    
    ```PowerShell
    New-SmbShare –Name <sharename> -Path <pathname> –EncryptData $true
    ```

### <a name="enable-smb-encryption-with-server-manager"></a>Attivare la crittografia SMB con Server Manager

1. In Server Manager, aprire **File e servizi di archiviazione**.
2. Selezionare **condivisioni** per aprire la pagina Gestione condivisioni.
3. La condivisione in cui si desidera abilitare la crittografia SMB pulsante destro del mouse e quindi scegliere **proprietà**.
4. Nella pagina **Impostazioni** di condivisione, selezionare **Crittografa l'accesso ai dati**. Accesso remoto ai file per la condivisione viene crittografato.

### <a name="considerations-for-deploying-smb-encryption"></a>Considerazioni per la distribuzione di crittografia SMB

Per impostazione predefinita, quando la crittografia SMB è attivata per una condivisione file o il server, solo SMB 3.0 i client possono accedere alle condivisioni di file specificato. Si applica lo scopo dell'amministratore di la protezione dei dati per tutti i client che accedono a condivisioni. In alcuni casi, un amministratore può tuttavia consentire l'accesso non crittografata per i client che non supportano 3.0 SMB (ad esempio, periodo di transizione quando vengono utilizzate le versioni del sistema operativo client misto). Per consentire l'accesso non crittografata per i client che non supportano SMB 3.0, digitare il seguente script di Windows PowerShell:

```PowerShell
Set-SmbServerConfiguration –RejectUnencryptedAccess $false
```

La capacità di negoziazione sottolinguaggio sicura descritta nella sezione successiva impedisce che un attacco di tipo man-in-the-middle downgrade una connessione tra SMB 3.0 e 2.0 SMB (da utilizzare con l'accesso non crittografata). Tuttavia, ciò non impedisce un downgrade a SMB 1.0, anche genera l'accesso non crittografato. Per garantire che i client SMB 3.0 utilizzano sempre la crittografia SMB di accedere a condivisioni crittografate, è necessario disattivare il server SMB 1.0. (Per ulteriori informazioni, vedere la sezione [disattivazione SMB 1.0](#disabling-smb-1.0)). Se l'impostazione **– RejectUnencryptedAccess** è da sinistra sul valore predefinito **$true**, sono consentiti solo che supportano la crittografia SMB 3.0 client per accedere a condivisioni di file (client SMB 1.0 verrà inoltre rifiutato).

>[!NOTE]
>* Crittografia SMB utilizza la crittografia AES (Advanced Standard)-algoritmo CCM per la crittografia e decrittografia dei dati. AES CCM offre anche la convalida l'integrità dei dati (firma) per le condivisioni di file crittografati, indipendentemente dalle impostazioni di firma SMB. Se si desidera abilitare la firma senza crittografia SMB, è possibile continuare a tale scopo. Per ulteriori informazioni, vedere [The nozioni di base di SMB della firma](https://blogs.technet.microsoft.com/josebda/2010/12/01/the-basics-of-smb-signing-covering-both-smb1-and-smb2/).
>* Problemi possono verificarsi quando si tenta di accedere alla condivisione file o un server se l'organizzazione utilizza dispositivi di accelerazione WAN rete WAN.
>* Con una configurazione predefinita (se non è possibile accedere non crittografate consentito di condivisioni di file crittografati), se i client che non supportano SMB 3.0 tentativo di accedere a una condivisione di file crittografati, ID evento 1003 viene registrata nel registro eventi di Microsoft-Windows-SmbServer/Operational , e il client verrà visualizzato un messaggio di errore **accesso negato** .
>* Crittografia SMB e la crittografia File System (EFS) nel file system NTFS sono correlati e crittografia SMB non lo richiede o dipendono utilizzando EFS.
>* Crittografia SMB e crittografia unità BitLocker non sono correlati e crittografia SMB non lo richiede o dipendono tramite crittografia unità BitLocker.

## <a name="secure-dialect-negotiation"></a>Proteggere la negoziazione sottolinguaggio

SMB 3.0 è in grado di rilevare gli attacchi di tipo man-in-the-middle che tentano di effettuare il downgrade il protocollo SMB 2.0 o SMB 3.0 o le funzionalità che il client e server la negoziazione. Quando viene rilevato un attacco dal client o server, la connessione viene interrotta e viene registrato nel registro eventi di Microsoft-Windows-SmbServer/Operational evento 1005 ID. Secure sottolinguaggio negoziazione grado di rilevare o impedire riducono da SMB 2.0 o 3.0 per SMB 1.0. Per questo motivo e per usufruire di tutte le funzionalità di crittografia SMB, è consigliabile disabilitare il server SMB 1.0. Per ulteriori informazioni, vedere [disattivazione SMB 1.0](#disabling-smb-1.0).

La funzionalità di negoziazione sottolinguaggio sicura descritta nella sezione successiva impedisce un attacco di tipo man-in-the-middle downgrade una connessione da 3 SMB su 2 SMB (da utilizzare con l'accesso non crittografata). Tuttavia, ciò non impedisce riducono su 1, SMB in anche genera l'accesso non crittografato. Per ulteriori informazioni sui possibili problemi con le versioni precedenti non Windows implementazioni di SMB, vedere l' [articolo della Microsoft Knowledge Base](http://support.microsoft.com/kb/2686098).

## <a name="new-signing-algorithm"></a>Nuovo algoritmo di firma

SMB 3.0 viene utilizzato un algoritmo di crittografia più recente per la firma: la crittografia AES (Advanced Standard) - crittografia - basati su codice message authentication code (CMAC). SMB 2.0 utilizzato l'algoritmo di crittografia HMAC SHA256 precedente. AES CMAC e AES CCM in grado di accelerare notevolmente la crittografia dei dati in più moderne CPU con istruzioni AES supportare. Per ulteriori informazioni, vedere [The nozioni di base di SMB della firma](https://blogs.technet.microsoft.com/josebda/2010/12/01/the-basics-of-smb-signing-covering-both-smb1-and-smb2/).

## <a name="disabling-smb-10"></a>Disattivazione SMB 1.0

Il servizio browser di computer legacy e le funzionalità del protocollo di amministrazione remota in SMB 1.0 sono ora separate e può essere eliminate. Queste funzionalità sono ancora abilitate per impostazione predefinita, ma se non è meno recente SMB dai client come computer che eseguono Windows Server 2003 o Windows XP, è possibile rimuovere le caratteristiche SMB 1.0 per aumentare la protezione e potenzialmente ridurre l'applicazione di patch.

>[!NOTE]
>SMB 2.0 è stato introdotto in Windows Server 2008 e Windows Vista. Client precedenti, ad esempio computer che eseguono Windows Server 2003 o Windows XP non supportano SMB 2.0. e, pertanto, non potranno accedere a condivisioni di file o stampare condivisioni se il server SMB 1.0 è disabilitato. Inoltre, alcuni client SMB non Microsoft potrebbe non essere in grado di accedere a condivisioni di file SMB 2.0 o stampare condivisioni (ad esempio le stampanti con funzionalità "analisi da condividere").

Prima di avviare la disattivazione SMB 1.0, è necessario scoprire se i client SMB attualmente connessi al server che esegue SMB 1.0. A tale scopo, immettere il seguente cmdlet di Windows PowerShell:

```PowerShell
Get-SmbSession | Select Dialect,ClientComputerName,ClientUserName | ? Dialect -lt 2
```

>[!NOTE]
>È consigliabile eseguire questo script ripetutamente nel corso di una settimana (più volte al giorno) per creare un audit trail. È inoltre possibile eseguire questo come attività pianificata.

Per disabilitare SMB 1.0, immettere il seguente script di Windows PowerShell:

```PowerShell
Set-SmbServerConfiguration –EnableSMB1Protocol $false
```

>[!NOTE]
>Se una connessione client SMB viene negata perché è stato disabilitato nel server che esegue SMB 1.0, ID evento 1001 verrà registrata nel registro eventi di Microsoft-Windows-SmbServer/Operational.

## <a name="more-information"></a>Ulteriori informazioni

Di seguito sono riportate alcune risorse aggiuntive su SMB e le tecnologie correlate in Windows Server 2012.

- [Server Message Block](file-server-smb-overview.md)
- [Archiviazione in WindowsServer](../storage.md)
- [File di scalabilità orizzontale Server dei dati delle applicazioni](../../failover-clustering/sofs-overview.md)