---
title: tlntadmn
description: Argomento di riferimento per tlntadmn, che consente di amministrare un computer locale o remoto, eseguendo il servizio Server Telnet.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 78b61e8d-b953-44bb-8d57-f3b42da9e7a8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fcade72f7170e94463abb9b473d4c6bd2251f086
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/30/2020
ms.locfileid: "84222642"
---
# <a name="tlntadmn"></a>tlntadmn

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Amministra un computer locale o remoto che esegue il servizio Server Telnet.

## <a name="syntax"></a>Sintassi
```
tlntadmn [<computerName>] [-u <UserName>] [-p <Password>] [{start | stop | pause | continue}] [-s {<SessionID> | all}] [-k {<SessionID> | all}] [-m {<SessionID> | all}  <Message>] [config [dom = <Domain>] [ctrlakeymap = {yes | no}] [timeout = <hh>:<mm>:<ss>] [timeoutactive = {yes | no}] [maxfail = <attempts>] [maxconn = <Connections>] [port = <Number>] [sec {+ | -}NTLM {+ | -}passwd] [mode = {console | stream}]] [-?]
```
#### <a name="parameters"></a>Parametri

|                   Parametro                    |                                                                                                                                                       Descrizione                                                                                                                                                        |
|------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                \<computerName>                 |                                                                                                                    Specifica il nome del server a cui connettersi. Il valore predefinito è il computer locale.                                                                                                                    |
|         -u \<UserName> -p\<Password>          |                                                Specifica le credenziali amministrative per un server remoto che si desidera amministrare. Questo parametro è obbligatorio se si desidera amministrare un server remoto a cui non si è connessi con credenziali amministrative.                                                |
|                     start                      |                                                                                                                                            avvia il servizio Server Telnet.                                                                                                                                             |
|                      stop                      |                                                                                                                                             Arresta il servizio Server Telnet                                                                                                                                              |
|                     Sospendi                      |                                                                                                                          sospende il servizio Server Telnet. Verranno accettare nuove connessioni.                                                                                                                          |
|                    continue                    |                                                                                                                                            Riprende il servizio Server Telnet.                                                                                                                                            |
|          -s {\<SessionID> & #124; tutti}          |                                                                                                                                             Visualizza le sessioni Telnet attive.                                                                                                                                             |
|          -k {\<SessionID> & #124; tutti}          |                                                                                                        Termina le sessioni Telnet. digitare l'ID sessione per terminare una sessione specifica oppure digitare all per terminare tutte le sessioni.                                                                                                         |
|    -m { \<SessionID> &#124; all}<Message>     |                                                   Invia un messaggio a una o più sessioni. digitare l'ID di sessione per inviare un messaggio a una sessione specifica oppure digitare all per inviare un messaggio a tutte le sessioni. digitare il messaggio che si desidera inviare tra virgolette.                                                   |
|             configurazione Dom =\<Domain>             |                                                                                                                                      Consente di configurare il dominio predefinito per il server.                                                                                                                                       |
|      configurazione non qualificato = {Sì & #124; n}      |                                                                                     Specifica se si desidera che il server Telnet interpreti CTRL + A come ALT. Digitare **Yes** per eseguire il mapping del tasto di scelta rapida oppure digitare **No** per evitare il mapping.                                                                                     |
|       timeout configurazione = \<hh> : \<mm> :\<ss>       |                                                                                                                                 Imposta il periodo di timeout in ore, minuti e secondi.                                                                                                                                 |
|     configurazione timeoutactive = {Sì & #124; no      |                                                                                                                                            Consente il timeout di sessione inattiva.                                                                                                                                             |
|          configurazione maxfail =\<attempts>          |                                                                                                                          Imposta il numero massimo di tentativi di accesso non riusciti prima della disconnessione.                                                                                                                          |
|        configurazione maxconn =\<Connections>         |                                                                                                                                         Imposta il numero massimo di connessioni.                                                                                                                                          |
|            porta config = < \Numero>             |                                                                                                                    Imposta la porta Telnet. È necessario specificare la porta con un valore integer minore di 1024.                                                                                                                    |
| configurazione sec {+ & #124; -} NTLM {+ & #124; -} passwd | Specifica se si desidera utilizzare NTLM, una password o entrambi per autenticare i tentativi di accesso. Per usare un particolare tipo di autenticazione, digitare un segno più ( **+** ) prima del tipo di autenticazione. Per impedire l'utilizzo di un particolare tipo di autenticazione, digitare un segno meno ( **-** ) prima di tale tipo di autenticazione. |
|     modalità di configurazione = {console & #124; flusso}      |                                                                                                                                             Specifica la modalità di funzionamento.                                                                                                                                             |
|                       -?                       |                                                                                                                                           Visualizza la guida al prompt dei comandi.                                                                                                                                           |

## <a name="remarks"></a>Commenti
-   Per visualizzare le impostazioni del server, digitare **tlntadmn** senza parametri.
-   Utilizzare il **tlntadmn** comando, è necessario accedere al computer locale con credenziali amministrative. Per amministrare un computer remoto, è inoltre necessario fornire credenziali amministrative per il computer remoto. È possibile farlo accedendo al computer locale con un account con credenziali amministrative per il computer locale sia nel computer remoto. Se è possibile utilizzare questo metodo, è possibile utilizzare il **-u** e **-p** parametri per fornire credenziali amministrative per il computer remoto.

## <a name="examples"></a>Esempi
Configurare il timeout di sessione inattiva per 30 minuti.
```
tlntadmn config timeout=0:30:0
```
Visualizza sessioni Telnet attive.
```
tlntadmn -s
```

## <a name="additional-references"></a>Riferimenti aggiuntivi
-   [Guida operativa di Telnet](https://technet.microsoft.com/library/cc753164(v=ws.10).aspx)
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
