---
ms.assetid: 8be8c48d-790c-4199-b9d3-9f4a07d5ced2
title: Raccolta di informazioni di rete
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d88cc2fafd9aa4fc221efc901c48fa41ef007a21
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867042"
---
# <a name="collecting-network-information"></a>Raccolta di informazioni di rete

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Il primo passaggio nella progettazione di una topologia del sito effettivo in Active Directory Domain Services (AD DS) è consultare gruppo rete dell'organizzazione per raccogliere informazioni e la comunicazione con essi regolarmente sulla topologia di rete fisica.  
  
## <a name="creating-a-location-map"></a>Creazione di una mappa delle posizioni

Creare una mappa di percorso che rappresenta l'infrastruttura di rete fisica dell'organizzazione. Nella mappa del percorso, identificare le aree geografiche che contengono gruppi di computer con connettività interna di 10 megabit al secondo (Mbps) o superiore (velocità di rete locale (LAN) o versione successiva).  
  
## <a name="listing-communication-links-and-available-bandwidth"></a>Elenco di collegamenti di comunicazione e larghezza di banda disponibile

Dopo aver creato la mappa delle località, documentare il tipo di collegamento di comunicazione, la velocità di collegamento e la larghezza di banda disponibile tra ogni posizione. Ottenere una topologia di rete WAN WAN dal gruppo di rete. Per un elenco di tipi comuni di circuito WAN e le larghezze di banda, vedere la sezione "Determinazione del costo" nella [crea un'infrastruttura di collegamento di sito](../../ad-ds/plan/Creating-a-Site-Link-Design.md). È necessario che queste informazioni per creare collegamenti di sito in un secondo momento nel processo di progettazione della topologia del sito.  
  
Larghezza di banda si riferisce alla quantità di dati che è possibile trasmettere attraverso un canale di comunicazione entro un determinato periodo di tempo. Larghezza di banda disponibile si riferisce alla quantità di larghezza di banda effettiva disponibile per l'uso da Active Directory Domain Services. È possibile ottenere le informazioni di larghezza di banda disponibile dal gruppo di rete, oppure è possibile analizzare il traffico su ogni collegamento utilizzando un analizzatore di protocollo, ad esempio monitoraggio di rete. Per informazioni sull'installazione di Network Monitor, vedere l'articolo [il monitoraggio del traffico di rete](https://go.microsoft.com/fwlink/?LinkId=107058).  
  
Ogni posizione e le altri posizioni che sono collegate a esso del documento. Inoltre, registrare il tipo di collegamento di comunicazione e la larghezza di banda disponibile. Per un foglio di lavoro semplificare l'elenco di collegamenti di comunicazione e larghezza di banda disponibile, vedere [processo di supporto per Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558), scaricare Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip, e aprire "Geografica posizioni e collegamenti di comunicazione" (DSSTOPO_1.doc).  
  
## <a name="listing-ip-subnets-within-each-location"></a>Elencare le subnet IP all'interno di ogni percorso

Una volta documentare i collegamenti di comunicazione e la larghezza di banda disponibile tra ogni posizione, registrare le subnet IP all'interno di ogni località. Se non si conosce già la subnet mask e un indirizzo IP all'interno di ogni percorso, consultare il gruppo di rete.  
  
Active Directory Domain Services consente di associare una workstation con un sito tramite il confronto tra indirizzo IP della workstation con le subnet in cui sono associati a ogni sito. Quando si aggiungono i controller di dominio a un dominio, Active Directory Domain Services esamina i rispettivi indirizzi IP e li inserisce nel sito più appropriato.  
  
Per un foglio di lavoro agevolare la visualizzazione di un elenco di subnet IP all'interno di ogni percorso, vedere [processo di supporto per Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558), scaricare Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip e aprire " Percorsi e subnet"(DSSTOPO_2.doc).  
  
> [!NOTE]  
> Oltre a IP version 4 indirizzi (IPv4), Windows Server supporta anche IP versione 6 (IPv6) i prefissi di subnet. Per un foglio di lavoro semplificare l'elenco di prefissi di subnet IPv6, vedere [appendice a: Posizioni e prefissi di subnet](../../ad-ds/plan/Appendix-A--Locations-and-Subnet-Prefixes.md).  

## <a name="listing-domains-and-number-of-users-for-each-location"></a>Elenco di domini e numero di utenti per ogni posizione

Il numero di utenti per ogni dominio regionale rappresentato in una posizione è uno dei fattori che determinano il posizionamento dei controller di dominio internazionali e server di catalogo globale, ovvero il passaggio successivo nel processo di progettazione della topologia del sito. Ad esempio, prevede di inserire un controller di dominio regionale in una posizione che contiene più di 100 utenti di dominio regionale in modo che possono accedere al dominio se si verifica un errore di collegamento WAN.  
  
Registrare i percorsi, i domini che sono rappresentati in ogni posizione e il numero di utenti per ogni dominio che viene rappresentato in ogni posizione. Per un foglio di lavoro semplificare l'elenco di domini e il numero di utenti che sono rappresentati in ogni posizione, vedere [processo di supporto per Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558), scaricare Job_Aids_Designing_and_Deploying_Directory_and_ Security_Services.zip e "Domini e agli utenti in ogni posizione aperta" (DSSTOPO_3.doc).  
