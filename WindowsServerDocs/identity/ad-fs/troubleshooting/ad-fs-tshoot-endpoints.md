---
title: AD FS risoluzione dei problemi-AD FS endpoint
description: Questo documento descrive come risolvere i problemi relativi agli endpoint di AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/03/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 807b5c5de14bf6a43419d0b9d2d3a4e6953d0075
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366217"
---
# <a name="ad-fs-troubleshooting---ad-fs-metadata-endpoints"></a>AD FS risoluzione dei problemi-AD FS endpoint dei metadati
Gli endpoint forniscono accesso alla funzionalità server federativo di AD FS, ad esempio la pubblicazione di metadati federativi.  Per verificare che il server AD FS stia rispondendo alle richieste Web, è possibile controllare i vari endpoint.


## <a name="federation-metadata-test"></a>Test dei metadati federativi
La federazione passiva si riferisce agli scenari in cui il browser viene reindirizzato alla pagina di accesso AD FS.  Testando l'endpoint dei metadati è possibile determinare se il server AD FS sta rispondendo alle richieste Web in questi scenari passivi.  Utilizzare la procedura seguente per testare l'endpoint.

1.  Usando un Web browser, passare all'endpoint di metadati della Federazione AD FS.  Ad esempio: https://sts.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml
2. Il file XML dovrebbe essere scaricato localmente nel computer.
3. Aprirlo e verificare che contenga informazioni simili a infomration: ![Passive @ no__t-1

## <a name="ws-mex-test-active-test"></a>Test WS-MEX (test attivo)
WS-MetaDataExchange è un protocollo di servizi Web e fa parte della roadmap di WS-Federation.  Usa un messaggio SOAP per richiedere i metadati.  Testando l'endpoint, è possibile determinare se il server AD FS sta rispondendo alle richieste Web per WS-MetaDataExchange.  Utilizzare la procedura seguente per testare l'endpoint.
1.  Usando un Web browser, passare all'endpoint di metadati della Federazione AD FS.  Ad esempio: https://sts.contoso.com/adfs/services/trust/mex
2. Il file XML dovrebbe essere visualizzato automaticamente nel browser.  Dovrebbe essere simile all'immagine seguente:

![Attivo](media/ad-fs-tshoot-endpoints/meta3.png)


## <a name="next-steps"></a>Passaggi successivi

- [Risoluzione dei problemi relativi ad AD FS](ad-fs-tshoot-overview.md)