---
title: Bitsadmin util e SETIEPROXY
description: Windows Commands Topic for **Bitsadmin util and SETIEPROXY**, che consente di impostare le impostazioni proxy da utilizzare durante il trasferimento di file utilizzando un account del servizio.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0e9f31ba-3070-4ffd-a94c-388c8d78f688
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2ebb33ff917ddd43bbc62413755ca28478ad5a95
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122522"
---
# <a name="bitsadmin-util-and-setieproxy"></a>Bitsadmin util e SETIEPROXY

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
| autorizzato | Specifica l'account del servizio di cui si desidera definire le impostazioni del proxy. I valori possibili sono:<ul><li>LOCALSYSTEM</li><li>   NETWORKSERVICE</li><li>LocalService.</li></ul> |
| usage | Specifica il modulo di rilevamento del proxy da utilizzare. I valori possibili sono:<ul><li>**NO_PROXY.** Non usare un server proxy.</li><li>**Rilevamento automatico.** Rilevare automaticamente le impostazioni del proxy.</li><li>**MANUAL_PROXY.** Usare un elenco di proxy e un elenco di bypass specificati. È necessario specificare gli elenchi subito dopo il tag Usage. Ad esempio, `MANUAL_PROXY proxy1,proxy2 NULL`<ul><li>**Elenco proxy.** Elenco delimitato da virgole di server proxy da usare.</li><li>**Elenco di bypass.** Elenco delimitato da spazi di nomi host o indirizzi IP, o entrambi, per cui i trasferimenti non devono essere instradati tramite un proxy. Questo può essere \<> locale per fare riferimento a tutti i server nella stessa LAN. I valori NULL o possono essere usati per un elenco di bypass proxy vuoto.</li></ul><li>**AUTOSCRIPT.** Uguale al **rilevamento automatico**, con la differenza che viene eseguito anche uno script. È necessario specificare l'URL dello script subito dopo il tag Usage. Ad esempio, `AUTOSCRIPT http://server/proxy.js`</li><li>**Reimpostazione.** Come **no_proxy**, ad eccezione del fatto che rimuove gli URL proxy manuali (se specificati) ed eventuali URL individuati con il rilevamento automatico.</li></ul> |
| ConnectionName | Facoltativa. Usato con il parametro **/conn** per specificare quale connessione modem usare. Se non si specifica il parametro **/conn** , BITS usa la connessione LAN. |

## <a name="remarks"></a>Note

Ogni chiamata successiva che usa questa opzione sostituisce l'utilizzo specificato in precedenza, ma non i parametri dell'utilizzo definito in precedenza. Se, ad esempio, si specifica **no_proxy**, **rileva automaticamente**e **MANUAL_PROXY** su chiamate separate, BITS usa l'ultimo utilizzo fornito, ma mantiene i parametri dall'utilizzo definito in precedenza.

## <a name="examples"></a>Esempi

Nell'esempio seguente imposta l'utilizzo di proxy per l'account del SERVIZIO di RETE.

```
C:\>bitsadmin /Util /SetIEProxy localsystem AUTODETECT
```

Di seguito sono riportati altri esempi.

```
bitsadmin /util /setieproxy localsystem MANUAL_PROXY proxy1,proxy2,proxy3 NULL
```

```
bitsadmin /util /setieproxy localsystem MANUAL_PROXY proxy1:80
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)