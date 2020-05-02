---
title: pubprn
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0bc7f7e3-84e1-4359-b477-7b1a1a0bd639
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 17ca9e98ef9e3423521b03c5c21be4b3f1538b62
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722783"
---
# <a name="pubprn"></a>pubprn

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Pubblica una stampante in servizi di dominio Active Directory.

## <a name="syntax"></a>Sintassi
```
cscript pubprn {<ServerName> | <UNCprinterpath>} 
LDAP://CN=<Container>,DC=<Container>
```

### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|\<Nomeserver>|Specifica il nome del server Windows che ospita la stampante che si desidera pubblicare. Se non si specifica un computer, viene usato il computer locale.|
|\<> UNCprinterpath|Percorso Universal Naming Convention (UNC) della stampante condivisa che si desidera pubblicare.|
|LDAP://CN =<Container>, DC =<Container>|Specifica il percorso del contenitore in servizi di dominio Active Directory in cui si desidera pubblicare la stampante.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni
-   Il comando **pubprn** è uno script Visual Basic che si trova nella directory\\ <language> %windir%\system32\. printing_Admin_Scripts. Per usare questo comando, al prompt dei comandi digitare **cscript** seguito dal percorso completo del file pubprn o passare alla cartella appropriata. Ad esempio:
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\pubprn
    ```
-   Se le informazioni fornite contengono spazi, racchiudere il testo tra virgolette (ad esempio, `computer Name`).

## <a name="examples"></a>Esempi
Per pubblicare tutte le stampanti nel \\computer \Server1 nel contenitore di contenitori del dominio mydomain.Company.com, digitare:
```
cscript Ppubprn Server1 LDAP://CN=MyContainer,DC=MyDomain,DC=company,DC=Com
```
Per pubblicare la stampante stampa laserprinter1 nel server \\\Server1 nel contenitore di contenitori nel dominio mydomain.Company.com, digitare:
```
cscript Ppubprn \\Server1\Laserprinter1 LDAP://CN=MyContainer,DC=MyDomain,DC=company,DC=Com
```

## <a name="additional-references"></a>Riferimenti aggiuntivi
- [Informazioni di](command-line-syntax-key.md)
[riferimento sui comandi di stampa](print-command-reference.md) della sintassi della riga di comando
