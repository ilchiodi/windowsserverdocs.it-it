---
title: Hardware compatibile con la protezione basata sulla virtualizzazione di Windows Server per l'integrità del codice
ms.prod: windows-server
ms.topic: article
ms.assetid: 15ded82c-f70f-4efb-9e26-2731127931af
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 32194d6f0634ab9cee90b321ea7a1f3e2769542d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856874"
---
# <a name="compatible-hardware-with-windows-server-virtualization-based-protection-of-code-integrity"></a>Hardware compatibile con la protezione basata sulla virtualizzazione di Windows Server per l'integrità del codice

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

Windows Server 2016 ha introdotto una nuova protezione del codice basata sulla virtualizzazione per proteggere le macchine fisiche e virtuali dagli attacchi che modificano il codice di sistema. Per ottenere questo livello di protezione elevato, Microsoft lavora insieme all'hardware del computer (Original Equipment Manufacturer) per evitare scritture dannose nel codice di esecuzione del sistema. Questa protezione può essere applicata a qualsiasi sistema e viene usata come uno dei blocchi predefiniti per l'implementazione dell'integrità dell'host Hyper-V per le macchine virtuali schermate. 

Come per qualsiasi protezione basata su hardware, alcuni sistemi potrebbero non essere conformi a causa di problemi quali il contrassegno errato di pagine di memoria come file eseguibili o il tentativo di modificare il codice in fase di esecuzione, che può causare errori imprevisti, ad esempio la perdita di dati o un errore di schermata blu (detto anche errore di arresto). 

Per essere compatibile e supportare completamente la nuova funzionalità di sicurezza, gli OEM devono implementare la tabella degli indirizzi di memoria definita in UEFI 2,6, pubblicata in Jan. 2016. L'adozione del nuovo standard UEFI richiede tempo; nel frattempo, per impedire ai clienti di riscontrare problemi, è opportuno fornire informazioni sui sistemi e le configurazioni di cui è stato testato il set di funzionalità, oltre ai sistemi che non sono compatibili. 

## <a name="non-compatible-systems"></a>Sistemi non compatibili

Le configurazioni seguenti sono note come non compatibili con la protezione basata sulla virtualizzazione dell'integrità del codice e non possono essere usate come host per le macchine virtuali schermate:

- Server Dell PowerEdge che eseguono controller RAID PERC H330 per altre informazioni, vedere l'articolo seguente di Dell Support [H330-abilitazione di "supporto di sorveglianza host per Hyper-V" o "Device Guard" nel sistema operativo Win 2016 causa un errore di avvio del sistema operativo](http://www.dell.com/Support/Article/us/en/19/QNA44045).  


## <a name="compatible-systems"></a>Sistemi compatibili

Questi sono i sistemi in cui i nostri partner sono stati testati nell'ambiente. Assicurarsi di verificare che il sistema funzioni come previsto nell'ambiente: 

- Macchine virtuali: è possibile abilitare la protezione basata sulla virtualizzazione dell'integrità del codice in macchine virtuali eseguite in un host Hyper-V a partire da Windows Server 2016.



