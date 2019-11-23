---
title: prnport
description: Informazioni su come creare, eliminare ed elencare le porte di stampa.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6a0ec638-a21e-4a34-be5c-bd0f7ca89ffe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c9c162cef2a3ae2f3de1e891691572130ae68f93
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372548"
---
# <a name="prnport"></a>prnport

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consente di creare, eliminare ed elencare le porte di stampa TCP/IP standard, oltre a visualizzare e modificare la configurazione delle porte.

## <a name="syntax"></a>Sintassi
```
cscript prnport {-a | -d | -l | -g | -t | -?} [-r <PortName>] 
[-s <ServerName>] [-u <UserName>] [-w <Password>] [-o {raw | lpr}] 
[-h <Hostaddress>] [-q <QueueName>] [-n <PortNumber>] -m{e | d} 
[-i <SNMPIndex>] [-y <CommunityName>] -2{e | -d}
```

## <a name="parameters"></a>Parametri

|          Parametro           |                                                                                                                                                                                                                                                                                                     Descrizione                                                                                                                                                                                                                                                                                                      |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              -a              |                                                                                                                                                                                                                                                                                       Crea una porta stampante TCP/IP standard.                                                                                                                                                                                                                                                                                        |
|              -d              |                                                                                                                                                                                                                                                                                       Elimina una porta stampante TCP/IP standard.                                                                                                                                                                                                                                                                                        |
|              -l              |                                                                                                                                                                                                                                                             elenca tutte le porte della stampante TCP/IP standard nel computer specificato con il parametro **-s** .                                                                                                                                                                                                                                                             |
|              -g              |                                                                                                                                                                                                                                                                            Consente di visualizzare la configurazione di una porta stampante TCP/IP standard.                                                                                                                                                                                                                                                                             |
|              -t              |                                                                                                                                                                                                                                                                           Configura le impostazioni della porta per una porta stampante TCP/IP standard.                                                                                                                                                                                                                                                                           |
|        -r \<Portaname >        |                                                                                                                                                                                                                                                                                Specifica la porta a cui è connessa la stampante.                                                                                                                                                                                                                                                                                 |
|       -s \<nomeserver >       |                                                                                                                                                                                                                               Specifica il nome del computer remoto che ospita la stampante che si desidera gestire. Se non si specifica un computer, viene usato il computer locale.                                                                                                                                                                                                                                |
| -u \<nome utente >-w <Password> |                                                                                                              Specifica un account con le autorizzazioni per la connessione al computer che ospita la stampante che si desidera gestire. Tutti i membri del gruppo Administrators locale del computer di destinazione dispongono di queste autorizzazioni, ma è possibile concedere anche le autorizzazioni ad altri utenti. Se non si specifica un account, è necessario effettuare l'accesso con un account con le autorizzazioni necessarie per il funzionamento del comando.                                                                                                               |
|     -o {RAW &#124; LPR}      |                                                                                                                                                                                                              Specifica il protocollo utilizzato dalla porta: TCP RAW o TCP LPR. Se si utilizza TCP RAW, è possibile specificare facoltativamente il numero di porta utilizzando il parametro **-n** . Il numero di porta predefinito è 9100.                                                                                                                                                                                                              |
|      -h \<HostAddress >       |                                                                                                                                                                                                                                                                   Specifica (per indirizzo IP) la stampante per la quale si desidera configurare la porta.                                                                                                                                                                                                                                                                    |
|       -q \<QueueName >        |                                                                                                                                                                                                                                                                                     Specifica il nome della coda per una porta TCP non elaborata.                                                                                                                                                                                                                                                                                     |
|       -n \<NumeroPorta >       |                                                                                                                                                                                                                                                                    Specifica il numero di porta per una porta TCP non elaborata. Il numero di porta predefinito è 9100.                                                                                                                                                                                                                                                                    |
|        -m {e &#124; d}        |                                                                                                                                                                                                                                                       Specifica se SNMP è abilitato. Il parametro **e** Abilita SNMP. Il parametro **d** Disabilita SNMP.                                                                                                                                                                                                                                                        |
|        -i \<SNMPIndex        |                                                                                                                                                                                                                             Specifica l'indice SNMP, se SNMP è abilitato. Per ulteriori informazioni, vedere RFC 1759 nel [sito Web RFC Editor](https://go.microsoft.com/fwlink/?LinkId=569).                                                                                                                                                                                                                              |
|     -y \<Communityname >      |                                                                                                                                                                                                                                                                                Specifica il nome della comunità SNMP, se SNMP è abilitato.                                                                                                                                                                                                                                                                                |
|       -2 {e &#124; -d}        | Specifica se sono abilitati i doppi spool (noti anche come rispooling) per le porte TCP LPR. I doppi spool sono necessari perché TCP LPR deve includere un numero di byte accurato nel file di controllo inviato alla stampante, ma il protocollo non è in grado di ottenere il conteggio dal provider di stampa locale. Pertanto, quando un file viene sottoposto a spooling in una coda di stampa LPR TCP, viene anche eseguito lo spooling come file temporaneo nella directory system32. TCP LPR determina le dimensioni del file temporaneo e invia le dimensioni al server che esegue LPD. Il parametro **e** Abilita i doppi spool. Il parametro **d** Disabilita i doppi spool. |
|              /?              |                                                                                                                                                                                                                                                                                         Visualizza la guida al prompt dei comandi.                                                                                                                                                                                                                                                                                         |

## <a name="remarks"></a>Osservazioni
-   Il comando **prnport** è uno script Visual Basic che si trova nella directory%windir%\system32\. printing_Admin_Scripts\\<language>. Per usare questo comando, al prompt dei comandi digitare **cscript** seguito dal percorso completo del file prnport o passare alla cartella appropriata. Ad esempio:
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnport
    ```
-   Se le informazioni fornite contengono spazi, racchiudere il testo tra virgolette, ad esempio `"computer Name"`.
-   Il protocollo TCP RAW è un protocollo di prestazioni più elevato in Windows rispetto al protocollo LPR.

## <a name="BKMK_examples"></a>Esempi
Per visualizzare tutte le porte di stampa TCP/IP standard nel server \\\Server1, digitare:
```
cscript prnport -l -s Server1
```
Per eliminare la porta di stampa TCP/IP standard nel server \\\Server1 che si connette a una stampante di rete in 10.2.3.4, digitare:
```
cscript prnport -d -s Server1 -r IP_10.2.3.4
```
Per aggiungere una porta di stampa TCP/IP standard nel server \\\Server1 che si connette a una stampante di rete in 10.2.3.4 e utilizza il protocollo TCP RAW sulla porta 9100, digitare:
```
cscript prnport -a -s Server1 -r IP_10.2.3.4 -h 10.2.3.4 -o raw -n 9100
```
Per abilitare SNMP, specificare il nome comunità "public" e impostare l'indice SNMP su 1 in una stampante di rete in 10.2.3.4 condiviso dal server \\\Server1, digitare:
```
cscript prnport -t -s Server1 -r IP_10.2.3.4 -me -y public -i 1 -n 9100
```
Per aggiungere una porta di stampa TCP/IP standard nel computer locale che si connette a una stampante di rete in 10.2.3.4 e ottenere automaticamente le impostazioni del dispositivo dalla stampante, digitare:
```
cscript prnport -a -r IP_10.2.3.4 -h 10.2.3.4
```

#### <a name="additional-references"></a>riferimenti aggiuntivi
[Chiave della sintassi della riga di comando](command-line-syntax-key.md)
[riferimento al comando stampa](print-command-reference.md)
