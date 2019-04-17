---
ms.assetid: 8be8c48d-790c-4199-b9d3-9f4a07d5ced2
title: Raccolta di informazioni di rete
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 174f11ca85a659f9d0c52e220d5ce37b1804f39b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="collecting-network-information"></a>Raccolta di informazioni di rete

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Il primo passaggio nella progettazione di una topologia del sito effettiva in servizi di dominio Active Directory (AD DS) è di consultare gruppo rete dell'organizzazione per raccogliere informazioni e comunicare con essi regolarmente sulla topologia di rete fisica.  
  
## <a name="creating-a-location-map"></a>Creazione di un mapping di posizione  
Creare una mappa che rappresenta l'infrastruttura di rete fisica dell'organizzazione. Nel mapping di posizione, identificare le posizioni geografiche che contengono gruppi di computer con connettività interna di 10 megabit al secondo (Mbps) o versione successiva (velocità della rete locale (LAN) o superiore).  
  
## <a name="listing-communication-links-and-available-bandwidth"></a>Elenco di collegamenti di comunicazione e larghezza di banda disponibile  
Dopo aver creato un mapping di posizione, documentare il tipo di collegamento di comunicazione, la velocità di collegamento e larghezza di banda disponibile tra ogni posizione. Ottenere una topologia di wide area network (WAN) da un gruppo di rete. Per un elenco dei tipi comuni di circuito WAN e le larghezze di banda, vedere la sezione "Definizione dei costi" [creazione di una progettazione di collegamento di sito](../../ad-ds/plan/Creating-a-Site-Link-Design.md). Ti servirà queste informazioni per creare i collegamenti di sito in un secondo momento nel processo di progettazione di topologia del sito.  
  
Larghezza di banda si riferisce alla quantità di dati che è possibile trasmettere attraverso un canale di comunicazione in una determinata quantità di tempo. Larghezza di banda disponibile si riferisce alla quantità di larghezza di banda effettivamente disponibile per l'uso di dominio Active Directory. È possibile ottenere informazioni di larghezza di banda disponibile dal gruppo di rete oppure è possibile analizzare il traffico su ogni collegamento utilizzando un analizzatore di protocolli, ad esempio Network Monitor, che è un componente che viene fornito con Windows Server 2008. Per informazioni sull'installazione di Network Monitor, vedere il monitoraggio del traffico di rete ([https://go.microsoft.com/fwlink/?LinkId=107058](https://go.microsoft.com/fwlink/?LinkId=107058)).  
  
Documentare ogni posizione e altri percorsi che sono collegati a esso. Inoltre, registrare il tipo di collegamento di comunicazione e la larghezza di banda disponibile. Per un foglio di lavoro di favorire l'elenco di collegamenti di comunicazione e larghezza di banda disponibile, vedere processo Aid per Windows Server 2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)), scaricare Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip, e Aprire (DSSTOPO_1.doc) "Geografica percorsi e collegamenti di comunicazione".  
  
## <a name="listing-ip-subnets-within-each-location"></a>Elenco di subnet IP all'interno di ogni percorso  
Dopo che è documentare i collegamenti di comunicazione e la larghezza di banda disponibile tra ogni percorso, registrare le subnet IP all'interno di ogni posizione. Se non si conosce già un indirizzo IP all'interno di ogni posizione e la subnet mask, consultare il gruppo di rete.  
  
Servizi di dominio Active Directory associa una workstation con un sito confrontando l'indirizzo IP della workstation con le subnet che sono associati a ogni sito. Quando si aggiungono i controller di dominio a un dominio, AD DS esamina gli indirizzi IP e li inserisce nel sito più appropriato.  
  
Per un foglio di lavoro facilitare la presentazione le subnet IP all'interno di ogni percorso, vedere processo Aid per Windows Server 2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)), scaricare Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip e Apri "percorsi e subnet" (DSSTOPO_2.doc).  
  
> [!NOTE]  
> Oltre a IP versione 4 (IPv4) indirizzi, Windows Server 2008 supporta inoltre IP versione 6 (IPv6) i prefissi di subnet. Per un foglio di lavoro facilitare la presentazione i prefissi di subnet IPv6, vedere [appendice a: posizioni e prefissi di Subnet](../../ad-ds/plan/Appendix-A--Locations-and-Subnet-Prefixes.md).  
  
## <a name="listing-domains-and-number-of-users-for-each-location"></a>Elenco di domini e numero di utenti per ogni percorso  
Il numero di utenti per ogni dominio regionale rappresentato in una posizione è uno dei fattori che determinano il posizionamento del controller di dominio regionale e server di catalogo globale, ovvero il passaggio successivo del processo di progettazione della topologia del sito. Ad esempio, pianificare inserire un controller di dominio regionale in una posizione che contiene più di 100 utenti di dominio regionale in modo che possono accedere al dominio se il collegamento WAN.  
  
Registrare le posizioni, i domini che sono rappresentati in ogni percorso e il numero di utenti per ogni dominio che è rappresentato in ogni posizione. Per un foglio di lavoro di favorire l'elenco di domini e il numero di utenti che sono rappresentati in ogni percorso, vedere processo Aid per Windows Server 2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)), scaricare < DICT__Job_Aids_Designing_and_Deploying_ Directory_and_Security_Services.zip>Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip</DICT__Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip >, aprire "Domini e utenti in ogni posizione" (DSSTOPO_3.doc).  
  


