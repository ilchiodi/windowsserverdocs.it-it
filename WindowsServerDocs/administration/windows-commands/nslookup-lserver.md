---
title: nslookup lserver
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aee5ea0b-bb17-4c14-bde7-2f7a91f2f22b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 347ad6e380f8d8163c4954771c9e985271b2d549
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373085"
---
# <a name="nslookup-lserver"></a>nslookup lserver

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Imposta il server predefinito sul dominio di Domain Name System (DNS) specificato.
## <a name="syntax"></a>Sintassi
```
lserver <DNSDomain> 
```
## <a name="parameters"></a>Parametri

|    Parametro    |                      Descrizione                      |
|-----------------|-------------------------------------------------------|
|   <DNSDomain>   | Specifica il nuovo dominio DNS per il server predefinito.  |
| {Help &#124; ?} | Viene visualizzato un breve riepilogo di **nslookup** sottocomandi. |

## <a name="remarks"></a>Osservazioni
- Il comando **lserver** usa il server iniziale per cercare le informazioni sul dominio DNS specificato. Questo si differenzia dal comando **Server** , che usa il server predefinito corrente.
  ## <a name="additional-references"></a>riferimenti aggiuntivi
  [Chiave della sintassi della riga di comando](command-line-syntax-key.md)
  [Server nslookup](nslookup-server.md)
