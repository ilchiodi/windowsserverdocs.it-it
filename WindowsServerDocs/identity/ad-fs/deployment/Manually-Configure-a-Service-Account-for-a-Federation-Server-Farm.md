---
ms.assetid: 5a1ae56b-adcb-447e-9e34-c0629d7cb241
title: Configurare manualmente un account del servizio per una server farm federativa
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 8240903b3c446d4f02ca93dc053e520480f5e8ca
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359493"
---
# <a name="manually-configure-a-service-account-for-a-federation-server-farm"></a>Configurare manualmente un account del servizio per una server farm federativa

Se si intende configurare un ambiente di server farm federativa in Active Directory Federation Services \(AD FS @ no__t-1, è necessario creare e configurare un account di servizio dedicato in Active Directory Domain Services \(AD DS @ no__t-3 in cui la farm si trova. In seguito si configurerà ogni server federativo della farm per l'uso di questo account. È necessario completare le attività seguenti nell'organizzazione quando si desidera consentire ai computer client nella rete aziendale di eseguire l'autenticazione a qualsiasi server federativo in una AD FS farm utilizzando l'autenticazione integrata di Windows.  

> [!IMPORTANT]
> A partire da AD FS 3,0 (Windows Server 2012 R2), AD FS supporta l'uso di un [account del servizio gestito del gruppo](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview) \(gMSA @ no__t-2 come account del servizio.  Si tratta dell'opzione consigliata, in quanto elimina la necessità di gestire la password dell'account del servizio nel tempo.  Questo documento illustra il caso alternativo di utilizzo di un account di servizio tradizionale, ad esempio nei domini che eseguono ancora un livello di funzionalità del dominio Windows Server 2008 R2 o versioni precedenti \(DFL @ no__t-1.

> [!NOTE]  
> Le operazioni di questa procedura devono essere effettuate solo una per volta per l'intera server farm federativa. Successivamente, quando si crea un server federativo utilizzando la configurazione guidata server federativo di AD FS, è necessario specificare lo stesso account nella pagina della procedura guidata dell' **account del servizio** in ogni server federativo della farm.  
  
#### <a name="create-a-dedicated-service-account"></a>Creare un account del servizio dedicato  
  
1.  Creare un account utente @ no__t-0service dedicato nella foresta Active Directory che si trova nell'organizzazione del provider di identità. Questo account è necessario per il funzionamento del protocollo di autenticazione Kerberos in uno scenario farm e per consentire l'autenticazione pass @ no__t-0through in ogni server federativo. Usare questo account solo a scopo di server farm della Federazione.  
  
2.  Modificare le proprietà dell'account utente e selezionare la casella di controllo **Nessuna scadenza password**. Questa azione assicura che la funzione dell'account del servizio non venga interrotta a causa dei requisiti di modifica della password del dominio.  
  
    > [!NOTE]  
    > L'uso dell'account del servizio di rete per questo account dedicato genererà errori casuali quando verrà tentato l'accesso tramite l'autenticazione integrata di Windows, perché i ticket Kerberos non verranno convalidati da un server a un altro.  
  
#### <a name="to-set-the-spn-of-the-service-account"></a>Per impostare il nome SPN dell'account del servizio  
  
1.  Poiché l'identità del pool di applicazioni per il AD FS AppPool è in esecuzione come account utente di dominio @ no__t-0service, è necessario configurare il nome dell'entità servizio \(SPN @ no__t-2 per l'account nel dominio con lo strumento Setspn. exe Command @ no__t-3line. Setspn. exe viene installato per impostazione predefinita nei computer che eseguono Windows Server 2008. Eseguire il comando seguente in un computer aggiunto allo stesso dominio in cui risiede l'account utente @ no__t-0service:  
  
    ```  
    setspn -a host/<server name> <service account>  
    ```  
  
    Ad esempio, in uno scenario in cui tutti i server federativi sono raggruppati in base al nome host Domain Name System \(DNS @ no__t-1 fs.fabrikam.com e il nome dell'account del servizio assegnato al AD FS AppPool è denominato adfs2farm, digitare il comando come indicato di seguito e quindi premere INVIO:  
  
    ```  
    setspn -a host/fs.fabrikam.com adfs2farm  
    ```  
  
    È necessario completare questa attività solo una volta per l'account.  
  
2.  Dopo aver modificato l'identità del AD FS AppPool nell'account del servizio, impostare gli elenchi di controllo di accesso \(ACLs @ no__t-1 nel database SQL Server per consentire l'accesso in lettura a questo nuovo account, in modo che il AD FS AppPool possa leggere i dati dei criteri.  
  

