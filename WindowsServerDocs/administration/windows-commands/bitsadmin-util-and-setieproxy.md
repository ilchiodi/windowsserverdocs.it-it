---
title: Bitsadmin util e SETIEPROXY
description: Windows Commands Topic for Bitsadmin util and SETIEPROXY, che consente di impostare le impostazioni proxy da utilizzare durante il trasferimento di file utilizzando un account del servizio.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0e9f31ba-3070-4ffd-a94c-388c8d78f688
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4e7d8a9ff4e2388b61ee5ae00ae7afe421de68e6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848884"
---
# <a name="bitsadmin-util-and-setieproxy"></a>Bitsadmin util e SETIEPROXY

Consente di impostare le impostazioni proxy da utilizzare durante il trasferimento di file utilizzando un account del servizio.

**BITSAdmin 1,5 e versioni precedenti**: non supportato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /Util /SetIEProxy <Account> <Usage>[/Conn <ConnectionName>]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Account|Specifica il tipo di account del servizio di cui si desidera definire le impostazioni proxy. I valori possibili sono:</br>-LOCALSYSTEM</br>-SERVIZIO DI RETE</br>-LOCALSERVICE|
|Utilizzo|Specifica il modulo di rilevamento del proxy da utilizzare. I valori possibili sono:</br>-NO_PROXY-non usare un server proxy.</br>-Autodetect: rileva automaticamente le impostazioni del proxy.</br>-MANUAL_PROXY: usare un elenco di proxy esplicito e un elenco di bypass. Specificare l'elenco di proxy e ignorare l'elenco che segue immediatamente il tag di utilizzo. Ad esempio, MANUAL_PROXY, proxy1 proxy2 NULL.</br>    -L'elenco dei proxy è un elenco delimitato da virgole di server proxy da usare.</br>    -L'elenco di bypass è un elenco delimitato da spazi di nomi host o indirizzi IP, o entrambi, per i quali i trasferimenti non devono essere instradati tramite un proxy. Questo può essere \<> locale per fare riferimento a tutti i server nella stessa LAN. I valori NULL o possono essere usati per un elenco di bypass proxy vuoto.</br>-Autoscript, uguale al rilevamento automatico, con la differenza che viene eseguito anche uno script. Specificare l'URL di script immediatamente dopo il tag di utilizzo. Ad esempio, Autoscript http://server/proxy.js.</br>-RESET: uguale a NO_PROXY, ad eccezione del fatto che rimuove gli URL del proxy manuali (se specificati) e gli URL individuati con il rilevamento automatico.|
|ConnectionName|Facoltativo: usato con il parametro **/conn** per specificare la connessione modem da usare. Se non si specifica il parametro **/conn** , BITS utilizzerà la connessione LAN. Specificare il nome della connessione modem immediatamente dopo il **/conn** parametro.|

## <a name="remarks"></a>Note

Ogni chiamata successiva che usa questa opzione sostituisce l'utilizzo specificato in precedenza ma non i parametri dell'utilizzo definito in precedenza. Ad esempio, se si specifica NO_PROXY, rilevamento AUTOMATICO e MANUAL_PROXY in chiamate separate, BITS utilizza l'ultimo utilizzo fornito ma mantiene i parametri dall'utilizzo definito in precedenza.

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
bitsadmin /util /setieproxy localsystem MANUAL_PROXY proxy1:80 
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)