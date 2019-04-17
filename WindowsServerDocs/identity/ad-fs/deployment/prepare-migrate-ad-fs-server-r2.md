---
title: Preparare la migrazione del Server ADFS 2.0 federazione per AD FS in Windows Server 2012 R2
description: Fornisce informazioni sui preparativi per la migrazione del server AD FS per Windows Server 2012 R2.
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4d0ff53b9118db1dd6ba5af94b3e627bf1597e0c
ms.sourcegitcommit: 03ce78a1624dbd7f4e6abf2ec1ef185b55de29a1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/12/2017
---
# <a name="prepare-to-migrate-the-ad-fs-20-federation-server-to-ad-fs-on-windows-server-2012-r2"></a>Preparare la migrazione del Server ADFS 2.0 federazione per AD FS in Windows Server 2012 R2

Questo documento descrive come eseguire la migrazione di un ADFS 2.0 o una server farm federativa di Windows Server 2012 a una farm di Windows Server 2012 R2 AD ADFS.  I passaggi possono essere usati con farm di ADFS che utilizzano database interno di Windows o SQL Server come database sottostante.  
  
-   [Panoramica del processo di migrazione](prepare-migrate-ad-fs-server-r2.md#migrate-process-outline)  
  
-   [Nuove funzionalità di ADFS in Windows Server 2012 R2](prepare-migrate-ad-fs-server-r2.md#new-ad-fs-functionality-in-windows-server-2012-r2)  
  
-   [Requisiti per ADFS in Windows Server 2012 R2](prepare-migrate-ad-fs-server-r2.md#ad-fs-requirements-in-windows-server-2012-r2)  
  
-   [Aumentare i limiti di Windows PowerShell](prepare-migrate-ad-fs-server-r2.md#increasing-your-windows-powershell-limits)  
  
-   [Altre attività di migrazione e considerazioni](prepare-migrate-ad-fs-server-r2.md#other-migration-tasks-and-considerations)  
  
##  <a name="migration-process-outline"></a>Panoramica del processo di migrazione  
 Per completare la migrazione della server farm federativa ADFS in Windows Server 2012 R2, è necessario completare le attività seguenti:  
  
1.  Esportare, registrare ed eseguire il backup di dati di configurazione seguenti nella farm ADFS esistente. Per istruzioni dettagliate su come eseguire queste attività, vedere [la migrazione del Server federativo ADFS](migrate-ad-fs-fed-server-r2.md).  
  
Le seguenti impostazioni vengano migrate con gli script presenti nella cartella \support\adfs nel CD di installazione di Windows Server 2012 R2:  
  
-   Provider di attestazioni, ad eccezione delle regole attestazioni personalizzate nell'attendibilità del provider di attestazioni di Active Directory. Per ulteriori informazioni, vedere [la migrazione del Server federativo ADFS](migrate-ad-fs-fed-server-r2.md).  
  
-   Trust della relying party.  
  
-   AD FS generati internamente, la firma di token autofirmati e i certificati di decrittografia di token.  
  
Le seguenti impostazioni personalizzate migrazione deve essere eseguite manualmente:  
  
 -   Impostazioni del servizio:  
  
     -   La firma di token non predefiniti e i certificati di decrittografia di token rilasciati da un'autorità di certificazione pubblica enterprise.  
  
     -   Certificato di autenticazione server SSL utilizzato da ADFS.  
  
     -   Il certificato di comunicazioni di servizio utilizzato da ADFS (per impostazione predefinita, questo è lo stesso certificato come certificato SSL.  
  
      -   Valori non predefiniti per qualsiasi proprietà servizio federativo, ad esempio AutoCertificateRollover o durata SSO.  
  
      -   Impostazioni di endpoint ADFS non predefiniti e descrizioni di attestazioni.  
  
-   Regole attestazione personalizzate nell'attendibilità del provider di attestazioni Active Directory.  
  
    -   Personalizzazioni di pagina di accesso AD ADFS  
  
Per ulteriori informazioni, vedere [la migrazione del Server federativo ADFS](migrate-ad-fs-fed-server-r2.md).  
  
2.  Creare una server farm federativa di Windows Server 2012 R2.  
  
3.  Importare i dati di configurazione originali nella nuova farm Windows Server 2012 R2 AD ADFS.  
  
4.  Configurare e personalizzare le pagine di accesso AD FS.  
  
##  <a name="new-ad-fs-functionality-in-windows-server-2012-r2"></a>Nuove funzionalità di ADFS in Windows Server 2012 R2  
 Modifica le seguenti funzionalità di ADFS in Windows Server 2012 R2 impatto una migrazione da ADFS 2.0 o ADFS in Windows Server 2012:  
  
**Dipendenza da IIS**  
   - ADFS in Windows Server 2012 R2 è Self-hosted e non richiede l'installazione di IIS. Assicurarsi di annotare il comando seguente in seguito a questa modifica:  
   -   Gestione dei certificati SSL per i server federativi e i computer proxy nella farm ADFS deve ora essere eseguita tramite Windows PowerShell.  
  
**Modifiche alle impostazioni e le personalizzazioni degli ADFS pagine di accesso**  
-   In ADFS in Windows Server 2012 R2, ci sono state introdotte varie modifiche progettate per migliorare l'esperienza di accesso per amministratori e utenti. Le pagine web ospitate da IIS che erano presenti nella versione precedente di ADFS sono ora rimosse. L'aspetto di AD FS sign-in pagine web sono self-hosted in ADFS e possono ora essere personalizzate per personalizzare l'esperienza utente. Le modifiche includono:  
    -   Personalizzazione di AD FS sign-in, inclusa la personalizzazione del nome della società, logo, immagine e descrizione di accesso.  
    -   Personalizzare i messaggi di errore.  
    -   Personalizzazione dell'esperienza di ADFS Home Realm Discovery, che include quanto segue:  
        -   Configurazione del provider di identità per l'utilizzo di suffissi di posta elettronica.  
        -   Configurazione di un elenco di provider di identità per ogni relying party.  
        -   Per la rete intranet, ignorando Home Realm Discovery.  
        -   Creazione di temi web personalizzati.  
  
Per istruzioni dettagliate su come configurare l'aspetto delle pagine di accesso di ADFS, vedere [Customizing the AD FS Sign-in Pages](../operations/AD-FS-Customization-in-Windows-Server-2016.md).  
  
Se hai la personalizzazione della pagina web della farm ADFS esistente che si desidera eseguire la migrazione a Windows Server 2012 R2, è possibile ricrearle nell'ambito del processo di migrazione utilizzando le nuove funzionalità di personalizzazione in Windows Server 2012 R2.  
  
-   **Altre modifiche**  
  
    -   ADFS in Windows Server 2012 R2 si basa su Windows Identity Foundation (WIF) 3.5, non su WIF 4.5. Pertanto, alcune funzionalità specifiche di WIF 4.5 (ad esempio, le attestazioni Kerberos e controllo dinamico degli accessi) non sono supportate in ADFS in Windows Server 2012 R2.  
  
    -   Servizio DRS (Device Registration) in Windows Server 2012 R2 opera sulla porta 443; ClientTLS per l'autenticazione dei certificati utente opera sulla porta 49443  
  
        -   Per i client attivi, non basate su browser autenticazione modalità trasporto dei certificati che sono in particolare hardcoded per puntare alla porta 443, una modifica del codice è necessario per continuare a utilizzare l'autenticazione del certificato utente sulla porta 49443.  
  
        -   Per le applicazioni passive è richiesta alcuna modifica perché ADFS gestisce il reindirizzamento alla porta corretta per l'autenticazione del certificato utente.  
  
        -   Le porte del firewall tra il client e il proxy devono consentire il traffico 49443 la porta pass-through per l'autenticazione del certificato utente.  
  
##  <a name="ad-fs-requirements-in-windows-server-2012-r2"></a>Requisiti per ADFS in Windows Server 2012 R2  
 Per eseguire correttamente la migrazione della farm ADFS a Windows Server 2012 R2, è necessario soddisfare i requisiti seguenti:  
  
 Per AD FS alla funzione, ogni computer che si desidera essere una federazione devono essere aggiunti a un dominio.  
  
 Per ADFS in esecuzione in Windows Server 2012 R2 a funzione, il dominio Active Directory deve eseguire una delle seguenti:  
  
-   Windows Server 2012 R2  
  
-   Windows Server 2012  
  
-   Windows Server 2008 R2  
  
-   Windows Server 2008  
  
 Se si prevede di utilizzare un gruppo di Account del servizio gestito (gMSA) come account del servizio per ADFS, è necessario disporre di almeno un controller di dominio nell'ambiente in cui è in esecuzione nel sistema operativo Windows Server 2012 o Windows Server 2012 R2.  
  
 Se si prevede di distribuire servizio DRS (Device Registration) per l'aggiunta all'area di lavoro di Active Directory come parte della distribuzione di ADFS, lo schema di Active Directory deve essere aggiornato a livello di Windows Server 2012 R2. Esistono tre modi per aggiornare lo schema:  
  
1.  In una foresta di Active Directory esistente, eseguire adprep /forestprep dalla cartella \support\adprep del sistema operativo Windows Server 2012 R2 DVD in qualsiasi server a 64 bit che esegue Windows Server 2008 o versione successiva. In questo caso, nessun controller di dominio aggiuntivo deve essere installato e nessun controller di dominio esistenti devono essere aggiornate.  
  
Per eseguire adprep/forestprep, è necessario essere un membro del gruppo Schema Admins, il gruppo Enterprise Admins e il gruppo Domain Admins del dominio che ospita il master schema.  
  
2.  In una foresta di Active Directory esistente, installare un controller di dominio che esegue Windows Server 2012 R2. In questo caso, adprep /forestprep viene eseguita automaticamente come parte dell'installazione del controller di dominio.  
  
Durante l'installazione di controller di dominio, è necessario specificare credenziali aggiuntive per eseguire adprep /forestprep.  
  
3.  Creare una nuova foresta di Active Directory installando Servizi di dominio Active Directory in un server che esegue Windows Server 2012 R2. In questo caso, adprep /forestprep non è necessario eseguire perché lo schema viene creato inizialmente con tutti i contenitori necessari e gli oggetti per il supporto di DRS.  
  
### <a name="sql-server-support-for-ad-fs-in-windows-server-2012-r2"></a>Supporto di SQL Server per AD FS in Windows Server 2012 R2  
 Se si desidera creare una farm ADFS e utilizzare SQL Server per archiviare i dati di configurazione, è possibile utilizzare SQL Server 2008 e versioni successive, incluso SQL Server 2012.  
  
##  <a name="increasing-your-windows-powershell-limits"></a>Aumentare i limiti di Windows PowerShell  
 Se hai più di 1000 attendibilità del provider di attestazioni e attendibilità componente nella farm ADFS, o se viene visualizzato il seguente errore durante il tentativo di eseguire lo strumento di esportazione/importazione migrazione di ADFS, è necessario aumentare i limiti di Windows PowerShell:  
  
```  
'Exception of type 'System.OutOfMemoryException' was thrown. At E:\dev\ds\security\ADFSv2\Product\Migration\Export-FederationConfiguration.ps1:176 char:21 + $configData = Invoke-Command -ScriptBlock $GetConfig -Argume ...  
```  
  
 Questo errore si verifica perché il limite di memoria predefinita di sessione di Windows PowerShell è troppo basso. In Windows PowerShell 2.0, la memoria predefinita per la sessione è 150MB. In Windows PowerShell 3.0, la memoria di sessione predefinita è 1024MB. È possibile verificare limite di memoria sessione remota di Windows PowerShell utilizzando il comando seguente:`Get-Item wsman:localhost\Shell\MaxMemoryPerShellMB`. È possibile aumentare il limite eseguendo il comando seguente:`Set-Item wsman:localhost\Shell\MaxMemoryPerShellMB 512`.  
  
## <a name="other-migration-tasks-and-considerations"></a>Altre attività di migrazione e considerazioni  
 Per eseguire correttamente la migrazione della farm ADFS a Windows Server 2012 R2, assicurarsi di essere a conoscenza delle operazioni seguenti:  
  
-   Gli script di migrazione disponibili nella cartella \support\adfs nel CD di installazione di Windows Server 2012 R2 richiedono che vengano mantenuti lo stessi nomi di farm di server federativi e nome identità account del servizio utilizzati nella farm ADFS legacy alla migrazione a Windows Server 2012 R2.  
  
-   Se si desidera eseguire la migrazione di una farm SQL Server AD FS, tieni presente che il processo di migrazione implica la creazione di una nuova istanza di database SQL in cui è necessario importare i dati di configurazione originali.  
  
## <a name="next-steps"></a>Passaggi successivi
 [Eseguire la migrazione di ruolo di Active Directory Federation Services a Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [La migrazione del Server federativo ADFS](migrate-ad-fs-fed-server-r2.md)   
 [La migrazione di Proxy Server federativo AD FS](migrate-fed-server-proxy-r2.md)   
 [Verifica della migrazione di ADFS a Windows Server 2012 R2](verify-ad-fs-migration.md)