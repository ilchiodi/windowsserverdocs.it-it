---
title: Verificare la configurazione dopo le modifiche dei criteri di rete
description: È possibile utilizzare questo argomento per verificare la configurazione di Server dei criteri di rete di Windows Server 2016 dopo un indirizzo IP o nome modificato nel server.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: fc77450e-2af1-47ba-bb23-1fd36d9efdbf
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 144e414e32d413e4863b90ada671753155bc96d7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880672"
---
# <a name="verify-configuration-after-nps-changes"></a>Verificare la configurazione dopo le modifiche dei criteri di rete

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per verificare la configurazione dei criteri di rete dopo la modifica un indirizzo IP o un nome per il server.

## <a name="verify-configuration-after-an-nps-ip-address-change"></a>Verificare la configurazione dopo un cambio di indirizzo IP dei criteri di rete

Potrebbero esserci casi in cui è necessario modificare l'indirizzo IP di un proxy, ad esempio quando si sposta il server a un'altra subnet IP o dei criteri di rete. 

Se si modifica un indirizzo IP dei criteri di rete o un proxy, è necessario riconfigurare parti della distribuzione dei criteri di rete. 

Usare le linee guida generali seguenti per facilitare la verifica che un cambio di indirizzo IP non interrompe l'autenticazione di accesso di rete, autorizzazione o accounting sulla rete per i server NPS RADIUS e server proxy RADIUS.

È necessario essere un membro del **gli amministratori**, o equivalente, per eseguire queste procedure.

### <a name="to-verify-configuration-after-an-nps-ip-address-change"></a>Per verificare la configurazione dopo un cambio di indirizzo IP NPS

1. Riconfigurare tutti i client RADIUS, ad esempio punti di accesso wireless e server VPN, con il nuovo indirizzo IP di NPS.

2. Se i criteri di rete è un membro di un gruppo di server RADIUS remoti, riconfigurare il proxy di criteri di rete con il nuovo indirizzo IP di NPS.

3. Se è stato configurato per usare la registrazione di SQL Server NPS, verificare che la connettività tra il computer che esegue SQL Server e i criteri di rete funziona ancora correttamente.

4. Se è stato distribuito IPsec per proteggere il traffico RADIUS tra i criteri di rete e un proxy di criteri di rete o altri server o dispositivi, riconfigurare i criteri IPsec o la regola di sicurezza di connessione in Windows Firewall con sicurezza avanzata per usare il nuovo indirizzo IP di NPS.

5. Se i criteri di rete è multihomed e aver configurato il server da associare a una scheda di rete specifica, riconfigurare le impostazioni di porta dei criteri di rete con il nuovo indirizzo IP.

### <a name="to-verify-configuration-after-an-nps-proxy-ip-address-change"></a>Per verificare la configurazione dopo un NPS di indirizzi IP proxy modifica

1. Riconfigurare tutti i client RADIUS, ad esempio punti di accesso wireless e server VPN, con il nuovo indirizzo IP del proxy di criteri di rete.

2. Se il proxy di criteri di rete è multihomed e aver configurato il proxy per l'associazione a una scheda di rete specifica, riconfigurare le impostazioni di porta dei criteri di rete con il nuovo indirizzo IP.

3. Riconfigurare tutti i membri di tutti i gruppi di server RADIUS remoti con l'indirizzo IP del server proxy. A tale scopo, in ogni NPS con il proxy NPS configurato come client RADIUS:

    a. Fare doppio clic su **dei criteri di rete (locale)**, fare doppio clic su **client e server RADIUS**, fare clic su **client RADIUS**e quindi nel riquadro dei dettagli fare doppio clic il client RADIUS che si Se si desidera modificare.

    b. Nel client RADIUS **delle proprietà**, nel **indirizzo \(IP o DNS\)**, digitare il nuovo indirizzo IP del proxy di criteri di rete.

4. Se è stato configurato il proxy di criteri di rete per usare la registrazione di SQL Server, verificare che la connettività tra il computer che esegue SQL Server e il proxy di criteri di rete funziona ancora correttamente.

## <a name="verify-configuration-after-renaming-an-nps"></a>Verificare la configurazione dopo aver rinominato un criteri di rete

Potrebbero esserci casi quando è necessario modificare il nome di un proxy, ad esempio quando si riprogettare le convenzioni di denominazione per i server o dei criteri di rete.

Se si modifica un nome dei criteri di rete o un proxy, è necessario riconfigurare parti della distribuzione dei criteri di rete. 

Usare le linee guida generali seguenti per facilitare la verifica che una modifica del nome server non interrompe l'autenticazione di accesso di rete, autorizzazione e accounting.

È necessario essere un membro del **gli amministratori**, o equivalente, per eseguire questa procedura.

### <a name="to-verify-configuration-after-an-nps-or-proxy-name-change"></a>Per verificare la configurazione dopo una modifica del nome dei criteri di rete o un proxy

1. Se i criteri di rete è un membro di un gruppo di server RADIUS remoti e il gruppo è configurato con i nomi di computer anziché degli indirizzi IP, riconfigurare il gruppo di server RADIUS remoto con il nuovo nome dei criteri di rete.

2. Se i metodi di autenticazione basata su certificato vengono distribuiti in NPS, la modifica del nome invalida il certificato del server. È possibile richiedere un nuovo certificato da parte dell'amministratore di autorità (CA) certificazione oppure, se il computer è un computer membro del dominio e registrazione automatica certificati per i membri del dominio, è possibile aggiornare i criteri di gruppo per ottenere un nuovo certificato tramite la registrazione automatica . Per aggiornare criteri di gruppo:

    a. Aprire il prompt dei comandi o Windows PowerShell.

    b. Digitare **gpupdate** e quindi premere INVIO.


3. Dopo aver creato un nuovo certificato del server, richiedere che l'amministratore della CA revocare il certificato precedente. 

     Il certificato precedente è stato revocato, NPS continua a usarlo fino a quando non scade il certificato precedente. Per impostazione predefinita, il certificato precedente rimane valido per un tempo massimo di una settimana e 10 ore. Questo periodo di tempo potrebbe essere diverso a seconda del fatto che la scadenza di Strumentazione gestione Windows (CRL, Certificate Revocation List) e la scadenza di tempo della cache di Transport Layer Security (TLS) sono stati modificati i valori predefiniti. La durata CRL predefinita è una settimana; il valore predefinito TLS di memorizzare nella cache ora di scadenza è 10 ore. 

     Tuttavia, se si vuole configurare criteri di rete per usare immediatamente il nuovo certificato, è possibile riconfigurare manualmente i criteri di rete con il nuovo certificato.

4. Dopo che il vecchio certificato scade, dei criteri di rete inizia automaticamente usando il nuovo certificato. 

5. Se è stato configurato per usare la registrazione di SQL Server NPS, verificare che la connettività tra il computer che esegue SQL Server e i criteri di rete funziona ancora correttamente.

