---
title: cmstp
description: Windows Commands argomento per cmstp, che consente di installare o rimuovere un profilo del servizio di gestione connessione.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 34aad544-11c3-4e85-8bbf-5bc5a971da93
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8516cbce34fb8129aa623b75f5c295a7b8c28331
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847504"
---
# <a name="cmstp"></a>cmstp

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Installa o rimuove un profilo del servizio di gestione connessione. Se utilizzato senza parametri facoltativi, **cmstp** viene installato un profilo del servizio con le impostazioni predefinite adatte per il sistema operativo e le autorizzazioni dell'utente. 

## <a name="syntax"></a>Sintassi
Sintassi 1:
```
ServiceProfileFileName .exe /q:a /c:cmstp.exe ServiceProfileFileName .inf [/nf] [/ni] [/ns] [/s] [/su] [/u]
```
Sintassi 2:
```
cmstp.exe [/nf] [/ni] [/ns] [/s] [/su] [/u]  [Drive:][path]ServiceProfileFileName.inf
```
#### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|< NomeFileProfiloServizio > .exe|Specifica il nome, il pacchetto di installazione che contiene il profilo che si desidera installare.<p>Obbligatorio per la sintassi 1, ma non è valido per la sintassi 2.|
|/q: un|Specifica che il profilo deve essere installato senza chiedere conferma all'utente. Verrà comunque visualizzato il messaggio di verifica che l'installazione ha avuto esito positivo.<p>Obbligatorio per la sintassi 1, ma non è valido per la sintassi 2.|
|[Unità:] [percorso] <ServiceProfileFileName>. inf|Obbligatoria. Specifica il nome, il file di configurazione che determina come deve essere installato il profilo.<p>Il parametro [unità:] [percorso] non è valido per la sintassi 1.|
|/Nf.|Specifica che i file di supporto non devono essere installati.|
|/Ni|Specifica che un'icona del desktop non deve essere creato. Questo parametro è valido solo per i computer che eseguono Windows 95, Windows 98, Windows NT 4,0 o Windows Millennium Edition.|
|/NS|Specifica che non deve essere creato un collegamento sul desktop. Questo parametro è valido solo per i computer che eseguono un membro della famiglia Windows Server 2003, Windows 2000 o Windows XP.|
|/s|Specifica che il profilo del servizio deve essere installato o disinstallato automaticamente (senza chiedere conferma per la risposta dell'utente o visualizzare messaggio di verifica).|
|/su|Specifica che il profilo del servizio deve essere installato per un singolo utente anziché per tutti gli utenti. Questo parametro è valido solo per computer Windows Server 2003, Windows 2000 o Windows XP.|
|/AU|Specifica che il profilo del servizio deve essere installato per tutti gli utenti. Questo parametro è valido solo per i computer che esegue Windows Server 2003, Windows 2000 o Windows XP.|
|/u|Specifica che il profilo del servizio deve essere disinstallato.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note
**/s** è l'unico parametro che è possibile utilizzare in combinazione con **/u**.
La sintassi 1 è la tipica sintassi utilizzata in un'applicazione di installazione personalizzata. Per utilizzare questa sintassi, è necessario eseguire **cmstp** dalla directory che contiene il <ServiceProfileFileName>file .exe.

## <a name="examples"></a><a name=BKMK_Examples></a>Esempi
Per installare il profilo del servizio fittizio senza i file di supporto, digitare:
```
fiction.exe /c:cmstp.exe fiction.inf /nf
```
Per installare automaticamente il profilo del servizio fittizio per un singolo utente, digitare:
```
fiction.exe /c:cmstp.exe fiction.inf /s /su
```
Per disinstallare il profilo del servizio fittizio, digitare:
```
fiction.exe /c:cmstp.exe fiction.inf /s /u
```
## <a name="additional-references"></a>Altre informazioni di riferimento
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
