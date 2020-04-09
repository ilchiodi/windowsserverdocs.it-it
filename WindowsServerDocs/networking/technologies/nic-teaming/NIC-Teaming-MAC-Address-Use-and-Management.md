---
title: Uso e gestione dell'indirizzo MAC del gruppo NIC
description: Quando si configura un gruppo NIC con modalità indipendente dal commutine e una distribuzione con hash di indirizzi o di carico dinamico, il team USA l'indirizzo Media Access Control (MAC) del membro del gruppo NIC primario nel traffico in uscita. Il membro del gruppo NIC primario è una scheda di rete selezionata dal sistema operativo dal set iniziale di membri del team.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-nict
ms.topic: article
ms.assetid: 26d105e0-afc3-44b5-bb5e-0c884a4c5d62
ms.author: lizross
author: eross-msft
ms.openlocfilehash: a1671b16bdadfcd159bc728f2d39ec45ad82fc0f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854724"
---
# <a name="nic-teaming-mac-address-use-and-management"></a>Uso e gestione dell'indirizzo MAC del gruppo NIC

>Si applica a: Windows Server 2016

Quando si configura un gruppo NIC con modalità indipendente dal commutine e una distribuzione con hash di indirizzi o di carico dinamico, il team USA l'indirizzo Media Access Control (MAC) del membro del gruppo NIC primario nel traffico in uscita. Il membro del gruppo NIC primario è una scheda di rete selezionata dal sistema operativo dal set iniziale di membri del team.  Si tratta del primo membro del team da associare al team dopo averlo creato o dopo il riavvio del computer host. Poiché il membro del team primario potrebbe cambiare in modo non deterministico a ogni avvio, azione Disabilita/Abilita NIC o altre attività di riconfigurazione, il membro del team primario potrebbe cambiare e l'indirizzo MAC del team potrebbe variare.  
  
Nella maggior parte dei casi ciò non provoca problemi, ma in alcuni casi potrebbero verificarsi problemi.  
  
Se il membro del team primario viene rimosso dal team e quindi inserito nell'operazione potrebbe verificarsi un conflitto di indirizzi MAC. Per risolvere questo conflitto, disabilitare e quindi abilitare l'interfaccia del team. Il processo di disabilitazione e abilitazione dell'interfaccia del team comporta la selezione di un nuovo indirizzo MAC da parte dei membri del team rimanenti, eliminando così il conflitto di indirizzi MAC.  
  
Per impostare l'indirizzo MAC del gruppo NIC su un indirizzo MAC specifico, è possibile impostarlo nell'interfaccia del team principale, esattamente come si può fare quando si configura l'indirizzo MAC di una scheda di interfaccia di rete fisica.  
  
## <a name="mac-address-use-on-transmitted-packets"></a>Indirizzo MAC usato nei pacchetti trasmessi  
Quando si configura un gruppo NIC in modalità indipendente dallo switch e si esegue l'hash degli indirizzi o la distribuzione del carico dinamico, i pacchetti provenienti da un'unica origine (ad esempio una singola macchina virtuale) vengono distribuiti simultaneamente tra più membri del team. Per evitare che i commutatori siano confusi e per evitare la visualizzazione di avvisi di MAC, l'indirizzo MAC di origine viene sostituito con un indirizzo MAC diverso nei frame trasmessi sui membri del team diversi dal membro del team primario. Per questo motivo, ogni membro del team usa un indirizzo MAC diverso e i conflitti di indirizzi MAC vengono evitati, a meno che non si verifichino errori.  
  
Quando viene rilevato un errore nella scheda di interfaccia di rete primaria, il software del gruppo NIC inizia a usare l'indirizzo MAC del membro del team primario sul membro del team scelto per fungere da membro del team primario temporaneo, ovvero quello che ora appare al compartitore come il team primario membro).  Questa modifica si applica solo al traffico che verrà inviato al membro del team primario con l'indirizzo MAC del membro del team primario come indirizzo MAC di origine. Il traffico continua a essere inviato con qualsiasi indirizzo MAC di origine usato prima dell'errore.  
  
Di seguito sono elencati gli elenchi che descrivono il comportamento di sostituzione degli indirizzi MAC del gruppo NIC, in base alla configurazione del team:  
  
1.  **In modalità indipendente dal cambio con distribuzione hash degli indirizzi**  
  
    -   Tutti i pacchetti ARP e NS vengono inviati al membro del team primario  
  
    -   Tutto il traffico inviato sulle schede di interfaccia di rete diverso dal membro del team primario viene inviato con l'indirizzo MAC di origine modificato in modo da corrispondere alla scheda di interfaccia di rete in cui vengono inviati  
  
    -   Tutto il traffico inviato al membro del team primario viene inviato con l'indirizzo MAC di origine originale (che potrebbe essere l'indirizzo MAC di origine del team)  
  
2.  **In modalità indipendente dal cambio con la distribuzione della porta Hyper-V**  
  
    -   Ogni porta vmSwitch è creata un'affinità a un membro del team  
  
    -   Ogni pacchetto viene inviato al membro del team a cui è creata un'affinità la porta  
  
    -   Non è stata eseguita alcuna sostituzione del MAC di origine  
  
3.  **In modalità indipendente dal cambio con distribuzione dinamica**  
  
    -   Ogni porta vmSwitch è creata un'affinità a un membro del team  
  
    -   Tutti i pacchetti ARP/NS vengono inviati al membro del team a cui è creata un'affinità la porta  
  
    -   I pacchetti inviati al membro del team che corrisponde al membro del team creata un'affinità non hanno completato la sostituzione degli indirizzi MAC di origine  
  
    -   I pacchetti inviati a un membro del team diverso dal membro del team creata un'affinità avranno completato la sostituzione degli indirizzi MAC di origine  
  
4.  **In modalità dipendente switch (tutte le distribuzioni)**  
  
    -   Non viene eseguita alcuna sostituzione degli indirizzi MAC di origine  
  
## <a name="related-topics"></a>Argomenti correlati
- [Gruppo NIC](NIC-Teaming.md): in questo argomento viene illustrata una panoramica del gruppo NIC (Network Interface Card) in Windows Server 2016. Gruppo NIC consente di raggruppare una o più schede di rete fisiche Ethernet da una a 32 in una o più schede di rete virtuali basate su software. Queste schede di rete virtuali garantiscono prestazioni elevate e tolleranza di errore in caso di errore delle schede di rete.  

- [Impostazioni gruppo NIC](nic-teaming-settings.md): in questo argomento viene illustrata una panoramica delle proprietà del gruppo NIC, ad esempio le modalità gruppo e bilanciamento del carico. Vengono inoltre illustrati i dettagli relativi all'impostazione della scheda standby e alla proprietà principale dell'interfaccia del team. Se si dispone di almeno due schede di rete in un gruppo NIC, non è necessario designare una scheda standby per la tolleranza di errore.
  
- [Creare un nuovo gruppo NIC in un computer host o](Create-a-New-NIC-Team-on-a-Host-Computer-or-VM.md)una macchina virtuale: in questo argomento si crea un nuovo gruppo NIC in un computer host o in una macchina virtuale Hyper-V che esegue Windows Server 2016.

- [Risoluzione dei problemi relativi al gruppo NIC](Troubleshooting-NIC-Teaming.md): in questo argomento vengono illustrati i modi per risolvere i problemi relativi al gruppo NIC, ad esempio l'hardware, i titoli del commutatore fisico e la disabilitazione o l'abilitazione di schede di rete con Windows PowerShell 
  


