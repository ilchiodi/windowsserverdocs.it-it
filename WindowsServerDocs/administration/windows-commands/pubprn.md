---
title: pubprn
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0bc7f7e3-84e1-4359-b477-7b1a1a0bd639
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 499ff2ade7ffc6c608791ba3da0ede0c3282c13d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831702"
---
# <a name="pubprn"></a>pubprn

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Pubblica una stampante servizi di dominio active directory.

## <a name="syntax"></a>Sintassi
```
cscript pubprn {<ServerName> | <UNCprinterpath>} 
"LDAP://CN=<Container>,DC=<Container>"
```

## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|\<ServerName>|Specifica il nome del server Windows che ospita la stampante che si desidera pubblicare. Se non si specifica un computer, viene utilizzato il computer locale.|
|\<UNCprinterpath>|Il percorso Universal Naming Convention (UNC) per la stampante condivisa a cui si desidera pubblicare.|
|"LDAP://CN=<Container>,DC=<Container>"|Specifica il percorso del contenitore in servizi di dominio active directory in cui si desidera pubblicare la stampante.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note
-   Il **pubprn** comando è uno script Visual Basic che si trova nel %WINdir%\System32\printing_Admin_Scripts\\ <language> directory. Per usare questo comando, un prompt dei comandi, digitare **cscript** aggiungendo il percorso completo del file pubprn, o cambiare le directory nella cartella appropriata. Ad esempio:
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\pubprn
    ```
-   Se le informazioni fornite contengono spazi, utilizzare le virgolette intorno al testo (ad esempio, `"computer Name"`).

## <a name="BKMK_examples"></a>Esempi
Per pubblicare tutte le stampanti nel \\\Server1 computer nel contenitore MyContainer nel dominio MyDomain.company.Com, digitare:
```
cscript Ppubprn Server1 "LDAP://CN=MyContainer,DC=MyDomain,DC=company,DC=Com"
```
Per pubblicare la stampante Laserprinter1 sul \\\Server1 server per il contenitore MyContainer nel dominio MyDomain.company.Com, tipo:
```
cscript Ppubprn \\Server1\Laserprinter1 "LDAP://CN=MyContainer,DC=MyDomain,DC=company,DC=Com"
```

#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave sintassi della riga di comando](command-line-syntax-key.md)
[riferimenti ai comandi di stampa](print-command-reference.md)
