---
title: nslookup server
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 608267f8-f7b4-412a-8dcd-e08b5ffc2085
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 52a846b1084380d0b40d58d81c11d20dacb407bf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869152"
---
# <a name="nslookup-server"></a>nslookup server

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cambia il server predefinito per il dominio del sistema DNS (Domain Name) specificato.
## <a name="syntax"></a>Sintassi
```
server <DNSDomain>
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|<DNSDomain>|Obbligatorio. Specifica il nuovo dominio DNS per il server predefinito.|
|{help &#124; ?}|Viene visualizzato un breve riepilogo di **nslookup** sottocomandi.|
## <a name="remarks"></a>Note
-   Il **server** comando Usa il server predefinito corrente per cercare le informazioni sul dominio DNS specificato. È in contrasto con la **lserver** comando, che usa l'iniziale del server.
## <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave sintassi della riga di comando](command-line-syntax-key.md)
[lserver nslookup](nslookup-lserver.md)
