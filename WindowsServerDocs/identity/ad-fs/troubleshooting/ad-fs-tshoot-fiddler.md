---
title: Risoluzione dei problemi AD FS-Fiddler
description: Questo documento descrive il Fiddler e come installare e configurare Fiddler per risolvere i problemi relativi alle attestazioni AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/02/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: f2abf1a0b844e8e8799458f5237d7a80059880ff
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366153"
---
# <a name="ad-fs-troubleshooting---fiddler"></a>Risoluzione dei problemi AD FS-Fiddler
Fiddler è uno strumento che può essere utilizzato per acquisire il traffico Web HTTP/HTTPS.  Questo strumento può essere usato per semplificare la risoluzione del processo di rilascio delle attestazioni.  Esaminando il traffico è possibile ottenere una migliore comprensione del punto in cui si interrompe l'interazione.  In questo documento viene descritto come installare e configurare Fiddler per acquisire il traffico AD FS.  Per un esempio di traccia di Fiddler con WS-Federation, vedere [ad FS risoluzione dei problemi-Fiddler-WS-Federation](ad-fs-tshoot-fiddler-ws-fed.md)

## <a name="download-and-install-fiddler"></a>Scaricare e installare Fiddler
È possibile scaricare Fiddler [qui](https://www.telerik.com/download/fiddler).  Una volta scaricato, procedere e installarlo.

## <a name="configure-fiddler-to-capture-ad-fs-traffic"></a>Configurare Fiddler per l'acquisizione del traffico AD FS
Per acquisire AD FS il traffico, è necessario configurare Fiddler per decrittografare il traffico SSL. 

### <a name="configure-the-fiddler-ssl-certificate"></a>Configurare il certificato SSL di Fiddler
 Usare la procedura seguente per configurare Fiddler per decrittografare il traffico SSL.

1.  Apri Fiddler
2.  Nella parte superiore, in **strumenti**, selezionare **Opzioni Fiddler**.
3.  Fare clic sulla scheda HTTPS.
4.  Inserire un segno di spunta in **decrittografare il traffico HTTPS** e selezionare **dai browser solo** dall'elenco a discesa.
5.  Inserire un segno di spunta in **Ignora errori certificato server**.
6.  Fare clic su **OK**.

![Fiddler](media/ad-fs-tshoot-fiddler/fiddler1.png)

## <a name="next-steps"></a>Passaggi successivi

- [Risoluzione dei problemi relativi ad AD FS](ad-fs-tshoot-overview.md)