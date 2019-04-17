---
title: Configurare modelli di certificato per i requisiti di EAP e PEAP
description: In questo argomento fornisce informazioni sull'uso dei certificati con Server dei criteri di rete e accesso remoto in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2af0a1df-5c44-496b-ab11-5bc340dc96f0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e597a65982aeead907c9a41f17f1785a0bf81016
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="configure-certificate-templates-for-peap-and-eap-requirements"></a>Configurare modelli di certificato per i requisiti di EAP e PEAP

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Tutti i certificati utilizzati per l'autenticazione di accesso di rete con Extensible Authentication Protocol\-Transport Layer Security \(EAP\-TLS\) Protected Extensible Authentication Protocol\-Transport Layer Security \(PEAP\-TLS\) e PEAP\-Microsoft Challenge Handshake Authentication Protocol versione 2 \ (v2\ MS\-CHAP) deve soddisfare i requisiti per i certificati x. 509 e lavoro per le connessioni che utilizzano Secure Socket Layer/Transport Level Security (TLS/SSL). I certificati client e server sono requisiti aggiuntivi.

>[!IMPORTANT]
>In questo argomento vengono fornite istruzioni per la configurazione di modelli di certificato. Per usare queste istruzioni, è necessario avere distribuito \(PKI\) propria infrastruttura a chiave pubblica con Servizi certificati Active Directory \(AD CS\).

## <a name="minimum-server-certificate-requirements"></a>Requisiti del certificato server minimo

Con PEAP\-MS\-CHAP v2, PEAP\ TLS o EAP\-TLS come metodo di autenticazione, il server dei criteri di rete deve utilizzare un certificato del server che soddisfi i requisiti del certificato server minimo. 

I computer client possono essere configurati per convalidare i certificati del server utilizzando il **Convalida certificato server** opzione nel computer client o in Criteri di gruppo. 

Il computer client accetta il tentativo di autenticazione del server quando il certificato del server soddisfi i requisiti seguenti:

- Il nome del soggetto contiene un valore. Se si utilizza un certificato per il server che esegue Server dei criteri rete (NPS) con un nome soggetto vuoto, il certificato non è disponibile per l'autenticazione del server dei criteri di rete. Per configurare il modello di certificato con un nome soggetto:

    1. Aprire modelli di certificato.
    2. Nel riquadro dei dettagli fare doppio clic sul modello di certificato che si desidera modificare e quindi fare clic su **proprietà**.
    3. Fare clic su di **nome soggetto** scheda e quindi fare clic su **genera a partire da queste informazioni di Active Directory**.
    4. In **formato nome oggetto**, selezionare un valore diverso da **Nessuno**.

- Il certificato del computer nel server è concatenato a un'autorità di certificazione (CA) radice attendibile e supera tutti i controlli che vengono eseguiti da CryptoAPI e vengono specificati nel criterio di accesso remoto o criteri di rete.

- Il certificato del computer per il server dei criteri di rete o server VPN è configurato con lo scopo di autenticazione Server nelle estensioni di utilizzo chiave esteso (EKU). (L'identificatore di oggetto per l'autenticazione Server è 1.3.6.1.5.5.7.3.1).

- Il certificato del server è configurato con un valore algoritmo **RSA**. Per configurare l'impostazione di crittografia:

    1. Aprire modelli di certificato.
    2. Nel riquadro dei dettagli fare doppio clic sul modello di certificato che si desidera modificare e quindi fare clic su **proprietà**.
    3. Fare clic su di **crittografia** scheda. In **nome dell'algoritmo**, fare clic su **RSA**. Assicurarsi che **dimensioni minime chiave** è impostato su **2048**.

- L'estensione di nome alternativo soggetto (SubjectAltName), se usato, deve contenere il nome DNS del server. Per configurare il modello di certificato con il nome del sistema DNS (Domain Name) del server di registrazione: 

    1. Aprire modelli di certificato.
    2. Nel riquadro dei dettagli fare doppio clic sul modello di certificato che si desidera modificare e quindi fare clic su **proprietà**.
    3. Fare clic su di **nome soggetto** scheda e quindi fare clic su **genera a partire da queste informazioni di Active Directory**.
    4. In **queste informazioni vengono incluse nel nome soggetto alternativo**selezionare **nome DNS**.

Quando si utilizza il protocollo PEAP ed EAP-TLS, server dei criteri di rete visualizzare un elenco di tutti i certificati installati nell'archivio certificati del computer, con le eccezioni seguenti:

- Non vengono visualizzati i certificati che non contengono lo scopo di autenticazione Server nelle estensioni EKU.

- Non vengono visualizzati i certificati che non contengono un nome soggetto.

- Il Registro di sistema e non vengono visualizzati i certificati di accesso con smart card.

Per ulteriori informazioni, vedere [distribuire certificati Server per le distribuzioni Wireless e cablate 802.1 X](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments).

## <a name="minimum-client-certificate-requirements"></a>Requisiti del certificato client minimo

Con EAP-TLS o PEAP-TLS, il server accetta il tentativo di autenticazione client quando il certificato soddisfa i requisiti seguenti:

- Il certificato client emesso da una CA dell'organizzazione o mappato a un account utente o computer in servizi di dominio Active Directory \(AD DS\).

- Il certificato utente o computer nel catene del client a una CA radice attendibile include lo scopo di autenticazione Client nelle estensioni EKU \ (l'identificatore di oggetto per l'autenticazione Client è 1.3.6.1.5.5.7.3.2\ #) e non i controlli che vengono eseguite da CryptoAPI e che vengono specificati nel criterio di accesso remoto o criteri di rete né i controlli di identificatore di oggetto certificato specificati nei criteri di rete dei criteri di rete.

- Il client 802.1 X non utilizza i certificati basati sul Registro di sistema che sono l'accesso con smart card o certificati protetta da password.

- Per i certificati utente, l'estensione \(SubjectAltName\) nome alternativo del soggetto nel certificato contiene l'entità utente nome \(UPN\). Per configurare il nome UPN in un modello di certificato:

    1. Aprire modelli di certificato.
    2. Nel riquadro dei dettagli fare doppio clic sul modello di certificato che si desidera modificare e quindi fare clic su **proprietà**.
    3. Fare clic su di **nome soggetto** scheda e quindi fare clic su **genera a partire da queste informazioni di Active Directory**.
    4. In **queste informazioni vengono incluse nel nome soggetto alternativo**selezionare **dell'entità utente nome \(UPN\)**.

- Per i certificati del computer, l'estensione \(SubjectAltName\) nome alternativo del soggetto del certificato deve contenere il nome di dominio completo nome \(FQDN\) del client, denominato anche il *nome DNS*. Per configurare questo nome nel modello di certificato:

    1. Aprire modelli di certificato.
    2. Nel riquadro dei dettagli fare doppio clic sul modello di certificato che si desidera modificare e quindi fare clic su **proprietà**.
    3. Fare clic su di **nome soggetto** scheda e quindi fare clic su **genera a partire da queste informazioni di Active Directory**.
    4. In **queste informazioni vengono incluse nel nome soggetto alternativo**selezionare **nome DNS**.

Con PEAP\ TLS ed EAP\-TLS, i client visualizzare un elenco di tutti i certificati installati nello snap-in certificati, con le eccezioni seguenti:

- I client wireless non visualizzano il Registro di sistema e i certificati di accesso con smart card. 

- I client wireless e i client VPN non vengono visualizzano i certificati protetti da password. 

- Non vengono visualizzati i certificati che non contengono lo scopo di autenticazione Client nelle estensioni EKU.


Per ulteriori informazioni sui criteri di rete, vedere [Server dei criteri di rete (NPS)](nps-top.md).
