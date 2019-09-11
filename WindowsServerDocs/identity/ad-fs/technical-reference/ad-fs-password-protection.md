---
title: Protezione da attacchi AD FS password
description: Questo documento descrive come proteggere AD FS utenti dagli attacchi con password
author: billmath
manager: mtillman
ms.reviewer: andandyMSFT
ms.date: 11/15/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: c446ec86d426148836267e29610f86691cf0798a
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865463"
---
# <a name="ad-fs-password-attack-protection"></a>Protezione da attacchi AD FS password

## <a name="what-is-a-password-attack"></a>Che cos'è un attacco con password?

Un requisito per la Single Sign-On federata è la disponibilità degli endpoint per l'autenticazione su Internet. La disponibilità degli endpoint di autenticazione in Internet consente agli utenti di accedere alle applicazioni anche quando non si trovano in una rete aziendale. 

Tuttavia, ciò significa anche che alcuni attori cattivi possono sfruttare gli endpoint federati disponibili su Internet e usare questi endpoint per provare a determinare le password o per creare attacchi di tipo Denial of Service. Uno di questi attacchi che sta diventando più comune è denominato **attacco con password**. 

Esistono due tipi di attacchi comuni alle password. Attacco spray per la password & attacco di forza bruta. 

### <a name="password-spray-attack"></a>Attacco spray per la password
In un attacco di spray per le password, questi cattivi attori proveranno le password più comuni tra molti account e servizi diversi per ottenere l'accesso a qualsiasi risorsa protetta da password che è in grado di trovare. In genere si estendono in molte organizzazioni e provider di identità diversi. Un utente malintenzionato utilizzerà, ad esempio, un toolkit di uso comune per enumerare tutti gli utenti in diverse organizzazioni, quindi provare "P @ $ $w 0RD" e "Password1" su tutti gli account. Per dare un'idea, un attacco potrebbe essere simile al seguente:


|  Utente di destinazione   | Password di destinazione |
|----------------|-----------------|
| User1@org1.com |    Password1    |
| User2@org1.com |    Password1    |
| User1@org2.com |    Password1    |
| User2@org2.com |    Password1    |
|       …        |        …        |
| User1@org1.com |    P @ $ $w 0RD     |
| User2@org1.com |    P @ $ $w 0RD     |
| User1@org2.com |    P @ $ $w 0RD     |
| User2@org2.com |    P @ $ $w 0RD     |

Questo modello di attacco sottrae la maggior parte delle tecniche di rilevamento poiché dal punto di vista di un singolo utente o società, l'attacco sembra un accesso non riuscito isolato.

Per gli utenti malintenzionati, si tratta di un gioco di numeri: sanno che alcune password sono molto comuni.  L'autore dell'attacco riuscirà ad avere pochi successi per ogni migliaia di account attaccati e questo è sufficiente per essere efficace. Usano gli account per ottenere i dati da messaggi di posta elettronica, raccogliere informazioni di contatto e inviare collegamenti di phishing o semplicemente espandere il gruppo di destinazione spray per le password. Gli utenti malintenzionati non sono interessati a chi sono gli obiettivi iniziali, ma hanno un certo successo che possono sfruttare.

Tuttavia, eseguendo alcuni passaggi per configurare correttamente il AD FS e la rete, è possibile proteggere gli endpoint AD FS da questi tipi di attacchi. Questo articolo illustra tre aree che devono essere configurate correttamente per garantire la protezione da questi attacchi.

### <a name="brute-force-password-attack"></a>Attacco di forza bruta alla password 
In questo tipo di attacco, un utente malintenzionato tenterà più tentativi di accesso tramite password a un set di account di destinazione. In molti casi questi account saranno destinati agli utenti che hanno un livello di accesso più elevato all'interno dell'organizzazione. Questi possono essere dirigenti all'interno dell'organizzazione o amministratori che gestiscono l'infrastruttura critica.  

Questo tipo di attacco può anche causare modelli DOS. Questo potrebbe essere a livello di servizio in cui ADFS non è in grado di elaborare un numero elevato di richieste a causa di un numero insufficiente di server o potrebbe essere a livello di utente in cui un utente è bloccato dall'account.  

## <a name="securing-ad-fs-against-password-attacks"></a>Protezione AD FS dagli attacchi con password 

Tuttavia, eseguendo alcuni passaggi per configurare correttamente il AD FS e la rete, è possibile proteggere gli endpoint AD FS da questi tipi di attacchi. Questo articolo illustra tre aree che devono essere configurate correttamente per garantire la protezione da questi attacchi. 


- Livello 1, linea di base: Queste sono le impostazioni di base che devono essere configurate in un server di AD FS per assicurarsi che gli attori cattivi non possano attaccare la forza bruta degli utenti federati. 
- Livello 2, protezione della rete Extranet: Queste sono le impostazioni che devono essere configurate per garantire che l'accesso Extranet sia configurato per l'utilizzo di protocolli protetti, criteri di autenticazione e applicazioni appropriate. 
- Livello 3, passare a senza password per l'accesso Extranet: Si tratta di impostazioni avanzate e linee guida per consentire l'accesso alle risorse federate con credenziali più sicure anziché password soggette ad attacchi. 

## <a name="level-1-baseline"></a>Livello 1: base

1. Se ad FS 2016, [l'implementazione del](../../ad-fs/operations/Configure-AD-FS-Extranet-Smart-Lockout-Protection.md) blocco Smart Extranet per il blocco intelligente Extranet rileva percorsi familiari e consente a un utente valido di passare attraverso se si è precedentemente connessi correttamente da tale percorso. Utilizzando il blocco intelligente Extranet, è possibile assicurarsi che gli attori non corretti non possano attaccare la forza bruta agli utenti e, allo stesso tempo, consentiranno agli utenti legittimi di essere produttivi.
    - Se non si è AD FS 2016, si consiglia vivamente di [eseguire l'aggiornamento](../../ad-fs/deployment/upgrading-to-ad-fs-in-windows-server.md) a ad FS 2016. Si tratta di un percorso di aggiornamento semplice da AD FS 2012 R2. Se si è AD FS 2012 R2, implementare il [blocco Extranet](../../ad-fs/operations/Configure-AD-FS-Extranet-Soft-Lockout-Protection.md). Uno svantaggio di questo approccio consiste nel fatto che gli utenti validi possono essere bloccati dall'accesso Extranet se si utilizza un modello di forza bruta. AD FS sul server 2016 non presenta questo svantaggio.

2. Monitorare & bloccare gli indirizzi IP sospetti 
    - Se si dispone di Azure AD Premium, implementare Connect Health per ADFS e utilizzare le notifiche dei [report IP rischiosi](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health-adfs#risky-ip-report-public-preview) fornite.

        a. Le licenze non sono destinate a tutti gli utenti e richiedono 25 licenze/server ADFS/WAP, che possono essere semplici per un cliente.

        b. È ora possibile esaminare gli IP che generano un numero elevato di accessi non riusciti

        c. Questa operazione richiederà l'abilitazione del controllo sui server ADFS.

3.  Blocca gli IP sospetti.  Questo blocca potenzialmente gli attacchi DOS.

    a. Se il 2016, utilizzare la funzionalità di [*indirizzi IP esclusi Extranet*](../../ad-fs/operations/configure-ad-fs-banned-ip.md) per bloccare le richieste dall'indirizzo IP contrassegnato da #3 (o analisi manuale).

    b. Se si è in AD FS 2012 R2 o versioni precedenti, bloccare l'indirizzo IP direttamente in Exchange Online e, facoltativamente, nel firewall.

4. Se si dispone di Azure AD Premium, utilizzare [Azure ad la protezione con password](https://docs.microsoft.com/azure/active-directory/authentication/concept-password-ban-bad-on-premises) per impedire l'inserimento di password indovinabili Azure ad  

    a. Si noti che, se si dispone di password indovinabili, è possibile craccarle con soli 1-3 tentativi. Questa funzionalità impedisce che questi siano impostati. 

    b. Dalle statistiche di anteprima, l'impostazione di quasi il 20-50% delle nuove password viene bloccata. Questo implica che il% degli utenti è vulnerabile a password facilmente indovinate.

## <a name="level-2-protect-your-extranet"></a>Livello 2: Proteggi la tua Extranet

5. Passare all'autenticazione moderna per tutti i client che accedono dalla rete Extranet. I client di posta sono una parte importante di questa operazione. 

    a. Sarà necessario utilizzare Outlook Mobile per dispositivi mobili. La nuova app di posta elettronica nativa iOS supporta anche l'autenticazione moderna. 

    b. Sarà necessario usare Outlook 2013 (con le patch CU più recenti) o Outlook 2016.

6. Abilitare l'autenticazione a più fattori per l'accesso Extranet. In questo modo si garantisce una maggiore protezione per qualsiasi accesso Extranet.

   a.  Se si dispone di Azure AD Premium, utilizzare [Azure ad criteri di accesso condizionale](https://docs.microsoft.com/azure/active-directory/conditional-access/overview) per controllarlo.  Si tratta di una soluzione migliore rispetto all'implementazione delle regole in AD FS.  Ciò è dovuto al fatto che le app client moderne vengono applicate su base più frequente.  Ciò si verifica, in Azure AD, quando si richiede un nuovo token di accesso (in genere ogni ora) utilizzando un token di aggiornamento.  

   b.  Se non si ha Azure AD Premium o si hanno app aggiuntive in AD FS che consentono l'accesso in base a Internet, implementare l'autenticazione a più fattori (può essere anche l'autenticazione a più fattori di Azure in AD FS 2016) ed eseguire i criteri di autenticazione a più fattori [globali](../../ad-fs/operations/configure-authentication-policies.md#to-configure-multi-factor-authentication-globally) per tutti gli accessi Extranet.

## <a name="level-3-move-to-password-less-for-extranet-access"></a>Livello 3: Passa alla password meno per l'accesso Extranet

7. Passare alla finestra 10 e usare [Hello for business](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification).

8. Per gli altri dispositivi, se il AD FS 2016, è possibile usare OTP di autenticazione a più fattori di [Azure](../../ad-fs/operations/configure-ad-fs-and-azure-mfa.md) come primo fattore e password come secondo fattore. 

9. Per i dispositivi mobili, se si concedono solo dispositivi gestiti da MDM, è possibile usare i [certificati](../../ad-fs/operations/configure-user-certificate-authentication.md) per l'accesso dell'utente. 

## <a name="urgent-handling"></a>Gestione urgente

Se l'ambiente AD FS è sotto attacco attivo, i passaggi seguenti devono essere implementati al più presto:

 - Disabilitare l'endpoint U/P in ADFS e richiedere a tutti gli utenti di eseguire la VPN per ottenere l'accesso o all'interno della rete. A questo scopo, è necessario aver completato il passaggio di **livello 2 #1a** . In caso contrario, tutte le richieste di Outlook interne verranno comunque indirizzate tramite il cloud tramite l'autenticazione proxy di EXO.
 - Se l'attacco è disponibile solo tramite EXO, è possibile disabilitare l'autenticazione di base per i protocolli di Exchange (POP, IMAP, SMTP, EWS e così via) usando i criteri di autenticazione, questi protocolli e metodi di autenticazione vengono usati nella maggior parte di questi attacchi. Inoltre, le regole di accesso client in EXO e l'abilitazione del protocollo per cassetta postale vengono valutate dopo l'autenticazione e non aiutano a mitigare gli attacchi. 
 - Offrire in modo selettivo l'accesso Extranet usando il livello 3 #1-3.

## <a name="next-steps"></a>Passaggi successivi

- [Eseguire l'aggiornamento a AD FS server 2016](../../ad-fs/deployment/upgrading-to-ad-fs-in-windows-server.md) 
- [Blocco intelligente Extranet in AD FS 2016](../../ad-fs/operations/Configure-AD-FS-Extranet-Smart-Lockout-Protection.md)
- [Configurare i criteri di accesso condizionale](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
- [Protezione Azure AD password](https://docs.microsoft.com/azure/active-directory/authentication/howto-password-ban-bad-on-premises)
