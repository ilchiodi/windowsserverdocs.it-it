---
title: dcgpofix
description: Argomento di riferimento per il comando Dcgpofix, che ricrea gli oggetti Criteri di gruppo predefiniti (GPO) per un dominio.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 81d5fa65-2aea-49d3-b353-357441846c00
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f11b7db8110cd2d7dcf08cd250eba411e7ff21a8
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993171"
---
# <a name="dcgpofix"></a>dcgpofix

Ricrea il valore predefinito dei criteri di gruppo (GPO) per un dominio. Per passare alla Console Gestione Criteri di gruppo (GPMC), è necessario installare Criteri di gruppo gestione come funzionalità tramite Server Manager.

>[!IMPORTANT]
> Come procedura consigliata, è necessario configurare l'oggetto Criteri di gruppo criterio dominio predefinito solo per gestire le impostazioni predefinite dei criteri di **account** , i criteri password, i criteri di blocco degli account e i criteri Kerberos. Inoltre, è necessario configurare l'oggetto Criteri di gruppo criterio controller di dominio predefinito solo per impostare i diritti utente e i criteri di controllo.

## <a name="syntax"></a>Sintassi

```
dcgpofix [/ignoreschema] [/target: {domain | dc | both}] [/?]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| /IgnoreSchema | Ignora la versione dello schema di Active Directory quando si esegue questo comando. In caso contrario, il comando funziona solo nella stessa versione di schema come la versione di Windows in cui è stato fornito il comando. |
| `/target {domain | dc | both` | Specifica se specificare come destinazione i criteri di dominio predefiniti, i criteri dei controller di dominio predefiniti o entrambi i tipi di criteri. |
| /? | Visualizza la Guida al prompt dei comandi. |

## <a name="examples"></a>Esempi

Per gestire le impostazioni predefinite dei **criteri di account** , i criteri password, i criteri di blocco degli account e i criteri Kerberos, ignorando la versione dello schema Active Directory, digitare:

```
dcgpofix /ignoreschema /target:domain
```

Per configurare l'oggetto Criteri di gruppo criterio controller di dominio predefinito solo per impostare i diritti utente e i criteri di controllo, ignorando la versione dello schema Active Directory, digitare:

```
dcgpofix /ignoreschema /target:dc
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)