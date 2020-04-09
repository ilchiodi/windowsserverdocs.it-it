---
title: nslookup root
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9c29edc3-ec49-43f2-bc49-86bf0612d816
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d11179ff3cd22acd9df67261e7ab752aa159201a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838644"
---
# <a name="nslookup-root"></a>nslookup root

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

imposta il server predefinito sul server per la radice dello spazio dei nomi di dominio di Domain Name System (DNS).
## <a name="syntax"></a>Sintassi
```
root 
```
### <a name="parameters"></a>Parametri

|    Parametro    |                      Descrizione                      |
|-----------------|-------------------------------------------------------|
| {Help &#124; ?} | Viene visualizzato un breve riepilogo di **nslookup** sottocomandi. |

## <a name="remarks"></a>Note
- Attualmente, viene usato il server dei nomi ns.nic.ddn.mil. Questo comando è un sinonimo di lserver ns.nic.ddn.mil. È possibile modificare il nome del server radice con il comando **Imposta radice** .
  ## <a name="additional-references"></a>Altre informazioni di riferimento
  - [Chiave della sintassi della riga di comando](command-line-syntax-key.md)
  [nslookup set root](nslookup-set-root.md)
