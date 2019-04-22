---
title: Kerberos impedendo modificare la password che usano le chiavi private di RC4
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: de207d55-aa3d-4c16-bd3b-496db43663a4
manager: alanth
author: justinha
ms.technology: security-crdential-protection-and-management
ms.date: 11/09/2016
ms.openlocfilehash: 3cfa4ae2442f0cd1e3e4110a355da675fa2d61db
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816272"
---
# <a name="preventing-kerberos-change-password-that-uses-rc4-secret-keys"></a>Impedire a Kerberos di modificare la password tramite chiavi segrete RC4

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2008 R2 e Windows Server 2008

Questo argomento destinato ai professionisti IT illustra alcune limitazioni nella specifica del protocollo Kerberos che potrebbero portare a un utente malintenzionato di assumere il controllo dell'account dell'utente. È presente una limitazione standard il servizio di autenticazione di rete Kerberos (V5) (RFC 4120), che è noto nel settore, in base al quale un utente malintenzionato può eseguire l'autenticazione come un utente o modificare la password dell'utente se l'autore dell'attacco SA chiave privata dell'utente.

Il possesso di derivata dalla password Kerberos chiavi private un utente (RC4 e Advanced Encryption Standard [AES] per impostazione predefinita) viene convalidato durante lo scambio di modifica password di Kerberos per RFC 4757. La password dell'utente in testo normale non viene mai fornita per il centro distribuzione chiavi (KDC) e per impostazione predefinita, i controller di dominio Active Directory non dispone di una copia della password in testo normale per gli account. Se il controller di dominio non supporta un tipo di crittografia Kerberos, tale chiave privata non può essere utilizzato per modificare la password. 

Nei sistemi operativi Windows designati nell'elenco si applica all'inizio di questo argomento, esistono tre modi per bloccare la possibilità di modificare le password usando Kerberos con le chiavi private RC4:

- Configurare l'utente di account da includere l'opzione relativa all'account della Smart card è obbligatorio per l'accesso interattivo. In questo modo all'utente di accedere solo con una smart card valida in modo che le richieste di servizio di autenticazione (AS-richieste) RC4 vengono rifiutate. Per impostare le opzioni di account in un account, fare doppio clic sull'account, fare clic su proprietà e fare clic sulla scheda Account. 

- Disabilitare il supporto di RC4 per Kerberos su tutti i controller di dominio. Ciò richiede un minimo di un ambiente in cui tutti i client, server applicazioni e le relazioni di trust da e verso il dominio Kerberos devono supportare AES e un livello di funzionalità dominio di Windows Server 2008. Supporto per AES è stato introdotto in Windows Server 2008 e Windows Vista.

    [!NOTE]
    Si verifica un problema noto con la disabilitazione di RC4 che possono causare il riavvio del sistema. Vedere i seguenti aggiornamenti rapidi:
    - [Windows Server 2012 R2](https://support.microsoft.com/en-us/kb/3038261)
    - [Windows Server 2012](https://support.microsoft.com/en-us/kb/3086213)
    - Hotfix non è disponibile per le versioni precedenti di Windows Server

- Distribuire set di domini a livello funzionale del dominio Windows Server 2012 R2 o versione successiva e configurare gli utenti come membri del gruppo di sicurezza utenti protetti. Poiché questa funzionalità determina la chiusura delle oltre la semplice RC4 utilizzo del protocollo Kerberos, vedere le risorse in quanto segue [vedere anche](#see-also) sezione.

## <a name="see-also"></a>Vedere anche

- Per informazioni su come evitare l'utilizzo del tipo di crittografia RC4 in domini di Windows Server 2012 R2, vedere [Protected Users Security Group](/../credentials-protection-and-management/protected-users-security-group.md), e [come configurare gli account protetti](/../credentials-protection-and-management/how-to-configure-protected-accounts.md).

- Per una spiegazione sulle 4120 RFC e RFC 4757, vedere [documenti IETF](http://tools.ietf.org/html/).
