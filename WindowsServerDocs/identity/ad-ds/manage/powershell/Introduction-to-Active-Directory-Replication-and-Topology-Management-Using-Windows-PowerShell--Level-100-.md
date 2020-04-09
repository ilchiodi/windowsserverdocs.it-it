---
ms.assetid: c54b544f-cc32-4837-bb2d-a8656b22f3de
title: Introduzione alla gestione della topologia e della replica di Active Directory mediante Windows PowerShell (Livello 100)
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 63ecad01ec6d4b4d72b7aaff315b74541cb0fadc
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822984"
---
# <a name="introduction-to-active-directory-replication-and-topology-management-using-windows-powershell-level-100"></a>Introduzione alla gestione della topologia e della replica di Active Directory mediante Windows PowerShell (Livello 100)

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Windows PowerShell per Active Directory consente di gestire replica, siti, domini e foreste, controller di dominio e partizioni. Gli utenti di strumenti di gestione precedenti, come lo snap-in Siti e servizi di Active Directory e repadmin.exe, noteranno che ora è disponibile una funzionalità simile nel contesto Windows PowerShell per Active Directory. I cmdlet sono inoltre compatibili con i cmdlet di Windows PowerShell per Active Directory esistenti, creando così un'esperienza ottimizzata e consentendo ai clienti di creare facilmente script per l'automazione.

> [!NOTE]
> I cmdlet di replica e topologia di Windows PowerShell per Active Directory sono disponibili negli ambienti seguenti:
> 
> -    Controller di dominio di Windows Server 2012
> -    Windows Server 2012 con gli strumenti di amministrazione Server remota per Active Directory e AD LDS installato.
> -   Windows&reg; 8 con strumenti di amministrazione Server remota per Active Directory e AD LDS installato.

## <a name="installing-the-active-directory-module-for-windows-powershell"></a>Installazione del modulo Active Directory per Windows PowerShell
Il modulo Active Directory per Windows PowerShell viene installato per impostazione predefinita, quando il ruolo di server di dominio Active Directory viene installato in un server che esegue Windows Server 2012. Non sono ulteriori passaggi rispetto all'aggiunta del ruolo server. È inoltre possibile installare il modulo Active Directory in un server che esegue Windows Server 2012 installando gli strumenti di amministrazione remota del Server e installare il modulo Active Directory in un computer che eseguono Windows 8 scaricando e installando il [remoto amministrativa strumenti Server (RSAT)](https://www.microsoft.com/download/details.aspx?id=28972). Vedere [Istruzioni](https://www.microsoft.com/download/details.aspx?id=28972)per i passaggi di installazione.

## <a name="scenarios-for-testing-windows-powershell-for-active-directory-replication-and-topology-management-cmdlets"></a>Scenari per il test dei cmdlet per la gestione della topologia e della replica di Windows PowerShell per Active Directory
Gli scenari seguenti sono studiati per consentire agli amministratori di acquisire dimestichezza con i nuovi cmdlet di gestione:

-   Ottenere l'elenco di tutti i controller di dominio e dei siti corrispondenti

-   Gestire la topologia di replica

-   Visualizzare le informazioni sulla replica e il suo stato

## <a name="lab-requirements"></a>Requisiti per il lab

-   Due controller di dominio di Windows Server 2012: **DC1** e **DC2** che fanno parte del dominio contoso.com e si trovano nel sito AZIENDA all'interno del dominio.

## <a name="view-domain-controllers-and-their-sites"></a>Visualizzare i controller di dominio e i relativi siti
In questo passaggio verrà utilizzato il Modulo di Active Directory per Windows PowerShell per visualizzare i controller di dominio esistenti e la topologia di replica del dominio.

Per completare i passaggi illustrati nelle procedure seguenti è necessario essere membri del gruppo Domain Admins o disporre di autorizzazioni equivalenti.

#### <a name="to-view-all-active-directory-sites"></a>Per visualizzare tutti i siti di Active Directory

1.  In **DC1** fare clic su **Windows PowerShell** sulla barra delle applicazioni.

2.  Digitare il comando seguente:

    `Get-ADReplicationSite -Filter *`

    Il comando restituisce informazioni dettagliate su ciascuno dei siti. Il parametro `Filter` è usato in tutti i cmdlet di Windows PowerShell per Active Directory per limitare l'elenco di oggetti restituiti. In questo caso l'asterisco (*) indica che devono essere restituiti tutti gli oggetti del sito.

    > [!TIP]
    > È possibile utilizzare il tasto TAB per il completamento automatico dei comandi in Windows PowerShell.
    > 
    > Esempio: digitare `Get-ADRep` e premere più volte il tasto TAB per scorrere i comandi corrispondenti fino a raggiungere `Get-ADReplicationSite`. Il completamento automatico funziona anche per i nomi dei parametri, ad esempio `Filter`.

    Per formattare l'output dal `Get-ADReplicationSite` comando sotto forma di tabella e limitare la visualizzazione a campi specifici, è possibile reindirizzare l'output di `Format-Table` comando (o "`ft`" breve):

    `Get-ADReplicationSite -Filter * | ft Name`

    Viene restituita una versione più breve dell'elenco dei siti, che include solo il campo Name.

#### <a name="to-produce-a-table-of-all-domain-controllers"></a>Per generare una tabella di tutti i controller di dominio

-   Al prompt del **Modulo di Active Directory per Windows PowerShell** digitare il comando seguente:

    `Get-ADDomainController -Filter * | ft Hostname,Site`

    Il comando restituisce il nome host dei controller di dominio e le relative associazioni di sito.

## <a name="manage-replication-topology"></a>Gestire la topologia di replica
Nel passaggio precedente, dopo l'esecuzione del comando `Get-ADDomainController -Filter * | ft Hostname,Site`, **DC2** è stato elencato come appartenente al sito **CORPORATE** . Nelle procedure seguenti verranno creati un nuovo sito di succursale, **BRANCH1** e un nuovo collegamento di sito, verrà impostato il costo e la frequenza di replica del collegamento di sito e quindi **DC2** verrà spostato in **BRANCH1**.

Per completare i passaggi illustrati nelle procedure seguenti è necessario essere membri del gruppo Domain Admins o disporre di autorizzazioni equivalenti.

#### <a name="to-create-a-new-site"></a>Per creare un nuovo sito

-   Al prompt del **Modulo di Active Directory per Windows PowerShell** digitare il comando seguente:

    `New-ADReplicationSite BRANCH1`

    Il comando crea il nuovo sito di succursale, branch1.

#### <a name="to-create-a-new-site-link"></a>Per creare un nuovo collegamento di sito

-   Al prompt del **Modulo di Active Directory per Windows PowerShell** digitare il comando seguente:

    `New-ADReplicationSiteLink 'CORPORATE-BRANCH1'  -SitesIncluded CORPORATE,BRANCH1 -OtherAttributes @{'options'=1}`

    Il comando crea il collegamento di sito a **BRANCH1** e attiva il processo di notifica delle modifiche.

    > [!TIP]
    > Utilizzare il tasto TAB per il completamento automatico di nomi di parametri quali `-SitesIncluded` e `-OtherAttributes` invece di digitarli manualmente.

#### <a name="to-set-the-site-link-cost-and-replication-frequency"></a>Per impostare il costo e la frequenza di replica del collegamento di sito

-   Al prompt del **Modulo di Active Directory per Windows PowerShell** digitare il comando seguente:

    `Set-ADReplicationSiteLink CORPORATE-BRANCH1 -Cost 100 -ReplicationFrequencyInMinutes 15`

    Il comando imposta il costo del collegamento di sito a **BRANCH1** su **100** e la frequenza di replica per il sito su **15 minuti**.

#### <a name="to-move-a-domain-controller-to-a-different-site"></a>Per spostare un controller di dominio in un altro sito

-   Al prompt del **Modulo di Active Directory per Windows PowerShell** digitare il comando seguente:

    `Get-ADDomainController DC2 | Move-ADDirectoryServer -Site BRANCH1`

    Il comando sposta il controller di dominio **DC2** nel sito **BRANCH1** .

### <a name="verification"></a>Verifica

##### <a name="to-verify-site-creation-new-site-link-and-cost-and-replication-frequency"></a>Per verificare la creazione del sito, il nuovo collegamento di sito, il costo e la frequenza di replica

-   Fare clic su **Server Manager** e quindi su **Strumenti**, quindi selezionare **Siti e servizi di Active Directory** e verificare quanto segue:

    Che il sito **BRANCH1** contenga tutti i valori corretti impostati mediante i comandi di Windows PowerShell.

    Che il collegamento di sito **CORPORATE-BRANCH1** sia stato creato e si connetta ai siti **BRANCH1** e **CORPORATE**.

    Che ora **DC2** si trovi nel sito **BRANCH1** . In alternativa, aprire il **Modulo di Active Directory per Windows PowerShell** e digitare il comando seguente per verificare che ora **DC2** si trovi nel sito **BRANCH1**: `Get-ADDomainController -Filter * | ft Hostname,Site`.

## <a name="view-replication-status-information"></a>Visualizzare le informazioni sullo stato della replica
Nelle procedure seguenti verrà utilizzato uno dei cmdlet per la replica e la gestione di Windows PowerShell per Active Directory, `Get-ADReplicationUpToDatenessVectorTable DC1`, per produrre un semplice rapporto sulla replica utilizzando la tabella dei vettori di aggiornamento gestita da ogni controller di dominio. Questa tabella tiene traccia del massimo USN scritto di origine rilevato da ogni controller di dominio della foresta.

Per completare i passaggi illustrati nelle procedure seguenti è necessario essere membri del gruppo Domain Admins o disporre di autorizzazioni equivalenti.

#### <a name="to-view-the-up-to-dateness-vector-table-for-a-single-domain-controller"></a>Per visualizzare la tabella dei vettori di aggiornamento per un singolo controller di dominio

1.  Al prompt del **Modulo di Active Directory per Windows PowerShell** digitare il comando seguente:

    `Get-ADReplicationUpToDatenessVectorTable DC1`

    Viene visualizzato un elenco del massimi USN rilevati da **DC1** per tutti i controller di dominio della foresta. Il valore **Server** è riferito al server che gestisce la tabella, in questo caso **DC1**. Il valore **Partner** è riferito al partner di replica (diretto o indiretto) in cui sono state effettuate modifiche. Il valore UsnFilter corrisponde al massimo USN rilevato da **DC1** da Partner. Se un nuovo controller di dominio viene aggiunto all'insieme di strutture, non verrà visualizzato **DC1**tabella fino a **DC1** riceve una modifica originata dal nuovo dominio.

#### <a name="to-view-the-up-to-dateness-vector-table-for-all-domain-controllers-in-a-domain"></a>Per visualizzare la tabella dei vettori di aggiornamento per tutti i controller di dominio di un dominio

1.  Al prompt del Modulo di Active Directory per Windows PowerShell digitare il comando seguente:

    `Get-ADReplicationUpToDatenessVectorTable * | sort Partner,Server | ft Partner,Server,UsnFilter`

    Il comando sostituisce **DC1** con `*`, raccogliendo in questo modo i dati delle tabelle dei vettori di aggiornamento di tutti i controller di dominio. I dati sono ordinati per **Partner** e **Server** e vengono quindi visualizzati in una tabella.

    L'ordinamento consente di confrontare facilmente l'ultimo USN rilevato da ogni controller di dominio per un determinato partner di replica. Si tratta di un modo rapido per verificare che la replica stia avvenendo in tutto l'ambiente. Se la replica funziona correttamente, il valore UsnFilter riportato per un determinato partner di replica deve essere simile in tutti i controller di dominio.

## <a name="see-also"></a>Vedi anche
[Gestione della topologia e della replica avanzata Active Directory &#40;usando il livello 200 di Windows PowerShell&#41;](Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md)


