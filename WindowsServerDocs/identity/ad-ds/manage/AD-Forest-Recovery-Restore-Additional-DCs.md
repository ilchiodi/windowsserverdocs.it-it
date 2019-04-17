---
title: Ripristino della foresta Active Directory - Ridistribuisci restanti controller di dominio
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adfs
ms.openlocfilehash: 66b0bc65b3b8e5dfbf5f1a85350dab60ac3a11c8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---redeploy-remaining-dcs"></a>Ripristino della foresta Active Directory - Ridistribuisci restanti controller di dominio

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

 I passaggi fino a questo punto si applicano a tutti gli insiemi di strutture: trovare un backup valido per ogni dominio, ripristinare i domini di isolamento, ricollegarli, ripristinare il catalogo globale e pulizia. In questo passaggio successivo si verrà ridistribuisce l'insieme di strutture. Il modo per eseguire questa operazione dipende notevolmente la progettazione della foresta, i contratti di servizio, la struttura del sito, larghezza di banda disponibile e numerosi altri fattori. È necessario progettare la progettazione di ridistribuzione in base ai principi e suggerimenti in questa sezione, in modo che è più adatta per i requisiti aziendali.  
  
 Il passaggio successivo consiste per installare servizi di dominio Active Directory in tutti i controller di dominio che erano presenti anteriore il ripristino della foresta. Se i controller di dominio continuano a esistere, sarà necessario il servizio di dominio Active Directory deve essere rimosso forzatamente o possibile reinstallare il controller di dominio. Qualsiasi backup esistente per questi controller di dominio non può essere riutilizzato, poiché i metadati corrispondenti sono stato rimosso durante il ripristino dell'insieme di strutture. In un ambiente non complicato questo processo di ridistribuzione può essere semplice come la riconnessione di controller di dominio ripristinato nella rete di produzione e l'innalzamento di livello nuovi controller di dominio in base alle esigenze.  
  
 In un'azienda di grandi dimensione in presenza di un'infrastruttura in tutto il mondo, è necessario un piano più complesse. La prima fase è in genere ripristinare Active Directory come un servizio. Questo significa che per installare in modo strategico posizionati i controller di dominio in modo che tutte le applicazioni e le divisioni aziendali critiche possono funzionare di nuovo. Potrebbe essere accettabile per le succursali temporaneamente ridotta prestazioni in seguito a questo. Come un secondo controller di dominio meno critiche e fase tutti rimanenti sono ridistribuito.  
  
 Esistono due metodi per installare controller di dominio aggiuntivo che può essere automatizzate:  
  
-   La clonazione  
  
     Negli ambienti virtualizzati che eseguono Windows Server 2012, la clonazione è il modo più semplice e veloce per ripristinare un numero elevato di controller di dominio. Dopo il ripristino dal backup di un singolo controller di dominio virtualizzato, è possibile automatizzare il ripristino di tutti i controller di dominio virtualizzati in un dominio.  
  
     Per ulteriori informazioni sulla clonazione e i prerequisiti, vedere [Introduzione alla virtualizzazione di servizi di dominio Active Directory (AD DS) (livello 100)](https://technet.microsoft.com/library/hh831734.aspx).  
  
-   Installare nuovamente di dominio Active Directory utilizzando Windows PowerShell nei server che eseguono Windows Server 2012 (o Dcpromo.exe nei server che eseguono versioni precedenti di Windows Server) o tramite l'interfaccia utente  
  
     Per accelerare la reinstallazione di dominio Active Directory, è possibile utilizzare Installazione da supporto (compatibili) con l'opzione per ridurre il traffico di replica durante l'installazione. Per ulteriori informazioni sull'utilizzo di **ifm ntdsutil** comando per creare supporti di installazione, vedere [l'installazione di dominio Active Directory dal supporto](https://technet.microsoft.com/library/cc770654\(WS.10\).aspx).  
  
 Considerare gli aspetti aggiuntivi seguenti per ogni replica controller di dominio che viene ripristinato nella foresta, la clonazione del controller di dominio virtualizzati oppure installare servizi di dominio Active Directory (anziché il ripristino da backup):  
  
-   Tutto il software su un controller di dominio utilizzato come origine per la clonazione deve essere in grado di clonazione. Servizi e applicazioni che non possono essere clonati devono essere rimossi prima che la clonazione viene avviata. Se questo non è possibile, un controller di dominio virtualizzato alternativo deve scelto come origine.  
  
-   Se si clona altri controller di dominio virtualizzati dal primo controller di dominio virtualizzati da ripristinare, il controller di dominio di origine sarà necessario arrestare il computer mentre verrà copiato il file VHDX. Quindi è necessario essere in esecuzione e disponibili online quando sono controller di dominio virtuali clone primo avvio. Se il tempo di inattività necessario per l'arresto non è accettabile per il primo controller di dominio ripristinato, distribuire un controller di dominio virtualizzato aggiuntive tramite l'installazione di dominio Active Directory per fungere da origine per la clonazione.  
  
-   Non esiste alcuna restrizione sul nome host del controller di dominio virtualizzati clonato o del server in cui si desidera installare Active Directory. È possibile utilizzare un nuovo nome host o il nome host che in precedenza era in uso. Per ulteriori informazioni sulla sintassi di nome host DNS, vedere [la creazione di nomi di Computer DNS](https://technet.microsoft.com/library/cc785282.aspx) ([https://go.microsoft.com/fwlink/?LinkId=74564](https://go.microsoft.com/fwlink/?LinkId=74564)).  
  
-   Configurare ogni server con il primo server DNS della foresta (il primo controller di dominio che è stato ripristinato nel dominio radice) come server DNS preferito nelle proprietà TCP/IP della scheda di rete. Per ulteriori informazioni, vedere [configurare TCP/IP da utilizzare DNS](https://technet.microsoft.com/library/cc779282.aspx).  
  
-   Ridistribuire tutti i controller del dominio, è possibile virtualizzato la clonazione di controller di dominio se diversi lettura viene distribuiti in una posizione centrale, oppure il metodo tradizionale di ricostruzione li da rimuovere e reinstallare Servizi di dominio Active Directory se vengono distribuiti singolarmente in posizioni trovare isolati, ad esempio le succursali.  
  
     La ricostruzione di lettura garantisce che non contengono tutti gli oggetti residui e consente di evitare conflitti di replica in un secondo momento. Quando si rimuove servizi di dominio Active Directory da un controller di dominio, *scegliere l'opzione per mantenere i metadati di controller di dominio*. Questa opzione consente di mantenere l'account krbtgt per il controller di dominio e mantiene le autorizzazioni per l'account amministratore delegato e la Password dei criteri di replica e consente di evitare di usare le credenziali di amministratore di dominio per rimuovere e reinstallare Servizi di dominio Active Directory su un controller di dominio. Inoltre, mantiene il server DNS e i ruoli di catalogo globale se sono installati in origine nel RODC.  
  
     Quando si ricostruire i controller di dominio (lettura o controller di dominio scrivibile), potrebbe esserci un aumento del traffico di replica durante la reinstallazione. Per ridurre tale impatto, è possibile sfalsare la pianificazione delle installazioni di controller di dominio ed è possibile utilizzare l'opzione di installazione da supporto (IFM). Se si utilizza l'opzione di installazione da supporto, eseguire il **ifm ntdsutil** comando in un controller di dominio scrivibile che si considerano attendibili privi di dati danneggiati. Questo consente di evitare il danneggiamento possibile che venga visualizzato nel RODC dopo la reinstallazione di dominio Active Directory è stata completata. Per ulteriori informazioni sulla funzionalità installazione da supporto, vedere [l'installazione di dominio Active Directory dal supporto](https://technet.microsoft.com/library/cc770654\(WS.10\).aspx).  
  
     Per ulteriori informazioni sulle modalità di ricreazione lettura, vedere [RODC rimozione e la reinstallazione](https://technet.microsoft.com/library/cc835490\(WS.10\).aspx).  
  
-   Se un controller di dominio è in esecuzione il servizio Server DNS prima il malfunzionamento dell'insieme di strutture, installare e configurare il servizio Server DNS durante l'installazione di servizi di dominio Active Directory. In caso contrario, configurare i client DNS precedenti con altri server DNS.  
  
-   Se sono necessari altri cataloghi globali di condividere l'autenticazione o il carico di query per gli utenti o applicazioni, è possibile aggiungere il catalogo globale per l'origine virtualizzato controller di dominio prima della clonazione alternativa, puoi creare un controller di dominio un server di catalogo globale durante l'installazione di servizi di dominio Active Directory.  
  
## <a name="next-steps"></a>Passaggi successivi
-   [Ripristino della foresta Active Directory - prerequisiti](AD-Forest-Recovery-Prerequisties.md)  
-   [Ripristino della foresta Active Directory - sviluppo di un piano di ripristino personalizzato foresta](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Ripristino della foresta Active Directory - identificare il problema](AD-Forest-Recovery-Identify-the-Problem.md)
-   [Ripristino della foresta Active Directory - determinare la modalità di ripristino](AD-Forest-Recovery-Determine-how-to-Recover.md)
-   [Ripristino della foresta Active Directory - eseguire il ripristino iniziale](AD-Forest-Recovery-Perform-initial-recovery.md)  
-   [Ripristino della foresta Active Directory - procedure](AD-Forest-Recovery-Procedures.md)  
-   [Ripristino della foresta Active Directory - domande frequenti](AD-Forest-Recovery-FAQ.md)  
-   [Ripristino della foresta Active Directory - ripristino di un singolo dominio all'interno di un insieme di strutture Multidomain](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
-   [Ripristino della foresta Active Directory - ripristino della foresta con i controller di dominio di Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)  