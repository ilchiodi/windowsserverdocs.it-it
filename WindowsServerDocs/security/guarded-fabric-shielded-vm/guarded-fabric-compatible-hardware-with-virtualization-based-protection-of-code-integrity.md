---
title: Componenti hardware compatibili con protezione basata sulla virtualizzazione di Server Windows di integrità del codice
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 15ded82c-f70f-4efb-9e26-2731127931af
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: a52ff808af94159fe50c72bf0466768047ceea90
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820372"
---
# <a name="compatible-hardware-with-windows-server-virtualization-based-protection-of-code-integrity"></a>Componenti hardware compatibili con protezione basata sulla virtualizzazione di Server Windows di integrità del codice

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

Windows Server 2016 ha introdotto una nuova protezione basata su virtualizzazione codice per la protezione fisica e le macchine virtuali da attacchi che modificano il codice di sistema. Per ottenere questo livello di protezione elevata, Microsoft si impegna in parallelo con i produttori di hardware di computer (Original Equipment Manufacturer o OEM) per impedire operazioni di scrittura dannosi nel codice di esecuzione del sistema. Questa protezione può essere applicata a tutti i sistemi e viene utilizzata come uno degli elementi costitutivi per implementare l'integrità di host Hyper-V per macchine virtuali schermate (VM). 

Come con qualsiasi protezione basata su hardware, alcuni sistemi potrebbero non essere conforme a causa di problemi, ad esempio il contrassegno non corretto della pagine in memoria come eseguibili o in effetti tentando di modificare il codice in fase di esecuzione che potrebbe comportare errori imprevisti, tra cui la perdita di dati o di colore blu Errore dello schermo (detto anche un errore irreversibile). 

Per essere compatibile e supportano completamente la nuova funzionalità di sicurezza, è necessario implementare la tabella di indirizzi di memoria definito in 2.6 UEFI, che è stata pubblicata in gennaio 2016 gli OEM. L'adozione del nuovo UEFI standard richiede tempo; Nel frattempo, per evitare che i clienti che si verifichi problemi, si vogliono fornire informazioni sui sistemi e le configurazioni che è stato testato questa funzionalità impostata con, nonché i sistemi che sappiamo di non essere compatibili. 

## <a name="non-compatible-systems"></a>Sistemi non compatibili

Le configurazioni seguenti sono noti come non compatibile con la protezione basata sulla virtualizzazione di integrità del codice e non può essere usato come host per le macchine virtuali schermate:

- Server Dell PowerEdge esecuzione PERC H330 RAID controller per altre informazioni, vedere l'articolo seguente dal supporto tecnico Dell [H330-abilitazione "Supporto Hyper-V per sorveglianza Host" o "Device Guard" nel sistema operativo di Windows 2016 causa un errore di avvio del sistema operativo](http://www.dell.com/Support/Article/us/en/19/QNA44045).  


## <a name="compatible-systems"></a>Sistemi compatibili

Questi sono i sistemi che Microsoft e i partner è sono testato nell'ambiente in uso. Assicurarsi di verificare che il sistema funziona come previsto nell'ambiente in uso: 

- Macchine virtuali: È possibile abilitare la protezione basata sulla virtualizzazione di integrità del codice nelle macchine virtuali in esecuzione su un host Hyper-V a partire da Windows Server 2016.



