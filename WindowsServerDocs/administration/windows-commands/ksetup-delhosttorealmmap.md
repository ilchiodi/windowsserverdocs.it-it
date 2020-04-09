---
title: 'che Ksetup: delhosttorealmmap'
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3faee482-a96c-4614-86fd-aaa446643ec4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 85e9f6b4a9f1c9050ed843f3837a2bd87aaf6eae
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841734"
---
# <a name="ksetupdelhosttorealmmap"></a>che Ksetup: delhosttorealmmap



Rimuove un mapping del nome dell'entità servizio (SPN) tra l'host dichiarato e l'area di autenticazione. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
ksetup /delhosttorealmmap <HostName> <RealmName>
```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Nome host \<>|Il nome host è il nome del computer e può essere dichiarato come nome di dominio completo del computer.|
|\<RealmName >|Il nome dell'area di autenticazione è specificato come un nome DNS lettere maiuscole, ad esempio CORP. CONTOSO.COM.|

## <a name="remarks"></a>Note

Quando esiste un mapping tra host per l'area di autenticazione (o più host), questo comando rimuove il mapping.

Il mapping viene registrato nel registro di sistema in **HKEY_LOCAL_MACHINE \system\currentcontolset\lsa\kerberos\hosttorealm**. È necessario verificare il mapping nel registro di sistema dopo aver usato questo comando.

## <a name="examples"></a><a name=BKMK_Examples></a>Esempi

Modificando la configurazione dell'area di autenticazione CONTOSO, eliminare il mapping del computer host IPops897 all'area di autenticazione:
```
ksetup /delhosttorealmmap IPops897 CONTOSO
```
Dopo aver eseguito questo comando, è possibile verificare nel registro di sistema che il mapping sia quello desiderato.

## <a name="additional-references"></a>Altre informazioni di riferimento

-   [Ksetup:addhosttorealmmap](ksetup-addhosttorealmmap.md)
-   [Che Ksetup](ksetup.md)
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)