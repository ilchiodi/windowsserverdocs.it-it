---
title: tlntadmn
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 78b61e8d-b953-44bb-8d57-f3b42da9e7a8 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8b4423e35d0c26819188001dea8d3d8497add7f4
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440948"
---
# <a name="tlntadmn"></a>tlntadmn

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consente di amministrare un computer locale o remoto che esegue il servizio Server telnet.   
## <a name="syntax"></a>Sintassi  
```  
tlntadmn [<computerName>] [-u <UserName>] [-p <Password>] [{start | stop | pause | continue}] [-s {<SessionID> | all}] [-k {<SessionID> | all}] [-m {<SessionID> | all}  <Message>] [config [dom = <Domain>] [ctrlakeymap = {yes | no}] [timeout = <hh>:<mm>:<ss>] [timeoutactive = {yes | no}] [maxfail = <attempts>] [maxconn = <Connections>] [port = <Number>] [sec {+ | -}NTLM {+ | -}passwd] [mode = {console | stream}]] [-?]  
```  
### <a name="parameters"></a>Parametri  

|                   Parametro                    |                                                                                                                                                       Descrizione                                                                                                                                                        |
|------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                \<computerName>                 |                                                                                                                    Specifica il nome del server a cui connettersi. Il valore predefinito è il computer locale.                                                                                                                    |
|         -u \<UserName > -p \<Password >          |                                                Specifica le credenziali amministrative per un server remoto che si desidera amministrare. Questo parametro è obbligatorio se si desidera amministrare un server remoto a cui non si è connessi con credenziali amministrative.                                                |
|                     start                      |                                                                                                                                            Avvia il servizio Server telnet.                                                                                                                                             |
|                      stop                      |                                                                                                                                             Arresta il servizio Server telnet                                                                                                                                              |
|                     Sospendi                      |                                                                                                                          Sospende il servizio Server telnet. Verranno accettare nuove connessioni.                                                                                                                          |
|                    continuare                    |                                                                                                                                            Riprende il servizio Server telnet.                                                                                                                                            |
|          -s {\<SessionID > &#124; tutti}          |                                                                                                                                             Visualizza sessioni attive di telnet.                                                                                                                                             |
|          -k {\<SessionID > &#124; tutti}          |                                                                                                        Terminata la sessione telnet. Digitare l'ID di sessione per terminare una sessione specifica oppure tutti per terminare tutte le sessioni.                                                                                                         |
|    -m {\<SessionID > &#124; tutti}  <Message>     |                                                   Invia un messaggio a una o più sessioni. Digitare l'ID di sessione per inviare un messaggio a una determinata sessione, oppure tutti per inviare un messaggio a tutte le sessioni. digitare il messaggio che si desidera inviare tra virgolette.                                                   |
|             configurazione dom = \<dominio >             |                                                                                                                                      Consente di configurare il dominio predefinito per il server.                                                                                                                                       |
|      configurazione non qualificato = {Sì &#124; n}      |                                                                                     Specifica se si desidera che il server telnet interpretare CTRL + A come ALT. tipo di **yes** alla mappa tasto di scelta rapida o di tipo **alcun** per impedire il mapping.                                                                                     |
|       config timeout = \<hh>:\<mm>:\<ss>       |                                                                                                                                 Imposta il periodo di timeout in ore, minuti e secondi.                                                                                                                                 |
|     configurazione timeoutactive = {Sì &#124; no      |                                                                                                                                            Consente il timeout di sessione inattiva.                                                                                                                                             |
|          config maxfail = \<attempts>          |                                                                                                                          Imposta il numero massimo di tentativi di accesso non riusciti prima della disconnessione.                                                                                                                          |
|        configurazione maxconn = \<connessioni >         |                                                                                                                                         Imposta il numero massimo di connessioni.                                                                                                                                          |
|            configurazione porta = < \Number >             |                                                                                                                    Imposta la porta di telnet. È necessario specificare la porta con un valore integer minore di 1024.                                                                                                                    |
| configurazione sec {+ &#124; -} NTLM {+ &#124; -} passwd | Specifica se si desidera utilizzare NTLM, una password o entrambi per autenticare i tentativi di accesso. Per utilizzare un particolare tipo di autenticazione, digitare un segno più ( **+** ) prima del tipo di autenticazione. Per impedire l'utilizzo di un determinato tipo di autenticazione, digitare un segno meno ( **-** ) prima del tipo di autenticazione. |
|     modalità di configurazione = {console &#124; flusso}      |                                                                                                                                             Specifica la modalità di funzionamento.                                                                                                                                             |
|                       -?                       |                                                                                                                                           Visualizza la guida al prompt dei comandi.                                                                                                                                           |

## <a name="remarks"></a>Note  
-   Per visualizzare le impostazioni del server, digitare **tlntadmn** senza parametri.  
-   Utilizzare il **tlntadmn** comando, è necessario accedere al computer locale con credenziali amministrative. Per amministrare un computer remoto, è inoltre necessario fornire credenziali amministrative per il computer remoto. È possibile farlo accedendo al computer locale con un account con credenziali amministrative per il computer locale sia nel computer remoto. Se è possibile utilizzare questo metodo, è possibile utilizzare il **-u** e **-p** parametri per fornire credenziali amministrative per il computer remoto.  

## <a name="BKMK_Examples"></a>Esempi  
Configurare il timeout di sessione inattiva per 30 minuti.  
```  
tlntadmn config timeout=0:30:0  
```  
Visualizza sessioni attive di telnet.  
```  
tlntadmn -s  
```  

## <a name="additional-references"></a>Altri riferimenti  
-   [Guida operativa Telnet](https://technet.microsoft.com/library/cc753164(v=ws.10).aspx)  
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
