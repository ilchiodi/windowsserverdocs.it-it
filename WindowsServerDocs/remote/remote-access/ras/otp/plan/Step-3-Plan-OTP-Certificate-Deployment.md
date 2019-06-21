---
title: Passaggio 3 piano di distribuzione del certificato OTP
description: Questo argomento fa parte della Guida alla distribuzione di accesso remoto con autenticazione OTP in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eca02eeb-d92d-463e-aae0-1f7038ba26fe
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4102fc4f7eacf0b407446717caa83e4e5f70ae57
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282372"
---
# <a name="step-3-plan-otp-certificate-deployment"></a>Passaggio 3 piano di distribuzione del certificato OTP

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Dopo la pianificazione del server RADIUS, è necessario pianificare i requisiti di autorità (CA) certificazione, tra cui l'autorità di certificazione che rilasceranno certificati One Time password (OTP), il modello di certificato OTP e il certificato dell'autorità di registrazione usato da remoto Server di accesso per accedere a tutte le richieste di certificato OTP nel client DirectAccess. Questi certificati vengono usati come indicato di seguito:  
  
1.  Il client DirectAccess richiede un certificato OTP, e il server di accesso remoto riceve la richiesta.  
  
2.  Il server di accesso remoto verifica le credenziali OTP e se sono validi, il server funge da un'autorità di registrazione e firma la richiesta di registrazione di certificato OTP tramite un certificato di firma di breve durato.  
  
3.  Il server di accesso remoto invia la richiesta di registrazione certificato firmato nuovamente al client DirectAccess  
  
4.  Il client registra il certificato OTP dalla CA che utilizza le richieste di registrazione certificato firmate dal server.  
  
5.  L'autorità di certificazione verifica le credenziali e la richiesta.  
  
|Attività|Descrizione|  
|----|--------|  
|[3.1 pianificare l'autorità di certificazione OTP](#bkmk_3_1_CA)|Pianificare l'autorità di certificazione (CA) da utilizzare per rilasciare certificati ai client DirectAccess per l'autenticazione OTP.|  
|[3.2 piano il modello di certificato OTP](#bkmk_3_2_OTP_Cert)|Pianificare il modello di certificato OTP.|
|[3.3 piano il certificato dell'autorità di registrazione](#bkmk_33RACert)|Pianificare il certificato dell'autorità di registrazione per accedere a tutte le richieste di certificato di autenticazione OTP.|

## <a name="bkmk_3_1_CA"></a>3.1 pianificare l'autorità di certificazione OTP  
Per distribuire DirectAccess usando l'autenticazione della password monouso (OTP), è necessaria un'autorità di certificazione interna emettere i certificati di autenticazione OTP ai computer client DirectAccess. A tale scopo, è possibile usare la stessa CA interna utilizzabili per emettere i certificati usati per l'autenticazione dei computer IPsec regolare.  
  
## <a name="bkmk_3_2_OTP_Cert"></a>3.2 piano il modello di certificato OTP  
Ogni client DirectAccess richiede un certificato di autenticazione OTP per ottenere l'accesso alla rete interna. È necessario configurare un modello nella CA interna per il certificato OTP. Quando si configura il modello di certificato OTP, tenere presente quanto segue:  
  
-   Tutti gli utenti che necessitano di eseguire l'autenticazione OTP è necessario leggere e autorizzazioni di registrazione per questo modello.  
  
-   Il nome del soggetto deve essere compilato da informazioni di Active Directory, per garantire che il nome del soggetto corrisponda al nome utente OTP e non il nome del server di accesso remoto che esegue la richiesta di certificato. Il nome del soggetto deve essere nel formato nome distinto completo e il nome alternativo del soggetto deve essere in formato UPN. Ciò garantisce che il certificato OTP registrato sia valido per l'autenticazione Smartcard Kerberos.  
  
-   Lo scopo previsto del certificato deve essere l'accesso con Smart Card  
  
-   Rilascio deve richiedere una firma autorizzata. La firma deve essere configurata con i criteri di applicazione OTP DirectAccess predefiniti impostati nel modello certificato di firma autorità di registrazione.  
  
-   Il periodo di validità deve essere impostato su un'ora.  
  
    > [!NOTE]  
    > In situazioni in cui il server di autorità di certificazione è un computer Windows Server 2003, quindi il modello deve essere configurato in un computer diverso. Ciò è dovuto al fatto che l'impostazione di **periodo di validità** nelle ore non è possibile quando si eseguono versioni di Windows precedenti a Vista/2008. Se il computer in uso per configurare il modello non dispone il ruolo del servizio di certificazione installato o è un computer client, è necessario installare lo snap-in modelli di certificato. Per altre informazioni sull'argomento fare clic su [qui](https://technet.microsoft.com/library/cc732445.aspx).  
  
-   Il periodo di rinnovo deve essere impostato su 0.  
  
-   (Facoltativo) I certificati e le richieste non devono essere memorizzate nel database CA.  
  
-   Il parametro di Enhanced Key Usage del certificato deve essere impostato correttamente, come indicato di seguito:  
  
    -   Per il modello di certificato firma DirectAccess registrazione usare la chiave 1.3.6.1.4.1.311.81.1.1.  
  
    -   Per il modello di certificato di autenticazione OTP usare chiave 1.3.6.1.4.1.311.20.2.2 della chiave.  
  
## <a name="bkmk_33RACert"></a>3.3 piano il certificato dell'autorità di registrazione  
Quando i client DirectAccess richiedono un certificato per OTP, il server di accesso remoto riceve la richiesta dal client. Il server di accesso remoto consente di firmare tutte le richieste di certificato OTP dai client che utilizzano il certificato dell'autorità di registrazione. L'autorità di certificazione emette certificati solo se la richiesta è firmata dal certificato dell'autorità di registrazione nel server di accesso remoto. Il certificato deve essere emesso da un'autorità di certificazione interna, il certificato non può essere auto-firmato. Non deve essere emesso dall'autorità di certificazione che ha emesso i certificati OTP, ma la CA che rilascia i certificati OTP deve considerare attendibile la CA che rilascia il certificato di firma dell'autorità di registrazione.  
  
## <a name="BKMK_Links"></a>Vedere anche  
  
-   [Passaggio 4: Pianificare OTP per il server di accesso remoto](Step-4-Plan-for-OTP-on-the-Remote-Access-Server.md)  
  


