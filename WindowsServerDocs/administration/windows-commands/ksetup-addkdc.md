---
title: 'che Ksetup: addkdc'
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 98bfc23a-14c4-401c-bcb3-9903c5cdde64
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 76d592e4f1c32305d6f939a66a6ad42cd582b032
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724760"
---
# <a name="ksetupaddkdc"></a>che Ksetup: addkdc



Aggiunge un indirizzo di Centro distribuzione chiavi (KDC) per l'area di autenticazione Kerberos specificata.

## <a name="syntax"></a>Sintassi

```
ksetup /addkdc <RealmName> [<KDCName>] 
```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<RealmName>|Il nome dell'area di autenticazione è specificato come un nome DNS lettere maiuscole, ad esempio CORP. CONTOSO.COM e è elencato come area di autenticazione predefinito quando **che ksetup** viene eseguito. È in questa area di autenticazione che si sta tentando di aggiungere l'altro KDC.|
|\<> KDCName|Il nome KDC viene indicato come nome di dominio completo senza distinzione tra maiuscole e minuscole, ad esempio mitkdc.microsoft.com. Se il nome KDC viene omesso, DNS individuerà KDC.|

## <a name="remarks"></a>Osservazioni

Questi mapping vengono archiviati nel registro di sistema in **HKEY_LOCAL_MACHINE \system\currentcontrolset\control\lsa\kerberos\domains**. Per distribuire i dati di configurazione dell'area di autenticazione Kerberos in più computer, utilizzare lo snap-in modello di protezione e la distribuzione dei criteri anziché utilizzare **che Ksetup** in modo esplicito nei singoli computer.

Il computer deve essere riavviato prima che venga utilizzata la nuova impostazione di area di autenticazione.

Per verificare il nome dell'area di autenticazione predefinito per il computer o per verificare che il comando funzioni come previsto, eseguire **che Ksetup** al prompt dei comandi e verificare l'output per il KDC aggiunto.

## <a name="examples"></a>Esempi

Configurare un server KDC non Windows e l'area di autenticazione che la workstation deve usare:
```
ksetup /addkdc CORP.CONTOSO.COM mitkdc.contoso.com
```
Eseguire lo strumento che Ksetup nella riga di comando dello stesso computer del comando precedente per impostare la password dell'account computer locale su p@sswrd1%. Quindi riavvia il computer.
```
Ksetup /setcomputerpassword p@sswrd1%
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

-   [Che Ksetup](ksetup.md)
-   [Ksetup:setcomputerpassword](ksetup-setcomputerpassword.md)
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)