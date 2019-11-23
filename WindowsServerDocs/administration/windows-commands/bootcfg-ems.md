---
title: bootcfg ems
description: "Argomento dei comandi di Windows per **bootcfg ems** : consente all'utente di aggiungere o modificare le impostazioni per il reindirizzamento della console dei servizi di gestione emergenze in un computer remoto."
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 57abdc50-c64a-45f1-8470-3f8c3a51f743
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c0e761f3796121cc71be88d23e25371369a3ee90
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379961"
---
# <a name="bootcfg-ems"></a>bootcfg ems

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consente all'utente di aggiungere o modificare le impostazioni per il reindirizzamento della console di servizi di gestione emergenze a un computer remoto. Abilitazione di servizi di gestione emergenze, è possibile aggiungere un "redirect = Port #" riga alla sezione [boot loader] del file Boot. ini e l'opzione /redirect alla riga del movimento del sistema operativo specificato. La funzionalità Servizi di gestione emergenze è abilitata solo nei server.

## <a name="syntax"></a>Sintassi
```
bootcfg /ems {ON | OFF | edit} [/s <computer> [/u <Domain>\<User> /p <Password>]] [/port {COM1 | COM2 | COM3 | COM4 | BIOSSET}] [/baud {9600 | 19200 | 38400 | 57600 | 115200}] [/id <OSEntryLineNum>]
```
## <a name="parameters"></a>Parametri

|                            Parametro                             |                                                                                                                                                                                                                                                                                                                                                              Descrizione                                                                                                                                                                                                                                                                                                                                                              |
|------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                    {ON &#124; OFF&#124; edit}                    | Specifica il valore per il reindirizzamento di servizi di gestione emergenze.<br /><br />**VIA** -Abilita l'output remoto per l'oggetto specificato <OSEntryLineNum>. aggiunge un'opzione/redirect al <OSEntryLineNum> specificato e un'impostazione di reindirizzamento = com<X> alla sezione [boot loader]. Il valore di com<X> è l'impostazione di **porta/** parametro.<br /><br />**DISATTIVARE** -Disabilita l'output a un computer remoto. Rimuove l'opzione/redirect dal <OSEntryLineNum> specificato e l'impostazione di reindirizzamento = com<X> dalla sezione [boot loader].<br /><br />**modifica** : consente di modificare le impostazioni della porta modificando l'impostazione di reindirizzamento = com<X> nella sezione [boot loader]. Il valore di com<X> viene reimpostato sul valore specificato per il **porta/** parametro. |
|                          /s <computer>                           |                                                                                                                                                                                                                                                                                                          Specifica il nome o l'indirizzo IP di un computer remoto (non utilizzare barre rovesciate). Il valore predefinito è il computer locale.                                                                                                                                                                                                                                                                                                           |
|                       /u <Domain>\\<User>                        |                                                                                                                                                                                                                                                                 Esegue il comando con le autorizzazioni dell'account dell'utente specificato da <User> o <Domain>\\<User>. Il valore predefinito è le autorizzazioni dell'oggetto utente nel computer eseguendo il comando connesso.                                                                                                                                                                                                                                                                  |
|                          /p <Password>                           |                                                                                                                                                                                                                                                                                                                         Specifica la password dell'account utente specificato nella **/u** parametro.                                                                                                                                                                                                                                                                                                                         |
| porta {COM1 & #124; COM2 & #124; COM3 & #124; COM4 & #124; BIOSSET}  |                                                                                                                                                                                                                              Specifica la porta COM da utilizzare per il reindirizzamento. **BIOSSET** indica ai servizi di gestione emergenze per ottenere le impostazioni del BIOS per determinare la porta deve essere utilizzata per il reindirizzamento. Non utilizzare il **porta/** parametro se amministrato da postazione remota output è stato disabilitato.                                                                                                                                                                                                                              |
| /baud {9600 & #124; 19200 & #124; 38400 & #124; 57600 & #124; 115200} |                                                                                                                                                                                                                                                                                               Specifica la velocità in baud da utilizzare per il reindirizzamento. Non utilizzare il **/baud** parametro se amministrato da postazione remota output è stato disabilitato.                                                                                                                                                                                                                                                                                               |
|                       <OSEntryLineNum>/ID                       |                                                                                                                                                                                              Specifica il numero di riga della voce del sistema operativo a cui viene aggiunta l'opzione servizi di gestione emergenze della sezione [operating systems] del file Boot. ini. La prima riga dopo la sezione [operating systems] sezione di intestazione è 1. Questo parametro è obbligatorio quando il valore di servizi di gestione emergenze è impostato su **ON** o **OFF**.                                                                                                                                                                                              |
|                                /?                                |                                                                                                                                                                                                                                                                                                                                                 Visualizza la guida al prompt dei comandi.                                                                                                                                                                                                                                                                                                                                                  |

## <a name="BKMK_examples"></a>Esempi
Gli esempi seguenti illustrano come utilizzare il **bootcfg /ems** comando:
```
bootcfg /ems on /port com1 /baud 19200 /id 2 
bootcfg /ems on /port biosset /id 3 
bootcfg /s srvmain /ems off /id 2 
bootcfg /ems edit /port com2 /baud 115200 
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /ems off /id 2
```
#### <a name="additional-references"></a>riferimenti aggiuntivi
[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
