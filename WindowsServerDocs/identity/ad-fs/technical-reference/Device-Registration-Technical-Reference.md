---
ms.assetid: 69ec592a-5499-4249-8ba0-afa356a8ff75
title: Guida di riferimento tecnico per la registrazione di dispositivi
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: ab78a5847c52650f2a608dfc89e2001cc43153ff
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407351"
---
# <a name="device-registration-technical-reference"></a>Guida di riferimento tecnico per la registrazione di dispositivi
Il servizio registrazione dispositivo \(DRS\) è un nuovo servizio Windows incluso nel ruolo di Servizio federativo Active Directory in Windows Server 2012 R2.  Il servizio DRS deve essere installato e configurato in tutti i server federativi della farm AD FS.  Per informazioni sulla distribuzione del servizio DRS, vedere [Configurare un server federativo con Device Registration Service](https://technet.microsoft.com/library/dn486831.aspx).  
  
## <a name="active-directory-objects-created-when-a-device-is-registered"></a>Oggetti Active Directory creati quando viene registrato un dispositivo  
Gli oggetti Active Directory seguenti vengono creati nell'ambito del servizio registrazione dispositivi.  
  
### <a name="device-registration-configuration"></a>Configurazione registrazione dispositivi  
La configurazione registrazione dispositivi viene archiviata nel contesto dei nomi di configurazione della foresta Active Directory. \(ad esempio, **cn\=Device Registration Configuration, cn\=Services, < configuration\-naming\-context** >\). Questo oggetto viene creato quando la foresta Active Directory viene inizializzata per la registrazione dispositivi.  
  
La configurazione registrazione dispositivi include gli elementi seguenti:  
  
-   **Chiavi dell'autorità di certificazione**  
  
    Le chiavi pubbliche e private usate per rilasciare il certificato X.509 associato a un dispositivo registrato.  Le chiavi private sono protette con DKM.  
  
-   **Configurazione del servizio Registrazione dispositivi**  
  
    Criteri relativi al servizio registrazione dispositivi.  
  
### <a name="registered-devices-container"></a>Contenitore di dispositivi registrati  
Il contenitore di dispositivi registrati viene creato in uno dei domini della foresta Active Directory.  Questo contenitore di oggetti conterrà tutti gli oggetti dispositivo per la foresta Active Directory.  
  
Per impostazione predefinita, il contenitore viene creato nello stesso dominio di AD FS.  \(ad esempio, **CN\=RegisteredDevices, DC\=< default\-Naming\-context** >\). Questo oggetto viene creato quando la foresta Active Directory viene inizializzata per la registrazione del dispositivo.  
  
### <a name="registered-devices"></a>Dispositivi registrati  
Gli oggetti dispositivo sono nuovi oggetti leggeri in Active Directory.  Vengono usati per rappresentare la relazione tra un utente, un dispositivo e la società.  Gli oggetti dispositivo usano un certificato firmato da AD FS per associare il dispositivo fisico all'oggetto dispositivo logico in Active Directory.  
  
I dispositivi registrati includono gli elementi seguenti:  
  
-   **Nome visualizzato**  
  
    Nome descrittivo del dispositivo.  Per i dispositivi Windows, è il nome host del computer.  
  
-   **ID dispositivo**  
  
    GUID generato dal server di registrazione dispositivi.  
  
-   **Identificazione personale del certificato**  
  
    Identificazione personale del certificato X.509 usato con il dispositivo registrato.  
  
-   **Tipo di sistema operativo**  
  
    Tipo di sistema operativo del dispositivo.  
  
-   **Versione del sistema operativo**  
  
    Versione del sistema operativo del dispositivo.  
  
-   **Abilitato**  
  
    Valore booleano che indica se il dispositivo è abilitato in Active Directory.  Solo ai dispositivi abilitati è consentito accedere ai servizi.  
  
-   **Ora ultimo utilizzo approssimativo**  
  
    Ora approssimativa in cui il dispositivo è stato usato per accedere a una risorsa.  Per limitare il traffico di replica, viene aggiornato solo ogni 14 giorni.  
  
-   **Proprietario registrato**  
  
    Identità di sicurezza \(SID\) dell'utente che ha aggiunto il dispositivo all'area di lavoro.  
  
## <a name="ad-fsdrs-server-ssl-certificate-revocation-checking"></a>Verifica della revoca del certificato SSL del server AD FS\/DRS  
Il client Aggiunta all'area di lavoro controlla la validità del certificato SSL del server AD FS.  Se il certificato SSL del server AD FS include un elenco di revoche di certificati \(endpoint\) CRL, il client deve essere in grado di raggiungere l'endpoint specificato per convalidare il certificato.  
  
Se si usa un ambiente di testing e un'autorità di certificazione di test \(CA\) per emettere i certificati SSL del server, è possibile scegliere di non includere l'endpoint CRL nei certificati del server rilasciati dalla CA.  In questo modo il client Aggiunta all'area di lavoro potrà ignorare la verifica CRL.  
  
> [!CAUTION]  
> Questo non è mai consigliato per i sistemi di produzione  
  

