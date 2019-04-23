---
title: verifier
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 782173d6-f196-4bc4-a547-76109829453c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2ab0833d4fdb11c4962d4916ec2e32097e08ca04
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865872"
---
# <a name="verifier"></a>verifier

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
### <a name="parameters"></a>Parametri  
|Parametro|Descrizione|  
|-------|--------|  
|\<flags>|Deve essere un numero decimale o esadecimale, combinazione di bit:<br /><br />-   **Valore: descrizione**<br />-   **bit 0:** controllo pool speciale<br />-   **bit 1:** Imponi controllo irql<br />-   **il bit 2:** bassa simulazione di risorse<br />-   **bit 3:** verifica pool<br />-   **bit 4:** Verifica dei/o<br />-   **bit 5:** rilevamento di deadlock<br />-   **bit 6:** inutilizzati<br />-   **bit 7:** Verifica DMA<br />-   **bit 8:** controlli di sicurezza<br />-   **bit 9:** forza richieste i/o in sospeso<br />-   **bit 10:** Registrazione IRP<br />-   **bit 11:** vari controlli<br /><br />ad esempio, **/flag 27** equivalente con **/flag 0x1B**|  
|/ volatile|Consente di modificare dinamicamente le impostazioni di verifier senza il riavvio del sistema. Le nuove impostazioni andranno persi quando il riavvio del sistema.|  
|\<probabilità >|Numero compreso tra 1 e 10.000 che specifica la probabilità di fault injection. Ad esempio, il valore 100 indica una probabilità di attacco intrusivo nel codice di errore di % 1 (100/10.000).<br /><br />Se questo parametro non viene specificato verrà utilizzata la probabilità di predefinito di % 6.|  
|\<i tag >|Specifica i tag del pool che verranno inseriti con errori, separati da spazi. Se questo parametro non viene specificato quindi qualsiasi allocazione del pool è possibile inserire con errori.|  
|\<le applicazioni >|Specifica il nome del file di immagine delle applicazioni che verranno inserite con errori, separati da spazi. Se questo parametro non viene specificato quindi simulazione di risorse insufficienti può avvenire in qualsiasi applicazione.|  
|\<minuti >|Un numero positivo che specifica la lunghezza del periodo dopo il riavvio, in minuti, durante il quale nessun errore si verificherà injection. Se non viene specificato questo parametro verrà utilizzata la lunghezza predefinita di 8 minuti.|  
|/?|Visualizza la guida al prompt dei comandi.|  

## <a name="additional-references"></a>Altri riferimenti  
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)  