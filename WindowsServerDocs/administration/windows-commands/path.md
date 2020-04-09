---
title: path
description: Informazioni su come impostare la variabile di ambiente PATH.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1bfa1349-e79a-472b-a9e6-d7a91149ae8f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cb77cac3871dcf4a411638409de68d038a317d24
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80837714"
---
# <a name="path"></a>path



Imposta il percorso del comando nella variabile di ambiente PATH (il set di directory utilizzato per la ricerca di file eseguibili). Se usato senza parametri, **path** Visualizza il percorso del comando corrente.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
path [[<Drive>:]<Path>[;...][;%PATH%]]
path ;
```

### <a name="parameters"></a>Parametri

|     Parametro     |                                                                                                     Descrizione                                                                                                      |
|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [\<unità >:]<Path> |                                                                            Specifica l'unità e la directory da impostare nel percorso del comando.                                                                             |
|         ;         | Separa le directory nel percorso dei comandi. Se utilizzata senza altri **parametri, Cancella** i percorsi dei comandi esistenti dalla variabile di ambiente Path e indirizza cmd. exe alla ricerca solo nella directory corrente. |
|      PERCORSO       |                                                         Accoda il percorso del comando al set di directory esistente elencato nella variabile di ambiente PATH.                                                         |
|        /?         |                                                                                         Visualizza la guida al prompt dei comandi.                                                                                         |

## <a name="remarks"></a>Note

-   Quando si include **% path%** nella sintassi, cmd. exe lo sostituisce con i valori del percorso del comando trovati nella variabile di ambiente PATH, eliminando la necessità di immettere manualmente questi valori al prompt dei comandi.
-   La directory corrente viene sempre ricercata prima delle directory specificate nel percorso dei comandi.
-   È possibile che siano presenti file in una directory che condividono lo stesso nome di file ma con estensioni diverse. Ad esempio, è possibile che si disponga di un file denominato Accnt.com che avvia un programma di contabilità e un altro file denominato accnt. bat che connette il server alla rete del sistema contabile.

    Il sistema operativo Windows cerca un file usando le estensioni di file predefinite nel seguente ordine di precedenza:. exe,. com,. bat e. cmd. Per eseguire accnt. bat quando Accnt.com è presente nella stessa directory, è necessario includere l'estensione. bat al prompt dei comandi.
-   Se due o più file nel percorso del comando hanno lo stesso nome e l'estensione del file, il **percorso** cerca innanzitutto il nome file specificato nella directory corrente. Quindi cerca le directory nel percorso dei comandi nell'ordine in cui sono elencate nella variabile di ambiente PATH.
-   Se si posiziona il comando **path** nel file Autoexec. NT, il sistema operativo Windows aggiunge automaticamente il percorso di ricerca del sottosistema MS-DOS specificato ogni volta che si accede al computer. Cmd. exe non utilizza il file Autoexec. NT. Quando viene avviato da un collegamento, cmd. exe eredita le variabili di ambiente impostate in Computer locale/Properties/Advanced/Environment.

## <a name="examples"></a><a name="BKMK_examples"></a>Esempi

Per eseguire una ricerca nei percorsi C:\User\Taxes, B:\Utenti\Investim e B:\archivio per i comandi esterni, digitare:

`path c:\user\taxes;b:\user\invest;b:\bin`

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)