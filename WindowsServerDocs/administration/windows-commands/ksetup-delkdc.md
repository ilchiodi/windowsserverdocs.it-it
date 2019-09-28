---
title: 'che Ksetup: delkdc'
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7d6ec389-094c-4a7b-a78b-605497ddc289
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b918f8c2fa0ec09c2aae77517a0ee2c9e77ce2dc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375145"
---
# <a name="ksetupdelkdc"></a>che Ksetup: delkdc



Elimina le istanze di nomi di Centro distribuzione chiavi (KDC) per l'area di autenticazione Kerberos. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
ksetup /delkdc <RealmName> <KDCName>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<RealmName >|Il nome dell'area di autenticazione è specificato come un nome DNS lettere maiuscole, ad esempio CORP. CONTOSO.COM e è elencato come area di autenticazione predefinito quando **che ksetup** viene eseguito. È l'area di autenticazione da cui si sta tentando di eliminare il KDC altri.|
|\<KDCName >|Il nome KDC è indicato come nome di dominio completo, tra maiuscole e minuscole, ad esempio mitkdc.contoso.com.|

## <a name="remarks"></a>Note

Questi mapping sono memorizzati nel Registro di sistema **HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\LSA\Kerberos\Domains**. Per rimuovere i dati di configurazione dell'area di autenticazione da più computer, utilizzare il modello di configurazione di sicurezza lo snap-in e distribuzione dei criteri anziché **che ksetup** in modo esplicito nei singoli computer.

Nei computer che eseguono Windows 2000 Server con Service Pack 1 (SP1) e versioni precedenti, è necessario riavviare il computer prima che la configurazione di impostazione dell'area di autenticazione modificato verrà utilizzata.

Per verificare il nome dell'area di autenticazione predefinito per il computer, o per verificare che questo comando funzioni come previsto, eseguire **che ksetup** al prompt dei comandi e verificare che il KDC è stato rimosso non esiste nell'elenco.

## <a name="BKMK_Examples"></a>Esempi

I requisiti di sicurezza per questo computer sono stati modificati in modo che il collegamento tra l'area di autenticazione di Windows e l'area di autenticazione di Windows non deve essere rimosso. Innanzitutto, determinare quale associazione da rimuovere e produrre l'output delle associazioni esistenti:
```
ksetup
```
Rimuovere l'associazione utilizzando il comando seguente:
```
Ksetup /delkdc CORP.CONTOSO.COM mitkdc.contoso.com
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Che Ksetup](ksetup.md)
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)