---
title: verifier
description: Windows Commands argomento per Verifier, che esegue Driver Verifier Manager.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 782173d6-f196-4bc4-a547-76109829453c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1f0f4c35e732f520076df9ec89b9417c744d5c44
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830174"
---
# <a name="verifier"></a>verifier

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Gestione di driver verifier.  

## <a name="syntax"></a>Sintassi  
```  
verifier /standard /driver <name> [<name> ...]  
verifier /standard /all  
verifier [/flags <flags>] [/faults [<probability> [<tags> [<applications> [<minutes>]]]] /driver <name> [<name>...]  
verifier [/flags FLAGS] [/faults [<probability> [<tags> [<applications> [<minutes>]]]] /all  
verifier /querysettings  
verifier /volatile /flags <flags>  
verifier /volatile /adddriver <name> [<name>...]  
verifier /volatile /removedriver <name> [<name>...]  
verifier /volatile /faults [<probability> [<tags> [<applications>]]  
verifier /reset  
verifier /query  
verifier /log <LogFileName> [/interval <seconds>]  
```  
#### <a name="parameters"></a>Parametri  
|Parametro|Descrizione|  
|-------|--------|  
|flag di \<>|Deve essere un numero decimale o esadecimale, combinazione di bit:<p>**valore -   : Descrizione**<br />-   **bit 0:** controllo pool speciale<br />-   **bit 1:** forzare il controllo del livello IRQL<br />-   **bit 2:** simulazione di risorse insufficienti<br />-   **bit 3:** rilevamento pool<br />-   **bit 4:** verifica di I/O<br />-   **bit 5:** rilevamento deadlock<br />-   **bit 6:** non usato<br />-   **bit 7:** verifica DMA<br />-   **bit 8:** controlli di sicurezza<br />-   **bit 9:** forzare richieste di I/O in sospeso<br />-   **bit 10:** registrazione IRP<br />-   **bit 11:** controlli vari<p>ad esempio, **/Flags 27** è equivalente a **/Flags 0x1B**|  
|/ volatile|Consente di modificare dinamicamente le impostazioni di verifier senza il riavvio del sistema. Le nuove impostazioni andranno persi quando il riavvio del sistema.|  
|\<probabilità >|Numero compreso tra 1 e 10.000 che specifica la probabilità di fault injection. Ad esempio, il valore 100 indica una probabilità di attacco intrusivo nel codice di errore di % 1 (100/10.000).<p>Se questo parametro non viene specificato, verrà usata la probabilità predefinita del 6%.|  
|Tag di \<>|Specifica i tag del pool che verranno inseriti con errori, separati da spazi. Se questo parametro non viene specificato quindi qualsiasi allocazione del pool è possibile inserire con errori.|  
|applicazioni \<>|Specifica il nome del file di immagine delle applicazioni che verranno inserite con errori, separati da spazi. Se questo parametro non viene specificato quindi simulazione di risorse insufficienti può avvenire in qualsiasi applicazione.|  
|\<minuti >|Un numero positivo che specifica la lunghezza del periodo dopo il riavvio, in minuti, durante il quale nessun errore si verificherà injection. Se non viene specificato questo parametro verrà utilizzata la lunghezza predefinita di 8 minuti.|  
|/?|Visualizza la guida al prompt dei comandi.|  

## <a name="additional-references"></a>Altri riferimenti  
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  