---
title: mmc
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7bfa4030-ce42-40fb-922f-2f5145a80872
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4bdc093bd16ea08153b7dbc4a0e3380251f2ed7d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437328"
---
# <a name="mmc"></a>mmc

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Utilizzando le opzioni della riga di comando di mmc, è possibile aprire uno specifico **mmc** console, aprire **mmc** in modalità di modifica, o specificare che la versione a 32 o 64 bit di **mmc** viene aperto.
## <a name="syntax"></a>Sintassi
```
mmc <path>\<filename>.msc [/a] [/64] [/32]
```
### <a name="parameters"></a>Parametri

|       Parametro        |                                                                                                 Descrizione                                                                                                 |
|------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <path>\\<filename>.msc |        viene avviata **mmc** e apre una console salvata. È necessario specificare il nome di file e percorso completo del file di console salvata. Se non si specifica un file di console **mmc** apre una nuova console.         |
|           /a           |                                                               Consente di aprire una console salvata in modalità di modifica.  Consente di apportare modifiche alla console salvate.                                                                |
|          /64           |                         Apre la versione a 64 bit del **mmc** (mmc64). Usare questa opzione solo se si esegue un sistema operativo Microsoft a 64 bit e si desidera utilizzare uno snap-in a 64 bit.                          |
|          /32           | Apre la versione a 32 bit del **mmc** (mmc32). Quando si esegue un sistema operativo Microsoft a 64 bit, è possibile eseguire 32 bit. gli snap-in per mmc aprire con questa opzione della riga di comando quando si dispone solo 32 bit snap-in. |
|           /?           |                                                                                    Visualizza la guida al prompt dei comandi.                                                                                     |

## <a name="remarks"></a>Note
- Usando il <path> **\\** <filename> **msc** opzione della riga di comando è possibile usare variabili di ambiente per creare le righe di comando o i tasti di scelta rapida che non dipendono l'impostazione esplicita percorso dei file di console. Ad esempio, se il percorso in un file di console è nella cartella di sistema (ad esempio, **mmc c:\winnt\system32\console_name.msc**), è possibile usare la stringa di dati espandibile **% systemroot %** per specificare il percorso (**mmc%systemroot%\system32\console_name.msc**). Ciò può essere utile se si consentono di delegare le attività per gli utenti dell'organizzazione che lavorano in computer diversi.
- Usando il **/a** opzione della riga di comando quando le console vengono aperti con questa opzione, vengono aperti in modalità di modifica, indipendentemente dalla modalità predefinita. Ciò non modifica in modo permanente l'impostazione della modalità predefinita per i file; Quando si omette questa opzione, verrà aperto i file in base alle impostazioni predefinite in modalità console mmc.
- Dopo aver aperto **mmc** o un file di console in modalità di modifica, è possibile aprire una console esistente facendo **aprire** sul **Console** menu.
- È possibile usare la riga di comando per creare tasti di scelta rapida per l'apertura **mmc** e salvate. Una riga di comando funziona con il **eseguiti** comando il **avviare** menu, in qualsiasi finestra del prompt dei comandi, nei tasti di scelta o in qualsiasi programma che chiama il comando o un file batch.
  ## <a name="additional-references"></a>Riferimenti aggiuntivi
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

