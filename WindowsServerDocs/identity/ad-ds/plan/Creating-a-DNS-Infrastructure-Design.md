---
ms.assetid: cd70b0aa-0a67-4966-a041-4dd3f302c98b
title: Creazione di un progetto di infrastruttura DNS
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 080c36f8410be4d6b1933c74730e2b55ce8d0a0b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856132"
---
# <a name="creating-a-dns-infrastructure-design"></a>Creazione di un progetto di infrastruttura DNS

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Dopo aver creato la progettazione di foresta e dominio di Active Directory, è necessario progettare un'infrastruttura Domain Name System (DNS) per supportare la struttura logica di Active Directory. DNS consente agli utenti di usare nomi descrittivi che sono facili da ricordare per connettersi al computer e altre risorse su reti IP. DNS è necessario in Active Directory Domain Services (AD DS) in Windows Server 2008.  
  
Il processo per la progettazione di DNS per il supporto di Active Directory Domain Services varia in base al fatto che l'organizzazione ha già un servizio Server DNS esistente o si distribuisce un nuovo servizio Server DNS:  
  
- Se si dispone già di un'infrastruttura DNS esistente, è necessario integrare lo spazio dei nomi Active Directory in tale ambiente. Per altre informazioni, vedere [integrazione Active Directory Domain Services in un'infrastruttura DNS esistente](../../ad-ds/plan/Integrating-AD-DS-into-an-Existing-DNS-Infrastructure.md).  
- Se non è un'infrastruttura DNS posto, è necessario progettare e distribuire una nuova infrastruttura DNS per il supporto di Active Directory Domain Services. Per altre informazioni, vedere [la distribuzione del sistema DNS (Domain Name)](https://go.microsoft.com/fwlink/?LinkId=93656).  
  
Se l'organizzazione ha un'infrastruttura DNS esistente, è necessario assicurarsi di aver compreso interazione infrastruttura DNS con lo spazio dei nomi Active Directory. Per un foglio di lavoro agevolare la documentazione della progettazione dell'infrastruttura DNS esistente, scaricare Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip dal [processo di supporto per Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558) e aprire "Inventario DNS" (DSSLOGI_8.doc).  
  
> [!NOTE]  
> Oltre a IP version 4 (IPv4) indirizzi, indirizzi di Windows Server inoltre supportano IP versione 6 (IPv6). Per un foglio di lavoro semplificare l'elenco degli indirizzi IPv6 mentre documentare il metodo di risoluzione ricorsiva della struttura dei DNS corrente, vedere [appendice a: DNS Inventory](../../ad-ds/plan/Appendix-A--DNS-Inventory.md).
  
Prima di progettare l'infrastruttura DNS per il supporto di Active Directory Domain Services, può essere utile conoscere la gerarchia DNS, il processo di risoluzione del nome DNS e come DNS supporta Active Directory Domain Services. Per altre informazioni sul processo di risoluzione gerarchia e il nome DNS, vedere i riferimenti tecnici per DNS ([https://go.microsoft.com/fwlink/?LinkID=48145](https://go.microsoft.com/fwlink/?LinkID=48145)). Per altre informazioni su come DNS supporta Active Directory Domain Services, vedere il supporto di DNS per tecnica Active Directory ([https://go.microsoft.com/fwlink/?LinkID=48147](https://go.microsoft.com/fwlink/?LinkID=48147)).  
  
## <a name="in-this-section"></a>Contenuto della sezione  

- [Revisione dei concetti relativi a DNS](../../ad-ds/plan/Reviewing-DNS-Concepts.md)  
- [DNS e Active Directory Domain Services](../../ad-ds/plan/DNS-and-AD-DS.md)  
- [Assegnazione del DNS per il ruolo di proprietario di dominio Active Directory di AD](../../ad-ds/deploy/Assigning-the-DNS-for-AD-DS-Owner-Role.md)  
- [L'integrazione di Active Directory Domain Services in un'infrastruttura DNS esistente](../../ad-ds/plan/../../ad-ds/plan/Integrating-AD-DS-into-an-Existing-DNS-Infrastructure.md)  
