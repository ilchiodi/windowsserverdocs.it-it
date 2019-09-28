---
title: Risoluzione dei problemi AD FS-rilascio di attestazioni
description: In questo documento viene descritto come risolvere i problemi di emissione di token con AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/03/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: ea0e6112f00f9cace6a0c580661a5319b5adaea5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366236"
---
# <a name="ad-fs-troubleshooting---claims-issuance"></a>Risoluzione dei problemi AD FS-rilascio di attestazioni
Un'attestazione è un'istruzione che un soggetto crea su se stesso o su un altro soggetto.  Le attestazioni vengono rilasciate da un relying party, a cui vengono assegnati uno o più valori e quindi inseriti in un pacchetto nei token di sicurezza emessi dal server di AD FS.  Poiché in questo processo sono presenti diverse parti mobili, il rilascio di attestazioni può essere suddiviso in queste parti principali.

>[!NOTE]  
>È possibile utilizzare [ClaimsXRay](https://adfshelp.microsoft.com/ClaimsXray/TokenRequest) nel sito della [Guida di ADFS](https://adfshelp.microsoft.com) per facilitare la risoluzione dei problemi relativi alle attestazioni.   

## <a name="token-request"></a>Richiesta di token
Quando si passa a un relying party viene reindirizzato all'AD FS con una richiesta di token.  I problemi possono verificarsi con la richiesta.  In particolare:

### <a name="the-request-formatting-with-3rd-parties-particularly-saml"></a>Formattazione della richiesta con terze parti (in particolare SAML)

### <a name="pre-formated-urls-that-have-typos"></a>URL pre-formattati con errori di digitazione
Quando si emette un token dalla relying party WS-Federaion, la richiesta di token viene fornita con i parametri della stringa di query dell'URL.  Se il relying party non specifica i parametri corretti in tale URL quando esegue il reindirizzamento a AD FS, potrebbe verificarsi un problema con la richiesta.


Per verificare il formato del token, è possibile usare uno strumento del debugger Web


## <a name="token-response"></a>Risposta token

## <a name="authentication"></a>Authentication

## <a name="claim-rule-processing"></a>Elaborazione delle regole attestazioni