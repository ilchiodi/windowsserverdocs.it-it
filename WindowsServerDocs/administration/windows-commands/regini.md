---
title: regini
description: Informazioni su come modificare il registro di sistema dal prompt dei comandi o tramite uno script.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5ff18dc3-5bd8-400a-b311-fd73a3267e8c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 80f8f4212d2054fc54ce33993a1cef8a1501c6d5
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868878"
---
# <a name="regini"></a>regini

Modifica del Registro di sistema dalla riga di comando o uno script e applica le modifiche che sono state preimpostate in uno o più file di testo. È possibile creare, modificare o eliminare le chiavi del Registro di sistema, oltre a modificare le autorizzazioni per le chiavi del Registro di sistema.

Per informazioni dettagliate sul formato e il contenuto del file di script di testo utilizzato da Regini. exe per apportare modifiche al registro di sistema, vedere [come modificare i valori o le autorizzazioni del registro di sistema da una riga di comando o uno script](https://support.microsoft.com/help/264584/how-to-change-registry-values-or-permissions-from-a-command-line-or-a).

## <a name="syntax"></a>Sintassi

```
regini [-m \\machinename | -h hivefile hiveroot][-i n] [-o outputWidth][-b] textFiles...
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |

|-m \< \\ nomecomputer\\>|Specifica il nome del computer remoto con un registro di sistema che devono essere modificati. Usare il  **\\ formato\\ComputerName**.|
|---------------------|-|
|-h \<hivefile hiveroot >|Specifica l'hive del Registro di sistema locale da modificare. È necessario specificare il nome del file di hive e la radice dell'hive nel formato **hivefile hiveroot**.|
|-i \<n >|Specifica il livello di rientro da utilizzare per indicare la struttura ad albero delle chiavi del Registro di sistema nell'output del comando. Lo strumento **Regdmp. exe** , che ottiene le autorizzazioni correnti della chiave del registro di sistema in formato binario, utilizza il rientro in multipli di quattro, quindi il valore predefinito è **4**.|
|-o \<OutputWidth >|Specifica la larghezza dell'output del comando, in caratteri. Se l'output verrà visualizzato nella finestra di comando, il valore predefinito è la larghezza della finestra. Se l'output viene indirizzato a un file, il valore predefinito è **240** caratteri.|
|-b|Specifica che **Regini.exe** output è compatibile con le versioni precedenti di **Regini.exe**. Vedere la sezione Osservazioni per informazioni dettagliate.|
|textfiles|Specifica il nome di uno o più file di testo che contengono dati del Registro di sistema. È possibile elencare qualsiasi numero di file di testo ANSI o Unicode.|

## <a name="remarks"></a>Note

Le linee guida seguenti si applicano principalmente per il contenuto dei file di testo che contengono dati del Registro di sistema che è possibile applicare utilizzando **Regini.exe**.
-   Utilizzare il punto e virgola come carattere di fine della riga commento. Deve essere il primo carattere non vuote in una riga.
-   Utilizzare la barra rovesciata per indicare di continuazione di una riga. Il comando verrà ignorati tutti i caratteri compresi la barra rovesciata fino a (ma non inclusa) il primo carattere non vuoto della riga successiva. Se si include più di uno spazio prima la barra rovesciata, viene sostituita da uno spazio singolo.
-   Utilizzare caratteri di tabulazione disco rigido per controllare il rientro. Questo rientro indica la struttura ad albero delle chiavi del Registro di sistema; Tuttavia, questi caratteri vengono convertiti in un singolo spazio indipendentemente dalla loro posizione.

#### <a name="additional-references"></a>Altri riferimenti

-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)