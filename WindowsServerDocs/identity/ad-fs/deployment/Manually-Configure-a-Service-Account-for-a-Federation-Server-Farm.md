---
ms.assetid: 5a1ae56b-adcb-447e-9e34-c0629d7cb241
title: Configurare manualmente un account del servizio per una server farm federativa
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: b027bff4645203c44e228f11c651b767fa4502e0
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192063"
---
# <a name="manually-configure-a-service-account-for-a-federation-server-farm"></a>Configurare manualmente un account del servizio per una server farm federativa

Se si prevede di configurare un ambiente server farm federativa in Active Directory Federation Services \(ADFS\), è necessario creare e configurare un account di servizio dedicato in servizi di dominio Active Directory \(Active Directory Domain Services\) dove risiederà la farm. In seguito si configurerà ogni server federativo della farm per l'uso di questo account. Quando si desidera consentire ai computer client nella rete aziendale per l'autenticazione dei server federativi in una farm AD FS con l'autenticazione integrata di Windows, è necessario completare le seguenti attività all'interno dell'organizzazione.  

> [!IMPORTANT]
> A partire da AD FS 3.0 (Windows Server 2012 R2), AD FS supporta l'uso di un [Account del servizio gestito del gruppo](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview) \(gMSA\) come account del servizio.  Questa è l'opzione consigliata, come rimuove la necessità di gestire la password dell'account del servizio nel corso del tempo.  Questo documento illustra il caso alternativo dell'uso di un account del servizio tradizionali, ad esempio nei domini ancora in esecuzione un Windows Server 2008 R2 o precedente a livello funzionale di dominio \(DFL\).

> [!NOTE]  
> Le operazioni di questa procedura devono essere effettuate solo una per volta per l'intera server farm federativa. In un secondo momento, quando si crea un server federativo utilizzando Configurazione guidata Server federativo AD FS, è necessario specificare questo stesso account nel **Account del servizio** pagina della procedura guidata in ogni server federativo nella farm.  
  
#### <a name="create-a-dedicated-service-account"></a>Creare un account del servizio dedicato  
  
1.  Creare un utente dedicato\/account nella foresta Active Directory che si trova nell'organizzazione del provider di identità del servizio. Questo account è necessario per il protocollo di autenticazione Kerberos per lavorare in uno scenario di farm e consentire pass\-tramite l'autenticazione in ognuno dei server federativi. Usare questo account solo per gli scopi della server farm federativa.  
  
2.  Modificare le proprietà dell'account utente e selezionare la casella di controllo **Nessuna scadenza password**. Questa azione assicura che la funzione dell'account del servizio non venga interrotta a causa dei requisiti di modifica della password del dominio.  
  
    > [!NOTE]  
    > L'uso dell'account del servizio di rete per questo account dedicato genererà errori casuali quando verrà tentato l'accesso tramite l'autenticazione integrata di Windows, perché i ticket Kerberos non verranno convalidati da un server a un altro.  
  
#### <a name="to-set-the-spn-of-the-service-account"></a>Per impostare il nome SPN dell'account del servizio  
  
1.  Perché l'identità del pool di applicazioni per il pool di applicazioni di AD FS è in esecuzione come utente di dominio\/account del servizio, è necessario configurare il nome dell'entità servizio \(SPN\) per l'account nel dominio con il comando Setspn.exe\-strumento della riga. Setspn.exe viene installato per impostazione predefinita nei computer che eseguono Windows Server 2008. Eseguire il comando seguente in un computer appartenente allo stesso dominio in cui l'utente\/si trova l'account del servizio:  
  
    ```  
    setspn -a host/<server name> <service account>  
    ```  
  
    Ad esempio, in uno scenario in cui federazione tutti i server appartengono a un cluster in Domain Name System \(DNS\) nome host fs.fabrikam.com e il nome dell'account del servizio assegnato per il pool di applicazioni di AD FS è denominato adfs2farm, digitare il comando come segue e quindi premere INVIO:  
  
    ```  
    setspn -a host/fs.fabrikam.com adfs2farm  
    ```  
  
    È necessario completare questa attività solo una volta per l'account.  
  
2.  Dopo avere modificato l'identità dell'AppPool di AD FS per l'account del servizio, impostare gli elenchi di controllo di accesso \(ACL\) nel database di SQL Server per consentire l'accesso in lettura a questo nuovo account in modo che il pool di applicazioni di AD FS può leggere i dati dei criteri.  
  

