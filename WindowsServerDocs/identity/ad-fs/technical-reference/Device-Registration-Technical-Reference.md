---
ms.assetid: 69ec592a-5499-4249-8ba0-afa356a8ff75
title: Documentazione tecnica di registrazione dispositivo
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fac6437e9b6c3893064769a8279c2cf96cbc47d6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
>Si applica a: Windows Server 2016, Windows Server 2012 R2

# <a name="device-registration-technical-reference"></a>Documentazione tecnica di registrazione dispositivo
Il servizio Registrazione dispositivi \(DRS\) è un nuovo servizio di Windows fornita con il servizio ruolo Active Directory Federation in Windows Server 2012 R2.  Il servizio DRS deve essere installato e configurato in tutti i server federativi nella farm ADFS.  Per informazioni sulla distribuzione del servizio DRS, vedere [configurare un server federativo con Device Registration Service](https://technet.microsoft.com/library/dn486831.aspx).  
  
## <a name="active-directory-objects-created-when-a-device-is-registered"></a>Oggetti Active Directory creati quando un dispositivo viene registrato  
Gli oggetti di Active Directory seguenti vengono creati come parte del servizio Registrazione dispositivi.  
  
### <a name="device-registration-configuration"></a>Configurazione registrazione del dispositivo  
La configurazione Registrazione dispositivi viene archiviata nel contesto dei nomi di configurazione della foresta di Active Directory. \ (Ad esempio, **CN\ = dispositivo registrazione Configuration, CN\ = Services, < contesto-naming\-configurazione >**\). Questo oggetto viene creato quando la foresta di Active Directory viene inizializzata per la registrazione del dispositivo.  
  
La configurazione Registrazione dispositivi include gli elementi seguenti:  
  
-   **Chiavi dell'autorità di certificazione**  
  
    Le chiavi pubbliche e private usate per emettere il certificato x. 509 associato a un dispositivo registrato.  Le chiavi private sono protette con DKM.  
  
-   **Configurazione del servizio Registrazione dispositivi**  
  
    Criteri relativi al servizio Registrazione dispositivi.  
  
### <a name="registered-devices-container"></a>Contenitore di dispositivi registrati  
Il contenitore di oggetti dispositivo viene creato in uno dei domini nella foresta Active Directory.  Questo contenitore di oggetti conterrà tutti gli oggetti dispositivo per la foresta di Active Directory.  
  
Per impostazione predefinita, il contenitore viene creato nello stesso dominio AD FS.  \ (Ad esempio, **CN\ = RegisteredDevices, DC\ = < contesto-naming\-default\ >**\). Questo oggetto viene creato quando la foresta di Active Directory viene inizializzata per la registrazione del dispositivo.  
  
### <a name="registered-devices"></a>Dispositivi registrati  
Gli oggetti dispositivo sono nuovi leggeri in Active Directory.  Vengono utilizzati per rappresentare la relazione tra: un utente, un dispositivo e la società.  Gli oggetti dispositivo usano un certificato firmato da AD FS per associare il dispositivo fisico all'oggetto dispositivo logico in Active Directory.  
  
I dispositivi registrati include gli elementi seguenti:  
  
-   **Nome visualizzato**  
  
    Nome descrittivo del dispositivo.  Per i dispositivi windows, questo è il nome host del computer.  
  
-   **Id del dispositivo**  
  
    GUID generato dal server di registrazione del dispositivo.  
  
-   **Identificazione personale del certificato**  
  
    L'identificazione personale del certificato x. 509 che viene utilizzato con il dispositivo registrato.  
  
-   **Tipo di sistema operativo**  
  
    Il tipo di sistema operativo nel dispositivo.  
  
-   **Versione del sistema operativo**  
  
    La versione del sistema operativo nel dispositivo.  
  
-   **È abilitata**  
  
    Un valore booleano che indica se il dispositivo è abilitato in Active Directory.  Solo ai dispositivi abilitati sono consentiti l'accesso ai servizi.  
  
-   **Ora approssimativa ultimo utilizzo**  
  
    Ora approssimativa in cui che il dispositivo è stato usato per accedere a una risorsa.  Per limitare il traffico di replica, viene aggiornato solo ogni 14 giorni.  
  
-   **Proprietario registrato**  
  
    \(SID\) identità di sicurezza dell'utente che ha aggiunto questo dispositivo all'area di lavoro.  
  
## <a name="ad-fsdrs-server-ssl-certificate-revocation-checking"></a>Verifica delle revoche dei certificati SSL del Server DRS/FS\ di Active Directory  
Il client aggiunta all'area di lavoro controlla la validità del certificato SSL del Server AD FS.  Se il certificato SSL del Server AD FS include un elenco di revoche di certificati \(CRL\) endpoint, il client deve essere in grado di raggiungere l'endpoint specificato per convalidare il certificato.  
  
Se si utilizza un ambiente di testing e un'autorità di certificazione test \(CA\) per rilasciare certificati SSL del server è possibile scegliere di non includere l'endpoint CRL nei certificati server rilasciati dalla CA.  In questo modo si consentirà il client aggiunta all'area di lavoro ignorare la verifica CRL.  
  
> [!CAUTION]  
> Questo non è mai consigliato per i sistemi di produzione  
  

