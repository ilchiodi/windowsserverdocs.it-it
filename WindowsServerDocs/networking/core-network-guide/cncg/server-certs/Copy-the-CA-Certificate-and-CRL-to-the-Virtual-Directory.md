---
title: Copiare il certificato CA e CRL per la Directory virtuale
description: Questo argomento fa parte della Guida alla distribuzione di un Server dei certificati per le distribuzioni Wireless e cablate 802.1 X
manager: dougkim
ms.topic: article
ms.assetid: a1b5fa23-9cb1-4c32-916f-2d75f48b42c7
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.date: 07/19/2018
ms.openlocfilehash: 15cc807db805e1be0349ea51119663e515c7e551
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874032"
---
# <a name="copy-the-ca-certificate-and-crl-to-the-virtual-directory"></a>Copiare il certificato CA e CRL per la Directory virtuale

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questa procedura per copiare il certificato CA radice aziendale ed elenco certificati revocati dall'autorità di certificazione in una directory virtuale sul server Web e per verificare che Servizi certificati Active Directory sia configurato correttamente. Prima di eseguire i comandi seguenti, assicurarsi di sostituire i nomi di directory e server con quelle che sono appropriate per la distribuzione.  
  
Per eseguire questa procedura è necessario essere un membro di **Domain Admins**.  
  
#### <a name="to-copy-the-certificate-revocation-list-from-ca1-to-web1"></a>Copiare l'elenco certificati revocati CA1 WEB1  
  
1.  Nel CA1, eseguire Windows PowerShell come amministratore e quindi pubblicare il CRL con il comando seguente:  
  
    - Digitare `certutil -crl` e quindi premere INVIO.  

    - Per copiare gli elenchi di revoche di certificati per la condivisione di file sul server Web, digitare `copy C:\Windows\system32\certsrv\certenroll\*.crl \\WEB1\pki`, quindi premere INVIO.  
  
2.  Per verificare che siano configurati correttamente i percorsi di estensione CDP e AIA, digitare `pkiview.msc`, quindi premere INVIO. Verrà aperto il pkiview Enterprise PKI MMC.  
  
3.  Nel riquadro sinistro, fare clic sul nome della CA.<p>Ad esempio, se il nome della CA è corp-CA1-CA, fare clic su **corp-CA1-CA**. 

4. Nella colonna stato del riquadro dei risultati, verificare che vengano visualizzati i valori per gli elementi seguenti **OK**:

    - **Certificato CA**
    - **Percorso di AIA #1**
    - **Percorso CDP #1**   
  
  
> [!TIP]  
> Se **stato** per qualsiasi elemento non è **OK**, eseguire le operazioni seguenti:  
> -   Aprire la condivisione sul server Web per verificare che il certificato e la revoca del certificato elencati i file copiati nella condivisione. Se non sono stati copiati correttamente per la condivisione, modificare i comandi di copia con l'origine del file corretto e condivisione di destinazione ed eseguire nuovamente i comandi.  
> -   Verificare di avere immesso i percorsi corretti per CDP e AIA nella scheda estensioni CA. Assicurarsi che siano senza spazi o altri caratteri nelle posizioni fornite.  
> -   Verificare copiato il certificato CA e CRL nella posizione corretta nel server Web e che il percorso corrisponda al percorso fornito per i percorsi CDP e AIA nella CA.  
> -   Verificare che sia configurato correttamente le autorizzazioni per la cartella virtuale in cui sono archiviati il certificato CA e CRL.  
  


