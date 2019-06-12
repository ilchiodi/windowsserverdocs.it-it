---
title: prnport
description: Informazioni su come creare, eliminare ed elencare le porte della stampante.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 5a7c48dcfa4db548d27722ee316eda797fd1ef2f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442136"
---
# <a name="prnport"></a>prnport

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea, elimina ed elenca le porte della stampante TCP/IP standard, oltre a visualizzare e modificare la configurazione della porta.

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
|              -l              |                                                                                                                                                                                                                                                             Elenca tutte le porte della stampante TCP/IP standard nel computer specificato con il **-s** parametro.                                                                                                                                                                                                                                                             |
|              -g              |                                                                                                                                                                                                                                                                            Visualizza la configurazione di una porta stampante TCP/IP standard.                                                                                                                                                                                                                                                                             |
|              -t              |                                                                                                                                                                                                                                                                           Consente di configurare le impostazioni della porta per una porta stampante TCP/IP standard.                                                                                                                                                                                                                                                                           |
|        -r \<PortName >        |                                                                                                                                                                                                                                                                                Specifica la porta a cui è connesso alla stampante.                                                                                                                                                                                                                                                                                 |
|       -s \<nomeserver >       |                                                                                                                                                                                                                               Specifica il nome del computer remoto che ospita la stampante che si desidera gestire. Se non si specifica un computer, viene utilizzato il computer locale.                                                                                                                                                                                                                                |
| -u \<nomeutente > -w <Password> |                                                                                                              Specifica un account con autorizzazioni sufficienti per connettersi al computer che ospita la stampante che si desidera gestire. Tutti i membri del gruppo Administrators locale del computer di destinazione dispongono di queste autorizzazioni, ma è anche possibile concedere le autorizzazioni ad altri utenti. Se non si specifica un account, è necessario essere connessi con un account con queste autorizzazioni per il comando lavorare.                                                                                                               |
|     -o {raw &#124; lpr}      |                                                                                                                                                                                                              Specifica quale protocollo utilizzato dalla porta: TCP non elaborati o TCP lpr. Se si usa TCP non elaborati, è possibile specificare facoltativamente il numero di porta usando il **- n** parametro. Il numero di porta predefinito è 9100.                                                                                                                                                                                                              |
|      -h \<Hostaddress>       |                                                                                                                                                                                                                                                                   Specifica (indirizzo IP) della stampante per il quale si desidera configurare la porta.                                                                                                                                                                                                                                                                    |
|       -q \<QueueName>        |                                                                                                                                                                                                                                                                                     Specifica il nome della coda per la porta TCP non elaborato.                                                                                                                                                                                                                                                                                     |
|       -n \<PortNumber >       |                                                                                                                                                                                                                                                                    Specifica il numero di porta per la porta TCP non elaborato. Il numero di porta predefinito è 9100.                                                                                                                                                                                                                                                                    |
|        -m{e &#124; d}        |                                                                                                                                                                                                                                                       Specifica se è abilitato SNMP. Il parametro **elettronica** Abilita il protocollo SNMP. Il parametro **1!d** disabilita il protocollo SNMP.                                                                                                                                                                                                                                                        |
|        -i \<SNMPIndex        |                                                                                                                                                                                                                             Specifica l'indice SNMP, se è abilitato SNMP. Per altre informazioni, vedere il documento Rfc 1759 nel [sito di Web Rfc editor](https://go.microsoft.com/fwlink/?LinkId=569).                                                                                                                                                                                                                              |
|     -y \<CommunityName>      |                                                                                                                                                                                                                                                                                Specifica il nome comunità SNMP, se è abilitato SNMP.                                                                                                                                                                                                                                                                                |
|       -2{e &#124; -d}        | Specifica se ne effettua lo spooling double (noto anche come respooling) è abilitato per le porte lpr TCP. Spool doppie sono necessari perché lpr TCP deve includere un conteggio accurato dei byte nel file di controllo che viene inviato alla stampante, ma il protocollo non è possibile ottenere il conteggio dal provider di stampa locale. Pertanto, quando viene eseguito lo spooling di un file a una TCP coda di stampa lpr, viene anche inserito come un file temporaneo nella directory system32. TCP lpr determina le dimensioni del file temporaneo e l'invia al server che esegue LPD. Il parametro **elettronica** consente doppi spool. Il parametro **1!d** Disabilita doppi spool. |
|              /?              |                                                                                                                                                                                                                                                                                         Visualizza la guida al prompt dei comandi.                                                                                                                                                                                                                                                                                         |

## <a name="remarks"></a>Note
-   Il **prnport** comando è uno script Visual Basic che si trova nel %WINdir%\System32\printing_Admin_Scripts\\ <language> directory. Per usare questo comando, un prompt dei comandi, digitare **cscript** aggiungendo il percorso completo del file prnport, o cambiare le directory nella cartella appropriata. Ad esempio:
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnport
    ```
-   Se le informazioni fornite contengono spazi, utilizzare le virgolette intorno al testo (ad esempio, `"computer Name"`).
-   Protocollo TCP non elaborato è un protocollo più alto di prestazioni in Windows più il protocollo lpr.

## <a name="BKMK_examples"></a>Esempi
Per visualizzare tutte le porte di stampa TCP/IP standard nel server \\\Server1, digitare:
```
cscript prnport -l -s Server1
```
Per eliminare la porta di stampa TCP/IP standard nel server \\\Server1 che si connette a una stampante di rete in 10.2.3.4, tipo:
```
cscript prnport -d -s Server1 -r IP_10.2.3.4
```
Per aggiungere una porta di stampa TCP/IP standard nel server \\\Server1 che si connette a una stampante di rete in 10.2.3.4 e utilizza il protocollo di raw TCP sulla porta 9100, tipo:
```
cscript prnport -a -s Server1 -r IP_10.2.3.4 -h 10.2.3.4 -o raw -n 9100
```
Per abilitare SNMP, specificare il nome community "pubblico" e impostare l'indice SNMP a 1 su una stampante di rete in 10.2.3.4 condiviso dal server \\\Server1, digitare:
```
cscript prnport -t -s Server1 -r IP_10.2.3.4 -me -y public -i 1 -n 9100
```
Per aggiungere una porta di stampa TCP/IP standard nel computer locale che si connette a una stampante di rete in 10.2.3.4 e ottengono automaticamente le impostazioni del dispositivo dalla stampante, digitare:
```
cscript prnport -a -r IP_10.2.3.4 -h 10.2.3.4
```

#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave sintassi della riga di comando](command-line-syntax-key.md)
[riferimenti ai comandi di stampa](print-command-reference.md)
