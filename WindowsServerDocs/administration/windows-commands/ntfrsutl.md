---
title: ntfrsutl
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: f1301b6876698e9eb552ae0ef9e70ed278319a7c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372618"
---
# <a name="ntfrsutl"></a>ntfrsutl

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esegue il dump delle tabelle interne, thread e informazioni sulla memoria per il servizio Replica File NT \(NTFRS\). Viene eseguito su server locali e remoti. L'impostazione di ripristino per NTFRS in Gestione controllo servizi \(\) SCM può essere fondamentale per individuare e mantenere importanti eventi di log nel computer. Questo strumento fornisce un metodo comodo per rivedere le impostazioni.   
  
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
|     dominio Active Directory      |                                                                                                                                                                                                                                                                                                                         elenca la visualizzazione del servizio NTFRS del servizio di dominio Active Directory.                                                                                                                                                                                                                                                                                                                          |
|    Imposta     |                                                                                                                                                                                                                                                                                                                             Specifica il set di repliche attivi                                                                                                                                                                                                                                                                                                                              |
|   version   |                                                                                                                                                                                                                                                                                                                       Specifica le versioni del servizio NTFRS e API.                                                                                                                                                                                                                                                                                                                        |
|    polling     | Specifica gli intervalli di polling corrente.<br /><br />Parametri:<br /><br /><ul><li>**\/rapidamente**\[ **\=** \[ <N>\]\]polling rapido \(  \)<br /><br /><ul><li>**rapidamente \- i** polling fino a quando la configurazione stabile non è rectrieved</li><li>**rapidamente\=** \- i polling rapidamente ogni minuti predefiniti.</li><li>**\=** rapidamente<N> \- polling rapido ogni *N* minuti</li></ul></li><li>**\/lentamente**\[ **\=** \[ <N>\]\] i polling lentamente \(\)<br /><br /><ul><li>**lentamente** \- esegue il polling lentamente fino a quando non viene recuperata la configurazione stabile</li><li>**lentamente\=** \- esegue il polling lentamente ogni minuti predefiniti</li><li>**\=lenta**<N> \- i polling rapidamente ogni *N* minuti</li></ul></li><li>**\/ora** \(i polling\)</li></ul> |
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
  
## <a name="additional-references"></a>riferimenti aggiuntivi  
  
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
  
  
  

