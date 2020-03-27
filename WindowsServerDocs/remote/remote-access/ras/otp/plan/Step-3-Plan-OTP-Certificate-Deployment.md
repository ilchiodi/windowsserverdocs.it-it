---
title: Passaggio 3 pianificare la distribuzione di certificati OTP
description: Questo argomento fa parte della Guida distribuire accesso remoto con l'autenticazione OTP in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eca02eeb-d92d-463e-aae0-1f7038ba26fe
ms.author: lizross
author: eross-msft
ms.openlocfilehash: d34630b4faa8012eee73967a99bc0541f1305a09
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80313526"
---
# <a name="step-3-plan-otp-certificate-deployment"></a>Passaggio 3 pianificare la distribuzione di certificati OTP

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Dopo aver pianificato il server RADIUS, è necessario pianificare i requisiti dell'autorità di certificazione (CA), inclusa la CA che emetterà certificati monouso (OTP), il modello di certificato OTP e il certificato dell'autorità di registrazione usato da Remote Accedere al server per firmare tutte le richieste di certificati OTP del client DirectAccess. Questi certificati vengono usati come indicato di seguito:  
  
1.  Il client DirectAccess richiede un certificato OTP e il server di accesso remoto riceve la richiesta.  
  
2.  Il server di accesso remoto verifica le credenziali OTP e, se sono valide, il server funge da autorità di registrazione e firma la richiesta di registrazione del certificato OTP usando un certificato di firma di breve durata.  
  
3.  Il server di accesso remoto invia nuovamente la richiesta di registrazione del certificato firmato al client DirectAccess  
  
4.  Il client registra quindi il certificato OTP dalla CA usando le richieste di registrazione del certificato firmate dal server.  
  
5.  La CA verifica le credenziali e la richiesta.  
  
|Attività|Descrizione|  
|----|--------|  
|[3,1 pianificare la CA OTP](#bkmk_3_1_CA)|Pianificare l'autorità di certificazione (CA) da usare per emettere certificati ai client DirectAccess per l'autenticazione OTP.|  
|[3,2 pianificare il modello di certificato OTP](#bkmk_3_2_OTP_Cert)|Pianificare il modello di certificato OTP.|
|[3,3 pianificare il certificato dell'autorità di registrazione](#bkmk_33RACert)|Pianificare il certificato dell'autorità di registrazione per firmare tutte le richieste di certificati di autenticazione OTP.|

## <a name="31-plan-the-otp-ca"></a><a name="bkmk_3_1_CA"></a>3,1 pianificare la CA OTP  
Per distribuire DirectAccess usando l'autenticazione della password monouso, è necessaria una CA interna per emettere i certificati di autenticazione OTP nei computer client DirectAccess. A questo scopo, è possibile utilizzare la stessa CA interna utilizzata per emettere i certificati utilizzati per l'autenticazione del computer IPsec normale.  
  
## <a name="32-plan-the-otp-certificate-template"></a><a name="bkmk_3_2_OTP_Cert"></a>3,2 pianificare il modello di certificato OTP  
Ogni client DirectAccess richiede un certificato di autenticazione OTP per ottenere l'accesso alla rete interna. È necessario configurare un modello nella CA interna per il certificato OTP. Quando si configura il modello di certificato OTP, tenere presente quanto segue:  
  
-   Tutti gli utenti che devono eseguire l'autenticazione OTP devono disporre delle autorizzazioni di lettura e registrazione per questo modello.  
  
-   Il nome del soggetto deve essere compilato da Active Directory informazioni, per assicurarsi che il nome del soggetto corrisponda al nome utente OTP e non al nome del server di accesso remoto che esegue la richiesta di certificato. Il nome del soggetto deve essere nel formato del nome distinto completo e il nome alternativo del soggetto deve essere in formato UPN. Ciò garantisce che il certificato OTP registrato sia valido per l'autenticazione con smart card Kerberos.  
  
-   Lo scopo designato del certificato deve essere accesso con smart card  
  
-   Il rilascio deve richiedere una firma autorizzata. La firma deve essere configurata con i criteri predefiniti dell'applicazione OTP DirectAccess impostati nel modello di certificato per la firma dell'autorità di registrazione.  
  
-   Il periodo di validità deve essere impostato su un'ora.  
  
    > [!NOTE]  
    > In situazioni in cui il server CA è un computer Windows Server 2003, il modello deve essere configurato in un computer diverso. Ciò è dovuto al fatto che non è possibile impostare il **periodo di validità** in ore quando si eseguono versioni di Windows precedenti a 2008/Vista. Se nel computer utilizzato per configurare il modello non è installato il ruolo del servizio di certificazione oppure è un computer client, potrebbe essere necessario installare lo snap-in modelli di certificato. Per ulteriori informazioni su questo argomento, fare clic [qui](https://technet.microsoft.com/library/cc732445.aspx).  
  
-   Il periodo di rinnovo deve essere impostato su 0.  
  
-   Opzionale I certificati e le richieste non devono essere archiviati nel database CA.  
  
-   Il parametro utilizzo chiavi avanzato del certificato deve essere impostato correttamente, come indicato di seguito:  
  
    -   Per il modello di certificato di firma della registrazione DirectAccess usare la chiave 1.3.6.1.4.1.311.81.1.1.  
  
    -   Per il modello di certificato di autenticazione OTP usare la chiave 1.3.6.1.4.1.311.20.2.2 chiave.  
  
## <a name="33-plan-the-registration-authority-certificate"></a><a name="bkmk_33RACert"></a>3,3 pianificare il certificato dell'autorità di registrazione  
Quando i client DirectAccess richiedono un certificato OTP, il server di accesso remoto riceve la richiesta dal client. Il server di accesso remoto firma tutte le richieste di certificati OTP dai client usando il certificato dell'autorità di registrazione. La CA rilascia certificati solo se la richiesta è firmata dal certificato dell'autorità di registrazione nel server di accesso remoto. Il certificato deve essere emesso da una CA interna, il certificato non può essere autofirmato. Non deve essere emesso dalla CA che ha emesso i certificati OTP, ma la CA che rilascia i certificati OTP deve considerare attendibile la CA che rilascia il certificato di firma dell'autorità di registrazione.  
  
## <a name="see-also"></a><a name="BKMK_Links"></a>Vedere anche  
  
-   [Passaggio 4: pianificare OTP per il server di accesso remoto](Step-4-Plan-for-OTP-on-the-Remote-Access-Server.md)  
  


