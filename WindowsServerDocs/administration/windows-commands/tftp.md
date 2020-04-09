---
title: tftp
description: Trasferire i file da e verso un computer remoto.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 772f19a8-dafe-45cd-878a-f5691f6568ef vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5ae76883798ddd2b61682700c56684b2ac3f214d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833001"
---
# <a name="tftp"></a>tftp

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Trasferisce i file da e verso un computer remoto, in genere un computer che esegue UNIX, che esegue il servizio o il daemon Trivial File Transfer Protocol (TFTP). TFTP viene in genere usato da dispositivi o sistemi incorporati che recuperano firmware, informazioni di configurazione o un'immagine di sistema durante il processo di avvio da un server TFTP.   

## <a name="syntax"></a>Sintassi  
```  
tftp [-i] [<Host>] [{get | put}] <Source> [<Destination>]  
```  

#### <a name="parameters"></a>Parametri  
|Parametro|Descrizione|  
|-------|--------|  
|-i|Specifica la modalità di trasferimento di immagini binarie (detta anche modalità ottetto). In modalità immagine binaria, il file viene trasferito in unità a un byte. Usare questa modalità per il trasferimento di file binari. Se **-i** viene omesso, il file viene trasferito in modalità ASCII. Questa è la modalità di trasferimento predefinita. Questa modalità converte i caratteri di fine riga (EOL) in un formato appropriato per il computer specificato. Usare questa modalità per il trasferimento di file di testo. Se un trasferimento di file ha esito positivo, viene visualizzata la velocità di trasferimento dei dati.|  
|\> host \<|Specifica il computer locale o remoto.|  
|put|Trasferisce l' *origine* del file nel computer locale alla *destinazione* file nel computer remoto. Poiché il protocollo TFTP non supporta l'autenticazione utente, l'utente deve essere connesso al computer remoto e i file devono essere scrivibili nel computer remoto.|  
|get|Trasferisce la *destinazione* del file nel computer remoto all' *origine* file nel computer locale.|  
|\<Origine\>|Specifica il file da trasferire.|  
|\> di destinazione \<|Specifica la posizione in cui trasferire il file.|  

## <a name="remarks"></a>Note  
-   È possibile installare il client TFTP utilizzando l'aggiunta guidata funzionalità.  
-   Il protocollo TFTP non supporta alcun meccanismo di autenticazione o crittografia e, di conseguenza, può comportare un rischio per la sicurezza, se presente. Non è consigliabile installare il client TFTP per i sistemi connessi a Internet.  
-   Il client TFTP è un software facoltativo e contrassegnato come deprecato in Windows Vista e nelle versioni successive del sistema operativo Windows. Un servizio server TFTP non è più fornito da Microsoft per motivi di sicurezza.  

## <a name="examples"></a><a name="BKMK_Examples"></a>Esempi  
Copiare il file **boot. img** dal computer remoto **host1**.  
```  
tftp  -i Host1 get boot.img  
```  

## <a name="additional-references"></a>Altri riferimenti  
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
