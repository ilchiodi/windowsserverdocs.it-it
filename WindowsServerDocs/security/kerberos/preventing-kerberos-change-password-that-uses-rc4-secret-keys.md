---
title: Impedisci la modifica della password Kerberos che usa chiavi segrete RC4
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: de207d55-aa3d-4c16-bd3b-496db43663a4
manager: alanth
author: justinha
ms.technology: security-crdential-protection-and-management
ms.date: 11/09/2016
ms.openlocfilehash: 21c2d3d79653bd02fea9d2ac0d09bd18690a388f
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/14/2020
ms.locfileid: "75949741"
---
# <a name="preventing-kerberos-change-password-that-uses-rc4-secret-keys"></a>Impedire a Kerberos di modificare la password tramite chiavi segrete RC4

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2008 R2 e Windows Server 2008

Questo argomento per i professionisti IT spiega alcune limitazioni nel protocollo Kerberos che possono causare a un utente malintenzionato di assumere il controllo dell'account di un utente. Esiste una limitazione nello standard Kerberos Network Authentication Service (V5) (RFC 4120), noto all'interno del settore, in base al quale un utente malintenzionato può eseguire l'autenticazione come utente o modificare la password di tale utente se l'autore dell'attacco conosce la chiave privata dell'utente.

Il possesso delle chiavi segrete Kerberos derivate dalla password di un utente (RC4 e Advanced Encryption Standard [AES] per impostazione predefinita) viene convalidato durante lo scambio di modifiche della password Kerberos per RFC 4757. La password in testo non crittografato dell'utente non viene mai fornita al Centro distribuzione chiavi (KDC) e, per impostazione predefinita, i controller di dominio Active Directory non dispongono di una copia delle password in testo non crittografato per gli account. Se il controller di dominio non supporta un tipo di crittografia Kerberos, la chiave privata non può essere utilizzata per modificare la password. 

Nei sistemi operativi Windows designati nell'elenco si applica a all'inizio di questo argomento sono disponibili tre modi per impedire la modifica delle password tramite Kerberos con chiavi segrete RC4:

- Configurare l'account utente per includere l'opzione relativa all'account è richiesta la smart card per l'accesso interattivo. Questo consente di limitare l'accesso dell'utente con una smart card valida in modo che le richieste di servizio di autenticazione RC4 (AS-Rich) vengano rifiutate. Per impostare le opzioni dell'account per un account, fare clic con il pulsante destro del mouse sull'account, scegliere Proprietà e fare clic sulla scheda account. 

- Disabilitare il supporto RC4 per Kerberos in tutti i controller di dominio. Questo richiede almeno un livello di funzionalità del dominio Windows Server 2008 e un ambiente in cui tutti i client Kerberos, i server applicazioni e le relazioni di trust da e verso il dominio devono supportare AES. Il supporto per AES è stato introdotto in Windows Server 2008 e Windows Vista.

    [!NOTE]
    Si è verificato un problema noto con la disabilitazione di RC4 che può causare il riavvio del sistema. Vedere gli hotfix seguenti:
    - [Windows Server 2012 R2](https://support.microsoft.com/kb/3038261)
    - [Windows Server 2012](https://support.microsoft.com/kb/3086213)
    - Nessun hotfix disponibile per le versioni precedenti di Windows Server

- Distribuire i domini impostati sul livello di funzionalità del dominio Windows Server 2012 R2 o versione successiva e configurare gli utenti come membri del gruppo di sicurezza utenti protetti. Dal momento che questa funzionalità consente di evitare solo l'utilizzo di RC4 nel protocollo Kerberos, vedere le risorse nella sezione [vedere anche](#see-also) .

## <a name="see-also"></a>Vedi anche

- Per informazioni su come evitare l'utilizzo del tipo di crittografia RC4 nei domini di Windows Server 2012 R2, vedere [gruppo di sicurezza utenti protetti](/../credentials-protection-and-management/protected-users-security-group.md)e [come configurare gli account protetti](/../credentials-protection-and-management/how-to-configure-protected-accounts.md).

- Per spiegazioni su RFC 4120 e RFC 4757, vedere [documenti IETF](http://tools.ietf.org/html/).
