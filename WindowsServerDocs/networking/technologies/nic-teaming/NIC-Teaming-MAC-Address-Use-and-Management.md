---
title: Uso e gestione dell'indirizzo MAC del gruppo NIC
description: Quando si configura un gruppo NIC con hash indirizzo o la distribuzione del carico dinamico e Cambia modalità indipendente, l'accesso ai supporti il team utilizza controllo indirizzo (MAC) del membro del Team di interfaccia di rete primario sul traffico in uscita. Membro del Team di interfaccia di rete primario è una scheda di rete selezionata per il sistema operativo del set iniziale di membri del team.
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 26d105e0-afc3-44b5-bb5e-0c884a4c5d62
ms.author: pashort
author: shortpatti
ms.openlocfilehash: be05c4b3069d5b6928765d7211ae8f3a2dc42a13
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283762"
---
# <a name="nic-teaming-mac-address-use-and-management"></a>Uso e gestione dell'indirizzo MAC del gruppo NIC

>Si applica a: Windows Server 2016

Quando si configura un gruppo NIC con hash indirizzo o la distribuzione del carico dinamico e Cambia modalità indipendente, l'accesso ai supporti il team utilizza controllo indirizzo (MAC) del membro del Team di interfaccia di rete primario sul traffico in uscita. Membro del Team di interfaccia di rete primario è una scheda di rete selezionata per il sistema operativo del set iniziale di membri del team.  È il primo membro del team da associare al team dopo averlo creato, o dopo il riavvio del computer host. Poiché il membro del team principale potrebbe cambiare in modo non deterministico a ogni avvio, interfaccia di rete abilita/disabilita azione o altre attività di riconfigurazione, membro del team principale modifica e l'indirizzo MAC del team potrebbe variare.  
  
Nella maggior parte dei casi ciò non causa problemi, ma esistono alcuni casi in cui potrebbero verificarsi problemi.  
  
Se il membro del team primario viene rimosso dal team e quindi inserito nell'operazione potrebbe esserci un conflitto di indirizzi MAC. Per risolvere questo conflitto, disabilitare e quindi abilitare l'interfaccia del team. Il processo di disabilitazione e abilitazione l'interfaccia del team fa in modo che l'interfaccia selezionare un nuovo indirizzo MAC da membri del team rimanenti, eliminando così il conflitto di indirizzi MAC.  
  
È possibile impostare l'indirizzo MAC del gruppo NIC a un indirizzo MAC specifico mediante l'impostazione nell'interfaccia gruppo primaria, esattamente come è possibile eseguire quando si configura l'indirizzo MAC di qualsiasi interfaccia di rete fisica.  
  
## <a name="mac-address-use-on-transmitted-packets"></a>Indirizzo MAC usare pacchetti trasmessi  
Quando si configura un gruppo NIC in modalità Independente switch e hash indirizzo o la distribuzione del carico dinamico, i pacchetti provenienti da un'unica origine (ad esempio, una singola macchina virtuale) viene distribuito simultaneamente tra più membri del team. Per impedire che i commutatori confusione e impedire MAC instabile allarmi, l'indirizzo MAC di origine viene sostituito con un indirizzo MAC diverso sul frame trasmessi su membri del team diverso dal membro primario del team. Per questo motivo, ogni membro del team Usa un indirizzo MAC diverso e conflitti di indirizzi MAC non è consentiti solo se e fino a quando non si verifica un errore.  
  
Quando viene rilevato un errore nella scheda di rete primaria, il software gruppo NIC viene avviato usando l'indirizzo MAC del membro del team primario nel membro del team che viene scelto come membro del team primario temporaneo (ad esempio, quello che verranno visualizzati al commutatore come il team principale me mero).  Questa modifica si applica solo al traffico che stava per essere inviati nel membro del team primario con un indirizzo MAC del membro del team primario come relativo indirizzo MAC di origine. Il resto del traffico continua a essere inviato con qualsiasi origine, indirizzo MAC sarebbe utilizzato prima dell'errore.  
  
Di seguito sono elenchi che descrivono il comportamento di sostituzione indirizzo MAC di NIC Teaming, basato sulla configurazione del team:  
  
1.  **In modalità Independente commutatore con distribuzione Hash indirizzo**  
  
    -   Tutti i pacchetti ARP e NS vengono inviati sul membro del team primario  
  
    -   Tutto il traffico inviato in schede di rete diverso dal membro primario team vengono inviate con l'indirizzo MAC di origine modificato per corrispondere la scheda di rete in cui vengono inviati  
  
    -   Tutto il traffico inviato sul membro del team primario viene inviato con l'indirizzo MAC (che potrebbe essere l'indirizzo MAC di origine del team) di origine originale  
  
2.  **In modalità Independente commutatore alla distribuzione di porta Hyper-V**  
  
    -   Ogni porta del commutatore virtuale viene creata un'affinità a un membro del team  
  
    -   Tutti i pacchetti vengono inviati per il membro del team a cui viene creata un'affinità della porta  
  
    -   Non viene eseguita alcuna sostituzione MAC di origine  
  
3.  **Nel commutatore indipendente dalla modalità con la distribuzione dinamica**  
  
    -   Ogni porta del commutatore virtuale viene creata un'affinità a un membro del team  
  
    -   Tutti i pacchetti ARP/NS vengono inviati sul membro del team a cui viene creata un'affinità della porta  
  
    -   I pacchetti inviati nel membro del team che è membro del team creata non dispone di alcuna origine sostituzione di indirizzo MAC eseguita  
  
    -   I pacchetti inviati in un membro del team diverso dal membro del team creata disporrà di sostituzione di indirizzo MAC di origine eseguita  
  
4.  **In modalità dipendenti Switch (tutte le distribuzioni)**  
  
    -   Non viene eseguita alcuna sostituzione di indirizzo MAC di origine  
  
## <a name="related-topics"></a>Argomenti correlati
- [Gruppo NIC](NIC-Teaming.md): In questo argomento viene fornita è una panoramica del gruppo di schede di interfaccia di rete (NIC) in Windows Server 2016. Gruppo NIC consente di raggruppare tra 1 e 32 schede di rete Ethernet fisiche in una o più schede di rete virtuale basata su software. Queste schede di rete virtuali garantiscono prestazioni elevate e tolleranza di errore in caso di errore delle schede di rete.  

- [Le impostazioni di gruppo NIC](nic-teaming-settings.md): In questo argomento è fornire una panoramica delle proprietà del Team di interfaccia di rete, ad esempio gruppo NIC e modalità di bilanciamento del carico. È inoltre offrono informazioni dettagliate sull'impostazione della scheda di Standby e la proprietà dell'interfaccia gruppo primaria. Se si dispone di almeno due schede di rete in un gruppo NIC, non devi designare una scheda di Standby per la tolleranza di errore.
  
- [Creare un nuovo gruppo NIC in una VM o computer host](Create-a-New-NIC-Team-on-a-Host-Computer-or-VM.md): In questo argomento, creare un nuovo gruppo NIC in un computer host o in una macchina virtuale di Hyper-V (VM) che esegue Windows Server 2016.

- [Risoluzione dei problemi di gruppo NIC](Troubleshooting-NIC-Teaming.md): In questo argomento vengono illustrati modi per risolvere i problemi gruppo NIC, ad esempio hardware, titoli commutatore fisico e l'abilitazione o disabilitazione schede di rete tramite Windows PowerShell. 
  


