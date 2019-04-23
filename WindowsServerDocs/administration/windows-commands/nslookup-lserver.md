---
title: nslookup lserver
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 0c4e1ed4697666062bb90f4a9c65054a3dd73661
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848052"
---
# <a name="nslookup-lserver"></a>nslookup lserver

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cambia il server predefinito per il dominio del sistema DNS (Domain Name) specificato.
## <a name="syntax"></a>Sintassi
```
lserver <DNSDomain> 
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|<DNSDomain>|Specifica il nuovo dominio DNS per il server predefinito.|
|{help &#124; ?}|Viene visualizzato un breve riepilogo di **nslookup** sottocomandi.|
## <a name="remarks"></a>Note
-   Il **lserver** comando Usa il primo server per cercare le informazioni sul dominio DNS specificato. È in contrasto con la **server** comando, che usa il server predefinito corrente.
## <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave sintassi della riga di comando](command-line-syntax-key.md)
[server nslookup](nslookup-server.md)
