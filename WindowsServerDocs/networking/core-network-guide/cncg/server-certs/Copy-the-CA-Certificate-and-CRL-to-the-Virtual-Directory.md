---
title: Copiare il certificato CA e CRL per la Directory virtuale
description: Questo argomento fa parte della Guida di certificati del Server di distribuzione per le distribuzioni Wireless e cablate 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: a1b5fa23-9cb1-4c32-916f-2d75f48b42c7
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e37bfce7f8cf33fd7fcb5e6227d783c28bd29d35
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="copy-the-ca-certificate-and-crl-to-the-virtual-directory"></a>Copiare il certificato CA e CRL per la Directory virtuale

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questa procedura per copiare il certificato CA radice aziendale ed elenco certificati revocati dall'autorità di certificazione in una directory virtuale sul server Web e per verificare che Servizi certificati Active Directory sia configurato correttamente. Prima di eseguire i comandi seguenti, assicurarsi di sostituire i nomi di directory e server con quelle che sono appropriati per la distribuzione.  
  
Per eseguire questa procedura è necessario essere un membro del **Domain Admins**.  
  
#### <a name="to-copy-the-certificate-revocation-list-from-ca1-to-web1"></a>Per copiare l'elenco certificati revocati CA1 Web1  
  
1.  Nel CA1, eseguire Windows PowerShell come amministratore e quindi pubblicare il CRL con il comando seguente:  
  
    - Tipo `certutil -crl`, quindi premere INVIO.  
  
    - Per copiare il certificato della CA per la condivisione di file sul server Web, digitare `copy C:\Windows\system32\certsrv\certenroll\*.crt \\WEB1\pki`, quindi premere INVIO.  
    - Per copiare gli elenchi di revoca del certificato per la condivisione di file sul server Web, digitare `copy C:\Windows\system32\certsrv\certenroll\*.crl \\WEB1\pki`, quindi premere INVIO.  
  
2. Per riavviare Servizi certificati Active Directory, digitare `Restart-Service certsvc`, quindi premere INVIO.  
  
2.  Per verificare che siano configurati correttamente i percorsi di estensione CDP e AIA, digitare `pkiview.msc`, quindi premere INVIO. Il pkiview Enterprise PKI MMC apre.  
  
3.  Fare clic sul nome della CA. Ad esempio, se il nome della CA è corp-CA1-CA, fare clic su **corp-CA1-CA**. Nel riquadro dei dettagli, verificare che il **stato** valore per il **certificato della CA**, **1 percorso AIA**, e **1 percorso CDP** sono tutti **OK**.  
  
Nella figura seguente viene illustrato il riquadro risultati pkiview con lo stato OK per tutti gli elementi.  
  
! [adcs_pkiviewmedia/adcs_pkiview.png)  
  
> [!IMPORTANT]  
> Se **stato** per qualsiasi elemento non **OK**, eseguire le operazioni seguenti:  
> -   Apri la condivisione sul server Web per verificare che il certificato e la revoca del certificato elencati i file copiati nella condivisione. Se non sono stati copiati correttamente per la condivisione, modificare i comandi di copia con l'origine del file corretto e condivisione di destinazione ed eseguire nuovamente i comandi.  
> -   Verificare di avere immesso i percorsi corretti per CDP e AIA nella scheda estensioni CA, assicurarsi che non ci siano senza spazi o altri caratteri nelle posizioni fornite.  
> -   Verificare copiato il certificato CA e CRL nella posizione corretta nel server Web e che il percorso corrisponda al percorso fornito per i percorsi CDP e AIA nella CA.  
> -   Verificare che sia configurato correttamente le autorizzazioni per la cartella virtuale in cui sono archiviati il certificato CA e CRL.  
  


