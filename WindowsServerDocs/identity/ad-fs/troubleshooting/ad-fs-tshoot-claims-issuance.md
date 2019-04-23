---
title: Risoluzione dei problemi di AD FS - rilascio di attestazioni
description: Questo documento descrive come risolvere i problemi di rilascio dei token di AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/03/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fdf8851fe9b35f82191458ba3313fda2dc3ee4cf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839662"
---
# <a name="ad-fs-troubleshooting---claims-issuance"></a>Risoluzione dei problemi di AD FS - rilascio di attestazioni
Un'attestazione è un'istruzione che un soggetto compie relativa a se stesso o un altro soggetto.  Le attestazioni vengono generate da una relying party e vengono assegnati uno o più valori e quindi inserite nei token di sicurezza emessi dal server AD FS.  Poiché sono presenti molti elementi in questo processo, rilascio delle attestazioni può essere suddivisa nelle parti seguenti chiavi.

>[!NOTE]  
>È possibile usare [ClaimsXRay](https://adfshelp.microsoft.com/ClaimsXray/TokenRequest) nel [Guida di ad FS](https://adfshelp.microsoft.com) sito per facilitare la risoluzione dei problemi di attestazioni.   

## <a name="token-request"></a>Richiesta di token
Quando si passa a una relying party si verrà reindirizzati ad AD FS con una richiesta di token.  Possono verificarsi problemi con la richiesta.  In particolare:

### <a name="the-request-formatting-with-3rd-parties-particularly-saml"></a>La richiesta di formattazione con parti 3rd (in particolare SAML)

### <a name="pre-formated-urls-that-have-typos"></a>Pre-formato URL con errori di digitazione
Quando si emette un token dalla relying party WS-Federaion tale richiesta di token include parametri di stringa di query dell'URL.  Se la relying party non specificare i parametri corretti in quell'URL quando esegue il reindirizzamento ad AD FS potrebbe causare un problema con la richiesta.


Per poter verificare il formato del token, è possibile utilizzare uno strumento di debugger web


## <a name="token-response"></a>Risposta del token

## <a name="authentication"></a>Autenticazione

## <a name="claim-rule-processing"></a>Elaborazione delle regole attestazione