---
title: Risoluzione dei problemi del gruppo NIC
description: In questo argomento vengono fornite informazioni sulla risoluzione dei problemi relativi al gruppo NIC in Windows Server 2016.
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fdee02ec-3a7e-473e-9784-2889dc1b6dbb
ms.author: pashort
author: shortpatti
ms.date: 09/13/2018
ms.openlocfilehash: 2f21301e0669fb593acda47787fed5f396618daf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405552"
---
# <a name="troubleshooting-nic-teaming"></a>Risoluzione dei problemi del gruppo NIC

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

In questo argomento vengono illustrati i modi per risolvere i problemi relativi al gruppo NIC, ad esempio i titoli hardware e commutatore fisico.  Quando le implementazioni hardware dei protocolli standard non sono conformi alle specifiche, le prestazioni del gruppo NIC potrebbero essere interessate. Inoltre, a seconda della configurazione, il gruppo NIC può inviare pacchetti dallo stesso indirizzo IP con più indirizzi MAC che interessano le funzionalità di sicurezza del comcambio fisico.

  
## <a name="hardware-that-doesnt-conform-to-specification"></a>Hardware non conforme alla specifica  
  
Durante il normale funzionamento, il gruppo NIC può inviare pacchetti dallo stesso indirizzo IP, ma con più indirizzi MAC. Secondo gli standard del protocollo, i destinatari di questi pacchetti devono risolvere l'indirizzo IP dell'host o della macchina virtuale in un indirizzo MAC specifico anziché rispondere all'indirizzo MAC dal pacchetto di destinazione.  I client che implementano correttamente i protocolli ARP e NDP (Address Resolution Protocol) inviano i pacchetti con gli indirizzi MAC di destinazione corretti, ovvero l'indirizzo MAC della macchina virtuale o dell'host che possiede tale indirizzo IP. 
  
Alcuni componenti hardware incorporati non implementano correttamente i protocolli di risoluzione degli indirizzi e potrebbero anche non risolvere in modo esplicito un indirizzo IP in un indirizzo MAC utilizzando ARP o NDP.  Ad esempio, un controller SAN (Storage Area Network) può essere eseguito in questo modo. Dispositivi non conformi copiare l'indirizzo MAC di origine da un pacchetto ricevuto e quindi usarlo come indirizzo MAC di destinazione nei pacchetti in uscita corrispondenti, ottenendo i pacchetti inviati all'indirizzo MAC di destinazione errato. Per questo motivo, i pacchetti vengono eliminati dal Commuter virtuale Hyper-V perché non corrispondono ad alcuna destinazione nota.  
  
Se si verificano problemi di connessione ai controller SAN o ad altri componenti hardware incorporati, è necessario eseguire acquisizioni di pacchetti per determinare se l'hardware sta implementando correttamente ARP o NDP e contattare il fornitore dell'hardware per assistenza.  

  
## <a name="physical-switch-security-features"></a>Funzionalità di sicurezza del comcambio fisico  
A seconda della configurazione, il gruppo NIC può inviare pacchetti dallo stesso indirizzo IP con più indirizzi MAC di origine per l'attivazione delle funzionalità di sicurezza del Commuter fisico. Ad esempio, l'ispezione ARP dinamica o la protezione origine IP, soprattutto se il Commuter fisico non è in grado di riconoscere che le porte fanno parte di un team, che si verifica quando si configura gruppo NIC in modalità switch indipendente. Esaminare i log delle opzioni per determinare se le funzionalità di sicurezza del commutire causano problemi di connettività. 
  
## <a name="disabling-and-enabling-network-adapters-by-using-windows-powershell"></a>Disabilitazione e abilitazione di schede di rete tramite Windows PowerShell  

Un motivo comune per cui un gruppo NIC ha esito negativo è che l'interfaccia del team è disabilitata e, in molti casi, per errore durante l'esecuzione di una sequenza di comandi.  Questa particolare sequenza di comandi non Abilita tutti i NetAdapters disabilitati perché la disabilitazione di tutti i membri fisici sottostanti delle schede di interfaccia di rete rimuove l'interfaccia del gruppo NIC. 

In questo caso, l'interfaccia del gruppo NIC non viene più visualizzata in Get-NetAdapter e per questo motivo **Enable-NetAdapter \\** * non Abilita il gruppo NIC. Il comando **Enable-NetAdapter \\** *, tuttavia, Abilita le NIC membri, che quindi (dopo un breve periodo di tempo) ricrea l'interfaccia del team. L'interfaccia del team rimane nello stato "disabilitato" fino a quando non viene abilitata di nuovo, consentendo il flusso del traffico di rete. 

La sequenza di comandi di Windows PowerShell seguente può disabilitare l'interfaccia del team per errore:  
  
```PowerShell 
Disable-NetAdapter *  
Enable-NetAdapter *  
```  
  

  
## <a name="related-topics"></a>Argomenti correlati  
- [Gruppo NIC](NIC-Teaming.md): In questo argomento viene illustrata una panoramica del gruppo NIC (Network Interface Card) in Windows Server 2016. Gruppo NIC consente di raggruppare una o più schede di rete fisiche Ethernet da una a 32 in una o più schede di rete virtuali basate su software. Queste schede di rete virtuali garantiscono prestazioni elevate e tolleranza di errore in caso di errore delle schede di rete.   

- [Gestione e utilizzo degli indirizzi MAC del gruppo NIC](NIC-Teaming-MAC-Address-Use-and-Management.md): Quando si configura un gruppo NIC con modalità indipendente dal commutine e una distribuzione con hash di indirizzi o di carico dinamico, il team USA l'indirizzo Media Access Control (MAC) del membro del gruppo NIC primario nel traffico in uscita. Il membro del gruppo NIC primario è una scheda di rete selezionata dal sistema operativo dal set iniziale di membri del team.

- [Impostazioni gruppo NIC](nic-teaming-settings.md): In questo argomento viene illustrata una panoramica delle proprietà del gruppo NIC, ad esempio le modalità gruppo e bilanciamento del carico. Vengono inoltre illustrati i dettagli relativi all'impostazione della scheda standby e alla proprietà principale dell'interfaccia del team. Se si dispone di almeno due schede di rete in un gruppo NIC, non è necessario designare una scheda standby per la tolleranza di errore.
  


