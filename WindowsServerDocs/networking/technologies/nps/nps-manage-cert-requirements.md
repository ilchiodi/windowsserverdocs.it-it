---
title: Configurare i modelli di certificato per i requisiti per PEAP ed EAP
description: In questo argomento vengono fornite informazioni sull'utilizzo di certificati con server dei criteri di rete e accesso remoto in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 2af0a1df-5c44-496b-ab11-5bc340dc96f0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f60e5a1da1388a6dd2432a3603f83d6ca2990a29
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405411"
---
# <a name="configure-certificate-templates-for-peap-and-eap-requirements"></a>Configurare i modelli di certificato per i requisiti per PEAP ed EAP

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Tutti i certificati usati per l'autenticazione di accesso alla rete con Extensible Authentication Protocol @ no__t-0Transport Layer Security \(EAP @ no__t-2TLS @ no__t-3, Protected Extensible Authentication Protocol @ no__t-4Transport Layer Security \(PEAP @ no__t-6TLS @ no__t-7 e PEAP @ no__t-8Microsoft Challenge Handshake Authentication Protocol versione 2 \(MS @ no__t-10CHAP V2 @ no__t-11 devono soddisfare i requisiti per i certificati X. 509 e funzionano per le connessioni che usano il socket sicuro Sicurezza a livello di livello/trasporto (SSL/TLS). I certificati client e server hanno requisiti aggiuntivi.

>[!IMPORTANT]
>In questo argomento vengono fornite le istruzioni per la configurazione dei modelli di certificato. Per usare queste istruzioni, è necessario aver distribuito l'infrastruttura a chiave pubblica \(PKI @ no__t-1 con Active Directory Servizi certificati \(AD CS @ no__t-3.

## <a name="minimum-server-certificate-requirements"></a>Requisiti minimi del certificato server

Con PEAP @ no__t-0MS @ no__t-1CHAP V2, PEAP @ no__t-2TLS o EAP @ no__t-3TLS come metodo di autenticazione, il server dei criteri di server deve usare un certificato del server che soddisfi i requisiti minimi del certificato server. 

I computer client possono essere configurati per convalidare i certificati server utilizzando l'opzione **convalida certificato server** nel computer client o in criteri di gruppo. 

Il computer client accetta il tentativo di autenticazione del server quando il certificato server soddisfa i requisiti seguenti:

- Il nome del soggetto contiene un valore. Se si rilascia un certificato al server che esegue Server dei criteri di rete con un nome soggetto vuoto, il certificato non è disponibile per autenticare il server dei criteri di rete. Per configurare il modello di certificato con un nome soggetto:

    1. Aprire Modelli di certificato.
    2. Nel riquadro dei dettagli fare clic con il pulsante destro del mouse sul modello di certificato che si desidera modificare e quindi scegliere **Proprietà** .
    3. Fare clic sulla scheda **nome soggetto** , quindi fare clic su **compila da questa Active Directory informazioni**.
    4. In **formato nome soggetto**selezionare un valore diverso da **nessuno**.

- Il certificato del computer nel server viene concatenato a un'autorità di certificazione radice attendibile (CA) e non ha esito negativo alcuno dei controlli eseguiti da CryptoAPI e specificati nei criteri di accesso remoto o di rete.

- Il certificato del computer per il server dei criteri di rete o il server VPN è configurato con lo scopo di autenticazione server nelle estensioni di utilizzo chiavi avanzato (EKU). L'identificatore di oggetto per l'autenticazione del server è 1.3.6.1.5.5.7.3.1.

- Configurare il certificato del server con l'impostazione di crittografia richiesta:

    1. Aprire Modelli di certificato.
    2. Nel riquadro dei dettagli fare clic con il pulsante destro del mouse sul modello di certificato che si desidera modificare e quindi scegliere **Proprietà**.
    3. Fare clic sulla scheda **crittografia** e assicurarsi di configurare quanto segue:
       - **Categoria provider:** Provider di archiviazione chiavi
       - **Nome algoritmo:** RSA
       - **Provider** Provider di crittografia della piattaforma Microsoft
       - **Dimensioni minime chiave:** 2048
       - **Algoritmo hash:** SHA2
    4. Fare clic su **Avanti**.

- L'estensione del nome alternativo del soggetto (SubjectAltName), se utilizzata, deve contenere il nome DNS del server. Per configurare il modello di certificato con il nome del Domain Name System (DNS) del server di iscrizione: 

    1. Aprire Modelli di certificato.
    2. Nel riquadro dei dettagli fare clic con il pulsante destro del mouse sul modello di certificato che si desidera modificare e quindi scegliere **Proprietà** .
    3. Fare clic sulla scheda **nome soggetto** , quindi fare clic su **compila da questa Active Directory informazioni**.
    4. In **includere le seguenti informazioni nel nome soggetto alternativo**selezionare **nome DNS**.

Quando si usa PEAP e EAP-TLS, NPSs Visualizza un elenco di tutti i certificati installati nell'archivio certificati del computer, con le eccezioni seguenti:

- I certificati che non contengono lo scopo di autenticazione server nelle estensioni EKU non vengono visualizzati.

- I certificati che non contengono un nome soggetto non vengono visualizzati.

- I certificati di accesso smart card e basati sul Registro di sistema non vengono visualizzati.

Per altre informazioni, vedere [distribuire certificati server per le distribuzioni wireless e cablate 802.1 x](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments).

## <a name="minimum-client-certificate-requirements"></a>Requisiti minimi del certificato client

Con EAP-TLS o PEAP-TLS, il server accetta il tentativo di autenticazione client quando il certificato soddisfa i requisiti seguenti:

- Il certificato client viene emesso da una CA globale (Enterprise) o mappato a un account utente o computer in Active Directory Domain Services \(AD DS @ no__t-1.

- Il certificato utente o computer nel client si concatena a una CA radice attendibile, include lo scopo di autenticazione client nelle estensioni EKU \(The identificatore oggetto per l'autenticazione client è 1.3.6.1.5.5.7.3.2 @ no__t-1 e non ha esito negativo né i controlli eseguita da CryptoAPI e specificate nei criteri di accesso remoto o nei criteri di rete, né nei controlli dell'identificatore di oggetto certificato specificati nei criteri di rete NPS.

- Il client 802.1 X non usa certificati basati sul Registro di sistema che sono certificati con accesso tramite smart card o protetti da password.

- Per i certificati utente, il nome alternativo del soggetto \(SubjectAltName @ no__t-1 Extension nel certificato contiene il nome dell'entità utente \(UPN @ no__t-3. Per configurare l'UPN in un modello di certificato:

    1. Aprire Modelli di certificato.
    2. Nel riquadro dei dettagli fare clic con il pulsante destro del mouse sul modello di certificato che si desidera modificare e quindi scegliere **Proprietà**.
    3. Fare clic sulla scheda **nome soggetto** , quindi fare clic su **compila da questa Active Directory informazioni**.
    4. In **includere le seguenti informazioni nel nome soggetto alternativo**selezionare **nome entità utente \(UPN @ no__t-3**.

- Per i certificati del computer, il nome alternativo del soggetto \(SubjectAltName @ no__t-1 Extension nel certificato deve contenere il nome di dominio completo \(FQDN @ no__t-3 del client, denominato anche *nome DNS*. Per configurare questo nome nel modello di certificato:

    1. Aprire Modelli di certificato.
    2. Nel riquadro dei dettagli fare clic con il pulsante destro del mouse sul modello di certificato che si desidera modificare e quindi scegliere **Proprietà**.
    3. Fare clic sulla scheda **nome soggetto** , quindi fare clic su **compila da questa Active Directory informazioni**.
    4. In **includere le seguenti informazioni nel nome soggetto alternativo**selezionare **nome DNS**.

Con PEAP @ no__t-0TLS e EAP @ no__t-1TLS, i client visualizzano un elenco di tutti i certificati installati nello snap-in certificati, con le eccezioni seguenti:

- I client senza fili non visualizzano certificati di accesso con smart card e basati sul Registro di sistema. 

- I client wireless e i client VPN non visualizzano i certificati protetti da password. 

- I certificati che non contengono lo scopo di autenticazione client nelle estensioni EKU non vengono visualizzati.


Per ulteriori informazioni su NPS, vedere [Server dei criteri di rete (NPS)](nps-top.md).
