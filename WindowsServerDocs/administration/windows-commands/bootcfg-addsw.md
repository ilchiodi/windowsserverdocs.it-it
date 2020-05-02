---
title: bootcfg addsw
description: Argomento di riferimento per il comando bootcfg Addsw, che aggiunge le opzioni di caricamento del sistema operativo per una voce del sistema operativo specificata.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d8389293-ecd9-42f0-b84b-b9ead4cf52e6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 17abdc1ba28afad173ea6486519277916f08ad3d
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82709939"
---
# <a name="bootcfg-addsw"></a>bootcfg addsw

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consente di aggiungere opzioni di caricamento del sistema operativo per una voce del sistema operativo specificato.

## <a name="syntax"></a>Sintassi

```
bootcfg /addsw [/s <computer> [/u <domain>\<user> /p <password>]] [/mm <maximumram>] [/bv] [/so] [/ng] /id <osentrylinenum>
```

### <a name="parameters"></a>Parametri

| Termine | Definizione |
| ---- | ---------- |
| `/s <computer>` | Specifica il nome o l'indirizzo IP di un computer remoto (non utilizzare barre rovesciate). Il valore predefinito è il computer locale. |
| `/u <domain>\<user>`  | Esegue il comando con le autorizzazioni dell'account dell'utente specificato da `<user>` o `<domain>\<user>`. Il valore predefinito è le autorizzazioni dell'oggetto utente nel computer eseguendo il comando connesso. |
| `/p <password>` | Specifica la password dell'account utente specificato nella **/u** parametro. |
| `/mm <maximumram>` | Specifica la quantità massima di RAM, in megabyte, che è possibile utilizzare il sistema operativo. Il valore deve essere uguale o maggiore di 32 MB. |
| /BV | Aggiunge il **/basevideo** opzione specificata `<osentrylinenum>`, indicando al sistema operativo di utilizzare la modalità VGA standard per il driver video installato. |
| /SO | Aggiunge l'opzione **/SOS** all'oggetto specificato `<osentrylinenum>`, indirizzando il sistema operativo in modo da visualizzare i nomi dei driver di dispositivo durante il caricamento. |
| /NG | Aggiunge il **/noguiboot** opzione specificata `<osentrylinenum>`, disabilitare l'indicatore di stato che precede il prompt di accesso CTRL + ALT + CANC. |
| `/id <osentrylinenum>` | Specifica il numero di riga voce del sistema operativo in della sezione [operating systems] del file Boot. ini alla quale aggiungere opzioni di caricamento del sistema operativo. La prima riga dopo la sezione [operating systems] sezione di intestazione è 1. |
| /? | Visualizza la guida al prompt dei comandi. |

## <a name="examples"></a>Esempi

Per usare il comando **bootcfg/Addsw** :

```
bootcfg /addsw /mm 64 /id 2
bootcfg /addsw /so /id 3
bootcfg /addsw /so /ng /s srvmain /u hiropln /id 2
bootcfg /addsw /ng /id 2
bootcfg /addsw /mm 96 /ng /s srvmain /u maindom\hiropln /p p@ssW23 /id 2
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando bootcfg](bootcfg.md)
