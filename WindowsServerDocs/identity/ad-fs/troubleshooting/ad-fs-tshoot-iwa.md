---
title: Risoluzione dei problemi di AD FS-autenticazione integrata di Windows
description: In questo documento viene descritto come risolvere i problemi di autenticazione integrata di Windows
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 98f6c2e39b2a5eeab76103c1ae477dde785a0e04
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385388"
---
# <a name="ad-fs-troubleshooting---integrated-windows-authentication"></a>Risoluzione dei problemi di AD FS-autenticazione integrata di Windows
Autenticazione integrata di Windows consente agli utenti di accedere con le proprie credenziali di Windows e di sperimentare l'accesso Single Sign-on (SSO) tramite Kerberos o NTLM.

## <a name="reason-integrated-windows-authentication-fails"></a>Motivo dell'autenticazione integrata di Windows non riuscita
Esistono tre motivi principali per cui l'autenticazione integrata di Windows avrà esito negativo. Sono:
    - Configurazione del nome dell'entità servizio (SPN) non configurata
    - Token di associazione di canale
    - Configurazione di Internet Explorer

## <a name="spn-misconfiguration"></a>Configurazione non configurata SPN
Un nome dell'entità servizio (SPN) è un identificatore univoco di un'istanza del servizio. Gli SPN vengono utilizzati dall'autenticazione Kerberos per associare un'istanza del servizio a un account di accesso al servizio. Questo consente a un'applicazione client di richiedere che il servizio esegua l'autenticazione di un account anche se il client non ha il nome dell'account.

Di seguito è riportato un esempio di come viene usato un nome SPN con AD FS:
1. Un Web browser esegue una query Active Directory per determinare l'account del servizio che esegue sts.contoso.com
2. Active Directory indica al browser che si tratta dell'account del servizio di AD FS.
3. Il browser otterrà un ticket Kerberos per l'account del servizio AD FS.

Se per l'account del servizio AD FS è stato configurato un nome SPN errato o errato, è possibile che si verifichino problemi.  Esaminando le tracce di rete, è possibile che vengano visualizzati errori come KRB Error: KRB5KDC_ERR_S_PRINCIPAL_UNKNOWN.

Utilizzando tracce di rete, ad esempio Wireshark, è possibile determinare il nome SPN che il browser sta tentando di risolvere e quindi utilizzare lo strumento da riga di comando Setspn-Q <spn>, è possibile eseguire una ricerca su tale SPN.  Potrebbe non essere possibile trovarlo o assegnarlo a un altro account diverso dall'account del servizio AD FS.

![integrato](media/ad-fs-tshoot-iwa/iwa3.png)

È possibile verificare il nome SPN esaminando le proprietà dell'account del servizio AD FS.

![integrato](media/ad-fs-tshoot-iwa/iwa1.png)

## <a name="channel-binding-token"></a>Token di associazione di canale
Attualmente, quando un'applicazione client esegue l'autenticazione al server utilizzando Kerberos, digest o NTLM mediante HTTPS, viene innanzitutto stabilito un canale TLS (Transport Level Security) e l'autenticazione viene eseguita utilizzando questo canale. 

Il token di associazione del canale è una proprietà del canale esterno protetto da TLS e viene usato per associare il canale esterno a una conversazione sul canale interno autenticato dal client.

Se si verifica un attacco "Man-in-the-Middle" ed è in corso la decrittografia e la nuova crittografia del traffico SSL, la chiave non corrisponderà.  AD FS determinerà un elemento in mezzo tra il Web browse r e se stesso.  Questa operazione causerà l'esito negativo dell'autenticazione Kerberos e all'utente verrà richiesto di usare una finestra di dialogo 401 anziché un'esperienza SSO.

Questo problema può essere causato da:
 - tutto ciò che si trova tra il browser e AD FS
 - Fiddler
 - Proxy inversi che eseguono il bridging SSL

Per impostazione predefinita, AD FS è impostato su "Consenti".  È possibile modificare questa impostazione usando PowerShell cmdlet `Set-ADFSProperties -ExtendProtectionTokenCheck`

Per ulteriori informazioni su questo argomento [, vedere procedure consigliate per la pianificazione e la distribuzione sicure di ad FS](../../ad-fs/design/best-practices-for-secure-planning-and-deployment-of-ad-fs.md).

## <a name="internet-explorer-configuration"></a>Configurazione di Internet Explorer
Per impostazione predefinita, Internet Explorer avrà il seguente metodo:

1. Internet Explorer riceverà una risposta 401 da AD FS con la parola NEGOTIATe nell'intestazione.
2. Ciò indica al Web browser di ottenere un ticket Kerberos o NTLM da restituire al AD FS.
3. Per impostazione predefinita, IE tenterà di eseguire questa operazione (SPNEGO) senza l'intervento dell'utente se la parola NEGOTIATe si trova nell'intestazione.  Funzionerà solo per i siti Intranet.

Esistono due aspetti principali che possono impedire questo problema da happeing.
   - Abilita autenticazione integrata di Windows non è selezionata nelle proprietà di Internet Explorer.  Disponibile in Opzioni Internet-> sicurezza > avanzata.
   
   ![integrato](media/ad-fs-tshoot-iwa/iwa4.png)
   
   - Le aree di sicurezza non sono configurate correttamente
       - I nomi di dominio completi non si trovano nell'area Intranet
       - AD FS URL non si trova nell'area Intranet.

      ![integrato](media/ad-fs-tshoot-iwa/iwa5.png)
## <a name="next-steps"></a>Passaggi successivi

- [Risoluzione dei problemi relativi ad AD FS](ad-fs-tshoot-overview.md)
