---
title: 'che Ksetup: dominio'
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2ef766e3-6071-44f2-946b-22ea5b61a508
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bfaa8a37ae4ee5c9669b09f27a73b3d016324dea
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841584"
---
# <a name="ksetupdomain"></a>che Ksetup: dominio



Imposta il nome di dominio per tutte le operazioni di Kerberos. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
ksetup /domain <DomainName>
```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<NomeDominio >|Nome del dominio a cui si desidera stabilire una connessione. Utilizzare il nome di dominio completo o un modulo semplice del nome, ad esempio contoso.com o contoso.|

## <a name="remarks"></a>Note

Nessuno

## <a name="examples"></a><a name=BKMK_Examples></a>Esempi

Stabilire una connessione a un dominio valido, ad esempio Microsoft utilizzando il sottocomando /mapuser:
```
ksetup /mapuser principal@realm domain-user /domain domain-name
```
Quando la connessione ha esito positivo, verrà visualizzato un nuovo TGT o un TGT esistenti verranno aggiornati.

## <a name="additional-references"></a>Altre informazioni di riferimento

-   [Che Ksetup](ksetup.md)
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)