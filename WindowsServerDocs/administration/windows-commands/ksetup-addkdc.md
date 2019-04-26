---
title: ksetup:addkdc
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 98bfc23a-14c4-401c-bcb3-9903c5cdde64
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d3e0d38bdec11618561ee4acaa32ffdd06695fab
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868532"
---
# <a name="ksetupaddkdc"></a>ksetup:addkdc



Aggiunge un indirizzo di Centro distribuzione chiavi (KDC) per l'area di autenticazione Kerberos specificato. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
ksetup /addkdc <RealmName> [<KDCName>] 
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<RealmName >|Il nome dell'area di autenticazione è specificato come un nome DNS lettere maiuscole, ad esempio CORP. CONTOSO.COM e è elencato come area di autenticazione predefinito quando **che ksetup** viene eseguito. È l'area di autenticazione che si sta provando ad aggiungere il KDC altri.|
|\<KDCName>|Il nome KDC è indicato come un nome di dominio completo di maiuscole e minuscole, ad esempio mitkdc.microsoft.com. Se viene omesso il nome KDC, DNS verrà individuare KDC.|

## <a name="remarks"></a>Note

Questi mapping sono memorizzati nel Registro di sistema **HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\LSA\Kerberos\Domains**. Per distribuire i dati di configurazione di Kerberos dell'area di autenticazione a più computer, usare il modello di configurazione di sicurezza lo snap-in e distribuzione dei criteri invece di usare **che ksetup** in modo esplicito nei singoli computer.

Prima di usare la nuova impostazione dell'area di autenticazione, è necessario riavviare il computer.

Per verificare il nome dell'area di autenticazione predefinito per il computer o per verificare che questo comando funzioni come previsto, eseguire **che ksetup** al prompt dei comandi e verificare l'output per il KDC aggiunto.

## <a name="BKMK_Examples"></a>Esempi

Configurare un server KDC non Windows e l'area di autenticazione che deve usare workstation:
```
ksetup /addkdc CORP.CONTOSO.COM mitkdc.contoso.com
```
Eseguire lo strumento che Ksetup nella riga di comando del computer stesso come nel comando precedente per impostare la password dell'account computer locale su "p@sswrd1%?. Quindi riavviare il computer.
```
Ksetup /setcomputerpassword p@sswrd1%
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Ksetup](ksetup.md)
-   [Ksetup:setcomputerpassword](ksetup-setcomputerpassword.md)
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)