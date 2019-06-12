---
title: Query SC
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ac365f89-4b20-4de6-a582-b204c5e7d0eb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 60b6e945c4b2944f97d40cbc27694acc2915c615
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441625"
---
# <a name="sc-query"></a>Query SC



Ottiene e visualizza le informazioni sul servizio specificato, driver, tipo di servizio o tipo di driver.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
sc [<ServerName>] query [<ServiceName>] [type= {driver | service | all}] [type= {own | share | interact | kernel | filesys | rec | adapt}] [state= {active | inactive | all}] [bufsize= <BufferSize>] [ri= <ResumeIndex>] [group= <GroupName>]
```

## <a name="parameters"></a>Parametri

|       Parametro        |                                                                                                                          Descrizione                                                                                                                          |
|------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     \<ServerName>      |                       Specifica il nome del server remoto in cui si trova il servizio. Il nome deve rispettare il formato di Universal Naming Convention (UNC) (ad esempio, \\ \\myserver). Per eseguire SC.exe in locale, omettere questo parametro.                        |
|     \<ServiceName>     |                                      Specifica il nome del servizio restituito dal **getkeyname** operazione. Questo **query** parametro non viene utilizzato in combinazione con altri **query** parametri (diverso da *ServerName*).                                      |
|     type= {driver      |                                                                                                                            servizio                                                                                                                            |
|       tipo = {proprietari       |                                                                                                                             condividi                                                                                                                             |
|     state= {active     |                                                                                                                           inattivo                                                                                                                            |
| bufsize= \<BufferSize> |                     Specifica la dimensione (in byte) del buffer di enumerazione. Dimensione del buffer predefinita è 1024 byte. Quando la visualizzazione risultante da una query supera i 1024 byte, è necessario aumentare le dimensioni del buffer di enumerazione.                      |
|   istanza riservata = \<ResumeIndex >   | Specifica il numero di indice in cui è necessario iniziare o riprendere l'enumerazione. Il valore predefinito è **0** (zero). Utilizzare questo parametro in combinazione con il **bufsize =** parametro quando informazioni viene restituite da una query che può visualizzare il buffer predefinito. |
|  group= \<GroupName>   |                                                                             Specifica il gruppo di servizio da enumerare. Per impostazione predefinita, vengono enumerati tutti i gruppi (**gruppo = ""** ).                                                                              |
|           /?           |                                                                                                             Visualizza la guida al prompt dei comandi.                                                                                                              |

## <a name="remarks"></a>Note

- Senza uno spazio tra un parametro e il relativo valore (ovvero, **tipo = proprio**, non **tipo = proprio**), l'operazione avrà esito negativo.
- Il **query** operazione consente di visualizzare le informazioni seguenti relative a un servizio: SERVICE_NAME (nome della sottochiave del Registro di sistema del servizio), tipo, stato (e gli Stati che non sono disponibili), WIN32_EXIT_B, SERVICE_EXIT_B, CHECKPOINT e WAIT_HINT.
- Il **tipo =** parametro può essere utilizzato due volte in alcuni casi. La prima occorrenza del **tipo =** parametro specifica se eseguire una query di servizi, driver o entrambi (**tutti**). La seconda occorrenza del **tipo =** parametro specifica un tipo di **creare** operazione per restringere ulteriormente l'ambito della query.
- Quando il risultato visualizzato da un **query** comando supera le dimensioni del buffer di enumerazione, viene visualizzato un messaggio simile al seguente:  
  ```
  Enum: more data, need 1822 bytes start resume at index 79
  ```  
  Per visualizzare il rimanente **query** informazioni, eseguire di nuovo **query**, l'impostazione **bufsize =** sia il numero di byte e impostazione **ri =** e l'indice specificato. Ad esempio, verrebbe visualizzato l'output rimanente digitando quanto segue al prompt dei comandi:  
  ```
  sc query bufsize= 1822 ri= 79
  ```

## <a name="BKMK_examples"></a>Esempi

Per visualizzare informazioni relative solo i servizi attivi, digitare uno dei seguenti comandi:
```
sc query
sc query type= service
```
Per visualizzare informazioni per i servizi attivi e per specificare una dimensione del buffer di 2.000 byte, digitare:
```
sc query type= all bufsize= 2000
```
Per visualizzare informazioni per il servizio WUAUSERV, digitare:
```
sc query wuauserv
```
Per visualizzare informazioni per tutti i servizi (attive e inattive), digitare:
```
sc query state= all
```
Per visualizzare informazioni per tutti i servizi (attive e inattive), a partire dalla riga 56, digitare:
```
sc query state= all ri= 56
```
Per visualizzare informazioni per i servizi interattivi, digitare:
```
sc query type= service type= interact
```
Per visualizzare informazioni relative solo driver, digitare:
```
sc query type= driver
```
Per visualizzare informazioni per i driver del gruppo Network Driver Interface Specification (NDIS), digitare:
```
sc query type= driver group= ndis
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)