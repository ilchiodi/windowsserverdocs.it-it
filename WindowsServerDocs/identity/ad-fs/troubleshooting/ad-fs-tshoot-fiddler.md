---
title: AD FS risoluzione dei problemi - Fiddler
description: Questo documento descrive che cos'è Fiddler e su come installare e configurare Fiddler per risolvere i problemi di attestazioni di AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/02/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 822300d0e4b6ae462a3c942e22530bbed5f93e86
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865632"
---
# <a name="ad-fs-troubleshooting---fiddler"></a>AD FS risoluzione dei problemi - Fiddler
Fiddler è uno strumento che può essere usato per acquisire il traffico web HTTP/HTTPS.  Questo strumento è utilizzabile per contribuire alla risoluzione il processo di rilascio di attestazioni.  Esaminando il traffico è possibile ottenere una migliore comprensione di in cui suddividere l'interazione.  Questo documento descrive come installare e configurare Fiddler per acquisire il traffico di AD FS.  Per una traccia fiddler dell'esempio mediante WS-Federation vedere [AD FS di risoluzione dei problemi - Fiddler - WS-Federation](ad-fs-tshoot-fiddler-ws-fed.md)

## <a name="download-and-install-fiddler"></a>Scaricare e installare Fiddler
È possibile scaricare Fiddler [qui](https://www.telerik.com/download/fiddler).  Dopo aver scaricato proseguire e installarlo.

## <a name="configure-fiddler-to-capture-ad-fs-traffic"></a>Configurare Fiddler per acquisire il traffico AD ADFS
Per acquisire il traffico AD ADFS, è necessario configurare Fiddler per decrittografare il traffico SSL. 

### <a name="configure-the-fiddler-ssl-certificate"></a>Configurare il certificato SSL di Fiddler
 Usare la procedura seguente per configurare Fiddler per decrittografare il traffico SSL.

1.  Aprire Fiddler
2.  Nella parte superiore, sotto **degli strumenti**, selezionare **Fiddler Options**.
3.  Fare clic sulla scheda HTTPS.
4.  Inserire un segno di spunta **il traffico HTTPS decrittografare** e selezionare **dai browser solo** dall'elenco a discesa.
5.  Inserire un segno di spunta **ignorare gli errori di certificato server**.
6.  Fare clic su **OK**.

![Fiddler](media/ad-fs-tshoot-fiddler/fiddler1.png)

## <a name="next-steps"></a>Passaggi successivi

- [Risoluzione dei problemi di AD FS](ad-fs-tshoot-overview.md)