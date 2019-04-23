---
title: tftp
description: Trasferire i file da e verso un computer remoto.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 772f19a8-dafe-45cd-878a-f5691f6568ef vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ad195409076840fda0e8d6bf5cd0c295a62cdede
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845902"
---
# <a name="tftp"></a>tftp

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Trasferisce i file da e verso un computer remoto, in genere un computer UNIX, che è in esecuzione il servizio Trivial File Transfer Protocol (tftp) o daemon. TFTP viene in genere utilizzato da sistemi che recuperano le informazioni di configurazione, firmware o un'immagine del sistema durante il processo di avvio da un server tftp o dispositivi incorporati.   

## <a name="syntax"></a>Sintassi  
```  
tftp [-i] [<Host>] [{get | put}] <Source> [<Destination>]  
```  

### <a name="parameters"></a>Parametri  
|Parametro|Descrizione|  
|-------|--------|  
|-i|Specifica la modalità di trasferimento di immagine binari (detto anche modalità octet). Nella modalità binaria, il file viene trasferito in unità di misura a un byte. Utilizzare questa modalità durante il trasferimento dei file binari. Se **-i** viene omesso, il file viene trasferito in modalità ASCII. Si tratta della modalità di trasferimento predefinito. Questa modalità Converte caratteri (fine) end-of-line in un formato appropriato per il computer specificato. Utilizzare questa modalità durante il trasferimento dei file di testo. Se un trasferimento di file ha esito positivo, viene visualizzata la velocità di trasferimento dei dati.|  
|\<Host\>|Specifica il computer locale o remoto.|  
|put|Trasferisce il file *origine* nel computer locale per il file *destinazione* nel computer remoto. Poiché il protocollo tftp non supporta l'autenticazione dell'utente, l'utente deve essere connesso al computer remoto e i file devono essere accessibile in scrittura nel computer remoto.|  
|ottieni|Trasferisce il file *destinazione* nel computer remoto per il file *origine* nel computer locale.|  
|\<Origine\>|Specifica il file da trasferire.|  
|\<Destinazione\>|Specifica la posizione trasferire il file.|  

## <a name="remarks"></a>Note  
-   È possibile installare il client tftp mediante l'aggiunta guidata funzionalità.  
-   Il protocollo tftp non supporta alcun meccanismo di autenticazione o crittografia e di conseguenza può introdurre un rischio di sicurezza. Non è consigliabile installare il client tftp per sistemi connessi a Internet.  
-   Il client tftp è un software facoltativo e contrassegnate come deprecate in Windows Vista e versioni successive del sistema operativo Windows. Un servizio del server tftp non è più viene fornito da Microsoft per motivi di sicurezza.  

## <a name="BKMK_Examples"></a>Esempi  
Copiare il file **boot.img** dal computer remoto **Host1**.  
```  
tftp  -i Host1 get boot.img  
```  

## <a name="additional-references"></a>Altri riferimenti  
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)  
