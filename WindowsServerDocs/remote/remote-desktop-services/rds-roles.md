---
title: Ruoli di Servizi Desktop remoto
description: Descrive i componenti di un servizio di hosting del desktop.
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 07/06/2018
ms.topic: article
author: heidilohr
manager: dougkim
ms.openlocfilehash: 48efbffa4f82b707b63e33e6416da43eb105221f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844722"
---
# <a name="remote-desktop-services-roles"></a>Ruoli di Servizi Desktop remoto

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Questo articolo descrive i ruoli all'interno di un ambiente di Servizi Desktop remoto.

## <a name="remote-desktop-session-host"></a>Host sessione Desktop remoto

L'Host sessione Desktop remoto (Host sessione Desktop remoto) contiene le app basate su sessione e desktop che si condivide con utenti. Gli utenti ottengono questi computer desktop e le app tramite uno dei client Desktop remoto eseguiti in Windows, MacOS, iOS e Android. Gli utenti possono inoltre connettersi tramite un browser supportato usando il client web.

È possibile organizzare i desktop e App in uno o più server Host sessione Desktop remoto, denominato "raccolte". È possibile personalizzare queste raccolte per specifici gruppi di utenti all'interno di ogni tenant. Ad esempio, è possibile creare una raccolta in cui un gruppo di utenti specifico possa accedere alle App specifiche, ma tutti gli utenti all'esterno del gruppo designato non sarà in grado di accedere alle App gestite.

Per le distribuzioni di piccole dimensioni, è possibile installare le applicazioni direttamente nei server Host sessione Desktop remoto. Per distribuzioni più grandi, è consigliabile creare un'immagine di base e il provisioning delle macchine virtuali da tale immagine.

È possibile espandere le raccolte aggiungendo macchine virtuali del server Host sessione Desktop remoto a una farm di raccolta con ogni macchina virtuale host sessione Desktop remoto in una raccolta assegnata allo stesso set di disponibilità. Questo garantisce una maggiore disponibilità di raccolta e aumenta la scalabilità per supportare più utenti o le applicazioni con intensa attività di risorse.

Nella maggior parte dei casi, più utenti condividono lo stesso server di Host sessione Desktop remoto, che usa in modo più efficiente le risorse di Azure per una soluzione di hosting del desktop. In questa configurazione, gli utenti devono accedere a raccolte con gli account senza privilegi di amministratore. È possibile anche assegnare alcuni utenti un accesso amministrativo completo al desktop remoto tramite la creazione di insiemi di desktop personali della sessione.

È possibile personalizzare ulteriormente creando e caricando un disco rigido virtuale con il sistema operativo Windows Server che è possibile usare come modello per la creazione di nuove macchine virtuali Host sessione Desktop remoto i desktop.

Per informazioni, vedi gli articoli seguenti:

* [Servizi Desktop remoto: archiviazione sicura dei dati](rds-plan-secure-data-storage.md)
* [Caricare un disco rigido virtuale generalizzato e usarlo per creare nuove macchine virtuali in Azure](https://docs.microsoft.com/azure/virtual-machines/windows/upload-generalized-managed?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json)
* [Aggiornare la raccolta di host sessione Desktop remoto (modello ARM)](https://azure.microsoft.com/resources/templates/rds-update-rdsh-collection/)

## <a name="remote-desktop-connection-broker"></a>Gestore connessione Desktop remoto

Gestore connessione Desktop remoto (Gestore connessione desktop remoto) gestisce le connessioni desktop remoto in ingresso per le server farm Host sessione Desktop remoto. Gestore connessione desktop remoto gestisce le connessioni a entrambe le raccolte di raccolte di RemoteApp e desktop completi. Gestore connessione desktop remoto può bilanciare il carico tra i server della raccolta quando si effettuano nuove connessioni. Se si disconnette una sessione, Gestore connessione desktop remoto si riconnetterà all'utente di server Host sessione Desktop remoto corretto e la sessione interrotta, che continua a esistere nella farm Host sessione Desktop remoto.

È necessario installare i certificati digitali server Gestore connessione desktop remoto sia il client per supportare l'accesso single sign-on e la pubblicazione delle applicazioni di corrispondenza. Durante lo sviluppo o test di una rete, è possibile usare un certificato autofirmato e generato automaticamente. Tuttavia, i servizi rilasciati richiedono un certificato digitale da un'autorità di certificazione attendibile. Nome con cui il certificato deve essere lo stesso come l'interna dominio nome (completo) della macchina virtuale Gestore connessione desktop remoto.

È possibile installare il Gestore connessione desktop remoto di Windows Server 2016 nella macchina virtuale stessa come Active Directory Domain Services per ridurre i costi. Se è necessario scalare orizzontalmente a più utenti, è anche possibile aggiungere altre macchine virtuali di Gestore connessione desktop remoto in stesso set di disponibilità per creare un cluster di Gestore connessione desktop remoto.

Prima di creare un cluster di Gestore connessione desktop remoto, è necessario distribuire un Database SQL di Azure nell'ambiente del tenant o creare un gruppo di disponibilità AlwaysOn di SQL Server.

Per informazioni, vedi gli articoli seguenti:

* [Aggiungere il server Gestore connessione desktop remoto alla distribuzione e configurare la disponibilità elevata](rds-connection-broker-cluster.md)
* [Database SQL](desktop-hosting-service.md#sql-database) nel servizio di hosting del Desktop.

## <a name="remote-desktop-gateway"></a>Gateway Desktop remoto

Gateway Desktop remoto (Gateway Desktop remoto) concede agli utenti accesso reti pubbliche per Windows desktop e applicazioni ospitate in servizi cloud di Microsoft Azure.

Il componente di Gateway Desktop remoto utilizza Secure Sockets Layer (SSL) per crittografare il canale di comunicazione tra client e server. La macchina virtuale Gateway Desktop remoto deve essere accessibile tramite un indirizzo IP pubblico che consente le connessioni TCP in ingresso alla porta 443 e porta 3391 alcuna connessione UDP in ingresso. Ciò consente agli utenti di connettersi tramite internet usando il protocollo di trasporto di comunicazione HTTPS e il protocollo UDP, rispettivamente.

I certificati digitali installati nel server e client devono corrispondere per il corretto funzionamento. Durante lo sviluppo o test di una rete, è possibile usare un certificato autofirmato e generato automaticamente. Tuttavia, un servizio rilasciato richiede un certificato da un'autorità di certificazione attendibile. Il nome del certificato deve corrispondere il nome di dominio completo usato per accedere a Gateway Desktop remoto, se il FQDN è il nome DNS accessibile pubblicamente degli indirizzo IP pubblico o il record DNS CNAME che punta all'indirizzo IP pubblico.

Per i tenant con un numero di utenti, i ruoli di accesso Web desktop remoto e Gateway Desktop remoto possono essere combinati in una singola macchina virtuale per ridurre i costi. È anche possibile aggiungere altre macchine virtuali del Gateway Desktop remoto a una farm di Gateway Desktop remoto per aumentare la disponibilità del servizio e la scalabilità orizzontale a più utenti. Macchine virtuali nelle farm di Gateway Desktop remoto più grandi devono essere configurate in un set con bilanciamento del carico. Quando si usa Gateway Desktop remoto in una macchina virtuale Windows Server 2016, ma è quando si esegue in una macchina virtuale Windows Server 2012 R2 non è necessaria l'affinità IP di.

Per informazioni, vedi gli articoli seguenti:

* [Ottenere una disponibilità elevata in primo piano web di Web desktop remoto e Gateway](rds-rdweb-gateway-ha.md)
* [Servizi Desktop remoto - Accesso ovunque](rds-plan-access-from-anywhere.md)
* [Servizi Desktop remoto - multi-factor authentication](rds-plan-mfa.md)

## <a name="remote-desktop-web-access"></a>Accesso Web Desktop remoto

Remote Desktop Web Access (accesso Web desktop remoto) consente agli utenti l'accesso desktop e applicazioni tramite un portale web e li avvia tramite l'applicazione client di Desktop remoto Microsoft native del dispositivo. È possibile usare il portale web per pubblicare i desktop Windows e alle applicazioni di Windows e dispositivi client non Windows ed è possibile pubblicare in modo selettivo desktop o le app a utenti o gruppi specifici.

Accesso Web desktop remoto richiede Internet Information Services (IIS) per il corretto funzionamento. Una connessione Hypertext Transfer Protocol Secure (HTTPS) fornisce un canale di comunicazione crittografato tra i client e il server Web desktop remoto. La macchina virtuale di accesso Web desktop remoto deve essere accessibile tramite un indirizzo IP pubblico che consente le connessioni TCP in ingresso alla porta 443 per consentire agli utenti del tenant per la connessione da internet usando il protocollo di trasporto delle comunicazioni HTTPS.

I certificati digitali di corrispondenza deve essere installato nel server e client. Per lo sviluppo e a scopo di test, può trattarsi di un certificato autofirmato e generato automaticamente. Per un servizio, rilasciato il certificato digitale deve essere ottenuto da un'autorità di certificazione attendibile. Il nome del certificato deve corrispondere il completamente dominio nome completo (FQDN) utilizzato per accedere a accesso Web desktop remoto. Nomi di dominio completi possibili includono il nome DNS accessibile pubblicamente per l'indirizzo IP pubblico e il record DNS CNAME che punta all'indirizzo IP pubblico.

Per i tenant con un numero di utenti, è possibile ridurre i costi tramite la combinazione di carichi di lavoro accesso Web desktop remoto e Gateway Desktop remoto in una singola macchina virtuale. È anche possibile aggiungere altre macchine virtuali di Web desktop remoto a una farm di accesso Web desktop remoto per aumentare la disponibilità del servizio e la scalabilità orizzontale a più utenti. In una farm di accesso Web desktop remoto con più macchine virtuali, è possibile configurare le macchine virtuali in un set con bilanciamento del carico.

Per altre informazioni su come configurare l'accesso Web desktop remoto, vedere gli articoli seguenti:

* [Configurare il client di web Desktop remoto per gli utenti](clients/remote-desktop-web-client-admin.md)
* [Creare e distribuire una raccolta di Servizi Desktop remoto](rds-create-collection.md)
* [Creare una raccolta di Servizi Desktop remoto per desktop e App per l'esecuzione](rds-create-collection.md)

## <a name="remote-desktop-licensing"></a>Servizio licenze Desktop remoto

Attivato i server licenze Desktop remoto (servizio licenze Desktop remoto) consentono agli utenti di connettersi ai server Host sessione Desktop remoto che ospita i desktop e le App del tenant. Ambienti tenant sono in genere con il server licenze Desktop remoto già installato, ma per gli ambienti di hosting è necessario configurare il server in modalità utente.

Il provider di servizi deve sufficiente RDS licenze SAL (Subscriber Access) per coprire tutti univoci (non simultanei) gli utenti autorizzati che l'accesso al servizio di ogni mese. I provider di servizi possono acquistare Microsoft Azure Infrastructure Services direttamente e possono acquistare SALs tramite il programma Microsoft Service Provider Licensing Agreement (SPLA). I clienti che cercano una soluzione di desktop ospitata devono acquistare la soluzione in hosting completata (Azure e servizi desktop remoto) dal provider del servizio.

Tenant di dimensioni ridotte può ridurre i costi tramite la combinazione di file server e componenti di servizio licenze Desktop remoto in una singola macchina virtuale. Per garantire una maggiore disponibilità del servizio, i tenant possono distribuire due macchine virtuali di server licenze Desktop remoto nello stesso set di disponibilità. Tutti i server di desktop remoto nell'ambiente del tenant sono associati entrambi i server licenze Desktop remoto per mantenere gli utenti in grado di connettersi alle nuove sessioni anche se uno dei server si arresta.

Per informazioni, vedi gli articoli seguenti:

* [La distribuzione di servizi desktop remoto di licenza con licenze di accesso client (CAL)](rds-client-access-license.md)
* [Attivare il server licenze di Servizi Desktop remoto](rds-activate-license-server.md)
* [Tenere traccia delle licenze di accesso client (CAL) di Servizi Desktop remoto](rds-track-cals.md)
* [Microsoft Volume Licensing: le opzioni per i provider di servizi di licenza](https://www.microsoft.com/en-us/Licensing/licensing-programs/spla-program.aspx)