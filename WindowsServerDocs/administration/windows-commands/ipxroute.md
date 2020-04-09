---
title: ipxroute
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3a30304f-655e-43d2-a4ac-7568abf8975c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f1e011835dbdbcf7be1daca2cdfbd47c39f9355c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842064"
---
# <a name="ipxroute"></a>ipxroute

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza e modifica le informazioni sulle tabelle di routing utilizzate dal protocollo IPX. Se utilizzato senza parametri,  **ipxroute** Visualizza le impostazioni predefinite per i pacchetti che vengono inviati a indirizzi sconosciuti, broadcast e multicast.   
## <a name="syntax"></a>Sintassi  
```  
ipxroute servers [/type=X]  
ipxroute ripout <Network>  
ipxroute resolve {guid | name} {GUID | <AdapterName>}  
ipxroute board= N [def] [gbr] [mbr] [remove=xxxxxxxxxxxx]  
ipxroute config  
```  
#### <a name="parameters"></a>Parametri  
|Parametro|Descrizione|  
|-------|--------|  
|server [/ type = X]|Visualizza la tabella Service Access Point (SAP) per il tipo di server specificato.  **X** deve essere un numero intero. Ad esempio, **/type = 4** Visualizza tutti i file server. Se non si specifica **/tipo**, **server ipxroute** Visualizza tutti i tipi di server, elencandoli in base al nome del server.|  
|ripout rete|Consente di individuare se *rete* è raggiungibile consultando la tabella di routing di stack IPX e inviando una richiesta rip se necessario.  *Rete* è il numero di segmenti di rete IPX.|  
|risoluzione {GUID & #124; nome} {GUID & #124; AdapterName}|Risolve il nome del GUID per il relativo nome descrittivo o il nome descrittivo per il relativo GUID.|  
|area = *N*|Specifica la scheda di rete per cui si desidera eseguire una query o impostare i parametri.|  
|DEF|Invia pacchetti per la trasmissione di TUTTE le ROUTE. Se un pacchetto viene trasmesso a un unico indirizzo di scheda MAC (Media Access) che non è presente nella tabella di routing di origine, **ipxroute** Invia il pacchetto per la SINGOLA ROUTE broadcast per impostazione predefinita.|  
|GBR|Invia pacchetti per la trasmissione di TUTTE le ROUTE. Se un pacchetto viene trasmesso all'indirizzo di broadcast (FFFFFFFFFFFF), **ipxroute** Invia il pacchetto per la SINGOLA ROUTE broadcast per impostazione predefinita.|  
|MBR|Invia pacchetti per la trasmissione di TUTTE le ROUTE. Se un pacchetto viene trasmesso a un indirizzo multicast (C000xxxxxxxx), **ipxroute** Invia il pacchetto per la SINGOLA ROUTE broadcast per impostazione predefinita.|  
|rimuovere = *xxxxxxxxxxxx*|Rimuove l'indirizzo del nodo specificato dalla tabella di routing di origine.|  
|config|Visualizza informazioni su tutte le associazioni per cui è configurata IPX.|  
|/?|Visualizza la guida al prompt dei comandi.|  
## <a name="examples"></a><a name=BKMK_Examples></a>Esempi  
Per visualizzare i segmenti di rete a cui è collegata la workstation, l'indirizzo del nodo workstation e il tipo di frame in uso, digitare:  
```  
ipxroute config  
```  
## <a name="additional-references"></a>Altre informazioni di riferimento  
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
