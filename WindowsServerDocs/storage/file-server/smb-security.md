---
title: Miglioramenti della sicurezza SMB
description: Descrizione della funzionalità Crittografia SMB in Windows Server 2012 R2, Windows Server 2012 e Windows Server 2016.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: d7b96574dcfc2a4417aa36780d7bd87c2556f61f
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950266"
---
# <a name="smb-security-enhancements"></a>Miglioramenti della sicurezza SMB

>Si applica a: Windows Server 2012 R2, Windows Server 2012, Windows Server 2016

Questo argomento descrive i miglioramenti relativi alla sicurezza SMB in Windows Server 2012 R2, Windows Server 2012 e Windows Server 2016.

## <a name="smb-encryption"></a>Crittografia SMB

Crittografia SMB fornisce la crittografia end-to-end dei dati SMB e protegge i dati dalle intercettazioni che possono verificarsi su reti non attendibili. Puoi distribuire Crittografia SMB con il minimo sforzo, ma ciò potrebbe comportare costi aggiuntivi di modesta entità per hardware o software specializzato. Non sono previsti requisiti per gli acceleratori WAN o Internet Protocol Security (IPsec). Crittografia SMB può essere configurata in base alle singole condivisioni o per l'intero file server e può essere abilitata per una vasta gamma di scenari in cui i dati attraversano reti non attendibili.

>[!NOTE]
>Crittografia SMB non copre la sicurezza nei momenti di inattività, aspetto in genere gestito da Crittografia unità BitLocker.

È consigliabile prendere in considerazione la possibilità di usare Crittografia SMB per qualsiasi scenario in cui i dati sensibili devono essere protetti da attacchi man-in-the-middle. Di seguito sono illustrati alcuni degli scenari possibili:

- I dati sensibili di un information worker vengono spostati usando il protocollo SMB. Crittografia SMB offre la garanzia di privacy e integrità end-to-end tra il file server e il client, indipendentemente dalle reti attraversate, ad esempio connessioni WAN (Wide Area Network) gestite da provider non Microsoft.
- SMB 3.0 consente ai file server di fornire continuamente spazio di archiviazione disponibile per le applicazioni server, ad esempio SQL Server o Hyper-V. L'abilitazione di Crittografia SMB offre la possibilità di proteggere tali informazioni da attacchi snooping. Crittografia SMB è più semplice da usare rispetto alle soluzioni hardware dedicate che sono necessarie per la maggior parte delle reti di archiviazione (SAN).

>[!IMPORTANT]
>Tieni presente che qualsiasi forma di protezione con crittografia end-to-end comporta un notevole costo operativo per le prestazioni rispetto a una forma non crittografata.

## <a name="enable-smb-encryption"></a>Abilitare Crittografia SMB

Puoi abilitare Crittografia SMB per l'intero file server o solo per specifiche condivisioni file. Per abilitare Crittografia SMB, usa una delle procedure descritte di seguito:

### <a name="enable-smb-encryption-with-windows-powershell"></a>Abilitare Crittografia SMB con Windows PowerShell

1. Per abilitare Crittografia SMB per una singola condivisione file, digita lo script seguente sul server:
    
    ```PowerShell
    Set-SmbShare –Name <sharename> -EncryptData $true
    ```
2. Per abilitare Crittografia SMB per l'intero file server, digita lo script seguente sul server:
    
    ```PowerShell
    Set-SmbServerConfiguration –EncryptData $true
    ```
3. Per creare una nuova condivisione file SMB con Crittografia SMB abilitata, digita lo script seguente:
    
    ```PowerShell
    New-SmbShare –Name <sharename> -Path <pathname> –EncryptData $true
    ```

### <a name="enable-smb-encryption-with-server-manager"></a>Abilitare Crittografia SMB con Server Manager

1. In Server Manager apri **Servizi file e archiviazione**.
2. Seleziona **Condivisioni** per visualizzare la pagina Gestione condivisioni.
3. Fai clic con il pulsante destro del mouse sulla condivisione in cui vuoi abilitare Crittografia SMB e quindi scegli **proprietà**.
4. Nella pagina **Impostazioni** della condivisione seleziona l'opzione per la **crittografia dell'accesso ai dati**. L'accesso remoto ai file in questa condivisione è crittografato.

### <a name="considerations-for-deploying-smb-encryption"></a>Considerazioni per la distribuzione di Crittografia SMB

Per impostazione predefinita, quando Crittografia SMB è abilitata per una condivisione file o un server, solo i client SMB 3.0 sono autorizzati ad accedere alle condivisioni file specificate. In questo modo, viene applicata la finalità dell'amministratore di salvaguardare i dati per tutti i client che accedono alle condivisioni. Tuttavia, in alcuni casi, un amministratore potrebbe voler consentire l'accesso non crittografato per i client che non supportano SMB 3.0, ad esempio durante un periodo di transizione quando vengono usate versioni miste del sistema operativo client. Per consentire l'accesso non crittografato per i client che non supportano SMB 3.0, digita lo script seguente in Windows PowerShell:

```PowerShell
Set-SmbServerConfiguration –RejectUnencryptedAccess $false
```

La funzionalità di negoziazione sicura dei dialetti descritta nella sezione seguente impedisce a un attacco man-in-the-middle di effettuare il downgrade di una connessione da SMB 3.0 a SMB 2.0 (che userebbe l'accesso non crittografato). Non impedisce però un downgrade a SMB 1.0, dando comunque luogo all'accesso non crittografato. Per essere certo che i client SMB 3.0 usino sempre Crittografia SMB per accedere alle condivisioni crittografate, devi disabilitare il server SMB 1.0. Per le istruzioni, vedi la sezione [Disabilitazione di SMB 1.0](#disabling-smb-10). Se l'impostazione **–RejectUnencryptedAccess** viene lasciata configurata sul valore predefinito ( **$true**), solo i client SMB 3.0 che supportano la crittografia possono accedere alle condivisioni file (verranno rifiutati anche i client SMB 1.0).

>[!NOTE]
>* Crittografia SMB usa l'algoritmo Advanced Encryption Standard (AES)-CCM per crittografare e decrittografare i dati. AES-CCM fornisce anche la convalida dell'integrità dei dati (firma) per le condivisioni file crittografate, indipendentemente dalle impostazioni di firma SMB. Se vuoi abilitare la firma SMB senza crittografia, puoi farlo. Per altre informazioni, vedi [The Basics of SMB Signing](https://blogs.technet.microsoft.com/josebda/2010/12/01/the-basics-of-smb-signing-covering-both-smb1-and-smb2/) (Introduzione alla firma SMB).
>* Potrebbero verificarsi problemi quando tenti di accedere alla condivisione file o al server se l'organizzazione usa appliance di accelerazione WAN (Wide Area Network).
>* Con una configurazione predefinita (in cui non è consentito alcun accesso non crittografato alle condivisioni file crittografate), se i client che non supportano SMB 3.0 tentano di accedere a una condivisione file crittografata, l'ID evento 1003 viene registrato nel registro eventi Microsoft-Windows-SmbServer/Operational e il client riceverà il messaggio di errore **Accesso negato**.
>* Crittografia SMB e la tecnologia EFS (Encrypting File System) nel file system NTFS non sono correlate e la prima non richiede o dipende dall'uso della seconda.
>* Crittografia SMB e Crittografia unità BitLocker non sono correlate e la prima non richiede o dipende dall'uso della seconda.

## <a name="secure-dialect-negotiation"></a>Negoziazione sicura dei dialetti

SMB 3.0 è in grado di rilevare attacchi man-in-the-middle che tentano di effettuare il downgrade del protocollo SMB 2.0 o SMB 3.0 oppure delle funzionalità negoziate dal client e dal server. Quando il client o il server rileva un attacco di questo tipo, la connessione viene interrotta e viene registrato l'ID evento 1005 nel registro eventi Microsoft-Windows-SmbServer/Operational. La negoziazione sicura dei dialetti non è in grado di rilevare o impedire i downgrade da SMB 2.0 o 3.0 a SMB 1.0. Per questo motivo, e per sfruttare le funzionalità complete di Crittografia SMB, è consigliabile disabilitare il server SMB 1.0. Per altre informazioni, vedi [Disabilitazione di SMB 1.0](#disabling-smb-10).

La funzionalità di negoziazione sicura dei dialetti descritta nella sezione seguente impedisce a un attacco man-in-the-middle di effettuare il downgrade di una connessione da SMB 3 a SMB 2 (che userebbe l'accesso non crittografato). Non impedisce però i downgrade a SMB 1, dando comunque luogo all'accesso non crittografato. Per altre informazioni sui potenziali problemi con le implementazioni non Windows di SMB di versioni precedenti, vedi la [Microsoft Knowledge Base](https://support.microsoft.com/kb/2686098).

## <a name="new-signing-algorithm"></a>Nuovo algoritmo di firma

SMB 3.0 usa un algoritmo di crittografia più recente per la firma: Advanced Encryption Standard (AES)-Cipher-based Message Authentication Code (CMAC). SMB 2.0 usa l'algoritmo di crittografia HMAC-SHA256 meno recente. AES-CMAC e AES-CCM possono accelerare in modo significativo la crittografia dei dati sulle CPU più moderne con supporto per le istruzioni AES. Per altre informazioni, vedi [The Basics of SMB Signing](https://blogs.technet.microsoft.com/josebda/2010/12/01/the-basics-of-smb-signing-covering-both-smb1-and-smb2/) (Introduzione alla firma SMB).

## <a name="disabling-smb-10"></a>Disabilitazione di SMB 1.0

Il servizio browser di computer legacy e le funzionalità RAP (Remote Administration Protocol) di SMB 1.0 ora sono separate e possono essere eliminate. Queste funzionalità sono ancora abilitate per impostazione predefinita, ma se non sono disponibili client SMB meno recenti, ad esempio computer che eseguono Windows Server 2003 o Windows XP, puoi rimuovere le funzionalità SMB 1.0 per una maggiore sicurezza e per ridurre potenzialmente l'applicazione di patch.

>[!NOTE]
>SMB 2.0 è stato introdotto in Windows Server 2008 e Windows Vista. I client meno recenti, ad esempio i computer che eseguono Windows Server 2003 o Windows XP, non supportano SMB 2.0, pertanto non saranno in grado di accedere alle condivisioni file o alle condivisioni di stampa se il server SMB 1.0 è disabilitato. Inoltre, alcuni client SMB non Microsoft potrebbero non essere in grado di accedere alle condivisioni file o alle condivisioni di stampa SMB 2.0, ad esempio le stampanti con la funzionalità "Scan to Share".

Prima di iniziare a disabilitare SMB 1.0, dovrai verificare se i client SMB sono attualmente connessi al server che esegue SMB 1.0. A tale scopo, immetti il cmdlet seguente in Windows PowerShell:

```PowerShell
Get-SmbSession | Select Dialect,ClientComputerName,ClientUserName | ? Dialect -lt 2
```

>[!NOTE]
>È consigliabile eseguire questo script ripetutamente nel corso di una settimana (più volte ogni giorno) per creare un audit trail. Potresti eseguirlo anche come attività pianificata.

Per disabilitare SMB 1.0, immetti lo script seguente in Windows PowerShell:

```PowerShell
Set-SmbServerConfiguration –EnableSMB1Protocol $false
```

>[!NOTE]
>Se una connessione client SMB viene negata perché il server che esegue SMB 1.0 è stato disabilitato, l'ID evento 1001 verrà registrato nel registro eventi Microsoft-Windows-SmbServer/Operational.

## <a name="more-information"></a>Ulteriori informazioni

Di seguito sono riportate alcune risorse aggiuntive su SMB e su tecnologie correlate in Windows Server 2012.

- [Server Message Block](file-server-smb-overview.md)
- [Archiviazione in Windows Server](../storage.md)
- [File server di scalabilità orizzontale per dati delle applicazioni](../../failover-clustering/sofs-overview.md)