---
title: ntfrsutl
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d7721a19-5a87-4ab6-b816-65d2da2c811f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e94d2b0a9ca764a6e8a25a087817598e3f158581
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436398"
---
# <a name="ntfrsutl"></a>ntfrsutl

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esegue il dump delle tabelle interne, thread e informazioni sulla memoria per il servizio Replica File NT \(NTFRS\). Viene eseguito su server locali e remoti. L'impostazione di ripristino per NTFRS in Gestione controllo servizi \(SCM\) possono essere critici per l'individuazione e la conservazione di eventi importanti nel computer. Questo strumento fornisce un metodo comodo per rivedere le impostazioni.   
  
## <a name="syntax"></a>Sintassi  
  
```  
ntfrsutl[idtable|configtable|inlog|outlog][<computer>]  
ntfrsutl[memory|threads|stage][<computer>]  
ntfrsutl ds[<computer>]  
ntfrsutl [sets][<computer>]  
ntfrsutl [version][<computer>]  
ntfrsutl poll[/quickly[=[<N>]]][/slowly[=[<N>]]][/now][<computer>]  
```  
  
### <a name="parameters"></a>Parametri  
  
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
|     dominio Active Directory      |                                                                                                                                                                                                                                                                                                                         Elenca la visualizzazione del servizio NTFRS di Active Directory.                                                                                                                                                                                                                                                                                                                          |
|    Imposta     |                                                                                                                                                                                                                                                                                                                             Specifica il set di repliche attivi                                                                                                                                                                                                                                                                                                                              |
|   version   |                                                                                                                                                                                                                                                                                                                       Specifica le versioni del servizio NTFRS e API.                                                                                                                                                                                                                                                                                                                        |
|    polling     | Specifica gli intervalli di polling corrente.<br /><br />Parametri:<br /><br /><ul><li>**\/rapidamente** \[ **\=** \[ <N> \] \] \(esegue il polling rapidamente\)<br /><br /><ul><li>**rapidamente** \- esegue il polling rapidamente fino a quando non configurazione stabile rectrieved</li><li>**rapidamente\=**  \- rapidamente minuti predefinito esegue il polling.</li><li>**rapidamente\=**  <N> \- rapidamente esegue il polling ogni *N* minuti</li></ul></li><li>**\/lentamente** \[ **\=** \[ <N> \] \] \(esegue il polling lenta\)<br /><br /><ul><li>**lentamente** \- esegue il polling lentamente fino a quando non viene recuperato configurazione stabile</li><li>**lentamente\=**  \- lentamente minuti predefinito esegue il polling</li><li>**lentamente\=**  <N> \- rapidamente esegue il polling ogni *N* minuti</li></ul></li><li>**\/a questo punto** \(ora esegue il polling\)</li></ul> |
|     \/?     |                                                                                                                                                                                                                                                                                                                            Visualizza la guida al prompt dei comandi.                                                                                                                                                                                                                                                                                                                            |
  
## <a name="BKMK_Examples"></a>Esempi  
Per determinare l'intervallo di polling per la replica di file:  
  
```  
C:\Program Files\SupportTools>ntfrsutl poll wrkstn-1  
```  
  
Per determinare le interfacce NTFRS corrente \(API\) versione:  
  
```  
C:\Program Files\SupportTools>ntfrsutl version  
```  
  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
  
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
  
  
  

