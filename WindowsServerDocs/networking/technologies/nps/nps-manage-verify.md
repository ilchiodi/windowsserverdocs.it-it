---
title: Verificare la configurazione dopo aver apportato modifiche di Server dei criteri di rete
description: È possibile utilizzare questo argomento per verificare la configurazione di Server dei criteri di rete di Windows Server 2016 dopo la modifica di un indirizzo IP o un nome per il server.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: fc77450e-2af1-47ba-bb23-1fd36d9efdbf
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f4d7e003fb037d18c5e5f2036a419383885eaf9e
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="verify-configuration-after-nps-server-changes"></a>Verificare la configurazione dopo aver apportato modifiche di Server dei criteri di rete

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per verificare la configurazione del server dei criteri di rete dopo la modifica di un indirizzo IP o un nome per il server.

## <a name="verify-configuration-after-an-nps-server-ip-address-change"></a>Verificare la configurazione dopo una modifica dell'indirizzo IP Server dei criteri di rete

Potrebbero essere presenti circostanze in cui è necessario modificare l'indirizzo IP di un server dei criteri di rete o un proxy, ad esempio quando si sposta il server a un'altra subnet IP. 

Se si modifica un server dei criteri di rete o indirizzo IP del proxy, è necessario riconfigurare le parti della distribuzione dei criteri di rete. 

Utilizzare le seguenti linee guida generali per facilitare la verifica che una modifica dell'indirizzo IP non interrompe l'autenticazione di accesso di rete, autorizzazione o accounting sulla rete per i server RADIUS dei criteri di rete e server proxy RADIUS.

È necessario essere un membro del **amministratori**, o equivalente, per eseguire queste procedure.

### <a name="to-verify-configuration-after-an-nps-server-ip-address-change"></a>Per verificare la configurazione dopo un NPS server l'indirizzo IP di modifica

1. Riconfigurare tutti i client RADIUS, ad esempio punti di accesso wireless e server VPN, con il nuovo indirizzo IP del server dei criteri di rete.

2. Se il server dei criteri di rete è un membro di un gruppo di server RADIUS remoto, riconfigurare il proxy di criteri di rete con il nuovo indirizzo IP del server dei criteri di rete.

3. Se è stato configurato il server dei criteri di rete per utilizzare la registrazione di SQL Server, verificare che la connettività tra il computer che esegue SQL Server e il server dei criteri di rete continui a funzionare correttamente.

4. Se è stato distribuito IPsec per proteggere il traffico RADIUS tra il server dei criteri di rete e un proxy dei criteri di rete o altri server o dispositivi, riconfigurare i criteri IPsec o la regola di sicurezza della connessione in Windows Firewall con sicurezza avanzata di usare il nuovo indirizzo IP del server dei criteri di rete.

5. Se il server dei criteri di rete è multihomed e aver configurato il server per eseguire il binding a una scheda di rete specifica, riconfigurare le impostazioni delle porte dei criteri di rete con il nuovo indirizzo IP.

### <a name="to-verify-configuration-after-an-nps-proxy-ip-address-change"></a>Per verificare la configurazione dopo un NPS proxy l'indirizzo IP di modifica

1. Riconfigurare tutti i client RADIUS, ad esempio punti di accesso wireless e server VPN, con il nuovo indirizzo IP del proxy di criteri di rete.

2. Se il proxy di criteri di rete è multihomed e aver configurato il proxy per il binding a una scheda di rete specifica, riconfigurare le impostazioni delle porte dei criteri di rete con il nuovo indirizzo IP.

3. Riconfigurare tutti i membri di tutti i gruppi di server RADIUS remoti con l'indirizzo IP del server proxy. Per eseguire questa operazione, in ogni server dei criteri di rete con il proxy di criteri di rete configurato come client RADIUS:

    un. Fare doppio clic su **dei criteri di rete (locale)**, fare doppio clic su **client e server RADIUS**, fare clic su **client RADIUS**e quindi nel riquadro dei dettagli fare doppio clic su client RADIUS che si desidera modificare.

    b. Nel client RADIUS **proprietà**, in **indirizzo \(IP or DNS\)**, digitare il nuovo indirizzo IP del proxy di criteri di rete.

4. Se è stato configurato il proxy di criteri di rete per utilizzare la registrazione di SQL Server, verificare che la connettività tra il computer che esegue SQL Server e il proxy di criteri di rete corretto funzionamento.

## <a name="verify-configuration-after-renaming-an-nps-server"></a>Verificare la configurazione dopo la ridenominazione di un Server dei criteri di rete

Potrebbero essere presenti circostanze quando è necessario modificare il nome di un server dei criteri di rete o un proxy, ad esempio quando si riprogettare le convenzioni di denominazione per i server.

Se si modifica un server dei criteri di rete o il nome del proxy, è necessario riconfigurare le parti della distribuzione dei criteri di rete. 

Utilizzare le seguenti linee guida generali per facilitare la verifica che una modifica del nome server non interrompe l'autenticazione di accesso di rete, autorizzazione o contabilità.

È necessario essere un membro del **amministratori**, o equivalente, per eseguire questa procedura.

### <a name="to-verify-configuration-after-an-nps-server-or-proxy-name-change"></a>Per verificare la configurazione dopo una modifica del nome dei criteri di rete del server o proxy

1. Se il server dei criteri di rete è un membro di un gruppo di server RADIUS remoto e il gruppo è configurato con i nomi di computer anziché con indirizzi IP, riconfigurare il gruppo di server RADIUS remoto con il nuovo nome del server dei criteri di rete.

2. Se il server dei criteri di rete vengono distribuiti i metodi di autenticazione basata su certificati, la modifica del nome invalida il certificato del server. È possibile richiedere un nuovo certificato da parte dell'amministratore di autorità di certificazione o, se il computer è un computer membro del dominio e registrare automaticamente i certificati per i membri del dominio, è possibile aggiornare i criteri di gruppo per ottenere un nuovo certificato tramite la registrazione automatica. Per aggiornare i criteri di gruppo:

    un. Aprire il prompt dei comandi o Windows PowerShell.

    b. Tipo **gpupdate**, quindi premere INVIO.


3. Dopo aver ottenuto un nuovo certificato server, richiedono che l'amministratore della CA revocare il certificato precedente. 

     Dopo che il certificato precedente viene revocato, dei criteri di rete continua a usarlo fino a quando non scade il certificato precedente. Per impostazione predefinita, il certificato precedente rimane valido per un tempo massimo di una settimana e 10 ore. Questo periodo di tempo potrebbe essere diverso a seconda se la scadenza dell'elenco di revoche di certificati (CRL) e la scadenza di tempo cache Transport Layer Security (TLS) sono state modificate le impostazioni predefinite. La durata CRL predefinita è una settimana; il valore predefinito TLS cache ora di scadenza è 10 ore. 

     Tuttavia, se si desidera configurare i criteri di rete per utilizzare il nuovo certificato immediatamente, è possibile riconfigurare manualmente i criteri di rete con il nuovo certificato.

4. Alla scadenza del certificato precedente, dei criteri di rete inizia con il nuovo certificato. 

5. Se è stato configurato il server dei criteri di rete per utilizzare la registrazione di SQL Server, verificare che la connettività tra il computer che esegue SQL Server e il server dei criteri di rete continui a funzionare correttamente.

