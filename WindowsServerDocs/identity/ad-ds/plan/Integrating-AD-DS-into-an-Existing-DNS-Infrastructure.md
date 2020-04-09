---
ms.assetid: 4981b32f-741e-4afc-8734-26a8533ac530
title: Integrazione di Active Directory Domain Services in un'infrastruttura DNS esistente
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: cf069102f409247832204546f3e1c15de7238bd3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822274"
---
# <a name="integrating-ad-ds-into-an-existing-dns-infrastructure"></a>Integrazione di Active Directory Domain Services in un'infrastruttura DNS esistente

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se l'organizzazione dispone già di un servizio server di Domain Name System (DNS) esistente, il DNS per Active Directory Domain Services proprietario di servizi di dominio Active Directory deve collaborare con il proprietario DNS dell'organizzazione per integrare servizi di dominio Active Directory nell'infrastruttura esistente. Ciò comporta la creazione di un server DNS e la configurazione del client DNS.  
  
## <a name="creating-a-dns-server-configuration"></a>Creazione di una configurazione del server DNS  
Quando si integra servizi di dominio Active Directory con uno spazio dei nomi DNS esistente, è consigliabile eseguire le operazioni seguenti:  
  
-   Installare il servizio server DNS in ogni controller di dominio della foresta. Questo garantisce la tolleranza di errore se uno dei server DNS non è disponibile. In questo modo, non è necessario che i controller di dominio si basino su altri server DNS per la risoluzione dei nomi. Questo semplifica anche l'ambiente di gestione perché tutti i controller di dominio hanno una configurazione uniforme.  
  
-   Configurare il controller di dominio radice della foresta Active Directory per ospitare la zona DNS per la foresta Active Directory.  
  
-   Configurare i controller di dominio per ogni dominio regionale per ospitare le zone DNS che corrispondono ai domini Active Directory.  
  
-   Configurare la zona contenente i record del localizzatore a livello di foresta Active Directory, ovvero l'_msdcs. *area forestaname* ) da replicare in ogni server DNS nella foresta usando la partizione di directory dell'applicazione DNS a livello di foresta.  
  
    > [!NOTE]  
    > Quando il servizio server DNS viene installato con il Installazione guidata di Active Directory Domain Services (si consiglia questa opzione), tutte le attività precedenti vengono eseguite automaticamente. Per ulteriori informazioni, vedere [la distribuzione di un dominio radice della foresta Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx).  
  
    > [!NOTE]  
    > Active Directory Domain Services utilizza record localizzatori a livello di foresta per consentire ai partner di replica di trovarsi reciprocamente e di consentire ai client di trovare i server di catalogo globale. Servizi di dominio Active Directory archivia i record del localizzatore a livello di foresta nel _msdcs. area *ForestName* . Poiché le informazioni nella zona devono essere ampiamente disponibili, questa zona viene replicata in tutti i server DNS della foresta per mezzo della partizione di directory applicativa DNS a livello di foresta.  
  
La struttura DNS esistente rimane intatta. Non è necessario spostare server o zone. È sufficiente creare una delega per le zone DNS integrate Active Directory dalla gerarchia DNS esistente.  
  
## <a name="creating-the-dns-client-configuration"></a>Creazione della configurazione del client DNS  
Per configurare DNS nei computer client, il DNS per il proprietario di servizi di dominio Active Directory deve specificare lo schema di denominazione dei computer e il modo in cui i client troveranno i server DNS. Nella tabella seguente sono elencate le configurazioni consigliate per questi elementi di progettazione.  
  
|Elemento di progettazione|Configurazione|  
|------------------|-----------------|  
|Denominazione computer|Utilizzare la denominazione predefinita. Quando un computer basato su Windows 2000, Windows XP, Windows Server 2003, Windows Server 2008 o Windows Vista viene aggiunto a un dominio, il computer assegna a se stesso un nome di dominio completo (FQDN) che comprende il nome host del computer e il nome del dominio Active Directory.|  
|Configurazione del resolver client|Configurare i computer client in modo che puntino a qualsiasi server DNS nella rete.|  
  
> [!NOTE]  
> Active Directory client e controller di dominio possono registrare dinamicamente i nomi DNS anche se non fanno riferimento al server DNS autorevole per i rispettivi nomi.  
  
Un computer potrebbe avere un altro nome DNS esistente, se l'organizzazione ha precedentemente registrato in modo statico il computer in DNS o se l'organizzazione ha distribuito in precedenza una soluzione DHCP (Integrated Dynamic Host Configuration Protocol). Se i computer client dispongono già di un nome DNS registrato, quando il dominio a cui sono stati aggiunti viene aggiornato a Windows Server 2008 servizi di dominio Active Directory, avranno due nomi diversi:  
  
-   Nome DNS esistente  
  
-   Il nuovo nome di dominio completo (FQDN)  
  
I client possono ancora trovarsi in uno dei due nomi. Eventuali soluzioni DNS, DHCP o DNS/DHCP integrate rimangono intatte. I nuovi nomi primari vengono creati automaticamente e aggiornati per mezzo di aggiornamento dinamico. Vengono puliti automaticamente per mezzo dello scavenging.  
  
Se si desidera utilizzare l'autenticazione Kerberos quando ci si connette a un server che esegue Windows 2000, Windows Server 2003 o Windows Server 2008, è necessario assicurarsi che il client si connetta al server utilizzando il nome primario.  
  


