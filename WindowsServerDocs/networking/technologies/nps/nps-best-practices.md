---
title: Rete criteri procedure consigliate per Server
description: Questo argomento fornisce le procedure consigliate per la distribuzione e gestione dei Server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 90e544bd-e826-4093-8c3b-6a6fc2dfd1d6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5a9a68965d0d19504352ad67f7ab268b3d344fb2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="network-policy-server-best-practices"></a>Rete criteri procedure consigliate per Server

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per informazioni sulle procedure consigliate per la distribuzione e gestione dei Server dei criteri di rete \(NPS\).

Le sezioni seguenti forniscono le procedure consigliate per diversi aspetti della distribuzione dei criteri di rete.

## <a name="accounting"></a>Accounting

Di seguito sono le procedure consigliate per la registrazione dei criteri di rete.

Esistono due tipi di contabilità, o di registrazione, in Criteri di rete:

- Registrazione degli eventi dei criteri di rete. È possibile utilizzare la registrazione degli eventi per registrare gli eventi dei criteri di rete nel registro eventi di sistema e sicurezza. Viene utilizzato principalmente per il controllo e la risoluzione dei problemi di tentativi di connessione.

- Registrazione delle richieste di accounting e l'autenticazione utente. È possibile registrare le richieste di autenticazione e accounting utente nei file di log in formato testo o database oppure è possibile accedere a una stored procedure in un database di SQL Server 2000. Registrazione delle richieste viene utilizzata principalmente per scopi di fatturazione e di analisi di connessione ed è anche utile come strumento di analisi di sicurezza, offrendo un metodo di rilevamento verso il basso l'attività di un utente malintenzionato.

Per rendere l'uso più efficace di registrazione dei criteri di rete:

- Attivare la registrazione \(initially\) per l'autenticazione e contabilità. Modificare queste selezioni dopo aver stabilito qual è appropriato per l'ambiente.

- Assicurarsi che la registrazione degli eventi è configurato con una capacità sufficiente per la gestione dei registri.

- Eseguire il backup di tutti i file di registro a intervalli regolari perché non possono essere ricreate quando vengono danneggiati o eliminati.

- Utilizzare l'attributo RADIUS classe per tenere traccia dell'utilizzo e semplificare l'identificazione del dipartimento o utente a effettuare l'addebito per l'utilizzo. Anche se l'attributo di classe generato automaticamente è univoco per ogni richiesta, i record duplicati possono esistere nei casi in cui viene persa la risposta al server di accesso e la richiesta inviata di nuovo. Potrebbe essere necessario eliminare le richieste duplicate dal log per tenere traccia in modo accurato l'utilizzo.

- Se il server di accesso alla rete e server proxy RADIUS invia periodicamente i messaggi di richiesta connessione fittizia a criteri di rete per verificare che il server dei criteri di rete sia in linea, utilizzare il **effettuare il ping del nome utente** impostazione del Registro di sistema. Questa impostazione consente di configurare criteri di rete per rifiutare automaticamente le richieste di connessione false senza elaborarle. Inoltre, dei criteri di rete non registra le transazioni che coinvolgono il nome utente fittizio in qualsiasi file di registro, che rende più facile da interpretare il registro eventi.

- Disabilitare l'inoltro di notifica NAS. È possibile disabilitare l'inoltro della schermata start e stop messaggi dal server di accesso di rete (NAS) ai membri di un server RADIUS remoto che sono configurato in Criteri di rete di gruppo. Per ulteriori informazioni, vedere [disabilitare inoltro notifica NAS](nps-disable-nas-notifications.md).

Per ulteriori informazioni, vedere [configurare Network Policy Server Accounting](nps-accounting-configure.md).

- Per fornire ridondanza e failover con la registrazione SQL Server, inserire due computer che eseguono SQL Server in subnet diverse. Utilizzare SQL Server **Creazione guidata pubblicazione** per impostare la replica di database tra i due server. Per ulteriori informazioni, vedere [documentazione tecnica di SQL Server](https://msdn.microsoft.com/library/ms130214.aspx) e [replica di SQL Server](https://msdn.microsoft.com/library/ms151198.aspx).

## <a name="authentication"></a>Autenticazione

Di seguito sono le procedure consigliate per l'autenticazione.

- Utilizzare i metodi di autenticazione basata su certificati, ad esempio \(PEAP\) Protected Extensible Authentication Protocol e \(EAP\) Extensible Authentication Protocol per l'autenticazione avanzata. Non utilizzare i metodi di autenticazione solo password perché sono vulnerabili a un'ampia gamma di attacchi e non sono sicure. Per l'autenticazione sicura wireless, uso PEAP\-MS\-CHAP v2 è consigliato, perché il server dei criteri di rete dimostra la propria identità ai client senza fili con un certificato del server, mentre gli utenti dimostrano la propria identità con il nome utente e password.  Per ulteriori informazioni sull'utilizzo dei criteri di rete nella distribuzione senza fili, vedere [basata su Password distribuire accesso autenticato 802.1 X Wireless](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/a-deploy-8021x-wireless-access).
- Distribuire la propria autorità di certificazione \(CA\) con Active Directory&reg; \(AD CS\) Servizi certificati quando si utilizzano metodi di autenticazione avanzata basata su certificati, ad esempio PEAP ed EAP, che richiedono l'utilizzo di un certificato del server nel server dei criteri di rete. È anche possibile utilizzare l'autorità di certificazione per registrare i certificati utente e computer. Per ulteriori informazioni sulla distribuzione di certificati server per server dei criteri di rete e accesso remoto, vedere [distribuire certificati Server per le distribuzioni Wireless e cablate 802.1 X](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments).

## <a name="client-computer-configuration"></a>Configurazione dei computer client

Di seguito sono le procedure consigliate per la configurazione di computer client.

- Configurare automaticamente tutte il membro del dominio 802.1 X i computer client utilizzando criteri di gruppo. Per ulteriori informazioni, vedere la sezione "Configurare i criteri di rete (IEEE 802.11) Wireless" nell'argomento [distribuzione di accesso Wireless](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/e-wireless-access-deployment#bkmk_policies).

## <a name="installation-suggestions"></a>Consigli per l'installazione

Di seguito sono le procedure consigliate per l'installazione dei criteri di rete.

- Prima di installare NPS, installare e testare ogni server di accesso di rete utilizzando i metodi di autenticazione locale prima di configurare come client RADIUS in Criteri di rete.

- Dopo avere installato e configurato, è possibile salvare la configurazione utilizzando il comando Windows PowerShell [Export-NpsConfiguration](https://technet.microsoft.com/en-us/library/jj872749.aspx). Salvare la configurazione dei criteri di rete con questo comando ogni volta che si riconfigura il server dei criteri di rete.

>[!CAUTION]
>- Il file di configurazione dei criteri di rete esportato contiene segreti condivisi non crittografati per i client RADIUS e membri dei gruppi di server RADIUS remoti. Per questo motivo, assicurati di salvare il file in una posizione sicura.
>- Il processo di esportazione non include le impostazioni di registrazione per Microsoft SQL Server nel file esportato. Se si importa il file esportato in un altro server dei criteri di rete, è necessario configurare manualmente la registrazione SQL Server nel nuovo server.

## <a name="performance-tuning-nps"></a>Criteri di rete di ottimizzazione delle prestazioni

Di seguito sono le procedure consigliate per l'ottimizzazione dei criteri di rete.

- Per ottimizzare i tempi di risposta di autenticazione e autorizzazione dei criteri di rete e ridurre al minimo il traffico di rete, installare il server NPS in un controller di dominio.

- Quando vengono utilizzati i nomi principali utente \(UPNs\) o domini di Windows Server 2008 e Windows Server 2003, dei criteri di rete utilizza il catalogo globale per autenticare gli utenti. Per ridurre al minimo il tempo che necessario per eseguire questa operazione, installare il server NPS in un server di catalogo globale o un server che si trova nella stessa subnet del server di catalogo globale.

- Quando si dispone di gruppi di server RADIUS remoti configurati e, in Criteri richiesta di connessione dei criteri di rete, cancellare il **registrare le informazioni di accounting sui server nel gruppo di server RADIUS remoto seguenti** casella di controllo, questi gruppi vengono inviati i avvio \(NAS\) server di accesso rete e arrestare i messaggi di notifica. Verrà creato il traffico di rete non necessari. Per eliminare il traffico, disabilitare l'inoltro delle notifiche NAS per i server singoli in ogni gruppo di server RADIUS remoto, deselezionando la **inoltrare l'avvio di rete e interrompere le notifiche a questo server** casella di controllo.

## <a name="using-nps-in-large-organizations"></a>Utilizzo dei criteri di rete in organizzazioni di grandi dimensioni

Di seguito sono le procedure consigliate per l'uso dei criteri di rete in organizzazioni di grandi dimensioni.

- Se si utilizza criteri di rete per limitare l'accesso a tutti, ma alcuni gruppi, creare un gruppo universale per tutti gli utenti per cui si desidera consentire l'accesso e quindi creare un criterio di rete che consente l'accesso per il gruppo universale. Non inserire tutti gli utenti direttamente il gruppo universale, in particolare se si dispone di un numero elevato di voci nella rete. Al contrario, creare gruppi distinti che sono membri del gruppo universale, quindi aggiungere gli utenti a tali gruppi.

- Per fare riferimento agli utenti quando possibile, utilizzare un nome dell'entità utente. Un utente può avere lo stesso nome dell'entità utente indipendentemente dall'appartenenza al dominio. Questo metodo assicura la scalabilità che potrebbe essere necessari in organizzazioni con un numero elevato di domini.

- Se è installato Server dei criteri di rete \(NPS\) in un computer diverso da un controller di dominio e il server dei criteri di rete riceve un numero elevato di richieste di autenticazione al secondo, è possibile migliorare le prestazioni dei criteri di rete, aumentando il numero di autenticazioni simultanee consentite tra il server dei criteri di rete e il controller di dominio. Per ulteriori informazioni, vedere 

## <a name="security-issues"></a>Problemi di sicurezza

Di seguito sono le procedure consigliate per ridurre i problemi di sicurezza.

Quando si amministra un server dei criteri di rete in modalità remota, non inviare dati riservati (ad esempio, i segreti condivisi o password) attraverso la rete in testo non crittografato. Esistono due metodi consigliati per l'amministrazione remota del server dei criteri di rete:

- Utilizzare Servizi Desktop remoto per accedere al server dei criteri di rete. Quando si utilizza Servizi Desktop remoto, i dati non vengono inviati tra client e server. Solo l'interfaccia utente del server (ad esempio, il sistema operativo desktop e l'immagine di console dei criteri di rete) viene inviato al client di Servizi Desktop remoto, denominata connessione Desktop remoto in Windows&reg; 10. Il client invia tastiera e mouse input, che viene elaborata in locale dal server che dispone di Servizi Desktop remoto abilitato. Quando gli utenti di Servizi Desktop remoto accedono, possono visualizzare solo le sessioni client singoli, che sono gestite dal server e sono indipendenti tra loro. Inoltre, connessione Desktop remoto fornisce la crittografia a 128 bit tra client e server.

- Utilizzare Internet Protocol security (IPsec) per crittografare i dati riservati. È possibile utilizzare IPsec per crittografare le comunicazioni tra il server dei criteri di rete e il computer client remoti che usi per l'amministrazione dei criteri di rete. Per amministrare il server in modalità remota, è possibile installare il [Remote Server Administration Tools per Windows 10](https://www.microsoft.com/download/details.aspx?id=45520) nel computer client. Dopo l'installazione, utilizzare Microsoft Management Console (MMC) per aggiungere lo snap-in server dei criteri di rete nella console.

>[!IMPORTANT]
>È possibile installare Remote Server Administration Tools per Windows 10 solo nella versione completa di Windows 10 Professional o Windows 10 Enterprise.

Per ulteriori informazioni sui criteri di rete, vedere [Server dei criteri di rete (NPS)](nps-top.md).

