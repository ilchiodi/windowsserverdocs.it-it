---
ms.assetid: c54b544f-cc32-4837-bb2d-a8656b22f3de
title: Introduzione alla replica di Active Directory e gestione della topologia mediante Windows PowerShell (livello 100)
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 006179bb3220f7bccfc7510e1b8ef69678321074
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="introduction-to-active-directory-replication-and-topology-management-using-windows-powershell-level-100"></a>Introduzione alla replica di Active Directory e gestione della topologia mediante Windows PowerShell (livello 100)

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Windows PowerShell per Active Directory include la possibilità di gestire replica, siti, domini e foreste, controller di dominio e partizioni. Snap-in utenti di strumenti di gestione precedenti, ad esempio i servizi e siti di Active Directory e repadmin.exe, noteranno che una funzionalità simile a questo punto è disponibile all'interno di Windows PowerShell per il contesto di Active Directory. Inoltre, i cmdlet sono compatibili con l'esistente cmdlet Windows PowerShell per Active Directory, pertanto la creazione di un'esperienza ottimizzata e consentendo ai clienti di creare facilmente script di automazione.

> [!NOTE]
> Windows PowerShell per la replica di Active Directory e i cmdlet di topologia sono disponibili nei seguenti ambienti:
> 
> -    Controller di dominio di Windows Server 2012
> -    Windows Server 2012 con strumenti di amministrazione Server remota per Active Directory e AD LDS installato.
> -   Windows&reg; 8 con strumenti di amministrazione Server remota per Active Directory e AD LDS installato.

## <a name="installing-the-active-directory-module-for-windows-powershell"></a>Installare il modulo Active Directory per Windows PowerShell
Di Active Directory modulo per Windows PowerShell viene installato per impostazione predefinita, quando il ruolo del server di dominio Active Directory viene installato in un server che esegue Windows Server 2012. Non sono ulteriori passaggi rispetto all'aggiunta del ruolo server. È inoltre possibile installare il modulo Active Directory in un server che esegue Windows Server 2012 installando gli strumenti di amministrazione remota del Server ed è possibile installare il modulo Active Directory in un computer che eseguono Windows 8 scaricando e installando il [remoto amministrativa strumenti Server (RSAT)](https://www.microsoft.com/download/details.aspx?id=28972). Vedere [istruzioni](https://www.microsoft.com/download/details.aspx?id=28972)per i passaggi dell'installazione.

## <a name="scenarios-for-testing-windows-powershell-for-active-directory-replication-and-topology-management-cmdlets"></a>Scenari per i test di Windows PowerShell per i cmdlet di gestione della replica e topologia di Active Directory
Gli scenari seguenti sono progettati per gli amministratori di acquisire dimestichezza con i nuovi cmdlet di gestione:

-   Ottenere un elenco di tutti i controller di dominio e dei siti corrispondenti

-   Gestire la topologia di replica

-   Visualizzare lo stato di replica e informazioni

## <a name="lab-requirements"></a>Requisiti del laboratorio

-   Due controller di dominio di Windows Server 2012: **DC1** e **DC2** che fanno parte del dominio contoso.com e residenti nel sito CORPORATE incluso in tale dominio.

## <a name="view-domain-controllers-and-their-sites"></a>Visualizzare i controller di dominio e i rispettivi siti
In questo passaggio, si utilizzerà il Active Directory modulo per Windows PowerShell per visualizzare i controller di dominio esistente e la topologia di replica per il dominio.

Per completare i passaggi nelle procedure seguenti, è necessario essere un membro del gruppo Domain Admins o disporre di autorizzazioni equivalenti.

#### <a name="to-view-all-active-directory-sites"></a>Per visualizzare tutti i siti di Active Directory

1.  In **DC1**, fare clic su **Windows PowerShell** della barra delle applicazioni.

2.  Digitare il comando seguente:

    `Get-ADReplicationSite -Filter *`

    Questo comando restituisce informazioni dettagliate su ciascuno dei siti. Il `Filter`parametro viene utilizzato in tutto il cmdlet PowerShell per Active Directory per limitare l'elenco di oggetti restituiti. In questo caso, l'asterisco (*) indica tutti gli oggetti del sito.

    > [!TIP]
    > È possibile utilizzare il tasto Tab per i comandi di completamento automatico in Windows PowerShell.
    > 
    > Esempio: Digitare `Get-ADRep`e premere più volte Tab per scorrere i comandi corrispondenti fino a raggiungere `Get-ADReplicationSite`. Completamento automatico funziona anche per i nomi dei parametri, ad esempio `Filter`.

    Per formattare l'output dal `Get-ADReplicationSite`comando come tabella e limitare la visualizzazione a campi specifici, è possibile inviare tramite pipe l'output per il `Format-Table`comando (o "`ft`" forma abbreviata):

    `Get-ADReplicationSite -Filter * | ft Name`

    Restituisce una versione più breve dell'elenco di siti, che include solo il campo nome.

#### <a name="to-produce-a-table-of-all-domain-controllers"></a>Per generare una tabella di tutti i controller di dominio

-   Digitare il comando seguente al **modulo Active Directory per Windows PowerShell** prompt dei comandi:

    `Get-ADDomainController -Filter * | ft Hostname,Site`

    Questo comando restituisce che i controller di dominio host name, nonché le associazioni di sito.

## <a name="manage-replication-topology"></a>Gestire la topologia di replica
Nel passaggio precedente, dopo l'esecuzione del comando, `Get-ADDomainController -Filter * | ft Hostname,Site`, **DC2** è stato elencato come parte di **aziendale** sito. Nelle procedure seguenti, si creerà un nuovo sito di succursale, **BRANCH1**, creare un nuovo collegamento di sito, impostare la frequenza dei costi e la replica del collegamento di sito e quindi sposta **DC2** a **BRANCH1**.

Per completare i passaggi nelle procedure seguenti, è necessario essere un membro del gruppo Domain Admins o disporre di autorizzazioni equivalenti.

#### <a name="to-create-a-new-site"></a>Per creare un nuovo sito

-   Digitare il comando seguente al **modulo Active Directory per Windows PowerShell** prompt dei comandi:

    `New-ADReplicationSite BRANCH1`

    Questo comando crea il nuovo sito di succursale, branch1.

#### <a name="to-create-a-new-site-link"></a>Per creare un nuovo collegamento di sito

-   Digitare il comando seguente al **modulo Active Directory per Windows PowerShell** prompt dei comandi:

    `New-ADReplicationSiteLink 'CORPORATE-BRANCH1'  -SitesIncluded CORPORATE,BRANCH1 -OtherAttributes @{'options'=1}`

    Questo comando crea il collegamento di sito per **BRANCH1** e attivato il processo di notifica di modifica.

    > [!TIP]
    > Utilizzare scheda per i nomi dei parametri di completamento automatico, ad esempio `-SitesIncluded`e `-OtherAttributes`invece di digitarli manualmente.

#### <a name="to-set-the-site-link-cost-and-replication-frequency"></a>Per impostare la frequenza dei costi e la replica del collegamento di sito

-   Digitare il comando seguente al **modulo Active Directory per Windows PowerShell** prompt dei comandi:

    `Set-ADReplicationSiteLink CORPORATE-BRANCH1 -Cost 100 -ReplicationFrequencyInMinutes 15`

    Questo comando imposta il costo del collegamento di sito **BRANCH1** in **100** e impostare la frequenza di replica con il sito per **15 minuti **.

#### <a name="to-move-a-domain-controller-to-a-different-site"></a>Per spostare un controller di dominio a un sito diverso

-   Digitare il comando seguente al **modulo Active Directory per Windows PowerShell** prompt dei comandi:

    `Get-ADDomainController DC2 | Move-ADDirectoryServer -Site BRANCH1`

    Questo comando Sposta il controller di dominio, **DC2** per il **BRANCH1** sito.

### <a name="verification"></a>Verifica

##### <a name="to-verify-site-creation-new-site-link-and-cost-and-replication-frequency"></a>Per verificare la creazione del sito, nuovo collegamento di sito e alla frequenza di costo e la replica

-   Fare clic su **Server Manager**, fare clic su **strumenti** e quindi fare clic su **Active Directory Sites and Services** e verificare quanto segue:

    Verificare che il **BRANCH1** il sito contiene tutti i valori corretti dei comandi Windows PowerShell.

    Verificare il **CORPORATE-BRANCH1** collegamento di sito viene creato e si connette il **BRANCH1** e **aziendale** siti.

    Verificare **DC2** è ora nel **BRANCH1** sito. In alternativa, è possibile aprire il **modulo Active Directory per Windows PowerShell** e digitare il comando seguente per verificare **DC2** è ora nel **BRANCH1** sito:`Get-ADDomainController -Filter * | ft Hostname,Site`.

## <a name="view-replication-status-information"></a>Informazioni sullo stato della replica
Nelle procedure seguenti, utilizzare uno di Windows PowerShell per la replica di Active Directory e i cmdlet di gestione, `Get-ADReplicationUpToDatenessVectorTable DC1`, per produrre un semplice rapporto sulla replica utilizzando la tabella dei vettori di aggiornamento gestita da ogni controller di dominio. Questa tabella tiene traccia del massimo USN scrittura origine rilevato da ogni controller di dominio nella foresta.

Per completare i passaggi nelle procedure seguenti, è necessario essere un membro del gruppo Domain Admins o disporre di autorizzazioni equivalenti.

#### <a name="to-view-the-up-to-dateness-vector-table-for-a-single-domain-controller"></a>Per visualizzare la tabella dei vettori di aggiornamento per un singolo controller di dominio

1.  Digitare il comando seguente al **modulo Active Directory per Windows PowerShell** prompt dei comandi:

    `Get-ADReplicationUpToDatenessVectorTable DC1`

    Viene visualizzato un elenco del massimi USN rilevati da **DC1** per ogni controller di dominio nella foresta. Il **Server** valore fa riferimento al server che gestisce la tabella, in questo caso **DC1**. Il **Partner** valore si riferisce al partner di replica (diretto o indiretto) in cui sono state apportate modifiche. Il valore UsnFilter corrisponde al massimo USN rilevato da **DC1** da Partner. Se viene aggiunto un nuovo controller di dominio alla foresta, non verrà visualizzato **DC1**tabella fino a **DC1** riceve una modifica originata dal nuovo dominio.

#### <a name="to-view-the-up-to-dateness-vector-table-for-all-domain-controllers-in-a-domain"></a>Per visualizzare la tabella dei vettori di aggiornamento per tutti i controller di dominio in un dominio

1.  Il modulo Active Directory per Windows PowerShell Prompt dei comandi digitare il comando seguente:

    `Get-ADReplicationUpToDatenessVectorTable * | sort Partner,Server | ft Partner,Server,UsnFilter`

    Questo comando sostituisce **DC1** con `*`, raccogliendo in questo modo i dati di tabella vettore di aggiornamento di tutti i controller di dominio. I dati vengono ordinati **Partner** e **Server** e quindi visualizzato in una tabella.

    L'ordinamento consente di confrontare facilmente l'ultimo USN rilevato da ogni controller di dominio per un determinato partner di replica. Questo è un modo rapido per verificare che la replica stia avvenendo in tutto l'ambiente. Se la replica funziona correttamente, il valore usnfilter riportato per un determinato partner di replica deve essere simile in tutti i controller di dominio.

## <a name="see-also"></a>Vedere anche
[Replica di Active Directory e gestione della topologia mediante Windows PowerShell & #40; avanzati Livello 200 & #41;](Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md)


