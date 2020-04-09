---
title: nslookup lserver
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: aee5ea0b-bb17-4c14-bde7-2f7a91f2f22b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9d0d8619101d2e7b1f7fb6d6ed99d801c7c264f1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838634"
---
# <a name="nslookup-lserver"></a>nslookup lserver

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Imposta il server predefinito sul dominio di Domain Name System (DNS) specificato.
## <a name="syntax"></a>Sintassi
```
lserver <DNSDomain> 
```
### <a name="parameters"></a>Parametri

|    Parametro    |                      Descrizione                      |
|-----------------|-------------------------------------------------------|
|   <DNSDomain>   | Specifica il nuovo dominio DNS per il server predefinito.  |
| {Help &#124; ?} | Viene visualizzato un breve riepilogo di **nslookup** sottocomandi. |

## <a name="remarks"></a>Note
- Il comando **lserver** usa il server iniziale per cercare le informazioni sul dominio DNS specificato. Questo si differenzia dal comando **Server** , che usa il server predefinito corrente.
  ## <a name="additional-references"></a>Altre informazioni di riferimento
  - [Chiave della sintassi della riga di comando](command-line-syntax-key.md)
  [Server nslookup](nslookup-server.md)
