---
title: setx
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: c206f36e2d0bc947329124b08fb797091e838bcd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384027"
---
# <a name="setx"></a>setx



Crea o modifica le variabili di ambiente nell'ambiente utente o di sistema, senza richiedere la programmazione o lo scripting. Il comando **Setx** recupera anche i valori delle chiavi del registro di sistema e li scrive in file di testo.

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
| /u [\<dominio >\]<User name> |                                                                                           Esegue lo script con le credenziali dell'account utente specificato. Il valore predefinito è le autorizzazioni di sistema.                                                                                            |
|      /p [\<> password]      |                                                                                                         Specifica la password dell'account utente specificato nella **/u** parametro.                                                                                                         |
|        \<variabile >         |                                                                                                                 Specifica il nome della variabile di ambiente che si desidera impostare.                                                                                                                  |
|          Valore \<>          |                                                                                                                Specifica il valore in cui si desidera impostare la variabile di ambiente.                                                                                                                 |
|         Percorso \</k >         | Specifica che la variabile viene impostata in base alle informazioni provenienti da una chiave del registro di sistema. Il p*ATH* usa la sintassi seguente:</br>`\\<HIVE>\<KEY>\...\<Value>`</br>Ad esempio, è possibile specificare il percorso seguente:</br>`HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName` |
|      /f \<nome file >       |                                                                                                                               Specifica il file che si desidera utilizzare.                                                                                                                                |
|        /a \<X >,<Y>         |                                                                                                                    Specifica le coordinate assolute e l'offset come parametri di ricerca.                                                                                                                    |
|   /r \<X >,<Y> "<String>"   |                                                                                                            Specifica le coordinate relative e l'offset dalla **stringa** come parametri di ricerca.                                                                                                            |
|             /m             |                                                                                                Specifica di impostare la variabile nell'ambiente di sistema. L'impostazione predefinita è l'ambiente locale.                                                                                                 |
|             /x             |                                                                                                       Visualizza le coordinate del file, ignorando le opzioni della riga di comando **/a**, **/r**e **/d** .                                                                                                        |
|      /d delimitatori \<>      |                    Specifica i delimitatori, ad esempio " **,** " o " **\\** ", da usare in aggiunta ai quattro delimitatori predefiniti, ovvero spazio, tabulazione, invio e avanzamento riga. I delimitatori validi includono qualsiasi carattere ASCII. Il numero massimo di delimitatori è 15, inclusi i delimitatori predefiniti.                    |
|             /?             |                                                                                                                                 Visualizza la guida al prompt dei comandi.                                                                                                                                  |

## <a name="remarks"></a>Osservazioni

-   Il comando **Setx** è simile all'utilità setenv di UNIX.
-   **Setx** fornisce l'unica riga di comando o a livello di codice per impostare direttamente e in modo permanente i valori dell'ambiente di sistema. Le variabili di ambiente di sistema sono configurabili manualmente tramite il **Pannello di controllo** o un editor del registro di sistema. Il comando **set** , che è interno all'interprete dei comandi (cmd. exe), imposta le variabili di ambiente utente solo per la finestra della console corrente.
-   È possibile usare il comando **Setx** per impostare i valori per le variabili di ambiente utente e di sistema da una delle tre origini (modalità), ovvero la modalità della riga di comando, la modalità del registro di sistema o la modalità file.
-   **Setx** scrive variabili nell'ambiente master nel registro di sistema. Le variabili impostate con le variabili **Setx** sono disponibili solo nelle finestre di comando future, non nella finestra di comando corrente.
-   **HKEY_CURRENT_USER** e **HKEY_LOCAL_MACHINE** sono gli unici hive supportati. REG_DWORD, REG_EXPAND_SZ, REG_SZ e REG_MULTI_SZ sono tipi di dati **chiave** validi.
-   Quando si ottiene l'accesso ai valori **REG_MULTI_SZ** nel registro di sistema, viene estratto e utilizzato solo il primo elemento.
-   Non è possibile usare il comando **Setx** per rimuovere i valori che sono stati aggiunti agli ambienti locali o di sistema. È possibile utilizzare **set** con un nome di variabile e nessun valore per rimuovere un valore corrispondente dall'ambiente locale.
-   REG_DWORD i valori del registro di sistema vengono estratti e utilizzati in modalità esadecimale.
-   La modalità file supporta solo l'analisi dei file di testo con ritorno a capo e avanzamento riga (CRLF).

## <a name="BKMK_examples"></a>Esempi

Per impostare la variabile di ambiente del computer nell'ambiente locale sul valore Brand1, digitare:
```
setx MACHINE Brand1
```
Per impostare la variabile di ambiente del computer nell'ambiente di sistema sul valore Brand1 Computer, digitare:
```
setx MACHINE "Brand1 Computer" /m
```
Per impostare la variabile di ambiente Path nell'ambiente locale in modo da usare il percorso di ricerca definito nella variabile di ambiente PATH, digitare:
```
setx MYPATH %PATH%
```
Per impostare la variabile di ambiente Path nell'ambiente locale in modo da usare il percorso di ricerca definito nella variabile di ambiente PATH dopo aver sostituito **~** con **%** , digitare:
```
setx MYPATH ~PATH~ 
```
Per impostare la variabile di ambiente del computer nell'ambiente locale su BRAND1 in un computer remoto denominato Computer1, digitare:
```
setx /s computer1 /u maindom\hiropln /p p@ssW23 MACHINE Brand1
```
Per impostare la variabile di ambiente PATH nell'ambiente locale in modo da utilizzare il percorso di ricerca definito nella variabile di ambiente PATH in un computer remoto denominato Computer1, digitare:
```
setx /s computer1 /u maindom\hiropln /p p@ssW23 MYPATH %PATH%
```
Per impostare la variabile di ambiente TZONE nell'ambiente locale sul valore trovato nella chiave del registro di sistema **HKEY_LOCAL_MACHINE \system\currentcontrolset\control\timezoneinformation\standardname** , digitare:
```
setx TZONE /k HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName 
```
Per impostare la variabile di ambiente TZONE nell'ambiente locale di un computer remoto denominato Computer1 sul valore trovato nella chiave del registro di sistema **HKEY_LOCAL_MACHINE \system\currentcontrolset\control\timezoneinformation\standardname** , digitare:
```
setx /s computer1 /u maindom\hiropln /p p@ssW23 TZONE /k HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName 
```
Per impostare la variabile di ambiente di compilazione nell'ambiente di sistema sul valore trovato nella chiave del registro di sistema **HKEY_LOCAL_MACHINE \software\microsoft\windowsnt\currentversion\currentbuildnumber** , digitare:
```
setx BUILD /k "HKEY_LOCAL_MACHINE\Software\Microsoft\WindowsNT\CurrentVersion\CurrentBuildNumber" /m
```
Per impostare la variabile di ambiente di compilazione nell'ambiente di sistema di un computer remoto denominato Computer1 sul valore trovato nella chiave del registro di sistema **\software\microsoft\windowsnt\currentversion\currentbuildnumber HKEY_LOCAL_MACHINE** , digitare:
```
setx /s computer1 /u maindom\hiropln /p p@ssW23  BUILD /k "HKEY_LOCAL_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\CurrentBuildNumber" /m
```
Per visualizzare il contenuto di un file denominato ipconfig. out, insieme alle coordinate corrispondenti del contenuto, digitare:
```
setx /f ipconfig.out /x
```
Per impostare la variabile di ambiente IPADDR nell'ambiente locale sul valore trovato in corrispondenza della coordinata 5, 11 nel file ipconfig. out, digitare:
```
setx IPADDR /f ipconfig.out /a 5,11
```
Per impostare la variabile di ambiente OCTET1 nell'ambiente locale sul valore trovato nella coordinata 5, 3 nel file ipconfig. out con Delimiters **"# $\*."** , digitare:
```
setx OCTET1 /f ipconfig.out /a 5,3 /d "#$*." 
```
Per impostare la variabile di ambiente IPGATEWAY nell'ambiente locale sul valore trovato nella coordinata 0, 7 rispetto alla coordinata di "gateway" nel file ipconfig. out, digitare:
```
setx IPGATEWAY /f ipconfig.out /r 0,7 Gateway 
```
Per visualizzare il contenuto di un file denominato ipconfig. out, insieme alle coordinate corrispondenti del contenuto, in un computer denominato Computer1, digitare:
```
setx /s computer1 /u maindom\hiropln /p p@ssW23 /f ipconfig.out /x 
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)