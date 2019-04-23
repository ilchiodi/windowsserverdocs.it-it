---
title: nslookup set srchlist
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8486266d-22ac-4ce5-aad6-1cd0c08110a2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c3bc06f82f557f136850872180a5c430f70da5fd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888482"
---
# <a name="nslookup-set-srchlist"></a>nslookup set srchlist

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

modifica il sistema DNS (Domain Name) dominio nome e la ricerca elenco predefinito.

## <a name="syntax"></a>Sintassi
```
Set srchlist=<DomainName>[/...]
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|<DomainName>|Specifica i nuovi nomi per l'elenco di ricerca e di dominio DNS predefinito. Il valore di nome di dominio predefinito si basa sul nome host. È possibile specificare un massimo di sei nomi separati da barre (/).|
|{help &#124; ?}|Viene visualizzato un breve riepilogo di **nslookup** sottocomandi.|
## <a name="remarks"></a>Note
-   Il **impostare srchlist**comando esegue l'override di DNS dominio nome e la ricerca elenco predefinito delle **dominio set** comando. Usare la **imposta tutti** comando per visualizzare l'elenco.
## <a name="BKMK_examples"></a>Esempi
L'esempio seguente imposta il dominio DNS costr.carrega.com e l'elenco di ricerca per i nomi di tre:
```
set srchlist=mfg.widgets.com/mrp2.widgets.com/widgets.com
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave sintassi della riga di comando](command-line-syntax-key.md)
[dominio set nslookup](nslookup-set-domain.md)
[tutti i set nslookup](nslookup-set-all.md)
