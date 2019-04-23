---
title: Procedure consigliate per il Server dei criteri di rete
description: In questo argomento illustra le procedure consigliate per la distribuzione e gestione dei Server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 90e544bd-e826-4093-8c3b-6a6fc2dfd1d6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6cbd606edec06a80767ee997ef6a1397b66b7843
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834232"
---
# <a name="network-policy-server-best-practices"></a>Procedure consigliate per il Server dei criteri di rete

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per apprendere le procedure consigliate per la distribuzione e gestione dei Server dei criteri di rete \(NPS\).

Le sezioni seguenti descrivono le procedure consigliate per i vari aspetti della distribuzione dei criteri di rete.

## <a name="accounting"></a>Accounting

Di seguito sono le procedure consigliate per la registrazione dei criteri di rete.

Esistono due tipi di accounting o la registrazione, in Criteri di rete:

- Registrazione eventi di criteri di rete. È possibile usare la registrazione degli eventi per registrare gli eventi dei criteri di rete nei registri eventi di sistema e sicurezza. Viene utilizzato principalmente per il controllo e risoluzione dei problemi relativi a tentativi di connessione.

- Registrazione delle richieste di accounting e autenticazione degli utenti. È possibile registrare le richieste di autenticazione e accounting utente nei file di log in formato testo o del database oppure è possibile accedere a una stored procedure in un database di SQL Server 2000. La registrazione delle richieste viene utilizzata principalmente per scopi di analisi e la fatturazione di connessione ed è inoltre utile come strumento di analisi di sicurezza, offrendo un metodo per rilevare l'attività di un utente malintenzionato.

Per rendere l'uso più efficiente della registrazione dei criteri di rete:

- Abilitare la registrazione \(inizialmente\) per l'autenticazione e contabilità. Modificare le selezioni dopo aver determinato che cosa è appropriata per l'ambiente.

- Assicurarsi che la registrazione degli eventi è configurato con una capacità sufficiente per la gestione dei registri.

- Eseguire il backup tutti i file di log a intervalli regolari, perché non può essere ricreate quando vengono danneggiati o eliminati.

- Usare l'attributo di classe RADIUS per tenere traccia dell'utilizzo e semplificare l'identificazione delle quali reparto o un utente ad addebitare i costi per l'utilizzo. Anche se l'attributo della classe generata automaticamente è univoco per ogni richiesta, potrebbero esistere record duplicati nei casi in cui la risposta al server di accesso viene persa e la richiesta viene inviata di nuovo. Potrebbe essere necessario eliminare le richieste duplicate dai log per monitorare l'utilizzo in modo accurato.

- Se il server di accesso di rete e server proxy RADIUS invii periodicamente messaggi di richiesta connessione fittizia per criteri di rete per verificare che i criteri di rete sia online, usare il **effettuare il ping del nome utente** impostazione del Registro di sistema. Questa impostazione consente di configurare criteri di rete per rifiutare automaticamente queste richieste di connessione false senza elaborarli. Inoltre, NPS non vengono registrate le transazioni che interessano il nome utente fittizio in tutti i file di log, che rende più semplice interpretare il registro eventi.

- Disabilitare l'inoltro di notifica NAS. È possibile disabilitare l'inoltro di avvio e arresto dei messaggi dal server di accesso di rete (NAS) ai membri di un server RADIUS remoto gruppo che è configurato in Criteri di rete. Per altre informazioni, vedere [disabilitare l'inoltro notifica NAS](nps-disable-nas-notifications.md).

Per altre informazioni, vedere [configurare del Accounting Server dei criteri di rete](nps-accounting-configure.md).

- Per fornire ridondanza e failover con la registrazione del Server SQL, inserire due computer che eseguono SQL Server in subnet diverse. Usare SQL Server **Creazione guidata pubblicazione** per impostare la replica di database tra i due server. Per altre informazioni, vedere [documentazione tecnica di SQL Server](https://msdn.microsoft.com/library/ms130214.aspx) e [replica di SQL Server](https://msdn.microsoft.com/library/ms151198.aspx).

## <a name="authentication"></a>Autenticazione

Di seguito sono le procedure consigliate per l'autenticazione.

- Usare i metodi di autenticazione basata su certificato, ad esempio Protected Extensible Authentication Protocol \(PEAP\) ed Extensible Authentication Protocol \(EAP\) per l'autenticazione avanzata. Non usare i metodi di autenticazione solo la password perché sono vulnerabili a un'ampia gamma di attacchi e non sono sicure. Per l'autenticazione protetta wireless, utilizzare il protocollo PEAP\-MS\-CHAP v2 è consigliata, perché i criteri di rete dimostra la propria identità ai client senza fili con un certificato del server, mentre gli utenti di dimostrare la propria identità con il nome utente e la password.  Per altre informazioni sull'uso dei criteri di rete nella distribuzione wireless, vedere [basato su Password distribuire accesso autenticato 802.1x Wireless](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/a-deploy-8021x-wireless-access).
- Distribuire l'autorità di certificazione \(autorità di certificazione\) con Active Directory&reg; Servizi certificati \(Servizi certificati Active Directory\) quando si usano metodi di autenticazione avanzata basata su certificati, ad esempio PEAP ed EAP, che richiedono l'uso di un certificato del server nel NPSs. È anche possibile usare l'autorità di certificazione di registrare i certificati del computer e i certificati utente. Per altre informazioni sulla distribuzione dei certificati del server a server dei criteri di rete e accesso remoto, vedere [distribuire certificati Server per le distribuzioni Wireless e cablate 802.1 X](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments).

## <a name="client-computer-configuration"></a>Configurazione dei computer client

Di seguito sono le procedure consigliate per la configurazione dei computer client.

- Configurare automaticamente tutti il membro del dominio 802.1 X nei computer client usando criteri di gruppo. Per altre informazioni, vedere la sezione "Criteri di Configura senza fili (IEEE 802.11) di rete" nell'argomento [distribuzione di accesso Wireless](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/e-wireless-access-deployment#bkmk_policies).

## <a name="installation-suggestions"></a>Consigli per l'installazione

Di seguito sono le procedure consigliate per l'installazione dei criteri di rete.

- Prima di installare NPS, installare e testare ognuno dei server di accesso di rete usando metodi di autenticazione locale prima di essere configurate come client RADIUS in Criteri di rete.

- Dopo aver installare e configurare criteri di rete, salvare la configurazione usando il comando di Windows PowerShell [Export-NpsConfiguration](https://technet.microsoft.com/library/jj872749.aspx). Salvare la configurazione dei criteri di rete con questo comando ogni volta che si riconfigura i criteri di rete.

>[!CAUTION]
>- Il file di configurazione dei criteri di rete esportato contiene segreti condivisi non crittografati per i client RADIUS e i membri di gruppi di server RADIUS remoti. Per questo motivo, assicurarsi di salvare il file in una posizione sicura.
>- Il processo di esportazione non include le impostazioni di registrazione per Microsoft SQL Server nel file esportato. Se si importa il file esportato in un altro dei criteri di rete, è necessario configurare manualmente la registrazione di SQL Server nel nuovo server.

## <a name="performance-tuning-nps"></a>Criteri di rete l'ottimizzazione delle prestazioni

Di seguito sono le procedure consigliate per le prestazioni dei criteri di rete di ottimizzazione.

- Per ottimizzare i tempi di risposta di autenticazione e autorizzazione dei criteri di rete e ridurre al minimo il traffico di rete, installare il server NPS in un controller di dominio.

- Quando i nomi principali di universali \(UPN\) o domini di Windows Server 2008 e Windows Server 2003 vengono usati, dei criteri di rete Usa il catalogo globale per l'autenticazione degli utenti. Per ridurre al minimo il tempo che necessario per eseguire questa operazione, installare il server NPS in un server di catalogo globale o un server che si trova nella stessa subnet del server di catalogo globale.

- Quando si dispone di gruppi di server RADIUS remoti e configurati e, in Criteri richiesta di connessione NPS, si deseleziona il **registrare le informazioni di accounting nei server nel gruppo di server RADIUS remoto seguente** casella di controllo, questi gruppi sono comunque server di accesso di rete inviati \(NAS\) avviare e arrestare i messaggi di notifica. Ciò consente di creare traffico di rete superfluo. Per eliminare questo tipo di traffico, disattivare l'inoltro di notifica NAS per un singolo server in ogni gruppo di server RADIUS remoto, deselezionando il **inoltrare l'avvio di rete e arrestare le notifiche per questo server** casella di controllo.

## <a name="using-nps-in-large-organizations"></a>Uso dei criteri di rete in organizzazioni di grandi dimensioni

Di seguito sono le procedure consigliate per l'uso dei criteri di rete in organizzazioni di grandi dimensioni.

- Se si usano criteri di rete per limitare l'accesso per tutti, ma alcuni gruppi, creare un gruppo universale per tutti gli utenti per cui si desidera consentire l'accesso e quindi creare un criterio di rete che concede l'accesso per questo gruppo universale. Non inserire tutti gli utenti direttamente al gruppo universale, in particolare se si dispone di un numero elevato di loro sulla rete. In alternativa, creare gruppi separati che sono membri del gruppo universale e aggiungere utenti a tali gruppi.

- Per fare riferimento agli utenti quando possibile, usare un nome entità utente. Un utente può avere lo stesso nome dell'entità utente indipendentemente dall'appartenenza al dominio. Questo metodo assicura la scalabilità che potrebbe essere necessarie nelle organizzazioni con un numero elevato di domini.

- Se è stato installato Server dei criteri di rete \(NPS\) in un computer diverso da un dominio controller e i criteri di rete riceve un numero elevato di richieste di autenticazione al secondo, è possibile migliorare le prestazioni dei criteri di rete, aumentando il numero di autenticazioni simultanee consentite tra i criteri di rete e il controller di dominio. Per altre informazioni, vedere l'articolo relativo alla 

## <a name="security-issues"></a>Problemi di sicurezza

Di seguito sono le procedure consigliate per ridurre i problemi di sicurezza.

Quando si amministra un server NPS in modalità remota, non inviare i dati sensibili o riservati (ad esempio, i segreti condivisi o password) attraverso la rete in testo non crittografato. Sono disponibili due metodi consigliati per l'amministrazione remota del NPSs:

- Usare Servizi Desktop remoto per accedere a NPS. Quando si usa Servizi Desktop remoto, i dati non vengono inviati tra client e server. Solo l'interfaccia utente del server (ad esempio, il sistema operativo desktop e l'immagine di console Criteri di rete) viene inviato al client Servizi Desktop remoto, che è denominato connessione Desktop remoto in Windows&reg; 10. Il client invia tastiera e mouse di input, che viene elaborato in locale dal server che dispone di Servizi Desktop remoto abilitato. All'accesso degli utenti di Servizi Desktop remoto, possono visualizzare solo le sessioni client singoli, che sono gestite dal server e sono indipendenti tra loro. Inoltre, connessione Desktop remoto fornisce la crittografia a 128 bit tra client e server.

- Utilizzare Internet Protocol security (IPsec) per crittografare dati riservati. È possibile utilizzare IPsec per crittografare le comunicazioni tra i criteri di rete e il computer client remoti che si usa per amministrare criteri di rete. Per amministrare il server in modalità remota, è possibile installare il [Remote Server Administration Tools per Windows 10](https://www.microsoft.com/download/details.aspx?id=45520) nel computer client. Dopo l'installazione, utilizzare Microsoft Management Console (MMC) per aggiungere lo snap-in Criteri di rete nella console.

>[!IMPORTANT]
>È possibile installare Remote Server Administration Tools per Windows 10 solo nella versione completa di Windows 10 Professional o Windows 10 Enterprise.

Per altre informazioni sui criteri di rete, vedere [Strumentazione gestione Windows (NPS, Network Policy Server)](nps-top.md).

