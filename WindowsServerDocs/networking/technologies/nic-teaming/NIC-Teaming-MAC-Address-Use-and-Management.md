---
title: Gruppo di gestione e l'utilizzo di indirizzi MAC NIC
description: Questo argomento fornisce informazioni su come gruppo NIC utilizza che l'accesso ai supporti (indirizzo MAC control) in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 26d105e0-afc3-44b5-bb5e-0c884a4c5d62
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6ab624665c964b100e6b1ed633120299b55e37df
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="nic-teaming-mac-address-use-and-management"></a>Gruppo di gestione e l'utilizzo di indirizzi MAC NIC

>Si applica a: Windows Server 2016

Quando si configura un gruppo NIC con hash indirizzo o la distribuzione del carico dinamico e Cambia modalità indipendenti, il team di utilizzare che l'accesso ai supporti (indirizzo MAC control) del membro del gruppo NIC primario sul traffico in uscita. Membro del Team NIC primario è selezionata per il sistema operativo del set iniziale dei membri del team di una scheda di rete.  
  
Membro del team primario è il primo membro del team per eseguire il binding al team dopo averlo creato, o dopo il riavvio del computer host. Perché il membro del gruppo primario può cambiare in modo non deterministico a ogni avvio e NIC disattivare o attivare l'azione o altre attività di riconfigurazione, il membro del gruppo primario potrebbe modifica e l'indirizzo MAC del team può variare.  
  
Nella maggior parte dei casi questo non causare problemi, ma esistono alcuni casi in cui potrebbero verificarsi problemi.  
  
Se il membro del gruppo primario viene rimossa dal team di e quindi inserito in operazione potrebbe esserci un conflitto di indirizzi MAC. Per risolvere il conflitto, disabilitare e abilitare l'interfaccia del team. Il processo di disabilitazione e abilitazione quindi l'interfaccia del team fa sì che l'interfaccia selezionare un nuovo indirizzo MAC dai membri del team rimanenti, eliminando così il conflitto di indirizzi MAC.  
  
È possibile impostare l'indirizzo MAC del gruppo NIC a un indirizzo MAC specifico mediante l'impostazione nell'interfaccia gruppo primaria, esattamente come è possibile eseguire durante la configurazione dell'indirizzo MAC di qualsiasi rete fisica.  
  
## <a name="mac-address-use-on-transmitted-packets"></a>Indirizzo MAC utilizzare pacchetti trasmessi  
Quando si configura un gruppo NIC in modalità indipendente commutatore e hash indirizzo o la distribuzione del carico dinamico, i pacchetti provenienti da un'unica origine (ad esempio, una singola macchina virtuale) viene distribuito contemporaneamente su più membri del team. Per impedire che i commutatori confusione e impedire MAC battere Sveglie, l'indirizzo MAC di origine viene sostituito con un indirizzo MAC diverso sui fotogrammi trasmessi sui membri del team diversi da membro del team primario. Per questo motivo, ogni membro del team Usa un indirizzo MAC diverso e conflitti di indirizzi MAC non sono autorizzati a meno che e fino a quando non si verifica un errore.  
  
Quando viene rilevato un errore nella scheda di rete primaria, il software gruppo NIC viene avviato utilizzando l'indirizzo MAC del membro del team primario nel membro del team che viene scelto come membro del team primario temporaneo (ad esempio quello che verrà ora visualizzato al commutatore come membro del team primario).  Questa modifica si applica solo al traffico che doveva essere inviato nel membro del gruppo primario con un indirizzo MAC del membro del team primario come il relativo indirizzo MAC di origine. Altro traffico continua a essere inviate con qualsiasi indirizzo MAC avresti usato prima dell'errore di origine.  
  
Di seguito sono gli elenchi di descrivono il comportamento di sostituzione indirizzo MAC del gruppo NIC, in base alle modalità di configurazione del team:  
  
1.  **In modalità indipendenti commutatore con distribuzione Hash indirizzo**  
  
    -   Tutti i pacchetti ARP e NS vengono inviati nel membro del gruppo primaria  
  
    -   Tutto il traffico inviato nelle schede NIC diverso dal membro del team primario vengono inviate con l'indirizzo MAC di origine modificato per corrispondere la scheda di rete in cui vengono inviati  
  
    -   Tutto il traffico inviato nel membro del gruppo primario viene inviato con l'indirizzo MAC (che potrebbe essere l'indirizzo MAC di origine del team) di origine originale  
  
2.  **In modalità indipendenti commutatore con distribuzione porta Hyper-V**  
  
    -   Ogni porta vmSwitch affinità con un membro del team  
  
    -   Ogni pacchetto viene inviato nel membro del gruppo a cui è affinità con la porta  
  
    -   Non viene eseguita alcuna sostituzione MAC di origine  
  
3.  **In modalità indipendenti commutatore con la distribuzione dinamica**  
  
    -   Ogni porta vmSwitch affinità con un membro del team  
  
    -   Tutti i pacchetti ARP/NS vengono inviati nel membro del gruppo a cui è affinità con la porta  
  
    -   I pacchetti inviati nel membro del team che è membro del team di affinità non avere alcuna origine sostituzione indirizzo MAC fatto  
  
    -   I pacchetti inviati in un membro del team diversi da membro del team di affinità avrà sostituzione di indirizzo MAC di origine fatto  
  
4.  **In modalità dipendenti commutatore (tutte le distribuzioni)**  
  
    -   Non viene eseguita alcuna sostituzione indirizzo MAC di origine  
  
## <a name="see-also"></a>Vedere anche  
[Creare un nuovo gruppo NIC in un Computer Host o macchina virtuale](Create-a-New-NIC-Team-on-a-Host-Computer-or-VM.md)  
[Gruppo NIC](NIC-Teaming.md)  
  


