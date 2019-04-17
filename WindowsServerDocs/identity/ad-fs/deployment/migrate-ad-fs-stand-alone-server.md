---
title: Eseguire la migrazione di un server federativo ADFS autonomo o una farm ADFS a nodo singolo
description: Fornisce informazioni sulla migrazione di un server ADFS 2.0 supporto AD da solo o a nodo singolo a Windows Server 2012
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 10371349fe19be92fb997c9c28f19def0ecad7e9
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="migrate-a-stand-alone-ad-fs-federation-server-or-a-single-node-ad-fs-farm"></a>Eseguire la migrazione di un server federativo ADFS autonomo o una farm ADFS a nodo singolo  
Questo documento fornisce informazioni dettagliate sulla migrazione di un server di ADFS 2.0 supporto solo a Windows Server 2012.

## <a name="migrate-a-stand-alone-ad-fs-20-server"></a>Eseguire la migrazione di un annuncio autonomo server ADFS 2.0

Utilizzare la procedura seguente per eseguire la migrazione AD FS 2.0 server a Windows Server 2012.
  
1.  Rivedere ed eseguire le procedure descritte in [preparare la migrazione di un server federativo ADFS autonomo o una farm ADFS a nodo singolo](prepare-to-migrate-a-stand-alone-ad-fs-federation-server.md).  
  
2.  Eseguire un aggiornamento sul posto del sistema operativo del server da Windows Server 2008 R2 o Windows Server 2008 a Windows Server 2012. Per ulteriori informazioni, vedere [l'installazione di Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).  
  
> [!IMPORTANT]
>  In seguito all'aggiornamento del sistema operativo, la configurazione di ADFS nel server andrà persa e il ruolo di AD FS 2.0 server viene rimosso. Suo posto verrà installato il ruolo server ADFS di Windows Server 2012, ma non è configurato. Manualmente, è necessario creare la configurazione originale di ADFS e ripristinare le impostazioni di ADFS rimanenti per completare la migrazione del server federativo.  
  
3.  Creare la configurazione originale di ADFS. È possibile creare la configurazione originale di ADFS usando uno dei metodi seguenti:  
  
-   Utilizzare il **configurazione guidata Server federativo di AD FS** per creare un nuovo server federativo. Per ulteriori informazioni, vedere [creare il primo Server federativo in una Server Farm federativa](Create-the-First-Federation-Server-in-a-Federation-Server-Farm.md).  
  
Man mano che la procedura guidata, utilizzare le informazioni che raccolte durante la preparazione alla migrazione di server federativo ADFS come segue:  
  
 |**Opzione di input configurazione guidata Server federativo**|**Utilizzare il valore seguente**| 
|-----|-----| 
|**Certificato SSL** nel **specificare il nome del servizio federativo** pagina|Selezionare il certificato SSL, il cui nome soggetto e l'identificazione personale registrate durante la preparazione per la migrazione del server federativo AD FS.|  
|**Account del servizio** e **Password** nel **specificare un Account del servizio** pagina|Immettere le informazioni sull'account del servizio registrate durante la preparazione per la migrazione del server federativo AD FS. **Nota:** se si seleziona il server federativo autonomo nella seconda pagina della procedura guidata, il servizio di rete viene utilizzato automaticamente come account del servizio.|  
  
> [!IMPORTANT] 
> È possibile utilizzare questo metodo solo se si utilizza la Database interno di Windows (WID) per archiviare il database di configurazione di ADFS per il server federativo autonomo o una farm ADFS a nodo singolo.  
>
>  Se si utilizza SQL Server per archiviare il database di configurazione di ADFS per la farm ADFS a nodo singolo, è necessario utilizzare Windows PowerShell per creare la configurazione originale di ADFS nel server federativo.  
  
-   Utilizzare Windows PowerShell  
  
> [!IMPORTANT]
>  Se si utilizza SQL Server per archiviare il database di configurazione di ADFS per il server federativo autonomo o una farm ADFS a nodo singolo, è necessario utilizzare Windows PowerShell.  
  
Di seguito è riportato un esempio di come utilizzare Windows PowerShell per creare la configurazione originale di ADFS in un server federativo in una farm SQL Server a nodo singolo.  Aprire il modulo di Windows PowerShell ed eseguire il comando seguente: `$fscredential = Get-Credential`. Immettere il nome e la password dell'account del servizio registrate durante la preparazione della farm SQL server per la migrazione. Quindi eseguire il comando seguente: `C:\PS> Add-AdfsFarmNode -ServiceAccountCredential $fscredential -SQLConnectionString "Data Source=<Data Source>;Integrated Security=True"` dove `Data Source` è il valore dell'origine dati nel valore stringa connessione archivio criteri nel file seguente: `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config`.  
  
4.  Ripristinare le impostazioni del servizio AD FS rimanenti e relazioni di trust. Si tratta di un passaggio manuale durante il quale è possibile utilizzare i file esportato e i valori che sono stati raccolti durante la preparazione per la migrazione di ADFS. Per istruzioni dettagliate, vedere ripristinare la configurazione della Farm ADFS rimanente Active Directory.  
  
> [!NOTE]
>  Questo passaggio è necessario se si sta migrando un server federativo autonomo o una farm database interno di Windows a nodo singolo.  Se il server federativo Usa un database SQL Server come archivio di configurazione, le impostazioni di servizio e le relazioni di trust vengono conservate nel database.  
  
5.  Aggiornare le pagine Web di ADFS. Si tratta di un passaggio manuale. Se è stato eseguito il backup delle pagine di ADFS durante la preparazione per la migrazione, è possibile utilizzare i dati di backup per sovrascrivere le pagine Web di ADFS che sono state create per impostazione predefinita nel **%systemdrive%\inetpub\adfs\ls** directory in seguito alla configurazione di ADFS in Windows Server 2012.  
  
6.  Ripristinare eventuali personalizzazioni di ADFS rimanenti, ad esempio gli archivi attributi personalizzati.  
  
## <a name="restoring-the-remaining-ad-fs-farm-configuration"></a>Ripristinare la configurazione di ADFS Farm AD rimanente  
  
-   Ripristinare le seguenti impostazioni del servizio ADFS in una farm database interno di Windows a nodo singolo o un servizio federativo autonomo nel modo seguente:  
  
    -   Nella console di gestione di ADFS selezionare **servizio** e fare clic su **Modifica proprietà servizio federativo... **. Verificare le impostazioni del servizio federativo, controllando ognuno dei valori rispetto ai valori esportati nel file properties.txt durante la preparazione per la migrazione:  
  
    
|**Nome della proprietà servizio federativo restituito da Get-ADFSProperties**|**Nome della proprietà servizio federativo nella console di gestione di ADFS**|  
|-----|-----|
|DisplayName|Nome visualizzato del servizio federativo|  
|Nome host|Nome servizio federativo|  
|Identificatore|Identificatore del servizio federativo|  
  
-   Nella console di gestione di ADFS selezionare **certificati**. Verificare le comunicazioni di servizio, i certificati di decrittografia di token e di firma di token, controllando ognuno in base ai valori esportati nel file certificates.txt durante i preparativi per la migrazione.  
  
Per modificare i certificati di decrittografia di token o firma di token da certificati autofirmati predefiniti a certificati esterni, è innanzitutto necessario disabilitare la funzionalità di attivazione automatica del certificato che è abilitata per impostazione predefinita.  A tale scopo, è possibile utilizzare il comando di Windows PowerShell seguente: `PSH: Set-ADFSProperties –AutoCertificateRollover $false`.  
  
-   Nella console di gestione di ADFS, selezionare **endpoint**. Controllare l'endpoint ADFS abilitati confrontandoli con l'elenco degli endpoint ADFS abilitati esportato in un file durante la preparazione per la migrazione di ADFS.  
  
-   Nella console di gestione di ADFS, selezionare **descrizioni di attestazioni**. Controllare l'elenco di descrizioni di attestazioni AD FS in base all'elenco di descrizioni di attestazioni esportato in un file durante la preparazione per la migrazione di ADFS. Aggiungere eventuali descrizioni di attestazioni personalizzate incluse nel file ma non inclusi nell'elenco predefinito di ADFS.  Si noti che l'identificatore di attestazione nella console di gestione corrisponde a ClaimType nel file.  Per ulteriori informazioni sull'aggiunta di descrizioni di attestazioni, vedere [aggiungere una descrizione di attestazione](../operations/add-a-claim-description.md). Per ulteriori informazioni, vedere la sezione "Passaggio 1 – Esporta servizio impostazioni" prepararsi per la migrazione del Server federativo ADFS 2.0.  
  
-   Nella console di gestione di ADFS, selezionare **Provider di attestazioni**. È necessario ricreare ogni trust di Provider di attestazioni manualmente tramite il **Aggiunta guidata attendibilità Provider di attestazioni**.  Usare l'elenco di provider di attestazioni esportato e registrato durante la preparazione per la migrazione di ADFS. Poiché si tratta di attendibilità provider di attestazioni "Active Directory" che fa parte del valore predefinito di configurazione di ADFS, è possibile ignorare il trust del provider di attestazioni con identificatore "Autorità AD" nel file.  Verificare tuttavia eventuali regole attestazione personalizzate che aggiunte per la relazione di trust di Active Directory prima della migrazione. Per ulteriori informazioni sulla creazione di provider di attestazioni, vedere [creare un'attestazioni Provider attendibilità usando metadati federativi](../operations/create-a-claims-provider-trust.md#to-create-a-claims-provider-trust-using-federation-metadata) o [creare un'attestazioni Provider attendibilità manualmente](../operations/create-a-claims-provider-trust.md#to-create-a-claims-provider-trust-manually).  
  
-   Nella console di gestione di ADFS, **selezionare l'attendibilità**. È necessario ricreare ogni trust della Relying Party manualmente usando il **Aggiunta guidata attendibilità componente**. Usare l'elenco di trust della relying party esportato e registrato durante la preparazione per la migrazione di ADFS. Per ulteriori informazioni sulla creazione di trust della relying party, vedere [creare una Relying Party Trust usando metadati federativi](../operations/create-a-relying-party-trust.md#to-create-a-claims-aware-relying-party-trust-using-federation-metadata) o [creare una Relying Party Trust manualmente](../operations/create-a-relying-party-trust.md#to-create-a-claims-aware-relying-party-trust-manually). 

## <a name="next-steps"></a>Passaggi successivi
 [Preparare la migrazione di Active Directory Server federativo ADFS 2.0](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparare la migrazione di AD FS 2.0 Server federativo](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Eseguire la migrazione di Active Directory Server federativo ADFS 2.0](migrate-the-ad-fs-fed-server.md)   
 [Eseguire la migrazione di AD FS 2.0 Server federativo](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Eseguire la migrazione di Active Directory agenti Web ADFS 1.1](migrate-the-ad-fs-web-agent.md)




-   
-    