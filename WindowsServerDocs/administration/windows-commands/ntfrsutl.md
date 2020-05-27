---
title: ntfrsutl
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d7721a19-5a87-4ab6-b816-65d2da2c811f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dc1275b7936a88b14b7658e2fe27d3958a035a04
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820926"
---
# <a name="ntfrsutl"></a>ntfrsutl

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esegue il dump delle tabelle interne, thread e informazioni sulla memoria per il servizio Replica File NT \(NTFRS\). Viene eseguito su server locali e remoti. L'impostazione di ripristino per NTFRS in Gestione controllo servizi \( SCM \) può essere fondamentale per individuare e mantenere importanti eventi di log nel computer. Questo strumento fornisce un metodo comodo per rivedere le impostazioni.

## <a name="syntax"></a>Sintassi

```
ntfrsutl[idtable|configtable|inlog|outlog][<computer>]
ntfrsutl[memory|threads|stage][<computer>]
ntfrsutl ds[<computer>]
ntfrsutl [sets][<computer>]
ntfrsutl [version][<computer>]
ntfrsutl poll[/quickly[=[<N>]]][/slowly[=[<N>]]][/now][<computer>]
```

#### <a name="parameters"></a>Parametri

|  Parametro  |                                                                                                                                                                                                                                                                                                                                        Descrizione                                                                                                                                                                                                                                                                                                                                         |
|-------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   IDTable   |                                                                                                                                                                                                                                                                                                                                          Tabella ID                                                                                                                                                                                                                                                                                                                                          |
| ConfigTable |                                                                                                                                                                                                                                                                                                                                  Tabella di configurazione del servizio Replica file                                                                                                                                                                                                                                                                                                                                   |
|    inlog    |                                                                                                                                                                                                                                                                                                                                        Log in ingresso                                                                                                                                                                                                                                                                                                                                         |
|   outlog    |                                                                                                                                                                                                                                                                                                                                        Registro in uscita                                                                                                                                                                                                                                                                                                                                        |
| <computer>  |                                                                                                                                                                                                                                                                                                                                  Specifica il computer.                                                                                                                                                                                                                                                                                                                                   |
|   memoria    |                                                                                                                                                                                                                                                                                                                                        Utilizzo della memoria                                                                                                                                                                                                                                                                                                                                        |
|   thread   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|    fase    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|     ds      |                                                                                                                                                                                                                                                                                                                         elenca la visualizzazione del servizio NTFRS del servizio di dominio Active Directory.                                                                                                                                                                                                                                                                                                                          |
|    Imposta     |                                                                                                                                                                                                                                                                                                                             Specifica il set di repliche attivi                                                                                                                                                                                                                                                                                                                              |
|   version   |                                                                                                                                                                                                                                                                                                                       Specifica le versioni del servizio NTFRS e API.                                                                                                                                                                                                                                                                                                                        |
|    polling     | Specifica gli intervalli di polling corrente.<p>Parametri<p><ul><li>** \/ rapidamente** \[ **\=** \[ <N>\]\]\( Esegue rapidamente il polling  \)<p><ul><li>**rapidamente** \- esegue il polling rapidamente fino a configurazione stabile rectrieved</li><li>**rapidamente\=** \- rapidamente minuti predefinito esegue il polling.</li><li>**rapidamente\=**<N> \- rapidamente esegue il polling ogni *N* minuti</li></ul></li><li>con ** \/ lentezza** \[ **\=** \[ <N>\]\] \(Polling lento\)<p><ul><li>**lentamente** \- esegue il polling lentamente fino a quando non viene recuperato configurazione stabile</li><li>**lentamente\=** \- lentamente minuti predefinito esegue il polling</li><li>**lentamente\=**<N> \- rapidamente esegue il polling ogni *N* minuti</li></ul></li><li>** \/ ora** \( Esegue ora il polling\)</li></ul> |
|     \/?     |                                                                                                                                                                                                                                                                                                                            Visualizza la guida al prompt dei comandi.                                                                                                                                                                                                                                                                                                                            |

## <a name="examples"></a>Esempi
Per determinare l'intervallo di polling per la replica di file:

```
C:\Program Files\SupportTools>ntfrsutl poll wrkstn-1
```

Per determinare le interfacce NTFRS corrente \(API\) versione:

```
C:\Program Files\SupportTools>ntfrsutl version
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)




