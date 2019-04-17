---
title: Kerberos che impedisce di modificare la password che usano le chiavi segrete RC4
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: de207d55-aa3d-4c16-bd3b-496db43663a4
manager: alanth
author: justinha
ms.technology: security-crdential-protection-and-management
ms.date: 11/09/2016
ms.openlocfilehash: 3cfa4ae2442f0cd1e3e4110a355da675fa2d61db
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="preventing-kerberos-change-password-that-uses-rc4-secret-keys"></a>Cambia password Kerberos impedendo che utilizza chiavi segrete RC4

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016, Windows Server 2008 R2 e Windows Server 2008

Questo argomento destinato ai professionisti IT illustra alcune limitazioni del protocollo Kerberos che possono portare a un utente malintenzionato di prendere il controllo dell'account dell'utente. Esiste una limitazione lo standard di servizio di autenticazione Kerberos rete (V5) (RFC 4120), che è noto del settore, in base al quale un utente malintenzionato può eseguire l'autenticazione come un utente o modificare la password dell'utente se l'autore dell'attacco conosce chiave privata dell'utente.

Durante lo scambio di modifica di password Kerberos per RFC 4757 verrà convalidato in possesso di derivate dalle password Kerberos chiavi segrete un utente (RC4 e Advanced Encryption Standard [AES per impostazione predefinita). Per impostazione predefinita, i controller di dominio Active Directory non possiede una copia della password non crittografate per gli account e la password dell'utente in testo normale non viene fornita per il centro distribuzione chiavi (KDC). Se il controller di dominio non supporta un tipo di crittografia Kerberos, tale chiave segreta non può essere utilizzato per modificare la password. 

Nei sistemi operativi Windows indicati nell'elenco si applica all'inizio di questo argomento, esistono tre modi per bloccare la possibilità di modificare le password tramite Kerberos con chiavi segrete RC4:

- Configurare l'utente, è necessario per l'accesso interattivo account per includere l'opzione Smart card. In questo modo all'utente di registrazione solo con una smart card valida in modo che le richieste di servizio di autenticazione (AS-requisiti) RC4 vengono rifiutate. Per impostare le opzioni di account in un account, fare doppio clic sull'account, scegliere Proprietà e fare clic sulla scheda Account. 

- Disabilitare il supporto di RC4 per Kerberos su tutti i controller di dominio. Ciò richiede un minimo di un ambiente in cui tutti i client Kerberos, i server applicazioni e le relazioni di trust da e verso il dominio devono supportare AES e un livello di funzionalità del dominio Windows Server 2008. Supporto per AES è stato introdotto in Windows Server 2008 e Windows Vista.

    [!NOTE]
    Esiste un problema noto con la disattivazione di RC4 che può causare il riavvio del sistema. Vedere i seguenti hotfix:
    - [Windows Server 2012 R2](https://support.microsoft.com/en-us/kb/3038261)
    - [Windows Server 2012](https://support.microsoft.com/en-us/kb/3086213)
    - Nessun hotfix è disponibile per le versioni precedenti di Windows Server

- Distribuire il set di domini a livello di funzionalità del dominio Windows Server 2012 R2 o versione successiva e configurare gli utenti come membri del gruppo di sicurezza utenti protetti. Poiché questa funzionalità compromette RC4 oltre l'utilizzo del protocollo Kerberos, vedere le risorse seguenti [vedere anche](#see-also) sezione.

## <a name="see-also"></a>Vedere anche

- Per informazioni su come evitare l'utilizzo del tipo di crittografia RC4 in domini di Windows Server 2012 R2, vedere [gruppo di sicurezza utenti protetti](/../credentials-protection-and-management/protected-users-security-group.md), e [come configurare gli account protetti](/../credentials-protection-and-management/how-to-configure-protected-accounts.md).

- Per una descrizione di RFC 4120 e 4757 RFC, vedere [documenti IETF](http://tools.ietf.org/html/).
