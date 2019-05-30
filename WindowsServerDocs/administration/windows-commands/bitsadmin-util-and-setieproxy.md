---
title: SETIEPROXY e bitsadmin util
description: Argomento i comandi di Windows per **util bitsadmin e setieproxy** -configurare impostazioni proxy da utilizzare durante il trasferimento di file usando un account del servizio.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0e9f31ba-3070-4ffd-a94c-388c8d78f688
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 81bb333e2bb776bc75789b52ab41d7ef64016f51
ms.sourcegitcommit: d84dc3d037911ad698f5e3e84348b867c5f46ed8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/28/2019
ms.locfileid: "66266470"
---
# <a name="bitsadmin-util-and-setieproxy"></a>SETIEPROXY e bitsadmin util

Configurare impostazioni proxy da utilizzare durante il trasferimento di file utilizzando un account del servizio.

**BITSAdmin 1.5 e versioni precedenti**: Non supportato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /Util /SetIEProxy <Account> <Usage>[/Conn <ConnectionName>]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Account|Specifica il tipo di account del servizio di cui si desidera definire le impostazioni di proxy. I valori possibili sono:</br>-LOCALSYSTEM</br>-SERVIZIO DI RETE</br>-LOCALSERVICE|
|Uso|Specifica il modulo di rilevamento del proxy da utilizzare. I valori possibili sono:</br>-NO_PROXY: non usare un server proxy.</br>-Rilevamento automatico, rilevare automaticamente le impostazioni del proxy.</br>-MANUAL_PROXY: utilizzare un elenco di proxy esplicito e l'elenco di esclusione. Specificare l'elenco di proxy e ignorare l'elenco che segue immediatamente il tag di utilizzo. Ad esempio, MANUAL_PROXY, proxy1 proxy2 NULL.</br>    -L'elenco di proxy è un elenco delimitato da virgole dei server proxy da utilizzare.</br>    -Elenco di esclusione è un elenco delimitato da spazi di nomi host o gli indirizzi IP o entrambi, per cui trasferimenti non sono devono essere instradati attraverso un proxy. Può trattarsi di \<locale > per fare riferimento a tutti i server alla stessa rete LAN. I valori null o "" può essere utilizzato per un elenco di esclusione proxy vuoto.</br>-AUTOSCRIPT: Uguale a rilevamento automatico, ad eccezione del fatto che anche l'esecuzione di uno script. Specificare l'URL di script immediatamente dopo il tag di utilizzo. Ad esempio, AUTOSCRIPT http://server/proxy.js.</br>-RESET, Identico NO_PROXY, con la differenza che rimuove gli URL proxy manuale (se specificato) e gli URL individuata mediante il rilevamento automatico.|
|ConnectionName|Facoltativo, usato con il **/Conn** parametro per specificare la connessione modem da utilizzare. Se non si specifica la **/Conn** parametro, BITS utilizza la connessione LAN. Specificare il nome della connessione modem immediatamente dopo il **/conn** parametro.|

## <a name="remarks"></a>Note

Ogni chiamata successiva mediante questa opzione sostituisce l'utilizzo specificato in precedenza ma non i parametri dell'utilizzo definito in precedenza. Ad esempio, se si specifica NO_PROXY, rilevamento AUTOMATICO e MANUAL_PROXY in chiamate separate, BITS utilizza l'ultimo utilizzo fornito ma mantiene i parametri dall'utilizzo definito in precedenza.

> [!IMPORTANT]
> È necessario eseguire questo comando da un prompt dei comandi con privilegi elevati venga completata correttamente.

## <a name="examples"></a>Esempi

Nell'esempio seguente imposta l'utilizzo di proxy per l'account del SERVIZIO di RETE.

```
C:\>bitsadmin /Util /SetIEProxy localsystem AUTODETECT
```

Di seguito sono riportati altri esempi.

```
bitsadmin /util /setieproxy localsystem MANUAL_PROXY proxy1,proxy2,proxy3 NULL
bitsadmin /util /setieproxy localsystem MANUAL_PROXY proxy1:80 ""
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)