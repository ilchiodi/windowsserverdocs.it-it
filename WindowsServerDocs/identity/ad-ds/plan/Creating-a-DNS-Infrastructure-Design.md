---
ms.assetid: cd70b0aa-0a67-4966-a041-4dd3f302c98b
title: Creazione di un progetto di infrastruttura DNS
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: b4b7cea18a6bb6b435b3c3fb6b4e94cfdddb2c04
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408970"
---
# <a name="creating-a-dns-infrastructure-design"></a>Creazione di un progetto di infrastruttura DNS

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Dopo aver creato la foresta Active Directory e le progettazioni di dominio, è necessario progettare un'infrastruttura Domain Name System (DNS) per supportare la struttura logica Active Directory. DNS consente agli utenti di utilizzare nomi descrittivi che è facile ricordare per connettersi a computer e altre risorse nelle reti IP. Active Directory Domain Services (AD DS) in Windows Server 2008 richiede DNS.  
  
Il processo di progettazione del DNS per il supporto di servizi di dominio Active Directory varia a seconda che l'organizzazione disponga già di un servizio server DNS esistente o che si stia distribuendo un nuovo servizio server DNS:  
  
- Se si dispone già di un'infrastruttura DNS esistente, è necessario integrare lo spazio dei nomi Active Directory in tale ambiente. Per ulteriori informazioni, vedere [integrazione di servizi di dominio Active Directory in un'infrastruttura DNS esistente](../../ad-ds/plan/Integrating-AD-DS-into-an-Existing-DNS-Infrastructure.md).  
- Se non si dispone di un'infrastruttura DNS sul posto, è necessario progettare e distribuire una nuova infrastruttura DNS per supportare servizi di dominio Active Directory. Per ulteriori informazioni, vedere la pagina relativa alla [distribuzione di Domain Name System (DNS)](https://go.microsoft.com/fwlink/?LinkId=93656).  
  
Se l'organizzazione dispone di un'infrastruttura DNS esistente, è necessario assicurarsi di aver compreso il modo in cui l'infrastruttura DNS interagirà con lo spazio dei nomi Active Directory. Per un foglio di lavoro che facilita la documentazione della progettazione dell'infrastruttura DNS esistente, scaricare Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip da [supporti per i processi per Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558) e aprire "inventario DNS" (DSSLOGI_8. doc).  
  
> [!NOTE]  
> Oltre agli indirizzi IP versione 4 (IPv4), Windows Server supporta anche indirizzi IP versione 6 (IPv6). Per un foglio di lavoro che assiste l'utente nell'elenco degli indirizzi IPv6 durante la documentazione del metodo di risoluzione dei nomi ricorsivo della struttura DNS corrente, vedere [appendice a: inventario DNS](../../ad-ds/plan/Appendix-A--DNS-Inventory.md).
  
Prima di progettare l'infrastruttura DNS per supportare servizi di dominio Active Directory, può essere utile leggere la gerarchia DNS, il processo di risoluzione dei nomi DNS e il modo in cui DNS supporta servizi di dominio Active Directory. Per ulteriori informazioni sulla gerarchia DNS e il processo di risoluzione dei nomi, vedere il riferimento tecnico DNS ([https://go.microsoft.com/fwlink/?LinkID=48145](https://go.microsoft.com/fwlink/?LinkID=48145)). Per ulteriori informazioni sul supporto di servizi di dominio Active Directory in DNS, vedere la pagina relativa al supporto DNS per Active Directory riferimento tecnico ([https://go.microsoft.com/fwlink/?LinkID=48147](https://go.microsoft.com/fwlink/?LinkID=48147)).  
  
## <a name="in-this-section"></a>Contenuto della sezione  

- [Revisione dei concetti relativi a DNS](../../ad-ds/plan/Reviewing-DNS-Concepts.md)  
- [DNS e Active Directory Domain Services](../../ad-ds/plan/DNS-and-AD-DS.md)  
- [Assegnazione del DNS per il ruolo di proprietario di Active Directory Domain Services](../../ad-ds/deploy/Assigning-the-DNS-for-AD-DS-Owner-Role.md)  
- [Integrazione di Active Directory Domain Services in un'infrastruttura DNS esistente](../../ad-ds/plan/../../ad-ds/plan/Integrating-AD-DS-into-an-Existing-DNS-Infrastructure.md)  
