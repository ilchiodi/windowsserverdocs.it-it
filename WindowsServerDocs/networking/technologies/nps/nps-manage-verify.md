---
title: Verificare la configurazione dopo le modifiche di NPS
description: È possibile utilizzare questo argomento per verificare la configurazione del server dei criteri di rete di Windows Server 2016 dopo che un indirizzo IP o un nome è stato modificato nel server.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: fc77450e-2af1-47ba-bb23-1fd36d9efdbf
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 5a99cfb62d3ce331ef2d90fe13afe99a6d14960c
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315884"
---
# <a name="verify-configuration-after-nps-changes"></a>Verificare la configurazione dopo le modifiche di NPS

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per verificare la configurazione del server dei criteri di configurazione dopo che un indirizzo IP o un nome è stato modificato nel server.

## <a name="verify-configuration-after-an-nps-ip-address-change"></a>Verificare la configurazione dopo la modifica di un indirizzo IP NPS

Potrebbero esserci casi in cui è necessario modificare l'indirizzo IP di un server dei criteri di server o proxy, ad esempio quando si sposta il server in una subnet IP diversa. 

Se si modifica un server dei criteri di server o un indirizzo IP proxy, è necessario riconfigurare le parti della distribuzione del server dei criteri di server. 

Usare le linee guida generali seguenti per verificare che la modifica di un indirizzo IP non interrompa l'autenticazione, l'autorizzazione o l'accounting di accesso alla rete per i server RADIUS e i server proxy RADIUS.

Per eseguire queste procedure, è necessario essere un membro del gruppo **Administrators**o di un gruppo equivalente.

### <a name="to-verify-configuration-after-an-nps-ip-address-change"></a>Per verificare la configurazione dopo una modifica dell'indirizzo IP del server dei criteri di server

1. Riconfigurare tutti i client RADIUS, ad esempio punti di accesso wireless e server VPN, con il nuovo indirizzo IP del server dei criteri di rete.

2. Se il server dei criteri di gruppo è membro di un gruppo di server RADIUS remoti, riconfigurare il proxy NPS con il nuovo indirizzo IP del server dei criteri di gruppo.

3. Se è stato configurato il server dei criteri di accesso per l'utilizzo di SQL Server registrazione, verificare che la connettività tra il computer che esegue SQL Server e NPS funzioni correttamente.

4. Se è stato distribuito IPsec per proteggere il traffico RADIUS tra il server dei criteri di dominio e un proxy server dei criteri di dominio o altri server o dispositivi, riconfigurare i criteri IPsec o la regola di sicurezza della connessione in Windows Firewall con sicurezza avanzata per usare il nuovo indirizzo IP del server dei criteri di dominio.

5. Se il server dei criteri di rete è multihomed e il server è stato configurato per l'associazione a una scheda di rete specifica, riconfigurare le impostazioni della porta NPS con il nuovo indirizzo IP.

### <a name="to-verify-configuration-after-an-nps-proxy-ip-address-change"></a>Per verificare la configurazione dopo la modifica dell'indirizzo IP di un server dei criteri di server

1. Riconfigurare tutti i client RADIUS, ad esempio punti di accesso wireless e server VPN, con il nuovo indirizzo IP del proxy server dei criteri di rete.

2. Se il proxy server dei criteri di rete è multihomed ed è stato configurato il proxy per l'associazione a una scheda di rete specifica, riconfigurare le impostazioni della porta NPS con il nuovo indirizzo IP.

3. Riconfigurare tutti i membri di tutti i gruppi di server RADIUS remoti con l'indirizzo IP del server proxy. Per eseguire questa operazione, in ogni server dei criteri di gruppo che dispone del proxy NPS configurato come client RADIUS:

    a. Fare doppio clic su **NPS (local)** , fare doppio clic su **client e server RADIUS**, fare clic su **client RADIUS**, quindi nel riquadro dei dettagli fare doppio clic sul client RADIUS che si desidera modificare.

    b. In **Proprietà**client RADIUS, in **Indirizzo \(ip o DNS\)** , digitare il nuovo indirizzo IP del proxy NPS.

4. Se il proxy NPS è stato configurato per l'uso di SQL Server la registrazione, verificare che la connettività tra il computer che esegue SQL Server e il proxy NPS funzioni ancora correttamente.

## <a name="verify-configuration-after-renaming-an-nps"></a>Verificare la configurazione dopo aver rinominato server dei criteri di server

In alcuni casi è necessario modificare il nome di un server dei criteri di server o di un proxy, ad esempio quando si riprogettano le convenzioni di denominazione per i server.

Se si modifica un nome di server dei criteri di server o proxy, è necessario riconfigurare parti della distribuzione del server dei criteri di server. 

Usare le linee guida generali seguenti per verificare che la modifica del nome di un server non interrompa l'autenticazione, l'autorizzazione o l'accounting di accesso alla rete.

Per eseguire questa procedura, è necessario essere un membro di **Administrators**o un gruppo equivalente.

### <a name="to-verify-configuration-after-an-nps-or-proxy-name-change"></a>Per verificare la configurazione dopo la modifica di un server dei criteri di server o del proxy

1. Se il server dei criteri di gruppo è membro di un gruppo di server RADIUS remoto e il gruppo è configurato con nomi computer anziché indirizzi IP, riconfigurare il gruppo di server RADIUS remoti con il nuovo nome NPS.

2. Se i metodi di autenticazione basati su certificato vengono distribuiti in NPS, la modifica del nome invalida il certificato del server. È possibile richiedere un nuovo certificato dall'amministratore dell'autorità di certificazione (CA) oppure, se il computer è un computer membro di dominio e registrare automaticamente i certificati ai membri del dominio, è possibile aggiornare Criteri di gruppo per ottenere un nuovo certificato tramite la registrazione automatica . Per aggiornare Criteri di gruppo:

    a. Aprire il prompt dei comandi o Windows PowerShell.

    b. Digitare **gpupdate** e quindi premere INVIO.


3. Quando si dispone di un nuovo certificato del server, richiedere all'amministratore della CA di revocare il certificato precedente. 

     Una volta revocato il certificato precedente, NPS continua a utilizzarlo fino alla scadenza del certificato precedente. Per impostazione predefinita, il certificato precedente rimane valido per un periodo di tempo massimo di una settimana e 10 ore. Questo periodo di tempo potrebbe variare a seconda che la scadenza dell'elenco di revoche di certificati (CRL) e la scadenza della cache del Transport Layer Security (TLS) siano state modificate rispetto ai valori predefiniti. La scadenza predefinita del CRL è di una settimana. la scadenza del tempo di memorizzazione nella cache TLS predefinita è 10 ore. 

     Se si desidera configurare server dei criteri di rete per l'utilizzo immediato del nuovo certificato, è tuttavia possibile riconfigurare manualmente i criteri di rete con il nuovo certificato.

4. Una volta scaduto il certificato precedente, NPS inizia automaticamente a usare il nuovo certificato. 

5. Se è stato configurato il server dei criteri di accesso per l'utilizzo di SQL Server registrazione, verificare che la connettività tra il computer che esegue SQL Server e NPS funzioni correttamente.

