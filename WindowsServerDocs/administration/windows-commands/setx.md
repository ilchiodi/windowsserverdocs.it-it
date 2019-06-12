---
title: setx
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ef37482f-f8a8-4765-951a-2518faac3f44
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0b2caceed6962bef22e7d546fa3b4469c9682b39
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441250"
---
# <a name="setx"></a>setx



Crea o modifica le variabili di ambiente nell'ambiente di sistema, di utente o senza la necessità di script o programmazione. Il **Setx** comando anche recupera i valori delle chiavi del Registro di sistema e li scrive nei file di testo.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
setx [/s <Computer> [/u [<Domain>\]<User name> [/p [<Password>]]]] <Variable> <Value> [/m]
setx [/s <Computer> [/u [<Domain>\]<User name> [/p [<Password>]]]] [<Variable>] /k <Path> [/m]
setx [/s <Computer> [/u [<Domain>\]<User name> [/p [<Password>]]]] /f <FileName> {[<Variable>] {/a <X>,<Y> | /r <X>,<Y> "<String>"} [/m] | /x} [/d <Delimiters>]
```

## <a name="parameters"></a>Parametri

|         Parametro          |                                                                                                                                              Descrizione                                                                                                                                              |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       /s \<computer >       |                                                                                  Specifica il nome o indirizzo IP di un computer remoto. Non utilizzare le barre rovesciate. Il valore predefinito è il nome del computer locale.                                                                                  |
| /u [\<Domain>\]<User name> |                                                                                           Esegue lo script con le credenziali dell'account utente specificato. Il valore predefinito è le autorizzazioni di sistema.                                                                                            |
|      /p [\<Password>]      |                                                                                                         Specifica la password dell'account utente specificato nella **/u** parametro.                                                                                                         |
|        \<Variable>         |                                                                                                                 Specifica il nome della variabile di ambiente che si desidera impostare.                                                                                                                  |
|          \<valore >          |                                                                                                                Specifica il valore a cui si desidera impostare la variabile di ambiente.                                                                                                                 |
|         /k \<Path>         | Specifica che la variabile è impostata in base alle informazioni da una chiave del Registro di sistema. Il p*ercorso dei* Usa la sintassi seguente:</br>`\\<HIVE>\<KEY>\...\<Value>`</br>Ad esempio, è possibile specificare il percorso seguente:</br>`HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName` |
|      /f \<nome file >       |                                                                                                                               Specifica il file che si desidera utilizzare.                                                                                                                                |
|        /a \<X>,<Y>         |                                                                                                                    Specifica le coordinate assolute e l'offset come parametri di ricerca.                                                                                                                    |
|   /r \<X>,<Y> "<String>"   |                                                                                                            Specifica le coordinate relative e l'offset dal **stringa** come i parametri di ricerca.                                                                                                            |
|             /m             |                                                                                                Specifica di impostare la variabile di ambiente di sistema. L'impostazione predefinita è l'ambiente locale.                                                                                                 |
|             /x             |                                                                                                       Consente di visualizzare file coordinate, ignorando il **/a**, **/r**, e **/d** opzioni della riga di comando.                                                                                                        |
|      /d \<Delimiters>      |                    Specifica i delimitatori, ad esempio " **,** "o" **\\** " da usare oltre i quattro delimitatori predefiniti, ovvero lo spazio, scheda, invio e avanzamento riga. Delimitatori validi includono qualsiasi carattere ASCII. Il numero massimo di delimitatori è 15, inclusi i delimitatori predefiniti.                    |
|             /?             |                                                                                                                                 Visualizza la guida al prompt dei comandi.                                                                                                                                  |

## <a name="remarks"></a>Note

-   Il **Setx** comando è simile all'utilità UNIX SETENV.
-   **Setx** consente solo dalla riga di comando o a livello di codice direttamente e in modo permanente impostare system i valori di ambiente. Variabili di ambiente di sistema sono configurate manualmente tramite **Pannello di controllo** o tramite un editor del Registro di sistema. Il **impostare** comando, che è interno all'interprete dei comandi (Cmd.exe), imposta le variabili di ambiente utente per la finestra della console corrente solo.
-   È possibile usare la **setx** comando per impostare i valori per utente e di sistema, variabili di ambiente da una delle tre origini (modalità): Modalità a riga di comando, la modalità del Registro di sistema o File modalità.
-   **Setx** scrive le variabili di ambiente nel Registro di sistema master. Le variabili impostate con **setx** le variabili sono disponibili comandi future solo in windows, non nella finestra di comando corrente.
-   **HKEY_CURRENT_USER** e **HKEY_LOCAL_MACHINE** sono gli unici hive supportati. REG_DWORD, REG_EXPAND_SZ, REG_SZ, REG_MULTI_SZ sono validi **RegKey** i tipi di dati.
-   Quando è possibile accedere a **REG_MULTI_SZ** valori nel Registro di sistema, solo il primo elemento viene estratto e usato.
-   Non è possibile usare la **setx** comando per rimuovere i valori che sono stati aggiunti per gli ambienti locali o di sistema. È possibile usare **impostare** con un nome di variabile e nessun valore per rimuovere un valore corrispondente da ambiente locale.
-   I valori del Registro di sistema REG_DWORD vengono estratti e utilizzati in modalità esadecimale.
-   Modalità file supporta l'analisi del ritorno a capo e avanzamento riga solo i file di testo (CRLF).

## <a name="BKMK_examples"></a>Esempi

Per impostare la variabile di ambiente computer nell'ambiente locale al valore Brand1, digitare:
```
setx MACHINE Brand1
```
Per impostare la variabile di ambiente computer nell'ambiente di sistema per il valore Brand1 Computer, digitare:
```
setx MACHINE "Brand1 Computer" /m
```
Per impostare la variabile di ambiente MYPATH nell'ambiente locale da usare il percorso di ricerca definito nella variabile di ambiente PATH, digitare:
```
setx MYPATH %PATH%
```
Per impostare la variabile di ambiente MYPATH nell'ambiente locale da utilizzare il percorso di ricerca definito nella variabile di ambiente PATH dopo aver sostituito **~** con **%** , tipo:
```
setx MYPATH ~PATH~ 
```
Per impostare la variabile di ambiente computer nell'ambiente locale a Brand1 in un computer remoto denominato Computer1, digitare:
```
setx /s computer1 /u maindom\hiropln /p p@ssW23 MACHINE Brand1
```
Per impostare la variabile di ambiente MYPATH nell'ambiente locale da usare il percorso di ricerca definito nella variabile di ambiente PATH in un computer remoto denominato Computer1, digitare:
```
setx /s computer1 /u maindom\hiropln /p p@ssW23 MYPATH %PATH%
```
Per impostare la variabile di ambiente TZONE nell'ambiente locale per il valore trovato nel **HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName** tipo di chiave, del Registro di sistema:
```
setx TZONE /k HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName 
```
Per impostare la variabile di ambiente TZONE nell'ambiente locale di un computer remoto denominato Computer1 per il valore trovato nel **HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName** Registro di sistema tipo di chiave:
```
setx /s computer1 /u maindom\hiropln /p p@ssW23 TZONE /k HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName 
```
Per impostare la variabile di ambiente di compilazione nell'ambiente di sistema per il valore trovato nel **HKEY_LOCAL_MACHINE\Software\Microsoft\WindowsNT\CurrentVersion\CurrentBuildNumber** tipo di chiave, del Registro di sistema:
```
setx BUILD /k "HKEY_LOCAL_MACHINE\Software\Microsoft\WindowsNT\CurrentVersion\CurrentBuildNumber" /m
```
Per impostare la variabile di ambiente di compilazione nell'ambiente di sistema di un computer remoto denominato Computer1 per il valore trovato nel **HKEY_LOCAL_MACHINE\Software\Microsoft\WindowsNT\CurrentVersion\CurrentBuildNumber** chiave del Registro di sistema digitare:
```
setx /s computer1 /u maindom\hiropln /p p@ssW23  BUILD /k "HKEY_LOCAL_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\CurrentBuildNumber" /m
```
Per visualizzare il contenuto di un file denominato ipconfig. out, insieme a coordinate corrispondente del contenuto, digitare:
```
setx /f ipconfig.out /x
```
Per impostare la variabile di ambiente IPADDR nell'ambiente locale per il valore della coordinata 5,11 trovato nel file ipconfig. out, digitare:
```
setx IPADDR /f ipconfig.out /a 5,11
```
Per impostare la variabile di ambiente ottetto 1 nell'ambiente locale per il valore della coordinata 5,3 trovato nel file ipconfig. out con delimitatori **"#$\*."** , tipo:
```
setx OCTET1 /f ipconfig.out /a 5,3 /d "#$*." 
```
Per impostare la variabile di ambiente IPGATEWAY nell'ambiente locale per il valore trovato nel file ipconfig. Out della coordinata 0,7 per quanto riguarda la coordinata della "Gateway", digitare:
```
setx IPGATEWAY /f ipconfig.out /r 0,7 Gateway 
```
Per visualizzare il contenuto di un file denominato ipconfig. out, ovvero con coordinate corrispondenti del contenuto, ovvero in un computer denominato Computer1, digitare:
```
setx /s computer1 /u maindom\hiropln /p p@ssW23 /f ipconfig.out /x 
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)