---
title: waitfor
description: Argomento dei comandi di Windows per Wait, che invia o attende un segnale in un sistema. Il valore di **aspetter** viene usato per sincronizzare i computer in una rete.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a48ef70d-4d28-4035-b6b0-7d7b46ac2157
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4542fc9d231b8150ab89e07e173d9671d6b7a3f3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80829934"
---
# <a name="waitfor"></a>waitfor



Invia o attende un segnale in un sistema. Il valore di **aspetter** viene usato per sincronizzare i computer in una rete.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
waitfor [/s <Computer> [/u [<Domain>\]<User> [/p [<Password>]]]] /si <SignalName>
waitfor [/t <Timeout>] <SignalName>
```

### <a name="parameters"></a>Parametri

|       Parametro       |                                                                                         Descrizione                                                                                          |
|-----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s \<computer >     | Specifica il nome o l'indirizzo IP di un computer remoto (non utilizzare barre rovesciate). Il valore predefinito è il computer locale. Questo parametro si applica a tutti i file e le cartelle specificati nel comando. |
| /u [\<dominio >\]<User> |                              Esegue lo script utilizzando le credenziali dell'account utente specificato. Per impostazione predefinita, il valore di **aspetter** usa le credenziali dell'utente corrente.                               |
|   /p [\<> password]    |                                                    Specifica la password dell'account utente specificato nella **/u** parametro.                                                     |
|          /si          |                                                                        Invia il segnale specificato attraverso la rete.                                                                        |
|     /t \<timeout >     |                                              Specifica il numero di secondi di attesa per un segnale. Per impostazione predefinita **, wait attende** per un periodo illimitato.                                               |
|     \<Signalname >     |                                                Specifica il **segnale che wait** attende o Invia. *Signalname* non distingue tra maiuscole e minuscole.                                                 |
|          /?           |                                                                             Visualizza la guida al prompt dei comandi.                                                                             |

## <a name="remarks"></a>Note

-   I nomi dei segnali non possono superare i 225 caratteri. I caratteri validi includono a-z, A-Z, 0-9 e il set di caratteri ASCII esteso (128-255).
-   Se non si usa **/s**, il segnale viene trasmesso a tutti i sistemi di un dominio. Se si usa **/s**, il segnale viene inviato solo al sistema specificato.
-   È possibile eseguire più istanze di **wait in un** singolo computer, ma ogni istanza di Wait **deve attendere** un segnale diverso. Solo un' **istanza di wait** può attendere un determinato segnale in un determinato computer.
-   È possibile attivare un segnale manualmente usando l'opzione della riga di comando **/si** .
-   Il servizio **aspetterà** viene eseguito solo in Windows XP e nei server che eseguono un sistema operativo windows Server 2003, ma può inviare segnali a qualsiasi computer che esegue un sistema operativo Windows.
-   I computer possono ricevere segnali solo se si trovano nello stesso dominio del computer che invia il segnale.
-   **Quando si** testano le compilazioni software, è possibile utilizzare il controllo. Il computer di compilazione, ad esempio, può inviare un segnale a diversi computer che eseguono **aspetter** dopo che la compilazione è stata completata correttamente. Al ricevimento del segnale, il file batch che include la funzione **aspetter** può indicare ai computer di avviare immediatamente l'installazione del software o l'esecuzione di test nella compilazione compilata.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per attendere la ricezione del segnale espresso\build007, digitare:
```
waitfor espresso\build007
```
Per impostazione predefinita **, wait attende** per un periodo illimitato per un segnale.

Per attendere 10 secondi per la ricezione del segnale espresso\compile007 prima del timeout, digitare:
```
waitfor /t 10 espresso\build007
```
Per attivare manualmente il segnale espresso\build007, digitare:
```
waitfor /si espresso\build007
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)