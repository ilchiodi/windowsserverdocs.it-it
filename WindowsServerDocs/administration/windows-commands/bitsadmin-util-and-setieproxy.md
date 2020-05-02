---
title: bitsadmin util e setieproxy
description: Argomento di riferimento per il comando Bitsadmin util e SETIEPROXY, che consente di impostare le impostazioni proxy da utilizzare durante il trasferimento di file utilizzando un account del servizio.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0e9f31ba-3070-4ffd-a94c-388c8d78f688
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f45ca0a1a27aaf41fc55a82bcbcd3019e73ec6c8
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82707674"
---
# <a name="bitsadmin-util-and-setieproxy"></a>bitsadmin util e setieproxy

Consente di impostare le impostazioni proxy da utilizzare durante il trasferimento di file utilizzando un account del servizio. È necessario eseguire questo comando da un prompt dei comandi con privilegi elevati venga completata correttamente.

> [!NOTE]
> Questo comando non è supportato da BITS 1,5 e versioni precedenti.

## <a name="syntax"></a>Sintassi

```
bitsadmin /util /setieproxy <account> <usage> [/conn <connectionname>]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ---------- |
| account | Specifica l'account del servizio di cui si desidera definire le impostazioni del proxy. I valori possibili sono:<ul><li>LOCALSYSTEM</li><li>   NETWORKSERVICE</li><li>LocalService.</li></ul> |
| utilizzo | Specifica il modulo di rilevamento del proxy da utilizzare. I valori possibili sono:<ul><li>**NO_PROXY.** Non usare un server proxy.</li><li>**Rilevamento automatico.** Rilevare automaticamente le impostazioni del proxy.</li><li>**MANUAL_PROXY.** Usare un elenco di proxy e un elenco di bypass specificati. È necessario specificare gli elenchi subito dopo il tag Usage. Ad esempio: `MANUAL_PROXY proxy1,proxy2 NULL`.<ul><li>**Elenco proxy.** Elenco delimitato da virgole di server proxy da usare.</li><li>**Elenco di bypass.** Elenco delimitato da spazi di nomi host o indirizzi IP, o entrambi, per cui i trasferimenti non devono essere instradati tramite un proxy. Può trattarsi \<di> locali per fare riferimento a tutti i server nella stessa LAN. I valori NULL o possono essere usati per un elenco di bypass proxy vuoto.</li></ul><li>**AUTOSCRIPT.** Uguale al **rilevamento automatico**, con la differenza che viene eseguito anche uno script. È necessario specificare l'URL dello script subito dopo il tag Usage. Ad esempio: `AUTOSCRIPT http://server/proxy.js`.</li><li>**Reimpostazione.** Come **no_proxy**, ad eccezione del fatto che rimuove gli URL proxy manuali (se specificati) ed eventuali URL individuati con il rilevamento automatico.</li></ul> |
| ConnectionName | Facoltativa. Usato con il parametro **/conn** per specificare quale connessione modem usare. Se non si specifica il parametro **/conn** , BITS usa la connessione LAN. |

### <a name="remarks"></a>Osservazioni

Ogni chiamata successiva che usa questa opzione sostituisce l'utilizzo specificato in precedenza, ma non i parametri dell'utilizzo definito in precedenza. Se, ad esempio, si specifica **no_proxy**, **rileva automaticamente**e **MANUAL_PROXY** su chiamate separate, BITS usa l'ultimo utilizzo fornito, ma mantiene i parametri dall'utilizzo definito in precedenza.

## <a name="examples"></a>Esempi

Per impostare l'utilizzo del proxy per l'account LOCALSYSTEM:

```
bitsadmin /util /setieproxy localsystem AUTODETECT
```

```
bitsadmin /util /setieproxy localsystem MANUAL_PROXY proxy1,proxy2,proxy3 NULL
```

```
bitsadmin /util /setieproxy localsystem MANUAL_PROXY proxy1:80
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando util Bitsadmin](bitsadmin-util.md)

- [comando Bitsadmin](bitsadmin.md)
