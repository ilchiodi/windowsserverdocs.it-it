---
title: nslookup root
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9c29edc3-ec49-43f2-bc49-86bf0612d816
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3eb3375df3a109685fc8dc5d23f0c5008339d09e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373395"
---
# <a name="nslookup-root"></a>nslookup root

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

imposta il server predefinito sul server per la radice dello spazio dei nomi di dominio di Domain Name System (DNS).
## <a name="syntax"></a>Sintassi
```
root 
```
## <a name="parameters"></a>Parametri

|    Parametro    |                      Descrizione                      |
|-----------------|-------------------------------------------------------|
| {Help &#124; ?} | Viene visualizzato un breve riepilogo di **nslookup** sottocomandi. |

## <a name="remarks"></a>Osservazioni
- Attualmente, viene usato il server dei nomi ns.nic.ddn.mil. Questo comando è un sinonimo di lserver ns.nic.ddn.mil. È possibile modificare il nome del server radice con il comando **Imposta radice** .
  ## <a name="additional-references"></a>riferimenti aggiuntivi
  [Chiave della sintassi della riga di comando](command-line-syntax-key.md)
  [nslookup set root](nslookup-set-root.md)
