---
title: Copiare il certificato CA e CRL per la Directory virtuale
description: Questo argomento fa parte della Guida alla distribuzione di un Server dei certificati per le distribuzioni Wireless e cablate 802.1 X
manager: dougkim
ms.topic: article
ms.assetid: a1b5fa23-9cb1-4c32-916f-2d75f48b42c7
ms.prod: windows-server
ms.technology: networking
ms.author: lizross
author: eross-msft
ms.date: 07/19/2018
ms.openlocfilehash: 275bec5c950ea20c3a7d5a933648cf7e068164d1
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318350"
---
# <a name="copy-the-ca-certificate-and-crl-to-the-virtual-directory"></a>Copiare il certificato CA e CRL per la Directory virtuale

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questa procedura per copiare il certificato CA radice aziendale ed elenco certificati revocati dall'autorità di certificazione in una directory virtuale sul server Web e per verificare che Servizi certificati Active Directory sia configurato correttamente. Prima di eseguire i comandi seguenti, assicurarsi di sostituire i nomi di directory e server con quelle che sono appropriate per la distribuzione.  
  
Per eseguire questa procedura è necessario essere un membro di **Domain Admins**.  
  
#### <a name="to-copy-the-certificate-revocation-list-from-ca1-to-web1"></a>Copiare l'elenco certificati revocati CA1 WEB1  
  
1.  Nel CA1, eseguire Windows PowerShell come amministratore e quindi pubblicare il CRL con il comando seguente:  
  
    - Digitare `certutil -crl` e quindi premere INVIO.  

    - Per copiare il certificato CA1 nella condivisione file nel server Web, digitare `copy C:\Windows\system32\certsrv\certenroll\*.crt \\WEB1\pki`e quindi premere INVIO.  
    
    - Per copiare gli elenchi di revoche di certificati per la condivisione di file sul server Web, digitare `copy C:\Windows\system32\certsrv\certenroll\*.crl \\WEB1\pki`, quindi premere INVIO.  
  
2.  Per verificare che siano configurati correttamente i percorsi di estensione CDP e AIA, digitare `pkiview.msc`, quindi premere INVIO. Verrà aperto il pkiview Enterprise PKI MMC.  
  
3.  Nel riquadro sinistro fare clic sul nome della CA.<p>Ad esempio, se il nome della CA è corp-CA1-CA, fare clic su **corp-CA1-CA**. 

4. Nella colonna stato del riquadro risultati verificare che i valori per gli elementi seguenti indichino **OK**:

    - **Certificato CA**
    - **Posizione AIA #1**
    - **#1 percorso CDP**   
  
  
> [!TIP]  
> Se **stato** per qualsiasi elemento non è **OK**, eseguire le operazioni seguenti:  
> -   Aprire la condivisione sul server Web per verificare che il certificato e la revoca del certificato elencati i file copiati nella condivisione. Se non sono stati copiati correttamente per la condivisione, modificare i comandi di copia con l'origine del file corretto e condivisione di destinazione ed eseguire nuovamente i comandi.  
> -   Verificare di avere immesso i percorsi corretti per CDP e AIA nella scheda estensioni CA. Assicurarsi che non siano presenti spazi o altri caratteri aggiuntivi nelle posizioni specificate.  
> -   Verificare copiato il certificato CA e CRL nella posizione corretta nel server Web e che il percorso corrisponda al percorso fornito per i percorsi CDP e AIA nella CA.  
> -   Verificare che sia configurato correttamente le autorizzazioni per la cartella virtuale in cui sono archiviati il certificato CA e CRL.  
  


