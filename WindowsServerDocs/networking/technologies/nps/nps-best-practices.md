---
title: Procedure consigliate per il Server dei criteri di rete
description: Questo argomento descrive le procedure consigliate per la distribuzione e la gestione di server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 90e544bd-e826-4093-8c3b-6a6fc2dfd1d6
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 4e6e6d2612af80bdaaa3900414bb08c3f0c18ea3
ms.sourcegitcommit: 3c3dfee8ada0083f97a58997d22d218a5d73b9c4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/03/2020
ms.locfileid: "80639908"
---
# <a name="network-policy-server-best-practices"></a>Procedure consigliate per il Server dei criteri di rete

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per informazioni sulle procedure consigliate per la distribuzione e la gestione di server dei criteri di rete \(server dei criteri di rete\).

Nelle sezioni seguenti vengono fornite le procedure consigliate per diversi aspetti della distribuzione di NPS.

## <a name="accounting"></a>Contabilità

Di seguito sono riportate le procedure consigliate per la registrazione di NPS.

Esistono due tipi di contabilità, o registrazione, in NPS:

- Registrazione eventi per NPS. È possibile utilizzare la registrazione degli eventi per registrare gli eventi del server dei criteri di rete nei registri eventi di sistema e di sicurezza. Viene utilizzato principalmente per il controllo e la risoluzione dei problemi relativi ai tentativi di connessione.

- Registrazione delle richieste di autenticazione e accounting degli utenti. È possibile registrare le richieste di autenticazione e accounting degli utenti per i file di log in formato testo o di database oppure è possibile accedere a un stored procedure in un database SQL Server 2000. La registrazione delle richieste viene utilizzata principalmente per scopi di analisi della connessione e fatturazione ed è utile anche come strumento di analisi della sicurezza, che fornisce un metodo per tenere traccia dell'attività di un utente malintenzionato.

Per sfruttare al meglio la registrazione NPS:

- Attivare la registrazione \(inizialmente\) per i record di autenticazione e di accounting. Modificare queste selezioni dopo avere determinato le informazioni appropriate per l'ambiente in uso.

- Verificare che la registrazione eventi sia configurata con una capacità sufficiente per gestire i log.

- Eseguire il backup di tutti i file di log regolarmente perché non possono essere ricreati quando vengono danneggiati o eliminati.

- Usare l'attributo della classe RADIUS per tenere traccia dell'utilizzo e semplificare l'identificazione del reparto o dell'utente da addebitare per l'utilizzo. Sebbene l'attributo della classe generato automaticamente sia univoco per ogni richiesta, potrebbero esistere record duplicati nei casi in cui la risposta al server di accesso vada persa e la richiesta venga inviata nuovamente. Potrebbe essere necessario eliminare richieste duplicate dai log per tenere traccia accuratamente dell'utilizzo.

- Se i server di accesso alla rete e i server proxy RADIUS inviano periodicamente messaggi di richiesta di connessione fittizia a NPS per verificare che il server dei criteri di rete sia online, usare l'impostazione del registro di sistema **nome utente ping** . Questa impostazione Configura NPS per rifiutare automaticamente le richieste di connessione false senza elaborarle. Inoltre, NPS non registra le transazioni che coinvolgono il nome utente fittizio in tutti i file di log, rendendo più semplice l'interpretazione del registro eventi.

- Disabilitare l'invio di notifiche NAS. È possibile disabilitare l'invio di messaggi di avvio e di arresto da server di accesso alla rete (NAS) ai membri di un gruppo di server RADIUS remoto configurato in NPS. Per altre informazioni, vedere [disabilitare l'invio di notifiche NAS](nps-disable-nas-notifications.md).

Per ulteriori informazioni, vedere [configurare l'accounting del server dei criteri di rete](nps-accounting-configure.md).

- Per garantire il failover e la ridondanza con SQL Server registrazione, collocare due computer che eseguono SQL Server su subnet diverse. Utilizzare la SQL Server **creazione guidata pubblicazione** per configurare la replica di database tra i due server. Per ulteriori informazioni, vedere [SQL Server documentazione tecnica](https://msdn.microsoft.com/library/ms130214.aspx) e [replica di SQL Server](https://msdn.microsoft.com/library/ms151198.aspx).

## <a name="authentication"></a>Autenticazione

Di seguito sono riportate le procedure consigliate per l'autenticazione.

- Usare metodi di autenticazione basati su certificato, ad esempio Protected Extensible Authentication Protocol \(PEAP\) ed Extensible Authentication Protocol \(EAP\) per l'autenticazione avanzata. Non usare i metodi di autenticazione solo password perché sono vulnerabili a una serie di attacchi e non sono sicuri. Per l'autenticazione wireless sicura, è consigliabile usare PEAP\-MS\-CHAP v2, perché il server dei criteri di rete dimostra la propria identità ai client wireless usando un certificato del server, mentre gli utenti dimostrano la propria identità con il nome utente e la password.  Per ulteriori informazioni sull'utilizzo del server dei criteri di rete nella distribuzione wireless, vedere [distribuire l'accesso wireless autenticato con 802.1 x basato su password](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/a-deploy-8021x-wireless-access).
- Distribuire la propria autorità di certificazione \(CA\) con Active Directory&reg; Servizi certificati \(AD CS\) quando si usano metodi di autenticazione avanzati basati su certificati, ad esempio PEAP e EAP, che richiedono l'uso di un certificato del server in NPSs. È anche possibile usare la CA per registrare i certificati del computer e i certificati utente. Per ulteriori informazioni sulla distribuzione di certificati server nei server dei criteri di rete e di accesso remoto, vedere [distribuire certificati server per le distribuzioni wireless e cablate 802.1 x](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments).

> [!IMPORTANT]
> Server dei criteri di rete non supporta l'utilizzo dei caratteri ASCII estesi all'interno delle password.

## <a name="client-computer-configuration"></a>Configurazione computer client

Di seguito sono riportate le procedure consigliate per la configurazione dei computer client.

- Configurare automaticamente tutti i computer client 802.1 X dei membri del dominio usando Criteri di gruppo. Per ulteriori informazioni, vedere la sezione relativa alla configurazione dei criteri di rete wireless (IEEE 802,11) nell'argomento [distribuzione di accesso wireless](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/e-wireless-access-deployment#bkmk_policies).

## <a name="installation-suggestions"></a>Suggerimenti per l'installazione

Di seguito sono riportate le procedure consigliate per l'installazione di NPS.

- Prima di installare NPS, installare e testare ciascun server di accesso alla rete utilizzando metodi di autenticazione locali prima di configurarli come client RADIUS in NPS.

- Dopo aver installato e configurato NPS, salvare la configurazione usando il comando di Windows PowerShell [Export-NpsConfiguration](https://technet.microsoft.com/library/jj872749.aspx). Salvare la configurazione NPS con questo comando ogni volta che si riconfigura il server dei criteri di server.

>[!CAUTION]
>- Il file di configurazione NPS esportato contiene segreti condivisi non crittografati per i client RADIUS e i membri dei gruppi di server RADIUS remoti. Per questo motivo, assicurarsi di salvare il file in una posizione sicura.
>- Il processo di esportazione non include le impostazioni di registrazione per Microsoft SQL Server nel file esportato. Se si importa il file esportato in un altro server dei criteri di server, è necessario configurare manualmente SQL Server la registrazione nel nuovo server.

## <a name="performance-tuning-nps"></a>Ottimizzazione delle prestazioni NPS

Di seguito sono riportate le procedure consigliate per l'ottimizzazione delle prestazioni NPS.

- Per ottimizzare i tempi di risposta dell'autenticazione e dell'autorizzazione server dei criteri di rete e ridurre il traffico di rete, installare NPS in un controller di dominio.

- Quando vengono utilizzati i nomi di entità universali \(UPN\) o i domini di Windows Server 2008 e Windows Server 2003, NPS utilizza il catalogo globale per autenticare gli utenti. Per ridurre al minimo il tempo necessario per eseguire questa operazione, installare NPS in un server di catalogo globale o in un server che si trova nella stessa subnet del server di catalogo globale.

- Quando sono configurati gruppi di server RADIUS remoti e, in criteri di richiesta di connessione server dei criteri di rete, è possibile deselezionare la casella di controllo **registra informazioni sull'accounting nei server del gruppo di server RADIUS remoto seguente** . questi gruppi vengono comunque inviati al server di accesso alla rete \(NAS\) avviare e arrestare i messaggi di notifica. Viene creato un traffico di rete non necessario. Per eliminare il traffico, disabilitare l'invio di notifiche NAS per i singoli server in ogni gruppo di server RADIUS remoti deselezionando la casella di controllo **Invia notifiche di avvio e arresto della rete al server** .

## <a name="using-nps-in-large-organizations"></a>Uso di NPS in organizzazioni di grandi dimensioni

Di seguito sono riportate le procedure consigliate per l'utilizzo di server dei criteri di società

- Se si utilizzano i criteri di rete per limitare l'accesso per tutti i gruppi tranne determinati, creare un gruppo universale per tutti gli utenti per i quali si desidera consentire l'accesso, quindi creare un criterio di rete che conceda l'accesso a questo gruppo universale. Non inserire tutti gli utenti direttamente nel gruppo universale, soprattutto se si dispone di un numero elevato di tali utenti nella rete. In alternativa, creare gruppi distinti che siano membri del gruppo universale e aggiungere utenti a tali gruppi.

- Usare un nome di entità utente per fare riferimento agli utenti quando possibile. Un utente può avere lo stesso nome dell'entità utente indipendentemente dall'appartenenza al dominio. Questa procedura fornisce scalabilità che potrebbe essere necessaria per le organizzazioni con un numero elevato di domini.

- Se è stato installato Server dei criteri di rete \(NPS\) in un computer diverso da un controller di dominio e il server dei criteri di rete riceve un numero elevato di richieste di autenticazione al secondo, è possibile migliorare le prestazioni di NPS aumentando il numero di autenticazioni simultanee consentite tra il server dei criteri di rete e il controller di dominio Per altre informazioni, vedere [aumentare le autenticazioni simultanee elaborate da NPS](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-concurrent-auth).

## <a name="security-issues"></a>Problemi di sicurezza

Di seguito sono riportate le procedure consigliate per ridurre i problemi di sicurezza.

Quando si amministra un server dei criteri di rete in modalità remota, non inviare dati sensibili o riservati (ad esempio, segreti o password condivise) sulla rete in testo non crittografato. Per l'amministrazione remota di NPSs sono disponibili due metodi:

- Utilizzare Servizi Desktop remoto per accedere al server dei criteri di accesso. Quando si utilizza Servizi Desktop remoto, i dati non vengono inviati tra il client e il server. Solo l'interfaccia utente del server (ad esempio, il desktop del sistema operativo e l'immagine della console server dei criteri di rete) viene inviata al client di Servizi Desktop remoto, denominato Connessione Desktop remoto in Windows&reg; 10. Il client invia l'input della tastiera e del mouse, che viene elaborato localmente dal server in cui è abilitato Servizi Desktop remoto. Quando Servizi Desktop remoto utenti eseguono l'accesso, possono visualizzare solo le singole sessioni client gestite dal server e sono indipendenti tra loro. Inoltre, Connessione Desktop remoto fornisce la crittografia a 128 bit tra client e server.

- Usare Internet Protocol Security (IPsec) per crittografare i dati riservati. È possibile utilizzare IPsec per crittografare le comunicazioni tra il server dei criteri di server e il computer client remoto utilizzato per amministrare NPS. Per amministrare il server in modalità remota, è possibile installare il [strumenti di amministrazione remota del server per Windows 10](https://www.microsoft.com/download/details.aspx?id=45520) nel computer client. Dopo l'installazione, utilizzare Microsoft Management Console (MMC) per aggiungere lo snap-in NPS alla console di.

>[!IMPORTANT]
>È possibile installare Strumenti di amministrazione remota del server per Windows 10 solo nella versione completa di Windows 10 Professional o Windows 10 Enterprise.

Per ulteriori informazioni su NPS, vedere [Server dei criteri di rete (NPS)](nps-top.md).

