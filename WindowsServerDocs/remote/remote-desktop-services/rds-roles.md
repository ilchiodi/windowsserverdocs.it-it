---
title: Ruoli di Servizi Desktop remoto
description: Descrive i componenti di un servizio di hosting desktop.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 07/06/2018
ms.topic: article
author: heidilohr
manager: dougkim
ms.openlocfilehash: f26f75b8ce3f5438362c15b84aeca9339b95ebbc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71387196"
---
# <a name="remote-desktop-services-roles"></a>Ruoli di Servizi Desktop remoto

>Si applica a: Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016

Questo articolo descrive i ruoli in un ambiente Servizi Desktop remoto.

## <a name="remote-desktop-session-host"></a>Host sessione Desktop remoto

Host sessione Desktop remoto contiene i desktop e le app basate su sessione che condividi con gli utenti. Gli utenti ottengono questi desktop e queste app tramite uno dei client Desktop remoto eseguiti in Windows, MacOS, iOS e Android. Gli utenti possono inoltre connettersi tramite un browser supportato usando il client Web.

Puoi organizzare i desktop e le app in uno o più server Host sessione Desktop remoto, denominati "raccolte". Queste raccolte possono essere personalizzate per specifici gruppi di utenti all'interno di ogni tenant. Puoi ad esempio creare una raccolta in cui un gruppo di utenti specifico può accedere a determinate app, mentre tutti gli utenti all'esterno del gruppo designato non saranno in grado di accedere a tali app.

Per distribuzioni di piccole dimensioni, puoi installare le applicazioni direttamente nei server Host sessione Desktop remoto. Per distribuzioni più grandi, è consigliabile creare un'immagine di base ed effettuare il provisioning delle macchine virtuali da tale immagine.

Puoi espandere le raccolte aggiungendo macchine virtuali del server Host sessione Desktop remoto a una farm di raccolta con ogni macchina virtuale Host sessione Desktop remoto di una raccolta assegnata allo stesso set di disponibilità. In questo modo garantisci una maggiore disponibilità della raccolta e aumenti la scalabilità per supportare più utenti o applicazioni che usano in modo intensivo le risorse.

Nella maggior parte dei casi, più utenti condividono lo stesso server Host sessione Desktop remoto, che usa in modo più efficiente le risorse di Azure per una soluzione di hosting desktop. In questa configurazione gli utenti devono accedere a raccolte con account non amministrativi. Puoi anche assegnare ad alcuni utenti l'accesso amministrativo completo al desktop remoto creando raccolte di desktop sessione personali.

Puoi personalizzare ulteriormente i desktop creando e caricando un disco rigido virtuale con il sistema operativo Windows Server che può essere usato come modello per la creazione di nuove macchine virtuali Host sessione Desktop remoto.

Per altre informazioni, vedere gli articoli seguenti:

* [Servizi Desktop remoto - Archiviazione sicura dei dati](rds-plan-secure-data-storage.md)
* [Caricare un disco rigido virtuale generalizzato e usarlo per creare nuove macchine virtuali in Azure](https://docs.microsoft.com/azure/virtual-machines/windows/upload-generalized-managed?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json)
* [Aggiornare la raccolta Host sessione Desktop remoto (modello ARM)](https://azure.microsoft.com/resources/templates/rds-update-rdsh-collection/)

## <a name="remote-desktop-connection-broker"></a>Gestore connessione Desktop remoto

Gestore connessione Desktop remoto gestisce le connessioni Desktop remoto in ingresso a server farm Host sessione Desktop remoto. Gestisce le connessioni sia a raccolte di desktop completi che a raccolte di app remote. Gestore connessione Desktop remoto può bilanciare il carico tra i server della raccolta quando vengono effettuate nuove connessioni. In caso di disconnessione di una sessione, Gestore connessione Desktop remoto riconnetterà l'utente al server Host sessione Desktop remoto corretto e alla sessione interrotta, che continua a esistere nella farm Host sessione Desktop remoto.

Dovrai installare i certificati digitali corrispondenti sia nel server Gestore connessione Desktop remoto sia nel client per supportare Single Sign-On e la pubblicazione delle applicazioni. Durante lo sviluppo o il test di una rete, puoi usare un certificato autofirmato e generato automaticamente. I servizi rilasciati richiedono tuttavia un certificato digitale di un'autorità di certificazione attendibile. Il nome che assegni al certificato deve corrispondere al nome di dominio completo (FQDN, Fully Qualified Domain Name) della macchina virtuale Gestore connessione Desktop remoto.

Puoi installare Gestore connessione Desktop remoto di Windows Server 2016 nella stessa macchina virtuale di Active Directory Domain Services per ridurre i costi. Se è necessario aumentare le istanze per più utenti, puoi anche aggiungere altre macchine virtuali Gestore connessione Desktop remoto nello stesso set di disponibilità per creare un cluster di Gestore connessione Desktop remoto.

Prima di creare un cluster di Gestore connessione Desktop remoto, devi distribuire un database SQL di Azure nell'ambiente del tenant o creare un gruppo di disponibilità AlwaysOn di SQL Server.

Per altre informazioni, vedere gli articoli seguenti:

* [Aggiungere il server Gestore connessione Desktop remoto alla distribuzione e configurare la disponibilità elevata](rds-connection-broker-cluster.md)
* [Database SQL](desktop-hosting-service.md#sql-database) in Servizio di hosting desktop

## <a name="remote-desktop-gateway"></a>Gateway Desktop remoto

Gateway Desktop remoto concede agli utenti di reti pubbliche l'accesso alle applicazioni e ai desktop Windows ospitati in servizi cloud di Microsoft Azure.

Il componente Gateway Desktop remoto usa Secure Sockets Layer (SSL) per crittografare il canale di comunicazioni tra client e server. La macchina virtuale Gateway Desktop remoto deve essere accessibile tramite un indirizzo IP pubblico che consente le connessioni TCP in ingresso alla porta 443 e le connessioni UDP in ingresso alla porta 3391. In questo modo gli utenti possono connettersi tramite Internet usando rispettivamente il protocollo di trasporto di comunicazioni HTTPS e il protocollo UDP.

Per il corretto funzionamento, i certificati digitali installati nel server e nel client devono corrispondere. Durante lo sviluppo o il test di una rete, puoi usare un certificato autofirmato e generato automaticamente. I servizi rilasciati richiedono tuttavia un certificato di un'autorità di certificazione attendibile. Il nome del certificato deve corrispondere all'FQDN usato per accedere a Gateway Desktop remoto, indipendentemente dal fatto che l'FQDN sia il nome DNS accessibile pubblicamente dell'indirizzo IP pubblico o il record DNS CNAME che punta all'indirizzo IP pubblico.

Per i tenant con un numero inferiore di utenti, i ruoli di Accesso Web Desktop remoto e Gateway Desktop remoto possono essere combinati in una singola macchina virtuale per ridurre i costi. Puoi anche aggiungere altre macchine virtuali Gateway Desktop remoto a una farm Gateway Desktop remoto per aumentare la disponibilità del servizio e aumentare le istanze per più utenti. Le macchine virtuali in farm Gateway Desktop remoto di maggiori dimensioni devono essere configurate in un set con bilanciamento del carico. L'affinità IP non è richiesta se usi Gateway Desktop remoto in una macchina virtuale Windows Server 2016, mentre è richiesta per l'esecuzione in una macchina virtuale Windows Server 2012 R2.

Per altre informazioni, vedere gli articoli seguenti:

* [Aggiungere disponibilità elevata al front-end Web e Gateway Desktop remoto](rds-rdweb-gateway-ha.md)
* [Servizi Desktop remoto - Accedere da qualsiasi luogo](rds-plan-access-from-anywhere.md)
* [Servizi Desktop remoto - Autenticazione a più fattori](rds-plan-mfa.md)

## <a name="remote-desktop-web-access"></a>Accesso Web Desktop remoto

Accesso Web Desktop remoto consente agli utenti di accedere a desktop e applicazioni tramite un portale Web ed esegue l'avvio tramite l'applicazione client Desktop remoto Microsoft nativa del dispositivo. Puoi usare il portale Web per pubblicare applicazioni e desktop Windows in dispositivi client Windows e non Windows, nonché per pubblicare in modo selettivo desktop o app per utenti o gruppi specifici.

Accesso Web Desktop remoto richiede Internet Information Services (IIS) per funzionare correttamente. Una connessione Hypertext Transfer Protocol Secure (HTTPS) fornisce un canale di comunicazioni crittografato tra i client e il server Web Desktop remoto. La macchina virtuale Accesso Web Desktop remoto deve essere accessibile tramite un indirizzo IP pubblico che consenta le connessioni TCP in ingresso alla porta 443 per permettere agli utenti del tenant di connettersi da Internet usando il protocollo di trasporto delle comunicazioni HTTPS.

Nel server e nei client devono essere installati certificati digitali corrispondenti. Ai fini dello sviluppo e dei test, è possibile usare un certificato autofirmato e generato automaticamente. Per i servizi rilasciati, il certificato digitale deve essere di un'autorità di certificazione attendibile. Il nome del certificato deve corrispondere all'FQDN usato per accedere ad Accesso Web Desktop remoto. Gli FQDN possibili includono il nome DNS accessibile pubblicamente per l'indirizzo IP pubblico e il record DNS CNAME che punta all'indirizzo IP pubblico.

Per i tenant con un numero inferiore di utenti, puoi ridurre i costi tramite la combinazione di carichi di lavoro Accesso Web Desktop remoto e Gateway Desktop remoto in una singola macchina virtuale. Puoi anche aggiungere altre macchine virtuali Web Desktop remoto a una farm di Accesso Web Desktop remoto per aumentare la disponibilità del servizio e aumentare le istanze per più utenti. In una farm di Accesso Web Desktop remoto con più macchine virtuali sarà necessario configurare le macchine virtuali in un set con bilanciamento del carico.

Per altre informazioni su come configurare Accesso Web Desktop remoto, vedi gli articoli seguenti:

* [Configurare il client Web di Desktop remoto per gli utenti](clients/remote-desktop-web-client-admin.md)
* [Creare e distribuire un insieme di Servizi Desktop remoto](rds-create-collection.md)
* [Creare una raccolta Servizi Desktop remoto per l'esecuzione di app e desktop](rds-create-collection.md)

## <a name="remote-desktop-licensing"></a>Servizio licenze Desktop remoto

I server di Servizio licenze Desktop remoto attivati consentono agli utenti di connettersi ai server Host sessione Desktop remoto che ospitano i desktop e le app del tenant. Negli ambienti tenant in genere è già installato il server di Servizio licenze Desktop remoto, ma per gli ambienti ospitati è necessario configurare il server nella modalità per utente.

Il provider di servizi necessita di licenze SAL (Subscriber Access License) di Servizi Desktop remoto sufficienti per coprire tutti gli utenti univoci (non simultanei) autorizzati che accedono al servizio ogni mese. I provider di servizi possono acquistare i servizi di infrastruttura Microsoft Azure direttamente e possono acquistare licenze SAL tramite il programma Microsoft SPLA (Service Provider Licensing Agreement). I clienti che cercano una soluzione desktop ospitata devono acquistare la soluzione ospitata completa (Azure e Servizi Desktop remoto) dal provider di servizi.

Tenant di dimensioni ridotte possono ridurre i costi tramite la combinazione di file server e componenti di Servizio licenze Desktop remoto in una singola macchina virtuale. Per garantire una maggiore disponibilità del servizio, i tenant possono distribuire due macchine virtuali server di Servizio licenze Desktop remoto nello stesso set di disponibilità. Tutti i server Desktop remoto nell'ambiente del tenant sono associati a entrambi i server di Servizio licenze Desktop remoto in modo che gli utenti possano connettersi alle nuove sessioni anche in caso di inattività di uno dei server.

Per altre informazioni, vedere gli articoli seguenti:

* [Concedere licenze CAL (Client Access License) per la distribuzione di Servizi Desktop remoto](rds-client-access-license.md))
* [Attivare il server licenze di Servizi Desktop remoto](rds-activate-license-server.md)
* [Tenere traccia delle licenze CAL di Servizi Desktop remoto](rds-track-cals.md)
* [Contratti multilicenza Microsoft: opzioni per le licenze per i provider di servizi](https://www.microsoft.com/en-us/Licensing/licensing-programs/spla-program.aspx)