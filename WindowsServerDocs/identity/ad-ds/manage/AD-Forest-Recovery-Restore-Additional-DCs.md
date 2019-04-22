---
title: Ripristino della foresta Active Directory - Redeploy rimanenti i controller di dominio
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: af9ec02a480b35e573edebcdd5928451174b6c1d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812002"
---
# <a name="ad-forest-recovery---redeploy-remaining-dcs"></a>Ripristino della foresta Active Directory - Redeploy rimanenti i controller di dominio

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

I passaggi fino a questo punto si applicano a tutte le foreste: trovare un backup valido per ogni dominio, ripristinare i domini in isolamento, ricollegarli, reimpostare il catalogo globale e la pulizia. In questo passaggio successivo verrà distribuito nuovamente l'insieme di strutture. Il modo per eseguire questa operazione dipenderà notevolmente la progettazione di foresta, i contratti di servizio, struttura del sito, larghezza di banda disponibile e numerosi altri fattori. È necessario progettare il proprio piano di ridistribuzione basato sui principi e i suggerimenti in questa sezione, in modo che è più adatto per i requisiti aziendali.  
  
Il passaggio successivo è installare Active Directory Domain Services in tutti i controller di dominio che erano presenti prima che si è verificato il ripristino della foresta. Se sono ancora presenti controller di dominio, è necessario il servizio Active Directory Domain Services deve essere rimossa forzatamente o i controller di dominio può essere reinstallato. I backup esistenti per tali controller di dominio non possono essere riutilizzati, perché i metadati corrispondenti sono stata rimossa durante il ripristino della foresta. In un ambiente non complicato questo processo di ridistribuzione può essere semplice come riconnettendo i controller di dominio ripristinato nella rete di produzione e alzare di livello nuovi controller di dominio in base alle esigenze.  
  
In una grande impresa di fronte a un'infrastruttura a livello mondiale, è necessario un piano più sofisticato. La prima fase consiste in genere per ripristinare Active Directory come un servizio. Questo significa che per installare in modo strategico inserito i controller di dominio in modo che tutte le applicazioni e alle divisioni aziendali critici possano iniziare a usare di nuovo. Può essere accettabile per succursali di memorizzare temporaneamente hanno una riduzione delle prestazioni di conseguenza. Come una seconda fase, tutti i rimanenti e i controller di dominio meno critici vengono ridistribuita.  
  
 Esistono due metodi per installare controller di dominio aggiuntivi, entrambi possono essere automatizzate:  
  
- Clonazione  
   - Negli ambienti virtualizzati che eseguono Windows Server 2012, la clonazione è il modo più semplice e rapido per recuperare un numero elevato di controller di dominio. Dopo il ripristino da backup di un singolo controller di dominio virtualizzato, è possibile automatizzare il ripristino di tutti i controller di dominio virtualizzati in un dominio.  
   - Per altre informazioni sui prerequisiti e la clonazione, vedere [Introduction to Active Directory Domain Services (AD DS) Virtualization (Level 100)](https://technet.microsoft.com/library/hh831734.aspx).  
- Installare nuovamente Active Directory Domain Services tramite Windows PowerShell nei server che eseguono Windows Server 2012 (o Dcpromo.exe nei server che eseguono versioni precedenti di Windows Server) o tramite l'interfaccia utente  
   - Per accelerare la reinstallazione di Active Directory Domain Services, è possibile utilizzare Installazione da supporto opzione per ridurre il traffico di replica durante l'installazione. Per altre informazioni sull'uso di **ifm ntdsutil** comando per creare supporti di installazione, vedere [dominio Active Directory di installazione dal supporto](https://technet.microsoft.com/library/cc770654\(WS.10\).aspx).  

Considerare gli aspetti aggiuntivi seguenti per ogni replica controller di dominio che viene recuperato l'insieme di strutture tramite la clonazione di controller di dominio virtualizzato oppure installare Active Directory Domain Services (in contrapposizione il ripristino da backup):  
  
- Tutto il software in un controller di dominio usato come origine per la clonazione deve essere in grado di clonazione. Le applicazioni e servizi che non è possibile clonare devono essere rimossi prima che la clonazione viene avviata. Se ciò non è possibile, un controller di dominio virtualizzato alternativi devono essere scelte come origine.  
- Se si clona altri controller di dominio virtualizzati dal primo controller di dominio virtualizzati da ripristinare, sarà necessario arrestata mentre il file VHDX verrà copiato il controller di dominio di origine. Deve essere in esecuzione e disponibili online quando il clone sono controller di dominio virtuali iniziato. Se il tempo di inattività necessario per l'arresto non è accettabile per il primo controller di dominio ripristinato, è possibile distribuire un controller di dominio virtualizzato aggiuntivo tramite l'installazione di Active Directory Domain Services per fungere da origine per la clonazione.  
- Non vi è alcuna restrizione sul nome host del controller di dominio virtualizzato clonato o del server in cui si desidera installare Active Directory Domain Services. È possibile usare un nuovo nome host o il nome host che in precedenza era in uso. Per altre informazioni sulla sintassi del nome host DNS, vedere [creazione di nomi di Computer DNS](https://technet.microsoft.com/library/cc785282.aspx) ([https://go.microsoft.com/fwlink/?LinkId=74564](https://go.microsoft.com/fwlink/?LinkId=74564)).  
- Configurare ogni server con il primo server DNS della foresta (il primo controller di dominio che è stato ripristinato nel dominio radice) come server DNS preferito nelle proprietà TCP/IP della scheda di rete. Per altre informazioni, vedere [configurare TCP/IP da usare DNS](https://technet.microsoft.com/library/cc779282.aspx).  
- Ridistribuire tutti i controller del dominio, virtualizzati la clonazione di controller di dominio se alcuni di questi controller vengono distribuiti in una posizione centrale, oppure dal metodo tradizionale di ricompilarle per rimozione e reinstallazione di Active Directory Domain Services se vengono distribuiti singolarmente nei percorsi trovare isolati come le succursali.  
   - La ricompilazione di questi controller assicura che non contengono tutti gli oggetti residui e che possono aiutare a evitare i conflitti di replica che si verifichi in un secondo momento. Quando si rimuove Active Directory Domain Services da un RODC *scegliere l'opzione per conservare i metadati di controller di dominio*. Usando questa opzione consente di mantenere l'account krbtgt per il controller di dominio e mantiene le autorizzazioni per l'account amministratore delegato e la Password dei criteri di replica e non sarà necessario usare le credenziali Domain Admin per rimuovere e reinstallare Servizi di dominio Active Directory in un RODC. Inoltre, mantiene il server DNS e i ruoli di catalogo globale se sono installati in origine nel RODC.  
   - Quando si ricompila i controller di dominio (RODC o controller di dominio scrivibili), potrebbe esserci un aumento del traffico di replica durante la reinstallazione. Per ridurre tale impatto, è possibile scaglionare la pianificazione delle installazioni di controller di dominio ed è possibile usare l'opzione Installa da supporto (IFM). Se si usa l'opzione di installazione da supporto, eseguire la **ifm ntdsutil** comando in un controller di dominio scrivibile che si ritiene attendibile privi di danneggiamento dei dati. In tal modo possibile danneggiamento dal RODC dopo la reinstallazione di Active Directory Domain Services è stata completata. Per altre informazioni sull'installazione da supporto, vedere [installazione di Active Directory Domain Services dal supporto](https://technet.microsoft.com/library/cc770654\(WS.10\).aspx).  
   - Per altre informazioni sulla ricompilazione di questi controller, vedere [RODC rimozione e reinstallazione](https://technet.microsoft.com/library/cc835490\(WS.10\).aspx).  
- Se un controller di dominio è stato in esecuzione il servizio Server DNS prima il malfunzionamento di foresta, installare e configurare il servizio Server DNS durante l'installazione di Active Directory Domain Services. In caso contrario, configurare i client DNS precedenti con altri server DNS.  
- Se sono necessari altri cataloghi globali per condividere l'autenticazione o carico di query per gli utenti o applicazioni, è possibile aggiungere il catalogo globale per l'origine virtualizzato controller di dominio prima di clonare o è possibile rendere un server di catalogo globale un controller di dominio durante l'installazione di Active Directory Domain Services.  
  
## <a name="next-steps"></a>Passaggi successivi

- [Ripristino della foresta Active Directory - prerequisiti](AD-Forest-Recovery-Prerequisties.md)  
- [Ripristino della foresta Active Directory - concepire un piano di ripristino personalizzata-foresta](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Ripristino della foresta Active Directory - identificare il problema](AD-Forest-Recovery-Identify-the-Problem.md)
- [Ripristino della foresta Active Directory - determinare la modalità di ripristino](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [Ripristino della foresta Active Directory - eseguire il ripristino iniziale](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [Ripristino della foresta Active Directory - procedure](AD-Forest-Recovery-Procedures.md)  
- [Ripristino della foresta Active Directory - domande frequenti](AD-Forest-Recovery-FAQ.md)  
- [Ripristino della foresta Active Directory - il ripristino di un singolo dominio all'interno di un Multidomain foresta](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [Ripristino della foresta Active Directory - ripristino della foresta con controller di dominio di Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)
