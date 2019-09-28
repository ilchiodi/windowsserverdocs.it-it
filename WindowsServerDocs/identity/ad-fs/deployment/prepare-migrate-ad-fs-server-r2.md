---
title: Preparare la migrazione del server federativo AD FS 2,0 AD FS in Windows Server 2012 R2
description: Fornisce informazioni su come preparare la migrazione del server di AD FS a Windows Server 2012 R2.
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 149e3d3fc4d4eee22fa9330475f0eed9d945f8b9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359314"
---
# <a name="prepare-to-migrate-the-ad-fs-20-federation-server-to-ad-fs-on-windows-server-2012-r2"></a>Preparare la migrazione del server federativo AD FS 2,0 AD FS in Windows Server 2012 R2

In questo documento viene descritto come eseguire la migrazione di un server farm di Federazione AD FS 2,0 o Windows Server 2012 a una farm di AD FS Windows Server 2012 R2.  I passaggi possono essere utilizzati con AD FS farm che utilizzano WID o SQL Server come database sottostante.  
  
-   [Struttura del processo di migrazione](prepare-migrate-ad-fs-server-r2.md#migration-process-outline)  
  
-   [Nuove funzionalità di AD FS in Windows Server 2012 R2](prepare-migrate-ad-fs-server-r2.md#new-ad-fs-functionality-in-windows-server-2012-r2)  
  
-   [Requisiti di AD FS in Windows Server 2012 R2](prepare-migrate-ad-fs-server-r2.md#ad-fs-requirements-in-windows-server-2012-r2)  
  
-   [Aumento dei limiti di Windows PowerShell](prepare-migrate-ad-fs-server-r2.md#increasing-your-windows-powershell-limits)  
  
-   [Altre attività e considerazioni sulla migrazione](prepare-migrate-ad-fs-server-r2.md#other-migration-tasks-and-considerations)  
  
##  <a name="migration-process-outline"></a>Panoramica del processo di migrazione

 Per completare la migrazione della farm di server federativi di ADFS a Windows Server 2012 R2, è necessario eseguire le attività seguenti:  
  
1.  Esportare, registrare ed eseguire il backup dei dati di configurazione seguenti nella farm ADFS esistente. Per istruzioni dettagliate su come eseguire queste attività, vedere [Migrazione del server federativo di ADFS](migrate-ad-fs-fed-server-r2.md).  
  
Le impostazioni seguenti vengono migrate con gli script disponibili nella cartella \support\adfs del CD di installazione di Windows Server 2012 R2:  
  
-   Attendibilità del provider di attestazioni, con l'eccezione delle regole attestazione personalizzate nell'attendibilità del provider di attestazioni di Active Directory. Per ulteriori informazioni, vedere [Migrazione del server federativo di ADFS](migrate-ad-fs-fed-server-r2.md).  
  
-   Attendibilità del componente.  
  
-   Certificati di firma di token autofirmati e di decrittografia di token, generati internamente da ADFS.  
  
La migrazione di qualsiasi impostazione personalizzata tra le seguenti deve essere eseguita manualmente:  
  
- Impostazioni del servizio:  
  
  - Certificati di firma di token e di decrittografia di token non predefiniti, rilasciati da un'Autorità di certificazione dell'organizzazione (enterprise) o pubblica.  
  
  - Certificato di autenticazione del server SSL utilizzato da ADFS.  
  
  - Certificato per le comunicazioni di servizi utilizzato da ADFS (per impostazione predefinita è lo stesso certificato del certificato SSL).  
  
    -   Valori non predefiniti per qualsiasi proprietà del servizio federativo, ad esempio AutoCertificateRollover o durata SSO.  
  
    -   Impostazioni dell'endpoint AD FS non predefinite e descrizioni delle attestazioni.  
  
-   Regole attestazione personalizzate nell'attendibilità del provider di attestazioni Active Directory.  
  
    -   Personalizzazioni delle pagine di accesso ad ADFS  
  
Per ulteriori informazioni, vedere [Migrazione del server federativo di ADFS](migrate-ad-fs-fed-server-r2.md).  
  
2. Creare una server farm federativa di Windows Server 2012 R2.  
  
3. Importare i dati di configurazione originali nella nuova farm ADFS di Windows Server 2012 R2.  
  
4. Configurare e personalizzare le pagine di accesso ad ADFS.  
  
##  <a name="new-ad-fs-functionality-in-windows-server-2012-r2"></a>Nuove funzionalità di ADFS in Windows Server 2012 R2  
 Di seguito sono riportate le modifiche alle funzionalità AD FS in Windows Server 2012 R2 che influiscano sulla migrazione da AD FS 2,0 o AD FS in Windows Server 2012:  
  
**Dipendenza IIS**  
   - ADFS in Windows Server 2012 R2 è self-hosted e non richiede l'installazione di IIS. Assicurarsi di tenere presente quanto segue, in conseguenza di tale modifica:  
   -   La gestione del certificato SSL sia per i server federativi che per i computer proxy nella farm ADFS deve ora essere eseguita tramite Windows PowerShell.  
  
**Modifiche alle impostazioni e alle personalizzazioni delle pagine di accesso AD FS**  
-   In AD FS in Windows Server 2012 R2, sono state apportate alcune modifiche per migliorare l'esperienza di accesso sia per gli amministratori che per gli utenti. Le pagine Web ospitate da IIS nella versione precedente di ADFS sono ora rimosse. Le pagine di accesso ad ADFS sono ora self-hosted in ADFS ed è possibile personalizzarne l'aspetto e il funzionamento per adattarli all'esperienza utente. Le modifiche includono:  
    -   Personalizzazione dell'esperienza di accesso ad ADFS, inclusa la personalizzazione del nome della società, del logo, dell'illustrazione e della descrizione per l'accesso.  
    -   Personalizzazione dei messaggi di errore.  
    -   Personalizzazione dell'esperienza di individuazione dell'area di autenticazione principale di ADFS, che include quanto segue:  
        -   Configurazione del provider di identità per l'utilizzo di specifici suffissi per la posta elettronica.  
        -   Configurazione di un elenco dei provider di identità per ogni componente.  
        -   Esclusione dell'individuazione dell'area di autenticazione principale per la rete Intranet.  
        -   Creazione di temi Web personalizzati.  
  
Per istruzioni dettagliate su come configurare l'aspetto delle pagine di accesso AD FS, vedere [personalizzazione delle pagine](../operations/AD-FS-Customization-in-Windows-Server-2016.md)di accesso ad FS.  
  
Se nella farm AD FS esistente si desidera eseguire la migrazione a Windows Server 2012 R2, è possibile ricrearli come parte del processo di migrazione utilizzando le nuove funzionalità di personalizzazione di Windows Server 2012 R2.  
  
-   **Altre modifiche**  
  
    -   AD FS in Windows Server 2012 R2 si basa su Windows Identity Foundation (WIF) 3,5, non su WIF 4,5. Pertanto, alcune funzionalità specifiche di WIF 4,5 (ad esempio, le attestazioni Kerberos e il controllo dinamico degli accessi) non sono supportate in AD FS in Windows Server 2012 R2.  
  
    -   Il servizio DRS (Device Registration Service) in Windows Server 2012 R2 opera sulla porta 443; ClientTLS per l'autenticazione dei certificati utente opera sulla porta 49443  
  
        -   Per i client non browser attivi che utilizzano l'autenticazione della modalità di trasporto dei certificati, hardcoded in modo specifico per l'utilizzo della porta 443, è richiesta una modifica del codice per continuare a utilizzare l'autenticazione dei certificati sulla porta 49443.  
  
        -   Per le applicazioni passive non è richiesta alcuna modifica, perché ADFS gestisce il reindirizzamento alla porta corretta per l'autenticazione dei certificati utente.  
  
        -   Le porte del firewall tra il client e il proxy devono abilitare il pass-through del traffico sulla porta 49443 per l'autenticazione dei certificati utente.  
  
##  <a name="ad-fs-requirements-in-windows-server-2012-r2"></a>Requisiti per ADFS in Windows Server 2012 R2  
 Per eseguire correttamente la migrazione della farm AD FS a Windows Server 2012 R2, è necessario soddisfare i requisiti seguenti:  
  
 Per il funzionamento di AD FS, ogni computer che si desidera essere una federazione deve essere aggiunto a un dominio.  
  
 Per il funzionamento di AD FS in esecuzione in Windows Server 2012 R2, il dominio Active Directory deve eseguire una delle operazioni seguenti:  
  
- Windows Server 2012 R2  
  
- Windows Server 2012  
  
- Windows Server 2008 R2  
  
- Windows Server 2008  
  
  Se si prevede di utilizzare un account del servizio gestito del gruppo (gMSA) come account del servizio per AD FS, è necessario disporre di almeno un controller di dominio nell'ambiente in cui è in esecuzione nel sistema operativo Windows Server 2012 o Windows Server 2012 R2.  
  
  Se si prevede di distribuire il servizio DRS (Device Registration Service) per AD Workplace Join come parte della distribuzione di AD FS, è necessario aggiornare lo schema di servizi di dominio Active Directory al livello Windows Server 2012 R2. Esistono tre modi per aggiornare lo schema:  
  
1.  In una foresta Active Directory esistente eseguire adprep/forestprep dalla cartella \support\adprep del DVD del sistema operativo Windows Server 2012 R2 in qualsiasi server a 64 bit che esegue Windows Server 2008 o versione successiva. In questo caso, non è necessario installare alcun controller di dominio aggiuntivo o aggiornare i controller di dominio esistenti.  
  
Per eseguire adprep/forestprep, è necessario essere membri del gruppo Schema Admins, Enterprise Admins e Domain Admins del dominio che ospita il master schema.  
  
2. In una foresta Active Directory esistente, installare un controller di dominio che esegue Windows Server 2012 R2. In questo caso, il comando adprep /forestprep viene eseguito automaticamente nell'ambito dell'installazione del controller di dominio.  
  
Durante l'installazione del controller di dominio, potrebbe essere necessario specificare credenziali aggiuntive per eseguire adprep /forestprep.  
  
3. Creare una nuova foresta Active Directory installando servizi di dominio Active Directory in un server che esegue Windows Server 2012 R2. In questo caso, non è necessario eseguire adprep/forestprep perché lo schema verrà inizialmente creato con tutti i contenitori e gli oggetti necessari per supportare DRS.  
  
### <a name="sql-server-support-for-ad-fs-in-windows-server-2012-r2"></a>Supporto di SQL Server per ADFS in Windows Server 2012 R2  
 Se si desidera creare una farm ADFS e utilizzare SQL Server per archiviare i dati di configurazione, è possibile utilizzare SQL Server 2008 e versioni successive, incluso SQL Server 2012.  
  
##  <a name="increasing-your-windows-powershell-limits"></a>Aumento dei limiti di Windows PowerShell  
 Se la farm ADFS include più di 1000 attendibilità di provider di attestazioni e del componente oppure se viene visualizzato l'errore seguente quando si tenta di eseguire lo strumento di esportazione/importazione per la migrazione di ADFS, è necessario aumentare i limiti di Windows PowerShell:  
  
```  
'Exception of type 'System.OutOfMemoryException' was thrown. At E:\dev\ds\security\ADFSv2\Product\Migration\Export-FederationConfiguration.ps1:176 char:21 + $configData = Invoke-Command -ScriptBlock $GetConfig -Argume ...  
```  
  
 Questo errore si verifica perché il limite di memoria predefinito per la sessione di Windows PowerShell è troppo basso. In Windows PowerShell 2.0, la memoria predefinita per la sessione è 150 MB. In Windows PowerShell 3,0 la memoria predefinita della sessione è 1024MB. È possibile utilizzare il comando seguente per verificare il limite di memoria per la sessione remota di Windows PowerShell: `Get-Item wsman:localhost\Shell\MaxMemoryPerShellMB`. Per aumentare il limite, è possibile eseguire il comando seguente: `Set-Item wsman:localhost\Shell\MaxMemoryPerShellMB 512`.  
  
## <a name="other-migration-tasks-and-considerations"></a>Altre attività e considerazioni per la migrazione  
 Per eseguire correttamente la migrazione della farm ADFS a Windows Server 2012 R2, assicurarsi di tenere presente quanto segue:  
  
-   Per gli script di migrazione disponibili nella cartella \support\adfs del CD di installazione di Windows Server 2012 R2, è necessario mantenere lo stesso nome server farm federazione e il nome di identità dell'account del servizio utilizzati nella farm di AD FS legacy quando si esegue la migrazione a Windows Server 2012 R2.  
  
-   Se si desidera eseguire la migrazione di una farm ADFS di SQL Server, tenere presente che il processo di migrazione implica la creazione di una nuova istanza del database di SQL Server, nel quale è necessario importare i dati di configurazione originali.  
  
## <a name="next-steps"></a>Passaggi successivi
 [Eseguire la migrazione dei servizi ruolo Active Directory Federation Services a Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [Migrazione del server federativo AD FS](migrate-ad-fs-fed-server-r2.md)   
 [Migrazione del proxy server federativo di AD FS](migrate-fed-server-proxy-r2.md)   
 [Verifica della migrazione del AD FS a Windows Server 2012 R2](verify-ad-fs-migration.md)