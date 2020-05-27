---
title: delkdc che Ksetup
description: Argomento di riferimento per il comando che Ksetup delkdc, che elimina le istanze dei nomi Centro distribuzione chiavi (KDC) per l'area di autenticazione Kerberos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7d6ec389-094c-4a7b-a78b-605497ddc289
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bd3901558f1cda0d2e1d4e7c12b0d2b151870fa8
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83817851"
---
# <a name="ksetup-delkdc"></a>delkdc che Ksetup

Elimina le istanze di nomi di Centro distribuzione chiavi (KDC) per l'area di autenticazione Kerberos.

Il mapping viene archiviato nel registro di sistema, in `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\LSA\Kerberos\Domains` . Dopo l'esecuzione di questo comando, è consigliabile verificare che il KDC sia stato rimosso e non sia più presente nell'elenco.

> [!NOTE]
> Per rimuovere i dati di configurazione dell'area di autenticazione da più computer, utilizzare lo snap-in **modello di configurazione di sicurezza** con la distribuzione dei criteri, anziché utilizzare il comando **che Ksetup** in modo esplicito nei singoli computer.

## <a name="syntax"></a>Sintassi

```
ksetup /delkdc <realmname> <KDCname>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<realmname>` | Specifica il nome DNS in maiuscolo, ad esempio CORP. CONTOSO.COM. Si tratta dell'area di autenticazione predefinita visualizzata quando si esegue il comando **che Ksetup** ed è l'area di autenticazione da cui si desidera eliminare il KDC. |
| `<KDCname>` | Specifica il nome di dominio completo con distinzione tra maiuscole e minuscole, ad esempio mitkdc.contoso.com. |

### <a name="examples"></a>Esempi

Per visualizzare tutte le associazioni tra l'area di autenticazione di Windows e l'area di autenticazione non Windows e per individuare quelle da rimuovere, digitare:

```
ksetup
```

Per rimuovere l'associazione, digitare:

```
ksetup /delkdc CORP.CONTOSO.COM mitkdc.contoso.com
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando che Ksetup](ksetup.md)

- [comando che Ksetup addkdc](ksetup-addkdc.md)