---
title: Miglioramenti della sicurezza SMB
description: Spiegazione della funzionalità di crittografia di SMB in Windows Server 2012 R2, Windows Server 2012 e Windows Server 2016.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: b1586c8c63e46452075b4106c944670395734142
ms.sourcegitcommit: 21165734a0f37c4cd702c275e85c9e7c42d6b3cb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/03/2019
ms.locfileid: "65034409"
---
# <a name="smb-security-enhancements"></a>Miglioramenti della sicurezza SMB

>Si applica a: Windows Server 2012 R2, Windows Server 2012, Windows Server 2016

Questo argomento illustra i miglioramenti della sicurezza SMB in Windows Server 2012 R2, Windows Server 2012 e Windows Server 2016.

## <a name="smb-encryption"></a>Crittografia SMB

La crittografia SMB offre la crittografia end-to-end dei dati SMB e protegge i dati dalle occorrenze di intercettazione su reti non attendibili. È possibile distribuire la crittografia SMB con il minimo sforzo, ma può risultare piccole altri costi del software o hardware specializzato. Non presenta requisiti per Internet Protocol security (IPsec) o acceleratori della WAN. La crittografia SMB può essere configurata su una base per ogni condivisione o per l'intero file server e può essere abilitata per un'ampia gamma di scenari in cui i dati attraversano reti non attendibili.

>[!NOTE]
>La crittografia SMB non viene illustrata la sicurezza quando sono inattivi, che viene in genere gestita da crittografia unità BitLocker.

La crittografia SMB deve essere considerata per qualsiasi scenario in cui i dati riservati devono essere protetti da attacchi man-in-the-middle. Di seguito sono illustrati alcuni degli scenari possibili:

- I dati sensibili dell'information worker viene spostati utilizzando il protocollo SMB. La crittografia SMB offre una garanzia di riservatezza e integrità end-to-end tra il file server e client, indipendentemente dalle reti attraversati, ad esempio connessioni wide area network (WAN) che sono gestite dal provider non Microsoft.
- SMB 3.0 consente ai file server fornire spazio di archiviazione continuamente disponibile per le applicazioni server, ad esempio SQL Server o Hyper-V. Abilitazione della crittografia SMB offre un'opportunità per proteggere tali informazioni dagli attacchi di snooping. La crittografia SMB è più facile da usare rispetto alle soluzioni per l'hardware dedicato che sono necessari per la maggior parte delle reti di archiviazione (SAN).

>[!IMPORTANT]
>Si noti che vi sia una prestazione da notare operativo costi con qualsiasi protezione crittografia end-to-end rispetto a non crittografate.

## <a name="enable-smb-encryption"></a>Abilitare la crittografia SMB

È possibile abilitare la crittografia SMB per l'intero file server o solo per specifiche condivisioni file. Per abilitare la crittografia SMB, usare una delle procedure riportate di seguito:

### <a name="enable-smb-encryption-with-windows-powershell"></a>Abilitare la crittografia SMB con Windows PowerShell

1. Per abilitare la crittografia SMB per una singola condivisione file, digitare lo script seguente nel server:
    
    ```PowerShell
    Set-SmbShare –Name <sharename> -EncryptData $true
    ```
2. Per abilitare la crittografia SMB per l'intero file server, digitare lo script seguente nel server:
    
    ```PowerShell
    Set-SmbServerConfiguration –EncryptData $true
    ```
3. Per creare una nuova condivisione file SMB con abilitata la crittografia SMB, digitare lo script seguente:
    
    ```PowerShell
    New-SmbShare –Name <sharename> -Path <pathname> –EncryptData $true
    ```

### <a name="enable-smb-encryption-with-server-manager"></a>Abilitare la crittografia SMB con Server Manager

1. In Server Manager, aprire **servizi File e archiviazione**.
2. Selezionare **condivisioni** per aprire la pagina di gestione di condivisioni.
3. Fare doppio clic la condivisione in cui si vuole abilitare la crittografia SMB e quindi selezionare **proprietà**.
4. Nel **le impostazioni** pagina della condivisione, seleziona **crittografa accesso ai dati**. Accesso remoto ai file a questa condivisione è crittografata.

### <a name="considerations-for-deploying-smb-encryption"></a>Considerazioni per la distribuzione della crittografia SMB

Per impostazione predefinita, quando è abilitata la crittografia SMB per una condivisione file o server, sono consentiti solo i client SMB 3.0 per accedere alle condivisioni di file specificato. Ciò consente di applicare le intenzioni dell'amministratore di salvaguardare i dati per tutti i client di accedere alle condivisioni. Tuttavia, in alcuni casi, un amministratore desideri consentire l'accesso non crittografato per i client che non supportano SMB 3.0 (ad esempio durante un periodo di transizione quando vengono usate versioni dei sistemi operativi client misti). Per consentire l'accesso non crittografato per i client che non supportano SMB 3.0, digitare il seguente script di Windows PowerShell:

```PowerShell
Set-SmbServerConfiguration –RejectUnencryptedAccess $false
```

La funzionalità di negoziazione protetta sottolinguaggio descritta nella sezione successiva si impedisce che un attacco man-in-the-middle il downgrade di una connessione da SMB 3.0 a SMB 2.0 (da utilizzare con l'accesso non crittografato). Tuttavia, non impedisce il downgrade a SMB 1.0, che comporta anche l'accesso non crittografato. Per garantire che i client SMB 3.0 usino sempre la crittografia SMB per accedere alle condivisioni crittografate, è necessario disabilitare il server SMB 1.0. (Per istruzioni, vedere la sezione [la disabilitazione di SMB 1.0](#disabling-smb-10).) Se il **– RejectUnencryptedAccess** impostazione è stata lasciata sul valore predefinito **$true**, sono consentiti solo che supporta la crittografia client SMB 3.0 per accedere alle condivisioni file (i client SMB 1.0 verranno inoltre rifiutati).

>[!NOTE]
>* La crittografia SMB Usa il Advanced Encryption Standard (AES)-algoritmo CCM per crittografare e decrittografare i dati. AES-CCM fornisce anche la convalida dell'integrità dei dati (firma) per le condivisioni di file crittografati, indipendentemente dalle impostazioni della firmare SMB. Se si desidera abilitare la firma senza crittografia SMB, è possibile continuare a eseguire questa operazione. Per altre informazioni, vedere [nozioni di base di SMB firma](https://blogs.technet.microsoft.com/josebda/2010/12/01/the-basics-of-smb-signing-covering-both-smb1-and-smb2/).
>* Si potrebbero verificarsi problemi quando si prova ad accedere alla condivisione file o server se l'organizzazione Usa dispositivi accelerazione di wide area network (WAN).
>* Con una configurazione predefinita (in cui è presente alcun accesso non crittografato consentito alle condivisioni file crittografato), se i client che non supportano SMB 3.0 tentativo di accedere a una condivisione file crittografata, ID evento 1003 viene registrato nel registro eventi Microsoft-Windows-SmbServer/Operational , e il client riceverà un **accesso negato** messaggio di errore.
>* La crittografia SMB e la crittografia dei File System (EFS) nel file system NTFS non sono correlate e la crittografia SMB non richiede o non dipendono da utilizza EFS.
>* La crittografia SMB e la crittografia unità BitLocker non sono correlate e la crittografia SMB non richiede o non dipendono da usando Crittografia unità BitLocker.

## <a name="secure-dialect-negotiation"></a>Negoziazione sicuro del linguaggio

SMB 3.0 è in grado di rilevare gli attacchi man-in-the-middle che tentano di effettuare il downgrade la funzionalità che il client e server di negoziare o il protocollo SMB 2.0 o SMB 3.0. Quando viene rilevato un attacco dal client o server, la connessione viene interrotta e 1005 ID evento viene registrato nel registro eventi Microsoft-Windows-SmbServer/Operational. Sottolinguaggio di proteggere la negoziazione non è possibile rilevare o prevenire effettua il downgrade da SMB 2.0 o 3.0 di SMB 1.0. Per questo motivo e per sfruttare le funzionalità complete della crittografia SMB, è consigliabile disabilitare il server SMB 1.0. Per altre informazioni, vedere [la disabilitazione di SMB 1.0](#disabling-smb-10).

La funzionalità di negoziazione protetta sottolinguaggio che è descritti nella sezione successiva impedisce a un attacco man-in-the-middle di downgrade di una connessione da SMB 3 a SMB 2 (da utilizzare con l'accesso non crittografato); Tuttavia, effettua il downgrade non impedisce a SMB 1, che comporta anche l'accesso non crittografato. Per altre informazioni sui problemi potenziali con versioni precedenti non Windows implementazioni di SMB, vedere la [della Microsoft Knowledge Base](http://support.microsoft.com/kb/2686098).

## <a name="new-signing-algorithm"></a>Nuovo algoritmo di firma

SMB 3.0 utilizza un algoritmo di crittografia più recente per la firma: Advanced Encryption Standard (AES) - cipher - basato su codice message authentication code (CMAC). SMB 2.0 utilizzato l'algoritmo di crittografia HMAC-SHA256 meno recente. AES-CMAC e AES-CCM consentono di accelerare in modo significativo la crittografia dei dati nelle CPU più moderne dotati di supporto di istruzioni di AES. Per altre informazioni, vedere [nozioni di base di SMB firma](https://blogs.technet.microsoft.com/josebda/2010/12/01/the-basics-of-smb-signing-covering-both-smb1-and-smb2/).

## <a name="disabling-smb-10"></a>Disabling SMB 1.0

Il servizio browser di computer legacy e le funzionalità Remote Administration Protocol in SMB 1.0 sono ora separate e può essere eliminate. Queste funzionalità sono ancora abilitate per impostazione predefinita, ma se non si dispone di client SMB meno recenti, ad esempio i computer che eseguono Windows Server 2003 o Windows XP, è possibile rimuovere le funzionalità di SMB 1.0 per aumentare la sicurezza e ridurre potenzialmente l'applicazione di patch.

>[!NOTE]
>SMB 2.0 è stato introdotto in Windows Server 2008 e Windows Vista. Alcuni client precedenti, ad esempio i computer che eseguono Windows Server 2003 o Windows XP, non supportano SMB 2.0. e non saranno pertanto in grado di accedere alle condivisioni file o condivisioni di stampa, se il server SMB 1.0 è disabilitato. Inoltre, alcuni client SMB non Microsoft potrebbe non essere in grado di accedere alle condivisioni file SMB 2.0 o stampare le condivisioni (ad esempio, le stampanti con la funzionalità "analisi-a-share").

Prima di iniziare la disabilitazione di SMB 1.0, è necessario scoprire se i client SMB sono attualmente connessi al server che esegue SMB 1.0. A tale scopo, immettere il cmdlet seguente in Windows PowerShell:

```PowerShell
Get-SmbSession | Select Dialect,ClientComputerName,ClientUserName | ? Dialect -lt 2
```

>[!NOTE]
>È consigliabile eseguire questo script più volte nel corso di una settimana (più volte al giorno) per creare un audit trail. È anche possibile eseguire questo come attività pianificata.

Per disabilitare SMB 1.0, immettere lo script seguente in Windows PowerShell:

```PowerShell
Set-SmbServerConfiguration –EnableSMB1Protocol $false
```

>[!NOTE]
>Se la connessione di un client SMB è negata perché il server che esegue SMB 1.0 è stato disabilitato, ID evento 1001 verrà registrato nel registro eventi Microsoft-Windows-SmbServer/Operational.

## <a name="more-information"></a>Altre informazioni

Di seguito sono segnalate altre risorse relative a SMB e alle tecnologie correlate in Windows Server 2012.

- [Server Message Block](file-server-smb-overview.md)
- [Archiviazione in Windows Server](../storage.md)
- [Scale-Out File Server per i dati dell'applicazione](../../failover-clustering/sofs-overview.md)