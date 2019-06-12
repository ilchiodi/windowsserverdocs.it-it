---
title: Risoluzione dei problemi del gruppo NIC
description: In questo argomento vengono fornite informazioni sulla risoluzione dei problemi NIC di Windows Server 2016.
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fdee02ec-3a7e-473e-9784-2889dc1b6dbb
ms.author: pashort
author: shortpatti
ms.date: 09/13/2018
ms.openlocfilehash: a6af3cbd038e97d889269b83d72c77c50680e513
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446176"
---
# <a name="troubleshooting-nic-teaming"></a>Risoluzione dei problemi del gruppo NIC

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

In questo argomento vengono illustrati modi per risolvere i problemi gruppo NIC, ad esempio titoli commutatore fisico e hardware.  Quando le implementazioni di hardware di protocolli standard non sono conformi alle specifiche, NIC Teaming prestazioni potrebbero risentirne. Inoltre, a seconda della configurazione, NIC Teaming può inviare pacchetti dallo stesso indirizzo IP con più indirizzi MAC far commettere errori funzionalità di sicurezza del commutatore fisico.

  
## <a name="hardware-that-doesnt-conform-to-specification"></a>Hardware che non sono conformi alla specifica  
  
Durante il normale funzionamento, NIC Teaming può inviare pacchetti dallo stesso indirizzo IP, ancora con più indirizzi MAC. In base agli standard di protocollo, i ricevitori di questi pacchetti è necessario risolvere l'indirizzo IP dell'host o macchina virtuale a un indirizzo MAC specifico piuttosto che rispondono all'indirizzo MAC del pacchetto ricevente.  I client che implementano correttamente i protocolli di risoluzione indirizzi (ARP e NDP) inviano pacchetti con gli indirizzi MAC di destinazione corretta, vale a dire, l'indirizzo MAC della macchina virtuale o un host a cui appartiene tale indirizzo IP. 
  
Alcuni dispositivi hardware incorporate non implementa correttamente i protocolli di risoluzione indirizzi e anche potrebbe non in modo esplicito risolve un indirizzo IP a un indirizzo MAC usando ARP o NDP.  Ad esempio, un controller di rete (SAN) area di archiviazione è possibile eseguire in questo modo. Dispositivi non conformi copiare l'indirizzo MAC di origine da un pacchetto ricevuto e quindi usano come l'indirizzo MAC di destinazione nei pacchetti in uscita corrispondenti, causando i pacchetti inviati per l'indirizzo MAC di destinazione errato. Per questo motivo, i pacchetti vengono eliminati dal commutatore virtuale Hyper-V perché non rispettavano qualsiasi destinazione noto.  
  
Nel caso di problemi con la connessione al controller di rete SAN o altro incorporato hardware, è consigliabile eseguire le acquisizioni di pacchetti per determinare se l'hardware sta implementando correttamente ARP o NDP e contattare il fornitore dell'hardware per il supporto.  

  
## <a name="physical-switch-security-features"></a>Funzionalità di sicurezza commutatore fisico  
A seconda della configurazione, NIC Teaming può inviare pacchetti dallo stesso indirizzo IP con più indirizzi MAC di origine far commettere errori funzionalità di sicurezza del commutatore fisico. Ad esempio, ispezione dinamica ARP o IP origine guard, soprattutto se commutatore fisico non tenere presente che le porte siano parte di un team, che si verifica quando si configura gruppo NIC in modalità commutatore indipendente. Esaminare i registri di switch per determinare se la funzionalità di sicurezza commutatore causano problemi di connettività. 
  
## <a name="disabling-and-enabling-network-adapters-by-using-windows-powershell"></a>Disabilitazione e abilitazione di schede di rete tramite Windows PowerShell  

Un motivo comune per un gruppo NIC esito negativo è che l'interfaccia del team è disabilitata e in molti casi, per errore durante l'esecuzione di una sequenza di comandi.  Questa particolare sequenza di comandi non abilita tutte le NetAdapters disabilitata perché la disabilitazione di tutti i membri di fisici sottostanti di schede di rete Elimina l'interfaccia del team NIC. 

In questo caso, l'interfaccia del team NIC non viene più visualizzato in Get-NetAdapter e per questo motivo, **Enable-NetAdapter \\** * non abilita il gruppo NIC. Il **Enable-NetAdapter \\** * comandi, tuttavia, abilitare il membro schede di rete, che quindi (dopo un breve periodo) ricrea l'interfaccia del team. L'interfaccia del team rimane nello stato "disabilitato" fino a abilitata di nuovo, che consente il traffico di rete avviare il flusso. 

La sequenza di comandi di Windows PowerShell seguente può disabilitare l'interfaccia del team venga rifiutata accidentalmente:  
  
```PowerShell 
Disable-NetAdapter *  
Enable-NetAdapter *  
```  
  

  
## <a name="related-topics"></a>Argomenti correlati  
- [Gruppo NIC](NIC-Teaming.md): In questo argomento viene fornita è una panoramica del gruppo di schede di interfaccia di rete (NIC) in Windows Server 2016. Gruppo NIC consente di raggruppare tra 1 e 32 schede di rete Ethernet fisiche in una o più schede di rete virtuale basata su software. Queste schede di rete virtuali garantiscono prestazioni elevate e tolleranza di errore in caso di errore delle schede di rete.   

- [Gestione e uso di indirizzi MAC di NIC Teaming](NIC-Teaming-MAC-Address-Use-and-Management.md): Quando si configura un gruppo NIC con hash indirizzo o la distribuzione del carico dinamico e Cambia modalità indipendente, l'accesso ai supporti il team utilizza controllo indirizzo (MAC) del membro del Team di interfaccia di rete primario sul traffico in uscita. Membro del Team di interfaccia di rete primario è una scheda di rete selezionata per il sistema operativo del set iniziale di membri del team.

- [Le impostazioni di gruppo NIC](nic-teaming-settings.md): In questo argomento è fornire una panoramica delle proprietà del Team di interfaccia di rete, ad esempio gruppo NIC e modalità di bilanciamento del carico. È inoltre offrono informazioni dettagliate sull'impostazione della scheda di Standby e la proprietà dell'interfaccia gruppo primaria. Se si dispone di almeno due schede di rete in un gruppo NIC, non devi designare una scheda di Standby per la tolleranza di errore.
  


