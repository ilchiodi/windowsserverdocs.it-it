---
title: freedisk
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 91c15166-5baa-4b80-9e0c-4cd815d00530
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5d652aa89c689a97bf2ecc67383bc2fd464a3054
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844444"
---
# <a name="freedisk"></a>freedisk

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Verifica se la quantità di spazio su disco specificata è disponibile prima di continuare con un processo di installazione.

## <a name="syntax"></a>Sintassi
```
freedisk [/s <computer> [/u [<Domain>\]<User> [/p [<Password>]]]] [/d <Drive>] [<Value>]
```
### <a name="parameters"></a>Parametri

|       Parametro       |                                                                                         Descrizione                                                                                          |
|-----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     /s <computer>     | Specifica il nome o l'indirizzo IP di un computer remoto (non utilizzare barre rovesciate). Il valore predefinito è il computer locale. Questo parametro si applica a tutti i file e le cartelle specificati nel comando. |
| /u [<Domain>\\]<User> |                                            Esegue lo script con le autorizzazioni dell'account utente specificato. Il valore predefinito è le autorizzazioni di sistema.                                            |
|    /p [<Password>]    |                                                           Specifica la password dell'account utente specificato in **/u**.                                                            |
|      /d <Drive>       |                              Specifica l'unità per cui si vuole trovare la disponibilità di spazio libero. È necessario specificare <Drive>per un computer remoto.                               |
|        <Value>        |                                     Verifica la presenza di una quantità specifica di spazio libero su disco. È possibile specificare <Value>in byte, KB, MB, GB, TB, PB, EB, es o YB.                                      |

## <a name="remarks"></a>Note
- Utilizzare le opzioni della riga di comando **/s**, **/u**e **/p** sono disponibili solo quando si utilizza **/s**. È necessario utilizzare **/p** con **/u**per fornire la password dell'utente.
- per le installazioni automatiche, è possibile usare **freedisk** nei file batch di installazione per verificare la quantità di spazio libero prerequisito prima di continuare con l'installazione.
- Quando si usa **freedisk** in un file batch, viene restituito **0** se lo spazio è sufficiente e **1** se lo spazio è insufficiente.
  ## <a name="examples"></a><a name=BKMK_examples></a>Esempi
  Per determinare se sono disponibili almeno 50 MB di spazio libero nell'unità C:, digitare:
  ```
  freedisk 50mb 
  ```
  Sullo schermo viene visualizzato un output simile al seguente:
  ```
  INFO: The specified 52,428,800 byte(s) of free space is available on current drive.
  ```
  ## <a name="additional-references"></a>Altre informazioni di riferimento
  - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
