---
title: bootcfg addsw
description: Argomento dei comandi di Windows per **bootcfg Addsw** -aggiunge le opzioni di caricamento del sistema operativo per una voce del sistema operativo specificata.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d8389293-ecd9-42f0-b84b-b9ead4cf52e6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2dd727c839babe1ae4f7743285844f35cf5bf76e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380177"
---
# <a name="bootcfg-addsw"></a>bootcfg addsw

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

aggiunge le opzioni di caricamento del sistema operativo per una voce del sistema operativo specificata.

## <a name="syntax"></a>Sintassi
```
bootcfg /addsw [/s <computer> [/u <Domain>\<User> /p <Password>]] [/mm <MaximumRAM>] [/bv] [/so] [/ng] /id <OSEntryLineNum>
```
## <a name="parameters"></a>Parametri

|         Termine         |                                                                                                            Definizione                                                                                                            |
|----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                                        Specifica il nome o l'indirizzo IP di un computer remoto (non utilizzare barre rovesciate). Il valore predefinito è il computer locale.                                                        |
| /u <Domain>\\<User>  |               Esegue il comando con le autorizzazioni dell'account dell'utente specificato da <User> o <Domain>\\<User>. Il valore predefinito è le autorizzazioni dell'oggetto utente nel computer eseguendo il comando connesso.               |
|    /p <Password>     |                                                                      Specifica la password dell'account utente specificato nella **/u** parametro.                                                                       |
|   <MaximumRAM>/mm   |                                          Specifica la quantità massima di RAM, in megabyte, che è possibile utilizzare il sistema operativo. Il valore deve essere uguale o maggiore di 32 MB.                                          |
|         /BV          |                                    aggiunge l'opzione **/basevideo** al <OSEntryLineNum>specificato, indicando al sistema operativo di utilizzare la modalità VGA standard per il driver video installato.                                     |
|         /SO          |                                      aggiunge l'opzione **/SOS** al *NumRigaVoceSO*specificato, indirizzando il sistema operativo in modo da visualizzare i nomi dei driver di dispositivo durante il caricamento.                                      |
|         /NG          |                                         aggiunge l'opzione **/noguiboot** al <OSEntryLineNum>specificato, disabilitando la barra di stato visualizzata prima del prompt di accesso CTRL + ALT + CANC.                                          |
| <OSEntryLineNum>/ID | Specifica il numero di riga voce del sistema operativo in della sezione [operating systems] del file Boot. ini alla quale aggiungere opzioni di caricamento del sistema operativo. La prima riga dopo la sezione [operating systems] sezione di intestazione è 1. |
|          /?          |                                                                                               Visualizza la guida al prompt dei comandi.                                                                                               |

## <a name="BKMK_examples"></a>Esempi
Gli esempi seguenti illustrano come utilizzare il **bootcfg /addsw** comando:
```
bootcfg /addsw /mm 64 /id 2 
bootcfg /addsw /so /id 3 
bootcfg /addsw /so /ng /s srvmain /u hiropln /id 2 
bootcfg /addsw /ng /id 2 
bootcfg /addsw /mm 96 /ng /s srvmain /u maindom\hiropln /p p@ssW23 /id 2
```
#### <a name="additional-references"></a>riferimenti aggiuntivi
[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
