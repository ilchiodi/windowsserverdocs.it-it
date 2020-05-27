---
title: addkdc che Ksetup
description: Argomento di riferimento per il comando che Ksetup addkdc, che consente di pubblicare un indirizzo Centro distribuzione chiavi (KDC) per l'area di autenticazione Kerberos specificata.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 98bfc23a-14c4-401c-bcb3-9903c5cdde64
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e51279166bf60196d12f877506d3228b78c4a711
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83818091"
---
# <a name="ksetup-addkdc"></a>addkdc che Ksetup

Aggiunge un indirizzo di Centro distribuzione chiavi (KDC) per l'area di autenticazione Kerberos specificata

Il mapping viene archiviato nel registro di sistema, in **HKEY_LOCAL_MACHINE \system\currentcontrolset\control\lsa\kerberos\domains** e il computer deve essere riavviato prima che venga utilizzata la nuova impostazione di area di autenticazione.

> [!NOTE]
> Per distribuire i dati di configurazione dell'area di autenticazione Kerberos in più computer, è necessario utilizzare lo snap-in e la distribuzione dei criteri di **configurazione della sicurezza** in modo esplicito nei singoli computer. Non è possibile usare questo comando.

## <a name="syntax"></a>Sintassi

```
ksetup /addkdc <realmname> [<KDCname>]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<realmname>` | Specifica il nome DNS in maiuscolo, ad esempio CORP. CONTOSO.COM. Questo valore viene visualizzato anche come area di autenticazione predefinita quando viene eseguito **che Ksetup** ed è l'area di autenticazione a cui si desidera aggiungere l'altro KDC. |
| `<KDCname>` | Specifica il nome di dominio completo senza distinzione tra maiuscole e minuscole, ad esempio mitkdc.contoso.com. Se il nome KDC viene omesso, DNS individuerà KDC. |

### <a name="examples"></a>Esempi

Per configurare un server KDC non Windows e l'area di autenticazione che la workstation deve usare, digitare:

```
ksetup /addkdc CORP.CONTOSO.COM mitkdc.contoso.com
```

Per impostare la password dell'account computer locale p@sswrd1 su% nello stesso computer dell'esempio precedente e quindi riavviare il computer, digitare:

```
ksetup /setcomputerpassword p@sswrd1%
```

Per verificare il nome dell'area di autenticazione predefinito per il computer o per verificare che il comando abbia funzionato come previsto, digitare:

```
ksetup
```
Controllare il registro di sistema per verificare che il mapping si sia verificato come previsto.

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando che Ksetup](ksetup.md)

- [comando che Ksetup setcomputerpassword](ksetup-setcomputerpassword.md)
