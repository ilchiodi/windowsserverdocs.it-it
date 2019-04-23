---
title: Configurare i modelli di certificato per i requisiti per PEAP ed EAP
description: In questo argomento vengono fornite informazioni sull'uso dei certificati con Server dei criteri di rete e accesso remoto in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2af0a1df-5c44-496b-ab11-5bc340dc96f0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 41f6f88705fa3d58be695fd825d960e7df21cd24
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885882"
---
# <a name="configure-certificate-templates-for-peap-and-eap-requirements"></a>Configurare i modelli di certificato per i requisiti per PEAP ed EAP

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Tutti i certificati utilizzati per l'autenticazione di accesso di rete con Extensible Authentication Protocol\-Transport Layer Security \(EAP\-TLS\), Protected Extensible Authentication Protocol\- Transport Layer Security \(PEAP\-TLS\)e PEAP\-Microsoft Challenge Handshake Authentication Protocol versione 2 \(MS\-CHAP v2\) necessario soddisfare i requisiti per i certificati X.509 e funziona per le connessioni che usano Secure Socket Layer/Transport Level Security (SSL/TLS). I certificati client e server prevedono requisiti aggiuntivi.

>[!IMPORTANT]
>In questo argomento fornisce istruzioni per configurare i modelli di certificato. Per usare queste istruzioni, è necessario avere distribuito la propria infrastruttura a chiave pubblica \(PKI\) con Servizi certificati Active Directory \(Servizi certificati Active Directory\).

## <a name="minimum-server-certificate-requirements"></a>Requisiti del certificato server minimo

Con PEAP\-MS\-CHAP v2, PEAP\-TLS o EAP\-TLS come metodo di autenticazione, i criteri di rete deve usare un certificato del server che soddisfi i requisiti del certificato server minimo. 

I computer client possono essere configurati per convalidare i certificati del server utilizzando il **Convalida certificato server** option nel computer client o nei criteri di gruppo. 

Il client accetta il tentativo di autenticazione del server quando il certificato del server soddisfi i requisiti seguenti:

- Il nome dell'oggetto contiene un valore. Se si esegue un certificato per il server che esegue Server dei criteri rete (NPS) che ha un nome di oggetto vuoto, il certificato non è disponibile per l'autenticazione di criteri di rete. Per configurare il modello di certificato con un nome soggetto:

    1. Aprire Modelli di certificato.
    2. Nel riquadro dei dettagli, fare clic sul modello di certificato che si desidera modificare e quindi fare clic su **proprietà** .
    3. Scegliere il **nome soggetto** scheda e quindi fare clic su **creazione dalle informazioni Active Directory**.
    4. Nelle **formato nome soggetto**, selezionare un valore diverso da **None**.

- Il certificato del computer nel server è concatenato a un'autorità di certificazione radice attendibile (CA) e supera uno dei controlli che vengono eseguiti da CryptoAPI e che sono specificato nel criterio di accesso remoto o criteri di rete.

- Il certificato del computer per il server dei criteri di rete o VPN è configurato con lo scopo di autenticazione Server nelle estensioni di utilizzo chiavi avanzato (EKU). (L'identificatore di oggetto per l'autenticazione Server è 1.3.6.1.5.5.7.3.1).

- Configurare il certificato del server con l'impostazione di crittografia necessari:

    1. Aprire Modelli di certificato.
    2. Nel riquadro dei dettagli, fare clic sul modello di certificato che si desidera modificare e quindi fare clic su **proprietà**.
    3. Scegliere il **Cryptography** scheda e assicurarsi di configurare le opzioni seguenti:
       - **Categoria provider:** Provider di archiviazione chiavi
       - **Nome dell'algoritmo:** RSA
       - **Provider:** Microsoft piattaforma Crypto Provider
       - **Dimensione minima della chiave:** 2048
       - **Algoritmo hash:** SHA2
    4. Fare clic su **Avanti**.

- L'estensione di nome alternativo soggetto (SubjectAltName), se utilizzata, deve contenere il nome DNS del server. Per configurare il modello di certificato con il nome di sistema DNS (Domain Name) del server di registrazione: 

    1. Aprire Modelli di certificato.
    2. Nel riquadro dei dettagli, fare clic sul modello di certificato che si desidera modificare e quindi fare clic su **proprietà** .
    3. Scegliere il **nome soggetto** scheda e quindi fare clic su **creazione dalle informazioni Active Directory**.
    4. Nelle **includere le seguenti informazioni nel nome alternativo soggetto**, selezionare **il nome DNS**.

Quando si utilizza PEAP ed EAP-TLS, NPSs visualizzare un elenco di tutti i certificati installati nell'archivio certificati del computer, con le eccezioni seguenti:

- Non vengono visualizzati i certificati che non hanno lo scopo di autenticazione Server nelle estensioni EKU.

- Non vengono visualizzati i certificati che non contengono un nome soggetto.

- Il Registro di sistema e i certificati di accesso con smart card non vengono visualizzati.

Per altre informazioni, vedere [distribuire certificati Server per le distribuzioni Wireless e cablate 802.1 X](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments).

## <a name="minimum-client-certificate-requirements"></a>Requisiti del certificato client minima

Con EAP-TLS o PEAP-TLS, il server accetta il tentativo di autenticazione client quando il certificato soddisfa i requisiti seguenti:

- Il certificato client emesso da una CA dell'organizzazione o associato a un account utente o computer in servizi di dominio Active Directory \(Active Directory Domain Services\).

- Il certificato utente o computer nel client sia concatenato a una CA radice attendibile include lo scopo di autenticazione Client nelle estensioni EKU \(l'identificatore di oggetto per l'autenticazione Client viene 1.3.6.1.5.5.7.3.2\)e non riesce né il controlli che vengono eseguite da CryptoAPI e che vengono specificati in Criteri di accesso remoto o criteri di rete né i controlli di identificatore di oggetto certificato che vengono specificati in Criteri di rete NPS.

- Il client 802.1x non usa i certificati basati sul Registro di sistema che sono l'accesso con smart card o certificati protetto da password.

- Per i certificati utente, nome alternativo del soggetto \(SubjectAltName\) estensione del certificato contiene il nome dell'entità utente \(UPN\). Per configurare l'UPN in un modello di certificato:

    1. Aprire Modelli di certificato.
    2. Nel riquadro dei dettagli, fare clic sul modello di certificato che si desidera modificare e quindi fare clic su **proprietà**.
    3. Scegliere il **nome soggetto** scheda e quindi fare clic su **creazione dalle informazioni Active Directory**.
    4. Nelle **includere le seguenti informazioni nel nome alternativo soggetto**, selezionare **nome entità utente \(UPN\)**.

- Per i certificati del computer, nome alternativo del soggetto \(SubjectAltName\) estensione del certificato deve contenere il nome di dominio completo \(FQDN\) del client, chiamato anche il  *Nome DNS*. Per configurare il nome specificato nel modello di certificato:

    1. Aprire Modelli di certificato.
    2. Nel riquadro dei dettagli, fare clic sul modello di certificato che si desidera modificare e quindi fare clic su **proprietà**.
    3. Scegliere il **nome soggetto** scheda e quindi fare clic su **creazione dalle informazioni Active Directory**.
    4. Nelle **includere le seguenti informazioni nel nome alternativo soggetto**, selezionare **il nome DNS**.

Con PEAP\-TLS ed EAP\-TLS, i client di visualizzare un elenco di tutti i certificati installati nello snap-in certificati, con le eccezioni seguenti:

- I computer client wireless non vengono visualizzati in base al Registro di sistema e i certificati di accesso con smart card. 

- I client wireless e VPN non vengono visualizzati i certificati protetto da password. 

- Non vengono visualizzati i certificati che non hanno lo scopo di autenticazione Client nelle estensioni EKU.


Per altre informazioni sui criteri di rete, vedere [Strumentazione gestione Windows (NPS, Network Policy Server)](nps-top.md).
