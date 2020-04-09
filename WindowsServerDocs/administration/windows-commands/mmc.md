---
title: mmc
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7bfa4030-ce42-40fb-922f-2f5145a80872
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0d143336db3369b3b319391967db879126d2fb29
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839444"
---
# <a name="mmc"></a>mmc

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Utilizzando le opzioni della riga di comando di MMC, è possibile aprire una console **MMC** specifica, aprire **MMC** in modalità autore oppure specificare che la versione a 32 bit o a 64 bit di **MMC** è aperta.
## <a name="syntax"></a>Sintassi
```
mmc <path>\<filename>.msc [/a] [/64] [/32]
```
#### <a name="parameters"></a>Parametri

|       Parametro        |                                                                                                 Descrizione                                                                                                 |
|------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <path>\\<filename>. msc |        Avvia **MMC** e apre una console salvata. È necessario specificare il percorso completo e il nome file per il file di console salvato. Se non si specifica un file di console, **MMC** apre una nuova console.         |
|           /a           |                                                               Apre una console salvata in modalità autore.  Utilizzato per apportare modifiche alle console salvate.                                                                |
|          /64           |                         Apre la versione a 64 bit di **MMC** (MMC64). Utilizzare questa opzione solo se si esegue un sistema operativo Microsoft a 64 bit e si desidera utilizzare uno snap-in a 64 bit.                          |
|          /32           | Apre la versione a 32 bit di **MMC** (MMC32). Quando si esegue un sistema operativo Microsoft a 64 bit, è possibile eseguire gli snap-in di 32 bit aprendo MMC con questa opzione della riga di comando quando si dispone solo di snap-in a 32 bit. |
|           /?           |                                                                                    Visualizza la guida al prompt dei comandi.                                                                                     |

## <a name="remarks"></a>Note
- Utilizzando l'opzione della riga di comando <path> **\\** <filename> **. msc** è possibile utilizzare le variabili di ambiente per creare le righe di comando o i tasti di scelta rapida che non dipendono dal percorso esplicito dei file della console. Ad esempio, se il percorso di un file di console si trova nella cartella di sistema, ad esempio **MMC c:\winnt\system32\ console_name. msc**, è possibile utilizzare la stringa di dati espandibile **% systemroot%** per specificare il percorso (**mmc% systemroot% \ system32 \ console_name. msc**). Questa operazione può essere utile se si delegano le attività agli utenti dell'organizzazione che lavorano su computer diversi.
- Usando l'opzione della riga di comando **/A** quando le console vengono aperte con questa opzione, vengono aperte in modalità autore, indipendentemente dalla modalità predefinita. L'impostazione della modalità predefinita per i file non viene modificata in modo permanente; Quando si omette questa opzione, MMC apre i file della console in base alle impostazioni della modalità predefinita.
- Dopo aver aperto **MMC** o un file di console in modalità di creazione, è possibile aprire qualsiasi console esistente facendo clic su **Apri** nel menu della **console** .
- È possibile utilizzare la riga di comando per creare collegamenti per l'apertura di **MMC** e le console salvate. Un comando della riga di comando funziona con il comando **Esegui** nel menu **Start** , in qualsiasi finestra del prompt dei comandi, nei collegamenti o in qualsiasi file batch o programma che chiama il comando.
  ## <a name="additional-references"></a>Altre informazioni di riferimento
- - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

