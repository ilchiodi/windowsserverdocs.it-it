---
title: AD FS risoluzione dei problemi - endpoint ADFS
description: Questo documento descrive come risolvere i problemi degli endpoint ADFS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/03/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 13b830c0317341280bd87499e3abd8dcd1a33fc2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857562"
---
# <a name="ad-fs-troubleshooting---ad-fs-metadata-endpoints"></a>AD FS risoluzione dei problemi - endpoint dei metadati di AD FS
Gli endpoint forniscono accesso alle funzionalità di server di federazione di AD FS, ad esempio la pubblicazione di metadati della federazione.  Per verificare che il server AD FS sta rispondendo alle richieste web, è possibile controllare i diversi endpoint.


## <a name="federation-metadata-test"></a>Test dei metadati di federazione
Federazione passiva si riferisce a scenari in cui il browser viene reindirizzato alla pagina di accesso di AD FS.  Testando l'endpoint dei metadati è possibile determinare se il server AD FS sta rispondendo alle richieste web in questi scenari passivi.  Utilizzare la procedura seguente per testare l'endpoint.

1.  Usando un web browser, passare all'endpoint di metadati della federazione AD FS.  Per esempio:  https://sts.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml
2. Il file xml deve scaricare localmente nel computer.
3. Aprirlo e verificare che contenga informazioni simili per le informazioni seguenti: ![Passiva](media/ad-fs-tshoot-endpoints/meta2.png)

## <a name="ws-mex-test-active-test"></a>WS-MEX prova (attivo)
WS-MetaDataExchange è un protocollo di servizi web e fa parte della roadmap WS-Federation.  Usa un messaggio SOAP per richiedere i metadati.  Eseguendo il test dell'endpoint è possibile determinare se il server AD FS risponde alle richieste web per WS-MetaDataExchange.  Utilizzare la procedura seguente per testare l'endpoint.
1.  Usando un web browser, passare all'endpoint di metadati della federazione AD FS.  Per esempio:  https://sts.contoso.com/adfs/services/trust/mex
2. Il file xml dovrebbe essere visualizzato automaticamente nel browser.  Dovrebbe essere simile all'immagine seguente:

![Attivo](media/ad-fs-tshoot-endpoints/meta3.png)


## <a name="next-steps"></a>Passaggi successivi

- [Risoluzione dei problemi di AD FS](ad-fs-tshoot-overview.md)