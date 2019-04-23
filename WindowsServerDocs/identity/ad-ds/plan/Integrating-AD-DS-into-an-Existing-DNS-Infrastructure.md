---
ms.assetid: 4981b32f-741e-4afc-8734-26a8533ac530
title: Integrazione di Active Directory Domain Services in un'infrastruttura DNS esistente
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 62405ea9ee38bb3fa457b7731e26fbffb2594797
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59891042"
---
# <a name="integrating-ad-ds-into-an-existing-dns-infrastructure"></a>Integrazione di Active Directory Domain Services in un'infrastruttura DNS esistente

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se l'organizzazione ha già un servizio del Server di sistema DNS (Domain Name) esistente, il DNS per il proprietario di Active Directory Domain Services (AD DS) necessario collaborare con il proprietario DNS per l'organizzazione per integrare l'infrastruttura esistente di Active Directory Domain Services. Ciò comporta la creazione di un server DNS e la configurazione del client DNS.  
  
## <a name="creating-a-dns-server-configuration"></a>Creazione di una configurazione del server DNS  
Quando l'integrazione di Active Directory Domain Services con uno spazio dei nomi DNS esistente, è consigliabile procedere nel modo seguente:  
  
-   Installare il servizio Server DNS in ogni controller di dominio nella foresta. Ciò garantisce tolleranza di errore se uno dei server DNS non è disponibile. In questo modo, i controller di dominio non sono necessario fare affidamento su altri server DNS per la risoluzione dei nomi. Questo semplifica inoltre l'ambiente di gestione perché tutti i controller di dominio dispongano di una configurazione uniforme.  
  
-   Configurare il controller di dominio Active Directory radice della foresta per ospitare la zona DNS per la foresta di Active Directory.  
  
-   Configurare i controller di dominio per ogni dominio regionale ospitare le zone DNS che corrispondono ai relativi domini di Active Directory.  
  
-   Configurare l'area che contiene i record di individuazione foresta Active Directory (vale a dire, la zona msdcs. *nomeforesta* zona) per eseguire la replica in ogni server DNS della foresta usando la partizione di directory applicativa DNS a livello di foresta.  
  
    > [!NOTE]  
    > Quando il servizio Server DNS viene installato con il dominio servizi di installazione guidata Active Directory (questa opzione è consigliata), tutte le attività precedenti vengono eseguite automaticamente. Per ulteriori informazioni, vedere [la distribuzione di un dominio radice della foresta Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx).  
  
    > [!NOTE]  
    > Active Directory Domain Services Usa record di individuazione a livello di foresta per aiutare i partner di replica individuarsi reciprocamente e per consentire ai client di individuare i server di catalogo globale. Active Directory Domain Services archivia i record di individuazione a livello di foresta nella zona msdcs. *nomeforesta* zona. Poiché le informazioni nella zona devono essere ampiamente disponibili, quest'area viene replicata in tutti i server DNS della foresta tramite la partizione di directory applicativa DNS a livello di foresta.  
  
La struttura DNS esistente rimanga invariata. Non devi spostare qualsiasi server o le zone. È sufficiente creare una delega per le zone DNS integrate in Active Directory dalla gerarchia DNS esistente.  
  
## <a name="creating-the-dns-client-configuration"></a>Creare la configurazione del client DNS  
Per configurare il DNS nei computer client, il DNS per il proprietario di Active Directory Domain Services deve specificare il computer di denominazione lo schema e il modo in cui il client individua i server DNS. La tabella seguente elenca i configurazioni consigliate per questi elementi di progettazione.  
  
|Elemento di progettazione|Configurazione|  
|------------------|-----------------|  
|Denominazione di computer|Utilizzare nomi predefiniti. Quando un Windows 2000, Windows XP, Windows Server 2003, Windows Server 2008 o Windows Vista in base al computer viene aggiunto a un dominio, il computer stesso assegna un nome di dominio completo (FQDN) che comprende il nome host del computer e il nome dell'oggetto attivo Dominio di directory.|  
|Configurazione del resolver del client|Configurare i computer client in modo da puntare a qualsiasi server DNS nella rete.|  
  
> [!NOTE]  
> I client Active Directory e controller di dominio può registrare in modo dinamico i nomi DNS anche se non fanno riferimento al server DNS autorevole per i relativi nomi.  
  
Un computer potrebbe essere un nome DNS esistente diverso se l'organizzazione registrati in precedenza, in modo statico del computer nel DNS o se l'organizzazione ha distribuito in precedenza una soluzione integrata di Dynamic Host Configuration Protocol (DHCP). Se i computer client dispongano già di un nome DNS registrato, quando il dominio a cui queste vengono unite viene aggiornato a Windows Server 2008 Active Directory, hanno due nomi diversi:  
  
-   Il nome DNS esistente  
  
-   Il nuovo nome di dominio completo (FQDN)  
  
I client possono essere individuati da un nome. Qualsiasi esistente DNS, DHCP o soluzione DNS e DHCP integrata rimanga invariata. I nuovi nomi primari vengono creati automaticamente e aggiornati tramite aggiornamento dinamico. Vengono eliminate automaticamente tramite lo scavenging.  
  
Se si desidera sfruttare i vantaggi dell'autenticazione Kerberos durante la connessione a un server che esegue Windows 2000, Windows Server 2003 o Windows Server 2008, è necessario assicurarsi che il client si connette al server utilizzando il nome primario.  
  


