---
title: break
description: Argomento di riferimento per il comando break, che annulla l'associazione di un volume di copia shadow a VSS e lo rende accessibile come volume normale.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: de2b6c95-1c2e-4a43-bec5-341a9014371b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7e8789ab68ecb98d190a79c3f1088aad05b83562
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82707780"
---
# <a name="break"></a>break

Annulla l'associazione di un volume di copia shadow a VSS e lo rende accessibile come volume normale. È quindi possibile accedere al volume utilizzando una lettera di unità (se assegnata) o il nome del volume. Se utilizzata senza parametri, **Interrompi** Visualizza la guida al prompt dei comandi.

> [!NOTE]
> Questo comando è pertinente solo per le copie shadow dell'hardware dopo l'importazione.
>
> Per impostazione predefinita, i volumi esposti, come le copie shadow da cui provengono, sono di sola lettura. L'accesso al volume viene effettuato direttamente al provider hardware senza che sia stata registrata una copia shadow del volume.

## <a name="syntax"></a>Sintassi

```
break [writable] <setid>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| scrivibile | Consente l'accesso in lettura/scrittura al volume. |
| \<> SetID | Specifica l'ID del set di copie shadow. L'alias dell'ID della copia shadow, archiviato come variabile di ambiente tramite il comando **Load Metadata** , può essere usato nel parametro *SetID* . |

## <a name="examples"></a>Esempi

Per creare una copia shadow usando il nome di alias Alias1 accessibile come volume scrivibile nel sistema operativo:

```
break writable %Alias1%
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)