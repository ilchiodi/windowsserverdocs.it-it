---
title: Ripristino della foresta di Active Directory-ridistribuzione di controller di dominio
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: 17e5ceec74277c888232d17adca5c2bbb305af97
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80823624"
---
# <a name="ad-forest-recovery---redeploy-remaining-dcs"></a>Ripristino della foresta di Active Directory-ridistribuzione di controller di dominio

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

I passaggi fino a questo punto si applicano a tutte le foreste: trova un backup valido per ogni dominio, ripristina i domini in isolamento, riconnettili, reimposta il catalogo globale e Pulisci. Nel passaggio successivo verrà ridistribuita la foresta. Il modo in cui eseguire questa operazione dipende in larga misura dalla progettazione della foresta, dai contratti di servizio, dalla struttura del sito, dalla larghezza di banda disponibile e da molti altri fattori. È necessario progettare un piano di ridistribuzione personalizzato in base ai principi e ai suggerimenti in questa sezione, in modo che sia più adatto ai requisiti aziendali.  
  
Il passaggio successivo consiste nell'installare servizi di dominio Active Directory in tutti i controller di dominio presenti prima del ripristino della foresta. Se i controller di dominio esistono ancora, il servizio servizi di dominio Active Directory deve essere rimosso forzatamente oppure i controller di dominio possono essere reinstallati. Non è possibile riutilizzare eventuali backup esistenti per tali controller di dominio, perché i metadati corrispondenti sono stati rimossi durante il ripristino della foresta. In un ambiente non complicato, questo processo di ridistribuzione può essere semplice quanto la riconnessione dei controller di dominio ripristinati alla rete di produzione e la promozione di nuovi controller di dominio in base alle esigenze.  
  
In un'azienda di grandi dimensioni in cui è stata affrontata un'infrastruttura globale, è necessario un piano più sofisticato. La prima fase è in genere il ripristino dell'annuncio come servizio; Ciò significa installare i controller di dominio posizionati in modo strategico in modo che tutte le applicazioni e le divisioni aziendali critiche possano iniziare a funzionare di nuovo. Potrebbe essere accettabile per le succursali ridurre temporaneamente le prestazioni a seguito di questa operazione. Come seconda fase, vengono ridistribuiti tutti i controller di dominio rimanenti e meno critici.  
  
 Esistono due metodi per installare controller di dominio aggiuntivi, che possono essere automatizzati:  
  
- Clonazione  
   - Per gli ambienti virtualizzati che eseguono Windows Server 2012, la clonazione è il modo più semplice e rapido per ripristinare un numero elevato di controller di dominio. È possibile automatizzare il ripristino di tutti i controller di dominio virtualizzati in un dominio dopo il ripristino di un singolo controller di dominio virtualizzato da un backup.  
   - Per ulteriori informazioni sulla clonazione e sui prerequisiti, vedere [Introduzione alla virtualizzazione di Active Directory Domain Services (ad DS) (livello 100)](https://technet.microsoft.com/library/hh831734.aspx).  
- Reinstallare Servizi di dominio Active Directory usando Windows PowerShell nei server che eseguono Windows Server 2012 (o Dcpromo. exe su server che eseguono versioni precedenti di Windows Server) o usando l'interfaccia utente  
   - Per velocizzare la reinstallazione di servizi di dominio Active Directory, è possibile usare l'opzione installa da supporto (installazione da supporto) per ridurre il traffico di replica durante l'installazione. Per ulteriori informazioni sull'utilizzo del comando **Ntdsutil installazione da supporto** per creare i supporti di installazione, vedere [installazione di servizi di dominio Active Directory da supporti](https://technet.microsoft.com/library/cc770654\(WS.10\).aspx).  

Considerare i seguenti punti aggiuntivi per ogni controller di dominio di replica ripristinato nella foresta tramite clonazione del controller di dominio virtualizzato oppure installando servizi di dominio Active Directory (anziché il ripristino dal backup):  
  
- Tutto il software in un controller di dominio utilizzato come origine per la clonazione deve poter essere clonato. Le applicazioni e i servizi che non possono essere clonati devono essere rimossi prima che venga avviata la clonazione. Se ciò non è possibile, come origine è necessario scegliere un controller di dominio virtualizzato alternativo.  
- Se si clonano i controller di dominio virtualizzati aggiuntivi dal primo controller di dominio virtualizzato da ripristinare, è necessario arrestare il controller di dominio di origine mentre viene copiato il file VHDX. Sarà quindi necessario che sia in esecuzione e disponibile online quando i controller di dominio virtuali clone vengono avviati per la prima volta. Se il tempo di inattività richiesto dall'arresto non è accettabile per il primo controller di dominio recuperato, distribuire un controller di dominio virtualizzato aggiuntivo installando servizi di dominio Active Directory in modo che funga da origine per la clonazione.  
- Non esiste alcuna restrizione sul nome host del controller di dominio virtualizzato clonato o del server in cui si desidera installare servizi di dominio Active Directory. È possibile utilizzare un nuovo nome host o il nome host in uso in precedenza. Per ulteriori informazioni sulla sintassi del nome host DNS, vedere [creazione di nomi di computer DNS](https://technet.microsoft.com/library/cc785282.aspx) ([https://go.microsoft.com/fwlink/?LinkId=74564](https://go.microsoft.com/fwlink/?LinkId=74564)).  
- Configurare ogni server con il primo server DNS nella foresta (il primo controller di dominio che è stato ripristinato nel dominio radice) come server DNS preferito nelle proprietà TCP/IP della scheda di rete. Per ulteriori informazioni, vedere la pagina relativa [alla configurazione di TCP/IP per l'utilizzo di DNS](https://technet.microsoft.com/library/cc779282.aspx).  
- Ridistribuire tutti i RODC nel dominio, tramite la clonazione del controller di dominio virtualizzato se diversi RODC vengono distribuiti in una posizione centrale o mediante il metodo tradizionale per ricompilarli rimuovendo e reinstallando Servizi di dominio Active Directory se vengono distribuiti singolarmente in posizioni isolate situate, ad esempio le succursali.  
   - La ricompilazione di RODC garantisce che non contengano oggetti residui e possa impedire che si verifichino conflitti di replica in un secondo momento. Quando si rimuove servizi di dominio Active Directory da un RODC, *scegliere l'opzione per mantenere i metadati del controller di*dominio. L'utilizzo di questa opzione consente di mantenere l'account krbtgt per il controller di dominio di sola lettura e di mantenere le autorizzazioni per l'account amministratore del controller di dominio di sola lettura delegato e il Criteri di replica password (PRP) e impedisce di dover utilizzare le credenziali di amministratore di dominio per rimuovere e reinstallare Servizi di dominio Active Directory in un controller di dominio Mantiene inoltre i ruoli del server DNS e del catalogo globale se sono installati originariamente nel controller di dominio di sola lettura.  
   - Quando si ricompilano i controller di dominio (RODC o i controller di dominio scrivibili), potrebbe verificarsi un aumento del traffico di replica durante la reinstallazione. Per ridurre questo effetto, è possibile scaglionare la pianificazione delle installazioni di RODC ed è possibile usare l'opzione installa da supporto (installazione da supporto). Se si usa l'opzione installazione da supporto, eseguire il comando **Ntdsutil installazione da supporto** in un controller di dominio scrivibile che si considera attendibile per essere privo di dati danneggiati. Ciò consente di evitare il danneggiamento del controller di dominio di sola lettura al termine della reinstallazione di servizi di dominio Active Directory. Per ulteriori informazioni su installazione da supporto, vedere [installazione di servizi di dominio Active Directory da supporti](https://technet.microsoft.com/library/cc770654\(WS.10\).aspx).  
   - Per ulteriori informazioni sulla ricompilazione di RODC, vedere la pagina relativa alla [rimozione e alla reinstallazione di RODC](https://technet.microsoft.com/library/cc835490\(WS.10\).aspx).  
- Se un controller di dominio eseguiva il servizio server DNS prima del malfunzionamento della foresta, installare e configurare il servizio server DNS durante l'installazione di servizi di dominio Active Directory. In caso contrario, configurare i client DNS precedenti con altri server DNS.  
- Se sono necessari cataloghi globali aggiuntivi per condividere l'autenticazione o il carico di query per utenti o applicazioni, è possibile aggiungere il catalogo globale al controller di dominio virtualizzato di origine prima della clonazione oppure è possibile impostare un controller di dominio come server di catalogo globale durante l'installazione di servizi di dominio Active Directory.  
  
## <a name="next-steps"></a>Passaggi successivi

- [Ripristino della foresta di Active Directory - Prerequisiti](AD-Forest-Recovery-Prerequisties.md)  
- [Ripristino della foresta di Active Directory-definizione di un piano di ripristino della foresta personalizzato](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Ripristino della foresta di Active Directory-identificare il problema](AD-Forest-Recovery-Identify-the-Problem.md)
- [Ripristino della foresta di Active Directory-determinare la modalità di ripristino](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [Ripristino della foresta di Active Directory-esecuzione del ripristino iniziale](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [Ripristino della foresta di Active Directory - Procedure](AD-Forest-Recovery-Procedures.md)  
- [Ripristino della foresta di Active Directory-Domande frequenti](AD-Forest-Recovery-FAQ.md)  
- [Ripristino della foresta di Active Directory-ripristino di un singolo dominio in una foresta con più domini](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [Ripristino della foresta di Active Directory-ripristino della foresta con i controller di dominio Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)
