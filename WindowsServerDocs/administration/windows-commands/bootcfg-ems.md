---
title: bootcfg ems
description: Argomento di riferimento per il comando bootcfg ems, che consente all'utente di aggiungere o modificare le impostazioni per il reindirizzamento della console dei servizi di gestione emergenze in un computer remoto.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 57abdc50-c64a-45f1-8470-3f8c3a51f743
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c85231e094852feb673eb4f99b183f06014234b2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82709527"
---
# <a name="bootcfg-ems"></a>bootcfg ems

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consente all'utente di aggiungere o modificare le impostazioni per il reindirizzamento della console di servizi di gestione emergenze a un computer remoto. Abilitando i servizi di gestione emergenze, viene aggiunta una `redirect=Port#` riga alla sezione [boot loader] del file Boot. ini insieme a un'opzione/Redirect alla riga di voce del sistema operativo specificata. La funzionalità Servizi di gestione emergenze è abilitata solo nei server.

## <a name="syntax"></a>Sintassi

```
bootcfg /ems {on | off | edit}[/s <computer> [/u <domain>\<user> /p <password>]] [/port {COM1 | COM2 | COM3 | COM4 | BIOSSET}] [/baud {9600 | 19200 | 38400 | 57600 | 115200}] [/id <osentrylinenum>]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `{on | off | edit}` | Specifica il valore per il reindirizzamento dei servizi di gestione emergenze, tra cui:<ul><li>**in.** Abilita l'output remoto per l' `<osentrylinenum>`oggetto specificato. Aggiunge anche un'opzione/redirect all'oggetto specificato <osentrylinenum> e un' `redirect=com<X>` impostazione alla sezione [boot loader]. Il valore di `com<X>` viene impostato dal parametro **/Port** .</li><li>**off.** Disabilita l'output in un computer remoto. Rimuove anche l'opzione/Redirect nell'oggetto specificato <osentrylinenum> e l' `redirect=com<X>` impostazione dalla sezione [boot loader].</li><li>**modifica.** Consente di modificare le impostazioni della porta modificando l' `redirect=com<X>` impostazione nella sezione [boot loader]. Il valore di `com<X>` viene impostato dal parametro **/Port** .</li></ul> |
| `/s <computer>` | Specifica il nome o l'indirizzo IP di un computer remoto (non utilizzare barre rovesciate). Il valore predefinito è il computer locale. |
| `/u <domain>\<user>`  | Esegue il comando con le autorizzazioni dell'account dell'utente specificato da `<user>` o `<domain>\<user>`. Il valore predefinito è le autorizzazioni dell'oggetto utente nel computer eseguendo il comando connesso. |
| `/p <password>` | Specifica la password dell'account utente specificato nella **/u** parametro. |
| `/port {COM1 | COM2 | COM3 | COM4 | BIOSSET}` |  Specifica la porta COM da utilizzare per il reindirizzamento. Il parametro BIOSSET indica ai servizi di gestione emergenze di ottenere le impostazioni del BIOS per determinare la porta da utilizzare per il reindirizzamento. Non usare questo parametro se l'output amministrato in remoto è disabilitato. |
| `/baud {9600 | 19200 | 38400 | 57600 | 115200}` | Specifica la velocità in baud da utilizzare per il reindirizzamento. Non usare questo parametro se l'output amministrato in remoto è disabilitato. |
| `/id <osentrylinenum>` | Specifica il numero di riga della voce del sistema operativo a cui viene aggiunta l'opzione servizi di gestione emergenze della sezione [operating systems] del file Boot. ini. La prima riga dopo la sezione [operating systems] sezione di intestazione è 1. Questo parametro è obbligatorio quando il valore di servizi di gestione emergenze è impostato **su on** o **off**. |
| /? | Visualizza la guida al prompt dei comandi. |

## <a name="examples"></a>Esempi

Per usare il comando **bootcfg/ems** :

```
bootcfg /ems on /port com1 /baud 19200 /id 2
bootcfg /ems on /port biosset /id 3
bootcfg /s srvmain /ems off /id 2
bootcfg /ems edit /port com2 /baud 115200
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /ems off /id 2
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando bootcfg](bootcfg.md)
