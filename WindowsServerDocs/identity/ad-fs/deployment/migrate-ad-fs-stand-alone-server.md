---
title: Eseguire la migrazione di un server federativo AD FS autonomo o di una farm di AD FS a nodo singolo
description: Fornisce informazioni sulla migrazione di un server autonomo o a nodo singolo AD FS 2,0 a Windows Server 2012
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: b8029d67a9f21e5189322692b8f1316306542c96
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359379"
---
# <a name="migrate-a-stand-alone-ad-fs-federation-server-or-a-single-node-ad-fs-farm"></a>Eseguire la migrazione di un server federativo AD FS autonomo o di una farm di AD FS a nodo singolo  
In questo documento vengono fornite informazioni dettagliate sulla migrazione di un server autonomo AD FS 2,0 a Windows Server 2012.

## <a name="migrate-a-stand-alone-ad-fs-20-server"></a>Eseguire la migrazione di un server AD FS autonomo 2,0

Usare la procedura seguente per eseguire la migrazione del server AD FS 2,0 a Windows Server 2012.
  
1.  Rivedere ed eseguire le procedure descritte in [preparare la migrazione di un server federativo ad FS autonomo o di una farm di ad FS a nodo singolo](prepare-to-migrate-a-stand-alone-ad-fs-federation-server.md).  
  
2.  Eseguire un aggiornamento sul posto del sistema operativo del server da Windows Server 2008 R2 o Windows Server 2008 a Windows Server 2012. Per altre informazioni, vedere [Installing Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).  
  
> [!IMPORTANT]
>  In seguito all'aggiornamento del sistema operativo, la configurazione del AD FS in questo server andrà persa e il ruolo del server AD FS 2,0 verrà rimosso. Viene invece installato il ruolo server AD FS Windows Server 2012, ma non è configurato. Per completare la migrazione del server federativo, è necessario creare manualmente la configurazione di AD FS originale e ripristinare le impostazioni del AD FS rimanenti.  
  
3. Creare la configurazione originale del AD FS. È possibile creare la configurazione di AD FS originale usando uno dei metodi seguenti:  
  
-   Utilizzare la **Configurazione guidata server federativo di ad FS** per creare un nuovo server federativo. Per altre informazioni, vedere [Creare il primo server federativo in una server farm federativa](Create-the-First-Federation-Server-in-a-Federation-Server-Farm.md).  
  
Man mano che la procedura guidata procede, usare le informazioni raccolte durante la preparazione alla migrazione del server federativo ADFS come segue:  
  
 |**Opzione di input della configurazione guidata del server federativo**|**Usare il valore seguente**| 
|-----|-----| 
|**Certificato SSL** nella pagina **Specifica il nome del servizio federativo**|Selezionare il certificato SSL con il nome soggetto e l'identificazione personale registrati durante la preparazione per la migrazione del server federativo di ADFS.|  
|**Account del servizio** e **Password** nella pagina **Specifica account del servizio**|Immettere le informazioni sull'account del servizio registrate durante la preparazione per la migrazione del server federativo di ADFS. **Nota:**  Se si seleziona server federativo autonomo nella seconda pagina della procedura guidata, servizio di rete viene utilizzato automaticamente come account del servizio.|  
  
> [!IMPORTANT] 
> È possibile utilizzare questo metodo solo se si utilizza database interno di Windows per archiviare il database di configurazione di AD FS per il server federativo autonomo o una farm di AD FS a nodo singolo.  
>
>  Se si utilizza SQL Server per archiviare il database di configurazione AD FS per la farm AD FS a nodo singolo, è necessario utilizzare Windows PowerShell per creare la configurazione originale del AD FS nel server federativo.  
  
-   Utilizzare Windows PowerShell  
  
> [!IMPORTANT]
>  È necessario utilizzare Windows PowerShell se si utilizza SQL Server per archiviare il database di configurazione AD FS per il server federativo autonomo o una farm di AD FS a nodo singolo.  
  
Di seguito è riportato un esempio di come utilizzare Windows PowerShell per creare la configurazione originale del AD FS in un server federativo in una farm a nodo singolo SQL Server.  Aprire il modulo Windows PowerShell ed eseguire il comando seguente: `$fscredential = Get-Credential`. Immettere il nome e la password dell'account del servizio registrato durante la preparazione della farm SQL Server per la migrazione. Eseguire quindi il comando seguente: `C:\PS> Add-AdfsFarmNode -ServiceAccountCredential $fscredential -SQLConnectionString "Data Source=<Data Source>;Integrated Security=True"` dove `Data Source` è il valore dell'origine dati nel valore stringa della connessione all'archivio criteri nel file seguente: `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config`.  
  
4. Ripristinare le restanti impostazioni e relazioni di trust del servizio ADFS. Si tratta di un passaggio manuale durante cui è possibile usare i file esportati e i valori raccolti durante la preparazione per la migrazione di ADFS. Per istruzioni dettagliate, vedere Ripristinare la restante configurazione della farm ADFS.  
  
> [!NOTE]
>  Questo passaggio è richiesto solo se si sta migrando un server federativo autonomo o una farm Database interno di Windows a nodo singolo.  Se il server federativo usa un database di SQL Server come archivio di configurazione, le impostazioni del servizio e le relazioni di trust verranno conservate nel database.  
  
5. Aggiornare le pagine Web di AD FS. Si tratta di un passaggio manuale. Se è stato eseguito il backup delle pagine Web personalizzate AD FS durante la preparazione per la migrazione, usare i dati di backup per sovrascrivere le pagine Web predefinite AD FS create per impostazione predefinita nella directory **%SystemDrive%\Inetpub\Adfs\Ls** in seguito alla configurazione di ad FS in Windows Server 2012.  
  
6. Ripristinare le eventuali personalizzazioni di ADFS rimanenti, ad esempio gli archivi di attributi personalizzati.  
  
## <a name="restoring-the-remaining-ad-fs-farm-configuration"></a>Ripristino della configurazione rimanente della farm AD FS  
  
-   Ripristinare le impostazioni del servizio ADFS in una farm Database interno di Windows a nodo singolo o in un servizio federativo autonomo nel modo seguente:  
  
    -   Nella console di gestione di ADFS selezionare **Servizio** quindi fare clic su **Modifica proprietà servizio federativo**. Verificare le impostazioni del servizio federativo contro ognuno dei valori esportati nel file properties.txt durante la preparazione per la migrazione:  
  
    
|**Servizio federativo nome della proprietà come riportato da Get-ADFSProperties**|**Nome della proprietà Servizio federativo in AD FS Management Console**|  
|-----|-----|
|DisplayName|Nome visualizzato del servizio federativo|  
|HostName|Nome servizio federativo|  
|Identificatore|Identificatore del servizio federativo|  
  
-   Nella console di gestione ADFS selezionare **Certificati**. Verificare i certificati per le comunicazioni di servizi, di decrittografia di token e di firma di token, controllando ognuno in base ai valori esportati nel file certificates.txt durante i preparativi per la migrazione.  
  
Per cambiare i certificati di decrittografia di token o di firma di token da certificati autofirmati predefiniti a certificati esterni, è innanzitutto necessario disabilitare la funzionalità di rollover automatico dei certificati, abilitata per impostazione predefinita.  A tale scopo, è possibile usare il comando seguente di Windows PowerShell: `PSH: Set-ADFSProperties –AutoCertificateRollover $false`.  
  
-   Nella console di gestione ADFS selezionare **Endpoint**. Controllare gli endpoint ADFS abilitati confrontandoli con l'elenco degli endpoint ADFS abilitati esportato su file durante i preparativi della migrazione di ADFS.  
  
-   Nella console di gestione ADFS selezionare **Descrizioni attestazione**. Controllare l'elenco delle descrizioni di attestazioni ADFS confrontandole con l'elenco delle descrizioni esportato su file durante i preparativi della migrazione di ADFS. Aggiungere le eventuali descrizioni di attestazioni personalizzate incluse nel file, ma non nell'elenco predefinito in ADFS.  Si noti che l'identificatore di attestazione nella console di gestione corrisponde a ClaimType nel file.  Per altre informazioni sull'aggiunta di descrizioni di attestazioni, vedere [Aggiungere una descrizione di attestazione](../operations/add-a-claim-description.md). Per altre informazioni, vedere la sezione "Passaggio 1 - Esportare le impostazioni del servizio" dell'argomento Prepararsi per la migrazione del server federativo ADFS 2.0.  
  
-   Nella console di gestione di ADFS selezionare **Attendibilità provider di attestazioni**. È necessario ricreare manualmente i singoli trust del provider di attestazioni tramite l'**Aggiunta guidata attendibilità provider di attestazioni**.  Usare l'elenco di trust del provider di attestazioni esportato e registrato durante la preparazione per la migrazione di ADFS. Il trust del provider di attestazioni con identificatore "AUTORITÀ AD" presente nel file può essere ignorato, poiché si tratta del trust del provider di attestazioni "Active Directory", che fa parte della configurazione predefinita di ADFS.  Verificare tuttavia eventuali regole attestazione personalizzate che potrebbero essere state aggiunte al trust Active Directory prima della migrazione. Per altre informazioni sulla creazione di trust del provider di attestazioni, vedere [Creare un'attendibilità del provider di attestazioni usando metadati federativi](../operations/create-a-claims-provider-trust.md#to-create-a-claims-provider-trust-using-federation-metadata) oppure [Creare un'attendibilità del provider di attestazioni manualmente](../operations/create-a-claims-provider-trust.md#to-create-a-claims-provider-trust-manually).  
  
-   Nella console di gestione di ADFS selezionare **Attendibilità componente**. È necessario ricreare un trust della relying party manualmente usando l'**Aggiunta guidata attendibilità componente**. Usare l'elenco di trust della relying party esportato e registrato durante la preparazione per la migrazione di ADFS. Per altre informazioni sulla creazione di trust della relying party, vedere [Creare un trust della relying party usando metadati federativi](../operations/create-a-relying-party-trust.md#to-create-a-claims-aware-relying-party-trust-using-federation-metadata) oppure [Creare un trust della relying party manualmente](../operations/create-a-relying-party-trust.md#to-create-a-claims-aware-relying-party-trust-manually). 

## <a name="next-steps"></a>Passaggi successivi
 [Preparare la migrazione del server federativo AD FS 2,0](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparare la migrazione del proxy server federativo AD FS 2,0](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Eseguire la migrazione del server federativo AD FS 2,0](migrate-the-ad-fs-fed-server.md)   
 [Eseguire la migrazione del proxy server federativo AD FS 2,0](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Eseguire la migrazione di Agenti Web di AD FS 1.1](migrate-the-ad-fs-web-agent.md)




-   
-    