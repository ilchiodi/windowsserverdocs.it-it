---
ms.assetid: 4981b32f-741e-4afc-8734-26a8533ac530
title: Integrazione di Active Directory in un'infrastruttura DNS esistente
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ab8d92a237a6d1fb623d9f4bb7dcc88561edf742
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="integrating-ad-ds-into-an-existing-dns-infrastructure"></a>Integrazione di Active Directory in un'infrastruttura DNS esistente

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se l'organizzazione ha già un servizio esistente di Server di sistema DNS (Domain Name), il DNS per il proprietario di servizi di dominio Active Directory (AD DS) necessario collaborare con il proprietario DNS per l'organizzazione per integrare l'infrastruttura esistente di dominio Active Directory. Questo implica la creazione di un server DNS e la configurazione del client DNS.  
  
## <a name="creating-a-dns-server-configuration"></a>Creazione di una configurazione di server DNS  
Durante l'integrazione di dominio Active Directory con uno spazio dei nomi DNS, si consiglia di procedere nel modo seguente:  
  
-   Installare il servizio Server DNS in ogni controller di dominio nella foresta. Questo fornisce tolleranza di errore se uno dei server DNS non è disponibile. In questo modo, i controller di dominio non è necessario fare affidamento su altri server DNS per la risoluzione dei nomi. Ciò semplifica inoltre l'ambiente di gestione perché tutti i controller di dominio dispongono di una configurazione uniforme.  
  
-   Configurare il controller di dominio radice foresta Active Directory per ospitare la zona DNS per la foresta di Active Directory.  
  
-   Configurare i controller di dominio per ogni dominio regionale ospitare le zone DNS che corrispondono ai relativi domini di Active Directory.  
  
-   Configurare la zona che contiene i record di individuazione foresta Active Directory (vale a dire la zona msdcs.* nomeforesta* zona) per la replica in tutti i server DNS della foresta utilizzando la partizione di directory applicativa DNS a livello di foresta.  
  
    > [!NOTE]  
    > Quando il servizio Server DNS viene installato con il dominio servizi di installazione guidata Active Directory (è consigliabile utilizzare questo metodo), tutte le attività precedenti vengono eseguite automaticamente. Per ulteriori informazioni, vedere [la distribuzione di un dominio radice della foresta Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx).  
  
    > [!NOTE]  
    > Servizi di dominio Active Directory Usa record di individuazione foresta per abilitare i partner di replica per trovare tra loro e per consentire ai client di individuare i server di catalogo globale. Active Directory archivia i record del localizzatore a livello di foresta nella zona msdcs. *nomeforesta* zona. Poiché le informazioni nella zona devono essere ampiamente disponibile, quest'area viene replicata in tutti i server DNS della foresta tramite la partizione di directory applicativa DNS a livello di foresta.  
  
La struttura DNS esistente rimane invariata. Non devi spostare tutti i server o zone. È sufficiente creare una delega per le zone DNS integrate in Active Directory da gerarchia DNS esistente.  
  
## <a name="creating-the-dns-client-configuration"></a>Creazione della configurazione di client DNS  
Per configurare il DNS nei computer client, il DNS per il proprietario di dominio Active Directory deve specificare la denominazione dei computer schema e come i client individua i server DNS. La tabella seguente elenca i nostri configurazioni consigliate per questi elementi di progettazione.  
  
|Elemento di progettazione|Configurazione|  
|------------------|-----------------|  
|La denominazione di computer|Utilizzare denominazione predefinito. Quando un Windows 2000, Windows XP, Windows Server 2003, Windows Server 2008 o Windows Vista nel computer viene aggiunto un dominio, il computer viene assegnato automaticamente un nome di dominio completo (FQDN) che include il nome host del computer e il nome del dominio di Active Directory.|  
|Configurazione del resolver client|Configurare i computer client in modo che punti a qualsiasi server DNS nella rete.|  
  
> [!NOTE]  
> I client Active Directory e controller di dominio può registrare dinamicamente i relativi nomi DNS anche se non fanno riferimento al server DNS autorevole per i relativi nomi.  
  
Un computer potrebbe essere un nome DNS esistente diverso se l'organizzazione registrato in precedenza, in modo statico del computer in DNS o se l'organizzazione ha precedentemente distribuito una soluzione integrata Dynamic Host Configuration Protocol (DHCP). Se i computer client già un nome DNS registrato, quando viene aggiornato il dominio a cui si sono stati aggiunti a Windows Server 2008 Active Directory, hanno due nomi diversi:  
  
-   Il nome DNS esistente  
  
-   Il nuovo nome di dominio completo (FQDN)  
  
I client possono essere individuati da qualsiasi nome. Qualsiasi esistente DNS, DHCP o soluzione integrata di DNS e DHCP viene apportata alcuna modifica. I nuovi nomi primari vengono creati automaticamente e aggiornati tramite aggiornamento dinamico. Vengono eliminate automaticamente tramite lo scavenging.  
  
Se si desidera sfruttare i vantaggi dell'autenticazione Kerberos quando ci si connette a un server che eseguono Windows 2000, Windows Server 2003 o Windows Server 2008, è necessario assicurarsi che il client si connette al server utilizzando il nome primario.  
  


