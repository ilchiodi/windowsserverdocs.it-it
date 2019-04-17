---
ms.assetid: cd70b0aa-0a67-4966-a041-4dd3f302c98b
title: Creazione di una progettazione dell'infrastruttura DNS
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 6e5093b05fd81a693cec87ddb00d39e70483df23
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="creating-a-dns-infrastructure-design"></a>Creazione di una progettazione dell'infrastruttura DNS

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Dopo aver creato i tuoi progetti di foresta e del dominio di Active Directory, è necessario progettare un'infrastruttura di sistema DNS (Domain Name) per il supporto della struttura logica di Active Directory. DNS consente agli utenti di usare nomi descrittivi che sono facili da ricordare per connettersi a computer e altre risorse su reti IP. Servizi Active Directory dominio (AD DS) in Windows Server 2008 richiede DNS.  
  
Il processo di progettazione DNS per il supporto di dominio Active Directory varia a seconda se l'organizzazione ha già un servizio Server DNS esistente o si distribuisce un nuovo servizio Server DNS:  
  
-   Se si dispone già di un'infrastruttura DNS esistente, è necessario integrare lo spazio dei nomi di Active Directory in tale ambiente. Per ulteriori informazioni, vedere [integrazione servizi di dominio Active Directory in un'infrastruttura DNS esistente](../../ad-ds/plan/Integrating-AD-DS-into-an-Existing-DNS-Infrastructure.md).  
  
-   Se non è un'infrastruttura DNS sul posto, è necessario progettare e distribuire una nuova infrastruttura DNS per il supporto di dominio Active Directory. Per ulteriori informazioni, vedere la distribuzione del sistema DNS (Domain Name) ([https://go.microsoft.com/fwlink/?LinkId=93656](https://go.microsoft.com/fwlink/?LinkId=93656)).  
  
Se l'organizzazione dispone di un'infrastruttura DNS esistente, è necessario assicurarsi di comprendere come interagirà l'infrastruttura DNS con lo spazio dei nomi di Active Directory. Per un foglio di lavoro agevolare la documentazione della progettazione dell'infrastruttura DNS esistente, scaricare Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip dal processo Aid per Windows Server 2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)) e aprire "Inventario DNS" (DSSLOGI_8.doc).  
  
> [!NOTE]  
> Oltre a IP versione 4 (IPv4) indirizzi, indirizzi di Windows Server 2008 anche supporta IP versione 6 (IPv6). Per un foglio di lavoro facilitare la presentazione gli indirizzi IPv6 mentre documentare il metodo di risoluzione nome ricorsivo della struttura di DNS corrente, vedere [appendice a: DNS inventario](../../ad-ds/plan/Appendix-A--DNS-Inventory.md).  
  
Prima di progettare l'infrastruttura DNS per il supporto di dominio Active Directory, può essere utile per ulteriori informazioni su gerarchia DNS, il processo di risoluzione dei nomi DNS e come DNS supporta servizi di dominio Active Directory. Per ulteriori informazioni sul processo di risoluzione gerarchia e il nome DNS, vedere la documentazione tecnica di DNS ([https://go.microsoft.com/fwlink/?LinkID=48145](https://go.microsoft.com/fwlink/?LinkID=48145)). Per ulteriori informazioni su come DNS supporta servizi di dominio Active Directory, vedere il supporto di DNS per Active Directory Technical Reference ([https://go.microsoft.com/fwlink/?LinkID=48147](https://go.microsoft.com/fwlink/?LinkID=48147)).  
  
## <a name="in-this-section"></a>In questa sezione  
  
-   [Revisione dei concetti relativi a DNS](../../ad-ds/plan/Reviewing-DNS-Concepts.md)  
  
-   [DNS e Active Directory](../../ad-ds/plan/DNS-and-AD-DS.md)  
  
-   [L'assegnazione del DNS per il ruolo di proprietario di directory Active Directory](../../ad-ds/deploy/Assigning-the-DNS-for-AD-DS-Owner-Role.md)  
  
-   [Integrazione di Active Directory in un'infrastruttura DNS esistente](../../ad-ds/plan/../../ad-ds/plan/Integrating-AD-DS-into-an-Existing-DNS-Infrastructure.md)  
  


