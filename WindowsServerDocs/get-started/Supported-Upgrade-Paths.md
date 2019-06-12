---
title: Opzioni di aggiornamento e conversione per Windows Server 2016
description: Illustra tutti i percorsi di aggiornamento a Windows Server 2016 supportati.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.date: 01/18/2017
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 74aa1da3-7076-4a1f-ad5b-9e17bd46dba2
author: jaimeo
ms.author: jaimeo
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: 299cf420b44e4a15985d00489edf84784316540d
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66810580"
---
# <a name="upgrade-and-conversion-options-for-windows-server-2016"></a>Opzioni di aggiornamento e conversione per Windows Server 2016

>Si applica a: Windows Server 2019, Windows Server 2016

Questo argomento include informazioni sull'aggiornamento a Windows Server® 2016 da una vasta gamma di sistemi operativi precedenti mediante metodi diversi.

Il processo di transizione a Windows Server 2016 può variare notevolmente a seconda del sistema operativo di partenza e del percorso scelto. Per distinguere le diverse azioni, ognuna delle quali potrebbe essere richiesta per una nuova distribuzione di Windows Server 2016, vengono usati i termini seguenti.

- Il termine**installazione** indica il concetto di base di aggiungere il nuovo sistema operativo nell'hardware disponibile. Nello specifico, per un'**installazione pulita** è necessario eliminare il sistema operativo precedente. Per informazioni sull'installazione di Windows Server 2016, vedere [System Requirements and Installation Information for Windows Server 2016](https://technet.microsoft.com/windows-server-docs/get-started/system-requirements--and-installation) (Requisiti di sistema e informazioni sull'installazione per Windows Server 2016). Per informazioni sull'installazione di altre versioni di Windows Server, vedere l'argomento relativo a [installazione e aggiornamento di Windows Server](https://technet.microsoft.com//windowsserver/dn527667).

- Il termine **migrazione** indica il passaggio dal sistema operativo esistente a Windows Server 2016 mediante il trasferimento di un set diverso di componenti hardware o macchine virtuali. Il processo di migrazione, che può variare notevolmente in base ai ruoli server installati, è illustrato in dettaglio in [Windows Server Installation, Upgrade, and Migration](https://technet.microsoft.com/windowsserver/dn458795) (Installazione, aggiornamento e migrazione di Windows Server).

- L'**aggiornamento in sequenza del sistema operativo del cluster** è una nuova funzionalità di Windows Server 2016 che consente a un amministratore di aggiornare il sistema operativo dei nodi del cluster da Windows Server 2012 R2 a Windows Server 2016 senza interrompere i carichi di lavoro del file server di scalabilità orizzontale o di Hyper-V. Questa funzionalità consente di evitare tempi di inattività che potrebbero influire sui contratti di servizio. La nuova funzionalità è illustrata in modo più dettagliato in [Aggiornamento in sequenza del sistema operativo del cluster](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade).

- La **conversione delle licenze** in alcune versioni di sistemi operativi consente di convertire una particolare edizione della versione in un'altra edizione della stessa versione con un'unica operazione, usando un semplice comando e il codice di licenza appropriato. Questo processo è definito come "conversione delle licenze". Se ad esempio si esegue Windows Server 2016 Standard, è possibile convertirlo in Windows Server 2016 Datacenter.

- Il termine **aggiornamento** indica il passaggio dalla versione del sistema operativo esistente a una versione più recente, sempre nello stesso hardware. È talvolta definito aggiornamento "sul posto". Ad esempio, un server su cui è in esecuzione Windows Server 2012 o Windows Server 2012 R2 può essere aggiornato a Windows Server 2016. È possibile effettuare l'aggiornamento da una versione di valutazione del sistema operativo a una versione definitiva, da una versione definitiva precedente a una più recente oppure, in alcuni casi, da un'edizione del sistema operativo con contratto multilicenza a una normale edizione definitiva.

> [!IMPORTANT]  
> L'aggiornamento funziona meglio in macchine virtuali in cui non sono necessari specifici driver hardware originali per l'aggiornamento.  

> [!IMPORTANT]  
> Per le versioni di Windows Server 2016 precedenti a 14393.0.161119-1705.RS1_REFRESH, **è possibile eseguire solo la conversione dalla versione di valutazione a quella definitiva** con Windows Server 2016 installato mediante l'opzione Esperienza desktop (non l'opzione Server Core). A partire dalla versione 14393.0.161119-1705. RS1_REFRESH, è possibile convertire le edizioni di valutazione in quella definitiva indipendentemente dall'opzione di installazione utilizzata.

> [!IMPORTANT]  
> Se il server usa Gruppo NIC, disabilitare questa funzionalità prima dell'aggiornamento e quindi riabilitarla dopo che l'aggiornamento è stato completato. Per informazioni dettagliate, vedere [Panoramica di Gruppo NIC](https://technet.microsoft.com/library/hh831648(v=ws.11).aspx).

## <a name="upgrading-previous-retail-versions-of-windows-server-to-windows-server-2016"></a>Aggiornamento delle versioni definitive precedenti di Windows Server a Windows Server 2016

La tabella seguente presenta un breve riepilogo delle versioni dei sistemi operativi Windows **con licenza** (ovvero versioni non di valutazione) che possono essere aggiornate a edizioni di Windows Server 2016.

Per i percorsi supportati valgono le linee guida generali seguenti:

- Gli aggiornamenti da un'architettura a 32 bit a un'architettura a 64 bit non sono supportati. Tutte le edizioni di Windows Server 2016 sono solo a 64 bit.
- Gli aggiornamenti da una lingua a un'altra non sono supportati.
- Se il server è un controller di dominio, vedere [Aggiornare controller di dominio a Windows Server 2012 R2 e Windows Server 2012](https://technet.microsoft.com/library/hh994618.aspx) per informazioni importanti.
- Gli aggiornamenti da versioni non definitive (anteprime) di Windows Server 2016 non sono supportati. Eseguire un'installazione pulita a Windows Server 2016.
- Gli aggiornamenti che prevedono il passaggio da un'installazione Server Core a un'installazione Server con Esperienza desktop (e viceversa) non sono supportati.
- Gli aggiornamenti da una precedente installazione di Windows Server a una copia di valutazione di Windows Server non sono supportati. Le versioni di valutazione devono essere installate con un'installazione pulita.

Se la versione in uso non è inclusa nella colonna a sinistra, l'aggiornamento a tale versione di Windows Server 2016 non è supportato.

Se nella colonna a destra sono presenti più edizioni, è supportato l'aggiornamento dalla stessa versione di partenza a **qualsiasi** edizione indicata.

|Edizioni eseguite|Edizioni a cui è possibile eseguire l'aggiornamento|  
|-------------------|----------|  
|Windows Server 2012 Standard|Windows Server 2016 Standard o Datacenter|
|Windows Server 2012 Datacenter|Windows Server 2016 Datacenter|
|Windows Server 2012 R2 Standard|Windows Server 2016 Standard o Datacenter|
|Windows Server 2012 R2 Datacenter|Windows Server 2016 Datacenter|
|Windows Server 2012 R2 Essentials|Windows Server 2016 Essentials|
|Windows Storage Server 2012 Standard|Windows Storage Server 2016 Standard|
|Windows Storage Server 2012 Workgroup|Windows Storage Server 2016 Workgroup|
|Windows Storage Server 2012 R2 Standard|Windows Storage Server 2016 Standard|
|Windows Storage Server 2012 R2 Workgroup|Windows Storage Server 2016 Workgroup|


## <a name="per-server-role-considerations-for-upgrading"></a>Considerazioni relative all'aggiornamento per ruolo del server

Anche nei percorsi di aggiornamento supportati da versioni definitive precedenti Windows Server 2016, per determinati ruoli server già installati possono essere necessarie altre operazioni di preparazione o azioni per consentire il corretto funzionamento del ruolo dopo l'aggiornamento. Per informazioni dettagliate sui passaggi aggiuntivi necessari, vedere gli argomenti della Libreria TechNet specifici per ogni ruolo del server che si vuole aggiornare.

## <a name="converting-a-current-evaluation-version-to-a-current-retail-version"></a>Conversione di una versione di valutazione corrente in una versione definitiva corrente

È possibile convertire la versione di valutazione di Windows Server 2016 Standard a Windows Server 2016 Standard (definitiva) o Datacenter (definitiva). Analogamente, è possibile convertire la versione di valutazione di Windows Server 2016 Datacenter alla versione definitiva.

> [!IMPORTANT]  
> Per le versioni di Windows Server 2016 precedenti a 14393.0.161119-1705.RS1_REFRESH, è possibile eseguire solo la conversione dalla versione di valutazione a quella definitiva con Windows Server 2016 installato mediante l'opzione Esperienza desktop (non l'opzione Server Core). A partire dalla versione 14393.0.161119-1705. RS1_REFRESH, è possibile convertire le edizioni di valutazione in quella definitiva indipendentemente dall'opzione di installazione utilizzata.

Prima di tentare di eseguire la conversione dalla versione di valutazione a quella definitiva, verificare che nel server sia effettivamente in esecuzione una versione di valutazione. A tale scopo, eseguire una delle operazioni seguenti:

- Da un prompt dei comandi con privilegi elevati eseguire **slmgr.vbs /dlv**. Per le versioni di valutazione nell'output è indicato "EVAL".

- Dalla schermata Start aprire il **Pannello di controllo**. Aprire **Sistema e sicurezza**e quindi **Sistema**. Visualizzare lo stato di attivazione di Windows nella relativa area della pagina **Sistema**. Fare clic su **Visualizza dettagli in attivazione di Windows** per altre informazioni sullo stato di attivazione di Windows.

Se Windows è già stato attivato, sul desktop viene visualizzato il tempo rimanente per il periodo di valutazione.

Se sul server è in esecuzione una versione definitiva anziché una versione di valutazione, vedere la sezione "Aggiornamento delle versioni definitive precedenti di Windows Server a Windows Server 2016" in questo documento per informazioni sull'aggiornamento a Windows Server 2016.

Per la **Windows Server 2016 Essentials**: È possibile convertire in versione definitiva completa immettendo delle vendite al dettaglio, per contratti multilicenza o tasto OEM nel comando **slmgr. vbs**.

Se nel server è in esecuzione una versione di valutazione di Windows Server 2016 Standard o Windows Server 2016 Datacenter, è possibile convertirla in una versione definitiva come indicato di seguito:

1.  Se il server è un **controller di dominio**, non è possibile eseguire la conversione a una versione definitiva. In questo caso, installare un controller di dominio aggiuntivo in un server che esegue una versione definitiva e rimuovere Servizi di dominio Active Directory dal controller di dominio in esecuzione nella versione di valutazione. Per altre informazioni, vedere [Aggiornare controller di dominio a Windows Server 2012 R2 e Windows Server 2012](https://technet.microsoft.com/library/hh994618.aspx).
2.  Leggere le condizioni di licenza.
3.  Da un prompt dei comandi con privilegi elevati determinare il nome dell'edizione corrente tramite il comando **DISM /online /Get-CurrentEdition**. Prendere nota dell'ID dell'edizione, che corrisponde a una forma abbreviata del nome. Eseguire quindi **DISM /online /Set-Edition:\<ID edizione\> /ProductKey:XXXXX-XXXXX-XXXXX-XXXXX-XXXXX /AcceptEula** specificando l'ID dell'edizione e un codice Product Key per attivazione singola. Il server verrà riavviato due volte.

Per la versione di valutazione di Windows Server 2016 Standard, è anche possibile eseguire la conversione alla versione definitiva di Windows Server 2016 Datacenter in un unico passaggio usando lo stesso comando e il codice Product Key appropriato.

> [!TIP] 
> Per altre informazioni sulle Dism.exe, vedere [opzioni della riga di comando DISM](https://go.microsoft.com/fwlink/?LinkId=192466).

## <a name="converting-a-current-retail-edition-to-a-different-current-retail-edition"></a>Conversione da una versione definitiva corrente a un'altra versione definitiva corrente

In qualsiasi momento dopo l'installazione di Windows Server 2016 è possibile eseguire il programma di installazione per ripristinare l'installazione (operazione talvolta denominata "ripristino sul posto") oppure, in determinati casi, per eseguire la conversione a un'edizione diversa.
È possibile eseguire il programma di installazione per eseguire un "ripristino sul posto" in qualsiasi edizione di Windows Server 2016. Il risultato sarà la stessa edizione di partenza.

Per Windows Server 2016 Standard, è possibile convertire il sistema in Windows Server 2016 Datacenter come segue: Da un prompt dei comandi con privilegi elevati determinare il nome dell'edizione corrente tramite il comando **DISM /online /Get-CurrentEdition**. Per Windows Server 2016 Standard sarà `ServerStandard`. Eseguire il comando **DISM /online /Enable-feature /Get-TargetEditions** per ottenere l'ID dell'edizione è possibile eseguire l'aggiornamento. Prendere nota dell'ID edizione, una forma abbreviata del nome dell'edizione. Quindi eseguire **DISM /online /Set-Edition:\<ID edizione\> /ProductKey: XXXXX-XXXXX-XXXXX-XXXXX /AcceptEula**, fornendo l'ID dell'edizione di destinazione e la chiave di prodotto delle vendite al dettaglio di. Il server verrà riavviato due volte.

## <a name="converting-a-current-retail-version-to-a-current-volume-licensed-version"></a>Conversione da una versione con contratto multilicenza corrente a una versione definitiva corrente

In qualsiasi momento dopo l'installazione di Windows Server 2016 puoi eseguire la conversione tra una versione definitiva, una versione con contratto multilicenza o una versione OEM. L'edizione resta la stessa durante questa conversione. Se inizi con una versione di valutazione, convertila prima di tutto in versione definitiva e quindi potrai convertirla di nuovo nei modi descritti qui.

A tale scopo, da un prompt dei comandi con privilegi elevati eseguire: **slmgr /ipk \<codice\>**

Dove \<codice\> corrisponde al codice Product Key per attivazione singola, per contratti multilicenza oppure OEM.
