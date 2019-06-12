---
title: Protezione di AD FS Password attacco
description: Questo documento descrive come proteggere gli utenti di AD FS da attacchi alle password
author: billmath
manager: mtillman
ms.reviewer: andandyMSFT
ms.date: 11/15/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 5666943138070cfa8cfe62f1ba932c2793daa003
ms.sourcegitcommit: cd12ace92e7251daaa4e9fabf1d8418632879d38
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2019
ms.locfileid: "66501595"
---
# <a name="ad-fs-password-attack-protection"></a>Protezione di AD FS Password attacco

## <a name="what-is-a-password-attack"></a>Che cos'è un attacco di tipo password?

Un requisito per l'accesso single sign-on federato è la disponibilità degli endpoint per l'autenticazione tramite internet. La disponibilità degli endpoint di autenticazione su internet consente agli utenti di accedere alle applicazioni anche quando non sono in una rete aziendale. 

Tuttavia, questo significa anche che alcuni cattivi attori può sfruttare i vantaggi degli endpoint federati disponibili su internet e utilizzare questi endpoint per tentare di determinare le password o per creare attacchi denial of service. Un attacco che sta diventando sempre più comune viene chiamato un **attacco password**. 

Esistono 2 tipi di attacchi di password comuni. Password spray attacco & brute force alla password. 

### <a name="password-spray-attack"></a>Password Spray attacco
In un attacco spray password, queste cattivi attori proverà la password più comuni tra numerosi account diversi e servizi per ottenere l'accesso a tutte le risorse protette da password che sono disponibili. In genere le soluzioni offerte comprendono molte organizzazioni diverse e provider di identità. Ad esempio, un utente malintenzionato utilizzerà un toolkit comunemente disponibile per enumerare tutti gli utenti in organizzazioni diverse e quindi provare a "P@$$w0rd" e "Password1" contro questi account. Per dare l'idea generale, un attacco potrebbe essere simile:


|  Utente di destinazione   | Password di destinazione |
|----------------|-----------------|
| User1@org1.com |    Password1    |
| User2@org1.com |    Password1    |
| User1@org2.com |    Password1    |
| User2@org2.com |    Password1    |
|       …        |        …        |
| User1@org1.com |    P@$$w0rd     |
| User2@org1.com |    P@$$w0rd     |
| User1@org2.com |    P@$$w0rd     |
| User2@org2.com |    P@$$w0rd     |

Questo schema di attacco sfugge la maggior parte delle tecniche di rilevamento perché dal punto di vista dell'utente privato o aziendale, l'attacco solo simile a un account di accesso non riusciti isolato.

Per gli utenti malintenzionati, è un gioco di numeri: sanno che esistono alcune password disponibili che sono molto comuni.  L'autore dell'attacco otterrà alcune operazioni riuscite per ogni migliaia account attaccato, e che è sufficiente per essere efficace. Usano gli account per ottenere dati dai messaggi di posta elettronica, raccogliere le informazioni di contatto e inviare collegamenti di phishing o semplicemente espandere il gruppo di destinazione spray password. Gli utenti malintenzionati non interessano molto chi sono le destinazioni iniziali, solo che dispongono di alcune operazioni riuscite che possono sfruttare.

Ma eseguendo alcuni passaggi per configurare AD FS e di rete in modo corretto, gli endpoint AD FS possono essere protetti contro questi tipi di attacchi. Questo articolo illustra 3 aree che devono essere configurati correttamente per la protezione da questi attacchi.

### <a name="brute-force-password-attack"></a>Attacco di forza bruta Password 
In questa forma di attacco, un utente malintenzionato prova più tentativi di password rispetto a un set di account di destinazione. In molti casi saranno destinati questi account di accesso agli utenti che hanno un maggiore livello di accesso all'interno dell'organizzazione. Può trattarsi di dirigenti all'interno dell'organizzazione o gli amministratori che gestiscono l'infrastruttura critica.  

Questo tipo di attacco potrebbe anche verificarsi nei modelli di DOS. Ciò può essere a livello di servizio in cui ADFS è riuscito a elaborare un & elevato di richieste dovuto a & insufficiente di server oppure potrebbe essere a livello di utente in cui un utente viene bloccato dal proprio account.  

## <a name="securing-ad-fs-against-password-attacks"></a>Protezione di AD FS contro gli attacchi alle password 

Ma eseguendo alcuni passaggi per configurare AD FS e di rete in modo corretto, gli endpoint AD FS possono essere protetti contro questi tipi di attacchi. Questo articolo illustra 3 aree che devono essere configurati correttamente per la protezione da questi attacchi. 


- Livello 1, linea di base: Queste sono le impostazioni di base che devono essere configurate in un server AD FS per garantire che gli attori dannosi non possono gli utenti federato attacco attacchi di forza bruta. 
- Livello 2, la protezione di rete extranet: Queste sono le impostazioni che devono essere configurate per garantire che l'accesso extranet è configurato per usare i protocolli sicuri, i criteri di autenticazione e applicazioni appropriate. 
- Livello 3, spostarsi senza password per l'accesso extranet: Questi sono avanzati delle impostazioni e le linee guida per abilitare l'accesso alle risorse federative con più sicura delle credenziali anziché le password che sono soggette agli attacchi. 

## <a name="level-1-baseline"></a>Livello 1: Linea di base

1. Se ad FS 2016, implementare [extranet blocco smart](../../ad-fs/operations/Configure-AD-FS-Extranet-Smart-Lockout-Protection.md) Extranet blocco smart tiene traccia delle posizioni note e consentirà a un utente valido per sempre passare attraverso se essi hanno precedentemente eseguito l'accesso da quel percorso. Usando il blocco smart extranet, è possibile assicurarsi che i cattivi attori non sarà in grado di attacchi di forza bruta gli utenti di attacco e nello stesso momento consentirà utente legittimo di essere produttivi.
    - Se non si usa AD FS 2016, è fortemente consigliabile [aggiornare](../../ad-fs/deployment/upgrading-to-ad-fs-in-windows-server.md) ad AD FS 2016. È un percorso di aggiornamento semplice di AD FS 2012 R2. Se si usa AD FS 2012 R2, implementare [blocco extranet](../../ad-fs/operations/Configure-AD-FS-Extranet-Soft-Lockout-Protection.md). Uno svantaggio di questo approccio è che gli utenti validi siano bloccati dagli accessi extranet in presenza di un modello di attacchi di forza bruta. ADFS in Server 2016 non presenta questo svantaggio.

2. Monitoraggio & blocco di indirizzi IP sospetti 
    - Se si dispone di Azure AD Premium, l'implementazione di Connect Health per ad FS e usare la [report sugli indirizzi IP rischiosi](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health-adfs#risky-ip-report-public-preview) notifiche che offre.

        a. Gestione delle licenze non è presente per tutti gli utenti e richiede 25 server licenze/ad FS/WAP che può essere facile per un cliente.

        b. È ora possibile esaminare IP che generano grandi & di accessi non riusciti

        c. Questo richiederà di abilitare il controllo sui server ad FS.

3.  Bloccare l'IP sospetto.  Questo potenzialmente consente di bloccare attacchi Denial of Service.

    a. Se nel 2016, usare il [ *indirizzi IP da escludere Extranet* ](../../ad-fs/operations/configure-ad-fs-banned-ip.md) funzionalità per bloccare tutte le richieste da indirizzi IP contrassegnati da #3 (o l'analisi manuale).

    b. Se si usa AD FS 2012 R2 o versione precedente, bloccare l'indirizzo IP direttamente in Exchange Online e, facoltativamente, nel firewall.

4. Se si dispone di Azure AD Premium, usare [protezione di Password di Azure AD](https://docs.microsoft.com/azure/active-directory/authentication/concept-password-ban-bad-on-premises) per impedire che le password da indovinare introduzione in Azure AD  

    a. Si noti che se si dispone di password da indovinare, è in grado di decifrare li con solo 1 a 3 tentativi. Questa funzionalità evita che queste informazioni dal recupero di set. 

    b. Dal nostro statistiche di anteprima, quasi 20 al 50% di nuove password bloccate da impostare. Ciò implica che % di utenti sono vulnerabili alle password facilmente identificabili.

## <a name="level-2-protect-your-extranet"></a>Livello 2: Proteggere la rete extranet

5. Spostare l'autenticazione moderna per i client che accedono dalla rete extranet. I client di posta elettronica rappresentano una parte importante di questo oggetto. 

    a. Dovrai usare Outlook per dispositivi mobili per dispositivi mobili. La nuova app di posta elettronica nativo di iOS supporta anche l'autenticazione moderna. 

    b. È necessario utilizzare Outlook 2013 (con le patch di aggiornamento Cumulativo più recente) o Outlook 2016.

6. Abilitare MFA per tutti gli accessi extranet. In questo modo è stata aggiunta la protezione per qualsiasi accesso alla rete extranet.

   a.  Se si dispone di Azure AD premium, usare [criteri di accesso condizionale di Azure AD](https://docs.microsoft.com/azure/active-directory/conditional-access/overview) per controllare questo problema.  Questa soluzione è preferibile rispetto all'implementazione di regole in AD FS.  Questo avviene perché le app client moderni vengono applicate in modo più frequente.  In questo caso, in Azure AD, quando viene richiesto un nuovo token di accesso (in genere ogni ora) con un token di aggiornamento.  

   b.  Se non si dispone di Azure AD premium o sono App aggiuntive su AD FS da internet consentire l'accesso basato su, implementare l'autenticazione a più fattori (può essere Azure MFA anche su AD FS 2016) ed eseguire un' [criteri di autenticazione a più fattori globali](../../ad-fs/operations/configure-authentication-policies.md#to-configure-multi-factor-authentication-globally) per tutti gli accessi extranet.

## <a name="level-3-move-to-password-less-for-extranet-access"></a>Livello 3: Passare alla password per l'accesso extranet

7. Sposta in Windows 10 e Usa [Hello For Business](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification).

8. Per altri dispositivi, nel caso di AD FS 2016, è possibile usare [Azure MFA OTP](../../ad-fs/operations/configure-ad-fs-and-azure-mfa.md) come il primo fattore e la password come 2nd factor. 

9. Per i dispositivi mobili, se si consentono solo dispositivi gestiti da MDM, è possibile usare [certificati](../../ad-fs/operations/configure-user-certificate-authentication.md) per l'accesso all'utente. 

## <a name="urgent-handling"></a>Gestione delle priorità alta

Se l'ambiente di AD FS è sotto attacco attivo, la procedura seguente deve essere implementata appena possibile:

 - Disabilitare l'endpoint U o P in ad FS e richiede tutti gli utenti VPN per accedere o essere all'interno della rete. Ciò è necessario disporre passo **livello 2 & 1a** completata. In caso contrario, tutte le richieste di Outlook interne verranno comunque indirizzate tramite il cloud tramite EXO proxy auth
 - Se l'attacco proviene solo tramite EXO, è possibile disabilitare l'autenticazione di base per i protocolli di Exchange (POP, IMAP, SMTP, EWS e così via) usando i criteri di autenticazione, questi protocolli e metodi di autenticazione in uso nella maggior parte di questi attacchi. Inoltre, regole di accesso Client di abilitazione di protocollo EXO e cassette postali vengono valutati post-autenticazione e sono inefficaci nella riduzione del rischio di attacchi. 
 - In modo selettivo offrono l'accesso extranet tramite il livello 3 #1-3.

## <a name="next-steps"></a>Passaggi successivi

- [Eseguire l'aggiornamento al server AD FS 2016](../../ad-fs/deployment/upgrading-to-ad-fs-in-windows-server.md) 
- [Blocco intelligente Extranet di AD FS 2016](../../ad-fs/operations/Configure-AD-FS-Extranet-Smart-Lockout-Protection.md)
- [Configurare i criteri di accesso condizionale](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
- [Protezione tramite password di Azure AD](https://docs.microsoft.com/azure/active-directory/authentication/howto-password-ban-bad-on-premises)
