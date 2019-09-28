---
title: pubprn
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 844a13c1a650ebedcc0d5b4fbf65b9de671b2180
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372008"
---
# <a name="pubprn"></a>pubprn

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Pubblica una stampante in servizi di dominio Active Directory.

## <a name="syntax"></a>Sintassi
```
cscript pubprn {<ServerName> | <UNCprinterpath>} 
"LDAP://CN=<Container>,DC=<Container>"
```

## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|\<ServerName >|Specifica il nome del server Windows che ospita la stampante che si desidera pubblicare. Se non si specifica un computer, viene usato il computer locale.|
|\<UNCprinterpath >|Percorso Universal Naming Convention (UNC) della stampante condivisa che si desidera pubblicare.|
|"LDAP://CN = <Container>, DC = <Container>"|Specifica il percorso del contenitore in servizi di dominio Active Directory in cui si desidera pubblicare la stampante.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note
-   Il comando **pubprn** è uno script Visual Basic che si trova nella directory%WINdir%\System32\printing_Admin_Scripts @ no__t-1 @ no__t-2. Per usare questo comando, al prompt dei comandi digitare **cscript** seguito dal percorso completo del file pubprn o passare alla cartella appropriata. Esempio:
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\pubprn
    ```
-   Se le informazioni fornite contengono spazi, racchiudere il testo tra virgolette, ad esempio `"computer Name"`.

## <a name="BKMK_examples"></a>Esempi
Per pubblicare tutte le stampanti nel computer \\ \ server1 nel contenitore di contenitori del dominio MyDomain.company.Com, digitare:
```
cscript Ppubprn Server1 "LDAP://CN=MyContainer,DC=MyDomain,DC=company,DC=Com"
```
Per pubblicare la stampante stampa laserprinter1 nel server \\ \ server1 nel contenitore di contenitori del dominio MyDomain.company.Com, digitare:
```
cscript Ppubprn \\Server1\Laserprinter1 "LDAP://CN=MyContainer,DC=MyDomain,DC=company,DC=Com"
```

#### <a name="additional-references"></a>Riferimenti aggiuntivi
Informazioni di[riferimento sui comandi di stampa](print-command-reference.md) 
 per la sintassi della riga di [comando](command-line-syntax-key.md)
