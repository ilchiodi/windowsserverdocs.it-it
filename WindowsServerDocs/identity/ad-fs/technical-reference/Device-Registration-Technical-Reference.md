---
ms.assetid: 69ec592a-5499-4249-8ba0-afa356a8ff75
title: Guida di riferimento tecnico per la registrazione di dispositivi
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fac6437e9b6c3893064769a8279c2cf96cbc47d6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833782"
---
>Si applica a: Windows Server 2016, Windows Server 2012 R2

# <a name="device-registration-technical-reference"></a>Guida di riferimento tecnico per la registrazione di dispositivi
Il servizio registrazione dispositivo \(DRS\) è un nuovo servizio Windows che è incluso con il servizio ruolo Active Directory Federation in Windows Server 2012 R2.  Il servizio DRS deve essere installato e configurato in tutti i server federativi della farm AD FS.  Per informazioni sulla distribuzione del servizio DRS, vedere [Configurare un server federativo con Device Registration Service](https://technet.microsoft.com/library/dn486831.aspx).  
  
## <a name="active-directory-objects-created-when-a-device-is-registered"></a>Oggetti Active Directory creati quando viene registrato un dispositivo  
Gli oggetti Active Directory seguenti vengono creati nell'ambito del servizio registrazione dispositivi.  
  
### <a name="device-registration-configuration"></a>Configurazione registrazione dispositivi  
La configurazione registrazione dispositivi viene archiviata nel contesto dei nomi di configurazione della foresta Active Directory. \(Ad esempio, **CN\=Device Registration Configuration, CN\=Services, < configuration\-denominazione\-contesto >**\). Questo oggetto viene creato quando la foresta Active Directory viene inizializzata per la registrazione dispositivi.  
  
La configurazione registrazione dispositivi include gli elementi seguenti:  
  
-   **Chiavi dell'autorità di certificazione**  
  
    Le chiavi pubbliche e private usate per rilasciare il certificato X.509 associato a un dispositivo registrato.  Le chiavi private sono protette con DKM.  
  
-   **Configurazione del servizio registrazione dispositivo**  
  
    Criteri relativi al servizio registrazione dispositivi.  
  
### <a name="registered-devices-container"></a>Contenitore di dispositivi registrati  
Il contenitore di dispositivi registrati viene creato in uno dei domini della foresta Active Directory.  Questo contenitore di oggetti conterrà tutti gli oggetti dispositivo per la foresta Active Directory.  
  
Per impostazione predefinita, il contenitore viene creato nello stesso dominio di AD FS.  \(Ad esempio, **CN\=RegisteredDevices, DC\=< impostazione predefinita\-denominazione\-contesto >**\). Questo oggetto viene creato quando la foresta di Active Directory viene inizializzata per la registrazione del dispositivo.  
  
### <a name="registered-devices"></a>Dispositivi registrati  
Gli oggetti dispositivo sono nuovi oggetti leggeri in Active Directory.  Vengono usati per rappresentare la relazione tra un utente, un dispositivo e la società.  Gli oggetti dispositivo usano un certificato firmato da AD FS per associare il dispositivo fisico all'oggetto dispositivo logico in Active Directory.  
  
I dispositivi registrati includono gli elementi seguenti:  
  
-   **Nome visualizzato**  
  
    Nome descrittivo del dispositivo.  Per i dispositivi Windows, è il nome host del computer.  
  
-   **Device Id**  
  
    GUID generato dal server di registrazione dispositivi.  
  
-   **Identificazione personale del certificato**  
  
    Identificazione personale del certificato X.509 usato con il dispositivo registrato.  
  
-   **Tipo di sistema operativo**  
  
    Tipo di sistema operativo del dispositivo.  
  
-   **Versione del sistema operativo**  
  
    Versione del sistema operativo del dispositivo.  
  
-   **È abilitata**  
  
    Valore booleano che indica se il dispositivo è abilitato in Active Directory.  Solo ai dispositivi abilitati è consentito accedere ai servizi.  
  
-   **Ora approssimativa ultimo utilizzo**  
  
    Ora approssimativa in cui il dispositivo è stato usato per accedere a una risorsa.  Per limitare il traffico di replica, viene aggiornato solo ogni 14 giorni.  
  
-   **Proprietario registrato**  
  
    L'identità di sicurezza \(SID\) dell'utente che ha aggiunto il dispositivo all'area di lavoro.  
  
## <a name="ad-fsdrs-server-ssl-certificate-revocation-checking"></a>AD FS\/revoche del certificato SSL del Server DRS  
Il client Aggiunta all'area di lavoro controlla la validità del certificato SSL del server AD FS.  Se il certificato Server SSL di AD FS include un elenco di revoche di certificati \(CRL\) endpoint, il client deve essere in grado di raggiungere l'endpoint specificato per convalidare il certificato.  
  
Se si usa un ambiente di test e un'autorità di certificazione di prova \(CA\) per rilasciare certificati SSL del server, quindi è possibile scegliere di non includere l'endpoint CRL nei certificati server rilasciati dall'autorità di certificazione.  In questo modo il client Aggiunta all'area di lavoro potrà ignorare la verifica CRL.  
  
> [!CAUTION]  
> Questo non è mai consigliato per i sistemi di produzione  
  

