---
ms.assetid: 8be8c48d-790c-4199-b9d3-9f4a07d5ced2
title: Raccolta di informazioni di rete
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 5642963caee47ac48f841a13b47852b6b7d9b241
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408984"
---
# <a name="collecting-network-information"></a>Raccolta di informazioni di rete

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Il primo passaggio nella progettazione di una topologia del sito efficace in Active Directory Domain Services (AD DS) consiste nel consultare il gruppo di rete dell'organizzazione per raccogliere informazioni e comunicarle regolarmente sulla topologia di rete fisica.  
  
## <a name="creating-a-location-map"></a>Creazione di una mappa del percorso

Creare una mappa del percorso che rappresenti l'infrastruttura di rete fisica dell'organizzazione. Nella mappa del percorso identificare le posizioni geografiche che contengono gruppi di computer con connettività interna di 10 megabit al secondo (Mbps) o superiore (velocità della rete locale (LAN) o superiore).  
  
## <a name="listing-communication-links-and-available-bandwidth"></a>Elenco di collegamenti di comunicazione e larghezza di banda disponibile

Dopo aver creato una mappa del percorso, documentare il tipo di collegamento di comunicazione, la relativa velocità di collegamento e la larghezza di banda disponibile tra le singole località. Ottenere una topologia di Wide Area Network (WAN) dal gruppo di rete. Per un elenco dei tipi di circuito WAN comuni e delle relative larghezze di banda, vedere la sezione "determinazione del costo" in [creazione di un collegamento di sito progettazione](../../ad-ds/plan/Creating-a-Site-Link-Design.md). Queste informazioni sono necessarie per creare collegamenti di sito in un secondo momento nel processo di progettazione della topologia del sito.  
  
La larghezza di banda si riferisce alla quantità di dati che è possibile trasmettere attraverso un canale di comunicazione in un determinato periodo di tempo. Larghezza di banda disponibile si riferisce alla quantità di larghezza di banda disponibile per l'utilizzo da parte di servizi di dominio È possibile ottenere informazioni sulla larghezza di banda disponibile dal gruppo di rete oppure è possibile analizzare il traffico di ogni collegamento utilizzando un analizzatore di protocollo, ad esempio Network Monitor. Per informazioni sull'installazione di Network Monitor, vedere l'articolo [monitoraggio del traffico di rete](https://go.microsoft.com/fwlink/?LinkId=107058).  
  
Documentare ogni percorso e gli altri percorsi collegati. Registrare inoltre il tipo di collegamento di comunicazione e la larghezza di banda disponibile. Per un foglio di lavoro in cui è possibile elencare i collegamenti di comunicazione e la larghezza di banda disponibile, vedere l'argomento relativo ai processi di supporto [per Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558), scaricare Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip e aprire "posizioni geografiche e collegamenti di comunicazione" (DSSTOPO_1. doc).  
  
## <a name="listing-ip-subnets-within-each-location"></a>Elenco di subnet IP all'interno di ogni località

Dopo aver documentato i collegamenti di comunicazione e la larghezza di banda disponibile tra le singole località, registrare le subnet IP all'interno di ogni percorso. Se non si conosce già il subnet mask e l'indirizzo IP all'interno di ogni località, consultare il gruppo di rete.  
  
Servizi di dominio Active Directory associa una workstation a un sito confrontando l'indirizzo IP della workstation con le subnet associate a ogni sito. Quando si aggiungono controller di dominio a un dominio, AD DS esamina anche i relativi indirizzi IP e li inserisce nel sito più appropriato.  
  
Per un foglio di lavoro che assiste l'utente nell'elenco delle subnet IP all'interno di ogni località, vedere l'argomento [relativo ai supporti per i processi per Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558), scaricare Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip e aprire "percorsi e subnet" (DSSTOPO_2. doc).  
  
> [!NOTE]  
> Oltre agli indirizzi IP versione 4 (IPv4), Windows Server supporta anche i prefissi di subnet IP versione 6 (IPv6). Per un foglio di un foglio di supporto per l'elenco dei prefissi di subnet IPv6, vedere [appendice a: posizioni e prefissi di subnet](../../ad-ds/plan/Appendix-A--Locations-and-Subnet-Prefixes.md).  

## <a name="listing-domains-and-number-of-users-for-each-location"></a>Elenco di domini e numero di utenti per ogni località

Il numero di utenti per ogni dominio regionale rappresentato in una posizione è uno dei fattori che determinano la posizione dei controller di dominio locali e dei server di catalogo globale, che rappresenta il passaggio successivo del processo di progettazione della topologia del sito. Ad esempio, pianificare la collocazione di un controller di dominio a livello di area in un percorso che contiene più di 100 utenti del dominio regionale, in modo che possano ancora accedere al dominio se il collegamento WAN ha esito negativo.  
  
Registrare i percorsi, i domini rappresentati in ogni posizione e il numero di utenti per ogni dominio rappresentato in ogni posizione. Per un foglio di lavoro che consente di elencare i domini e il numero di utenti che sono rappresentati in ogni posizione, vedere l'argomento relativo ai processi di supporto [per Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558), scaricare Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip e aprire "domini e utenti in ogni posizione" (DSSTOPO_3. doc).  
