---
title: mmc
description: Argomento di riferimento per il comando MMC, che consente di aprire una console MMC specifica, aprire MMC in modalità autore oppure specificare per aprire la versione a 32 bit o a 64 bit di MMC.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7bfa4030-ce42-40fb-922f-2f5145a80872
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1de1bc7912b995bf7df0e1acb50cfee18747b700
ms.sourcegitcommit: 5e313a004663adb54c90962cfdad9ae889246151
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2020
ms.locfileid: "84354591"
---
# <a name="mmc"></a>mmc

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Utilizzando le opzioni della riga di comando di MMC, è possibile aprire una console **MMC** specifica, aprire **MMC** in modalità autore oppure specificare che la versione a 32 bit o a 64 bit di **MMC** è aperta.

## <a name="syntax"></a>Sintassi

```
mmc <path>\<filename>.msc [/a] [/64] [/32]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<path>\<filename>.msc` | Avvia **MMC** e apre una console salvata. È necessario specificare il percorso completo e il nome file per il file di console salvato. Se non si specifica un file di console, **MMC** apre una nuova console. |
| /a | Apre una console salvata in modalità autore.  Utilizzato per apportare modifiche alle console salvate. |
| /64 | Apre la versione a 64 bit di **MMC** (MMC64). Utilizzare questa opzione solo se si esegue un sistema operativo Microsoft a 64 bit e si desidera utilizzare uno snap-in a 64 bit. |
| /32 | Apre la versione a 32 bit di **MMC** (MMC32). Quando si esegue un sistema operativo Microsoft a 64 bit, è possibile eseguire gli snap-in di 32 bit aprendo MMC con questa opzione della riga di comando quando si dispone solo di snap-in a 32 bit. |
| /? | Visualizza la guida al prompt dei comandi. |

## <a name="remarks"></a>Commenti

- È possibile usare le variabili di ambiente per creare le righe di comando o i collegamenti che non dipendono dal percorso esplicito dei file della console. Ad esempio, se il percorso di un file di console si trova nella cartella di sistema, ad esempio **MMC c:\winnt\system32\ console_name. msc**, è possibile utilizzare la stringa di dati espandibile **% systemroot%** per specificare il percorso (**mmc% systemroot% \ system32 \ console_name. msc**). Questa operazione può essere utile se si delegano le attività agli utenti dell'organizzazione che lavorano su computer diversi.

- Quando le console vengono aperte con l'opzione **/a** , vengono aperte in modalità autore, indipendentemente dalla modalità predefinita. Questa impostazione non modifica in modo permanente l'impostazione della modalità predefinita per i file. Quando si omette questa opzione, MMC apre i file della console in base alle impostazioni della modalità predefinita.

- Dopo aver aperto **MMC** o un file di console in modalità di creazione, è possibile aprire qualsiasi console esistente facendo clic su **Apri** nel menu della **console** .

- È possibile utilizzare la riga di comando per creare collegamenti per l'apertura di **MMC** e le console salvate. Un comando della riga di comando funziona con il comando **Esegui** nel menu **Start** , in qualsiasi finestra del prompt dei comandi, nei collegamenti o in qualsiasi file batch o programma che chiama il comando.

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
