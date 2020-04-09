---
title: pubprn
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0bc7f7e3-84e1-4359-b477-7b1a1a0bd639
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8696a372902f36f703670cf514bddf75cf4cc7e3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80837104"
---
# <a name="pubprn"></a>pubprn

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Pubblica una stampante in servizi di dominio Active Directory.

## <a name="syntax"></a>Sintassi
```
cscript pubprn {<ServerName> | <UNCprinterpath>} 
LDAP://CN=<Container>,DC=<Container>
```

### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|\<ServerName >|Specifica il nome del server Windows che ospita la stampante che si desidera pubblicare. Se non si specifica un computer, viene usato il computer locale.|
|\<UNCprinterpath >|Percorso Universal Naming Convention (UNC) della stampante condivisa che si desidera pubblicare.|
|LDAP://CN =<Container>, DC =<Container>|Specifica il percorso del contenitore in servizi di dominio Active Directory in cui si desidera pubblicare la stampante.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note
-   Il comando **pubprn** è uno script Visual Basic che si trova nella directory%windir%\system32\. printing_Admin_Scripts\\<language>. Per usare questo comando, al prompt dei comandi digitare **cscript** seguito dal percorso completo del file pubprn o passare alla cartella appropriata. Ad esempio,
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\pubprn
    ```
-   Se le informazioni fornite contengono spazi, racchiudere il testo tra virgolette, ad esempio `computer Name`.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi
Per pubblicare tutte le stampanti nel computer \\\Server1 nel contenitore di contenitori di dominio MyDomain.company.Com, digitare:
```
cscript Ppubprn Server1 LDAP://CN=MyContainer,DC=MyDomain,DC=company,DC=Com
```
Per pubblicare la stampante stampa laserprinter1 nel server di \\\Server1 nel contenitore di contenuti del dominio MyDomain.company.Com, digitare:
```
cscript Ppubprn \\Server1\Laserprinter1 LDAP://CN=MyContainer,DC=MyDomain,DC=company,DC=Com
```

## <a name="additional-references"></a>Altre informazioni di riferimento
- [Chiave della sintassi della riga di comando](command-line-syntax-key.md)
[riferimento al comando stampa](print-command-reference.md)
