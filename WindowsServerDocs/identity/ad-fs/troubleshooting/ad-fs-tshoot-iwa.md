---
title: Risoluzione dei problemi di AD FS - autenticazione di Windows integrata
description: Questo documento descrive come risolvere i problemi di autenticazione integrata di windows
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 91f252f5b0eca0f4c44e0b1a4564037298bf023c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814062"
---
# <a name="ad-fs-troubleshooting---integrated-windows-authentication"></a>Risoluzione dei problemi di AD FS - autenticazione di Windows integrata
L'autenticazione integrata di Windows consente agli utenti di accedere con le credenziali di Windows e l'esperienza single sign-on (SSO), tramite Kerberos o NTLM.

## <a name="reason-integrated-windows-authentication-fails"></a>L'autenticazione integrata di windows motivo non riesce
Esistono tre eseguita principalmente perché l'autenticazione integrata di windows non riuscirà. Sono:
    - Configurazione errata Name(SPN) entità servizio
    - Token di associazione di canale
    - Configurazione di Internet Explorer

## <a name="spn-misonfiguration"></a>SPN misonfiguration
Un nome dell'entità servizio (SPN) è un identificatore univoco di un'istanza del servizio. I nomi SPN vengono utilizzati dall'autenticazione Kerberos per associare un'istanza del servizio con un account di accesso del servizio. In questo modo un'applicazione client di richiedere che il servizio di autenticazione di un account anche se il client non ha il nome dell'account.

Un esempio di un nome SPN viene usato con AD FS è come segue:
1. Un web browser interroga Active Directory per determinare quale account del servizio è in esecuzione sts.contoso.com
2. Active Directory indica al browser che è l'account del servizio AD FS.
3. Il browser verrà visualizzato un ticket Kerberos per l'account del servizio AD FS.

Se l'account del servizio ADFS dispone di una configurazione errata o il nome SPN non corretto ciò può causare problemi.  Esaminando le tracce di rete, è possibile riscontrare errori, ad esempio errore KRB: KRB5KDC_ERR_S_PRINCIPAL_UNKNOWN.

Le tracce di rete (ad esempio Wireshark) consentono di determinare quale nome SPN il browser sta tentando di risolvere e quindi tramite la riga di comando allo strumento setspn - D <spn>, è possibile eseguire una ricerca nel nome dell'entità.  Non può essere trovata oppure può essere assegnato a un altro account diverso dall'account del servizio AD FS.

![Integrato](media/ad-fs-tshoot-iwa/iwa3.png)

È possibile verificare il nome SPN esaminando le proprietà dell'account del servizio AD FS.

![Integrato](media/ad-fs-tshoot-iwa/iwa1.png)

## <a name="channel-binding-token"></a>Token di associazione di canale
Attualmente, quando un'applicazione client si autentica al server tramite Kerberos, Digest o NTLM utilizzando il protocollo HTTPS, viene innanzitutto stabilito un canale di sicurezza TLS (Transport Level) e l'autenticazione viene eseguita utilizzando il canale. 

Il Token di associazione di canale è una proprietà del canale esterno protetto da TLS e viene utilizzato per associare il canale esterno a una conversazione sul canale interno autenticato dal client.

Se è presente che si verifichi un attacco "man-in-the-middle" e sono la crittografia di de e poi nuovamente crittografare il traffico SSL, quindi la chiave non corrisponderanno.  ADFS determinerà che c'è qualcosa fungendo da intermediario tra l'oggetto r esplorazione web e se stessa.  Questa operazione comporterà l'esito negativo dell'autenticazione Kerberos e l'utente verrà richiesto con una finestra di dialogo 401 anziché un'esperienza SSO.

Ciò può essere causato da:
 - tutto ciò che si trova tra il browser e AD FS
 - Fiddler
 - Proxy inverso esegue bridging SSL

Per impostazione predefinita, ADFS dispone di questa opzione impostata su "Consenti".  È possibile modificare questa impostazione usando il cmdlet di PowerShell `Set-ADFSProperties -ExtendProtectionTokenCheck`

Per altre informazioni, vedere [procedure consigliate per proteggere la pianificazione e distribuzione di AD FS](../../ad-fs/design/best-practices-for-secure-planning-and-deployment-of-ad-fs.md).

## <a name="internet-explorer-configuration"></a>Configurazione di Internet Explorer
Per impostazione predefinita, Internet explorer avranno le seguenti:

1. Esplora Internet riceverà una risposta 401 da AD FS con la parola NEGOTIATE nell'intestazione.
2. Ciò indica al browser web per ottenere un ticket Kerberos o NTLM per inviare nuovamente ADFS.
3. Per impostazione predefinita, Internet Explorer tenterà di eseguire questa (SPNEGO) senza interazione dell'utente se la parola NEGOTIATE è contenuto nell'intestazione.  Funzionerà solo per i siti intranet.

Esistono 2 elementi principali che possono impedire questa happeing.
   - Abilitare che l'autenticazione integrata di Windows non è selezionata nelle proprietà di Internet Explorer.  Ciò che si trova in Opzioni Internet -> Avanzate -> sicurezza.
![integrated](media/ad-fs-tshoot-iwa/iwa4.png)
   
   - Le aree di sicurezza non sono configurate correttamente
       - Nomi di dominio completi non sono presenti nell'area intranet
       - URL di AD FS non è presente nell'area intranet.

![Integrato](media/ad-fs-tshoot-iwa/iwa5.png)
## <a name="next-steps"></a>Passaggi successivi

- [Risoluzione dei problemi di AD FS](ad-fs-tshoot-overview.md)