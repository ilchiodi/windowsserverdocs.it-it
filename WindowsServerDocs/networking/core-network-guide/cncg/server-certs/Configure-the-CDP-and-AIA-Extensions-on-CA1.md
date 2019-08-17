---
title: Configurare le estensioni AIA e CDP su CA1
description: Questo argomento fa parte della Guida alla distribuzione di un Server dei certificati per le distribuzioni Wireless e cablate 802.1 X
manager: dougkim
ms.topic: article
ms.assetid: f77a3989-9f92-41ef-92a8-031651dd73a8
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.date: 07/26/2018
ms.openlocfilehash: 74d77347e0ca1dffbca4e2cf6f17b9faa25d55bf
ms.sourcegitcommit: 23a6e83b688119c9357262b6815c9402c2965472
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2019
ms.locfileid: "69560499"
---
# <a name="configure-the-cdp-and-aia-extensions-on-ca1"></a>Configurare le estensioni AIA e CDP su CA1

>Si applica a Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questa procedura per configurare il punto di distribuzione elenco di revoche di certificati (CRL) (CDP) e le impostazioni di accesso alle informazioni di autorità (AIA) su CA1.  
  
Per eseguire questa procedura, è necessario essere un membro del gruppo Domain Admins.  
  
#### <a name="to-configure-the-cdp-and-aia-extensions-on-ca1"></a>Per configurare le estensioni CDP e AIA su CA1  
  
1.  In Server Manager fare clic su **Strumenti** e quindi su **Autorità di certificazione**.  
  
2.  Nell'albero della console Autorità di certificazione, fare doppio clic su **corp-CA1-CA**, quindi fare clic su **proprietà**.  
  
    > [!NOTE]  
    > Se non si il nome computer CA1 e il nome di dominio è diverso da quello in questo esempio, il nome della CA è diverso. Il nome della CA è nel formato *dominio*-*NomeComputerCA*-CA.  
  
3.  Fare clic sulla scheda **Estensioni**. Assicurarsi che Selezionare l'estensione è impostato su **il punto di distribuzione CRL (CDP)** , e il **specificare i percorsi da cui gli utenti possono ottenere un elenco di revoche di certificati (CRL)** , eseguire le operazioni seguenti:  
  
    1.  Selezionare la voce `file://\\<ServerDNSName>\CertEnroll\<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`, quindi fare clic su **rimuovere**. In **Conferma rimozione**, fare clic su **Sì**.  
  
    2.  Selezionare la voce `http://<ServerDNSName>/CertEnroll/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`, quindi fare clic su **rimuovere**. In **Conferma rimozione**, fare clic su **Sì**.  
  
    3.  Selezionare la voce che inizia con il percorso `ldap:///CN=<CATruncatedName><CRLNameSuffix>,CN=<ServerShortName>`, quindi fare clic su **rimuovere**. In **Conferma rimozione**, fare clic su **Sì**.  
  
4.  In **specificare i percorsi da cui gli utenti possono ottenere un elenco di revoche di certificati (CRL)** , fare clic su **Aggiungi**. Il **Aggiungi percorso** verrà visualizzata la finestra di dialogo.  
  
5.  In **Aggiungi percorso**, in **percorso**, tipo `http://pki.corp.contoso.com/pki/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`, quindi fare clic su **OK**. Viene nuovamente visualizzata la finestra di dialogo Proprietà CA.  
  
6.  Nel **estensioni** Selezionare le caselle di controllo seguenti:  
  
    -   **Includi nei CRL. I client lo usano per trovare i percorsi Delta CRL**  
  
    -   **Includi nell'estensione CDP dei certificati emessi**  
  
7.  In **specificare i percorsi da cui gli utenti possono ottenere un elenco di revoche di certificati (CRL)** , fare clic su **Aggiungi**. Il **Aggiungi percorso** verrà visualizzata la finestra di dialogo.  
  
8.  In **Aggiungi percorso**, in **percorso**, tipo `file://\\pki.corp.contoso.com\pki\<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`, quindi fare clic su **OK**. Viene nuovamente visualizzata la finestra di dialogo Proprietà CA.  
  
9. Nel **estensioni** Selezionare le caselle di controllo seguenti:  
  
    -   **Pubblica CRL in questo percorso**  
  
    -   **Pubblica Delta CRL in questa posizione**  
  
10. Modifica **Selezionare estensione** per **accesso alle informazioni di autorità (AIA)** , e il **specificare i percorsi da cui gli utenti possono ottenere un elenco di revoche di certificati (CRL)** , eseguire le operazioni seguenti:  
  
    1.  Selezionare la voce che inizia con il percorso `ldap:///CN=<CATruncatedName>,CN=AIA,CN=Public Key Services`, quindi fare clic su **rimuovere**. In **Conferma rimozione**, fare clic su **Sì**.  
  
    2.  Selezionare la voce `http://<ServerDNSName>/CertEnroll/<ServerDNSName>_<CaName><CertificateName>.crt`, quindi fare clic su **rimuovere**. In **Conferma rimozione**, fare clic su **Sì**.  
  
    3.  Selezionare la voce `file://\\<ServerDNSName>\CertEnroll\<ServerDNSName><CaName><CertificateName>.crt`, quindi fare clic su **rimuovere**. In **Conferma rimozione**, fare clic su **Sì**.  
  
11. In **specificare i percorsi da cui gli utenti possono ottenere il certificato per questa CA**, fare clic su **Aggiungi**. Il **Aggiungi percorso** verrà visualizzata la finestra di dialogo.  
  
12. In **Aggiungi percorso**, in **percorso**, tipo `http://pki.corp.contoso.com/pki/<ServerDNSName>_<CaName><CertificateName>.crt`, quindi fare clic su **OK**. Viene nuovamente visualizzata la finestra di dialogo Proprietà CA.  
  
13. Nel **estensioni** selezionare **Includi in AIA dei certificati emessi**.  
  
14. Quando viene richiesto di riavviare Servizi certificati Active Directory, fare clic su **n**. Il servizio verrà riavviato in un secondo momento.  
  

