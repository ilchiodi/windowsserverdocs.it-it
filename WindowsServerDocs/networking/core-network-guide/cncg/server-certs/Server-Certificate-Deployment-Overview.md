---
title: Cenni preliminari sulla distribuzione di certificati server
description: Questo argomento fa parte della Guida di certificati del Server di distribuzione per le distribuzioni Wireless e cablate 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: ca5c3e04-ae25-4590-97f3-0376a9c2a9a2
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4650c99325def63fd0df25989c661230fada3dac
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="server-certificate-deployment-overview"></a>Cenni preliminari sulla distribuzione di certificati server

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

In questo argomento contiene le sezioni seguenti.  
  
-   [Componenti di distribuzione certificato server](#bkmk_components)
  
-   [Panoramica del processo di distribuzione certificato server](#bkmk_process)
  
## <a name="bkmk_components"></a>Componenti di distribuzione certificato server
Per installare Servizi certificati Active Directory (AD CS) come un'autorità di certificazione radice aziendale (CA) e per registrare i certificati server nei server che eseguono servizio Server dei criteri di rete (NPS), Routing e accesso remoto (RRAS), o dei criteri di rete e RRAS, è possibile utilizzare questa Guida.


Se si distribuisce SDN con l'autenticazione basata su certificato, sono necessari server per utilizzare un certificato server per dimostrare la propria identità in altri server in modo che raggiungano le comunicazioni protette.
  
L'illustrazione seguente mostra i componenti necessari per distribuire certificati server a server dell'infrastruttura SDN.
  
![Infrastruttura richiesta di distribuzione del certificato server](../../../media/Nps-Certs/Nps-Certs.jpg)  
  
> [!NOTE]  
> Nella figura sopra riportata, vengono rappresentati con più server: DC1, CA1, WEB1 e SDN molti server. Questa guida fornisce istruzioni per la distribuzione e configurazione CA1 e WEB1 e per la configurazione di DC1, in questa guida si presuppone che è già stato installato nella rete. Se il dominio Active Directory non è già installato, è possibile farlo usando il [Guida alla rete Core](https://technet.microsoft.com/library/mt604042.aspx) per Windows Server 2016.  
  
Per ulteriori informazioni su ogni elemento illustrato nella figura sopra riportata, vedere quanto segue:  
  
-   [CA1](#bkmk_ca1)  
  
-   [WEB1](#bkmk_web1)  
  
-   [DC1](#bkmk_dc1)  
  
-   [NPS1](#bkmk_nps1)  
  
### <a name="bkmk_ca1"></a>CA1 esegue il ruolo server Servizi certificati Active Directory  
In questo scenario, l'autorità di certificazione radice aziendale (CA) è anche una CA emittente. L'autorità di certificazione emette certificati per i computer server che dispone delle autorizzazioni di sicurezza corrette per registrare un certificato. Servizi certificati Active Directory (AD CS) viene installato su CA1.  
  
Per reti di grandi dimensioni o in cui giustificare i problemi di sicurezza, è possibile separare i ruoli della CA radice e CA emittente e distribuire le CA subordinate sono CA emittenti.  
  
Nelle distribuzioni più sicure, la CA principale dell'organizzazione viene portata offline e fisicamente protetta.   
  
#### <a name="capolicyinf"></a>File CAPolicy. inf  
Prima di installare Servizi certificati Active Directory, configurare file CAPolicy. inf con impostazioni specifiche per la distribuzione.  
  
#### <a name="copy-of-the-ras-and-ias-servers-certificate-template"></a>Copia del **server RAS e IAS** modello di certificato  
Quando si distribuiscono i certificati del server, eseguire una copia del **server RAS e IAS** modello di certificato e quindi configurare il modello in base alle esigenze e le istruzioni riportate in questa Guida.   
  
Utilizzare una copia del modello anziché il modello originale in modo che la configurazione del modello originale viene mantenuta per possibili utilizzi futuri. Si configura la copia del **server RAS e IAS** modello in modo che l'autorità di certificazione possono creare i certificati del server che ha emesso i gruppi in Active Directory Users and Computers specificato.  
  
#### <a name="additional-ca1-configuration"></a>Configurazione aggiuntiva CA1  
L'autorità di certificazione pubblica un elenco di revoche di certificati (CRL) che devono controllare i computer per garantire che i certificati che sono presentati nel come prova di identità sono certificati validi e non sono stati revocati. È necessario configurare l'autorità di certificazione con il percorso corretto dell'elenco CRL in modo che i computer sapere dove cercare il CRL durante il processo di autenticazione.  
  
### <a name="bkmk_web1"></a>Web1 esegue il ruolo server servizi Web (IIS)  
Nel computer che esegue il ruolo server Server Web (IIS), WEB1, è necessario creare una cartella in Esplora risorse per l'utilizzo come posizione per il CRL e AIA.  
  
#### <a name="virtual-directory-for-the-crl-and-aia"></a>Directory virtuale per il CRL e AIA  
Dopo aver creato una cartella in Esplora risorse, è necessario configurare la cartella come una directory virtuale in Gestione Internet Information Services (IIS), nonché configurare l'elenco di controllo di accesso per la directory virtuale per consentire ai computer di accedere ai AIA e CRL dopo la pubblicazione non esiste.  
  
### <a name="bkmk_dc1"></a>DC1 esegue i ruoli del server di dominio Active Directory e DNS  
DC1 è il controller di dominio e server DNS sulla rete.  
  
#### <a name="group-policy-default-domain-policy"></a>Criterio dominio predefinito dei criteri di gruppo  
Dopo aver configurato il modello di certificato della CA, è possibile configurare il criterio dominio predefinito in Criteri di gruppo in modo che i certificati sono registrazione automatica per i server dei criteri di rete e RAS. Criteri di gruppo sono configurato in Active Directory nel server DC1.  
  
#### <a name="dns-alias-cname-resource-record"></a>DNS record di risorse alias (CNAME)  
È necessario creare un record di risorse alias (CNAME) per il server Web in modo da altri computer possono individuare il server, nonché di AIA e CRL archiviati nel server. Inoltre, un record di risorse CNAME alias offre flessibilità in modo che è possibile utilizzare il server Web per altri scopi, ad esempio l'hosting di siti Web e FTP.  
  
### <a name="bkmk_nps1"></a>NPS1 in esecuzione il servizio ruolo Server dei criteri di rete del ruolo server Servizi di accesso e criteri di rete  
Il server dei criteri di rete viene installato quando si eseguono le attività nella Guida alla rete Windows Server 2016 Core, pertanto prima di eseguire le attività in questa Guida, è necessario disporre già uno o più server dei criteri di rete installato nella rete.  
  
#### <a name="group-policy-applied-and-certificate-enrolled-to-servers"></a>Criteri di gruppo applicati e certificati registrati nei server  
Dopo aver configurato il modello di certificato e la registrazione automatica, è possibile aggiornare i criteri di gruppo in tutti i server di destinazione. In questo momento, il server di registrare il certificato del server da CA1.  
  
### <a name="bkmk_process"></a>Panoramica del processo di distribuzione certificato server  
  
> [!NOTE]  
> Vengono forniti i dettagli di come eseguire questi passaggi nella sezione [distribuzione del certificato Server](../../../core-network-guide/cncg/server-certs/Server-Certificate-Deployment.md).  
  
Il processo di configurazione registrazione del certificato server si verifica in tali fasi:  
  
1.  In WEB1, installare il ruolo Server Web (IIS).  
  
2.  Su DC1, creare un record alias (CNAME) per il server Web, WEB1.  
  
3.  Configurare il server Web per ospitare il CRL dalla CA, quindi pubblicare il CRL e copiare il certificato CA radice aziendale nella nuova directory virtuale.  
  
4.  Nel computer in cui si prevede di installare Servizi certificati Active Directory, assegnare al computer un indirizzo IP statico, rinominare il computer, aggiungere il computer al dominio e quindi accedere al computer con un account utente che è un membro dei gruppi Enterprise Admins e Domain Admins.  
  
5.  Nel computer in cui si prevede di installare Servizi certificati Active Directory, configurare il file CAPolicy. inf con impostazioni specifiche per la distribuzione.  
  
6.  Installare il ruolo server Servizi certificati Active Directory e di eseguire un'ulteriore configurazione dell'autorità di certificazione.  
  
7.  Copiare il certificato CA e CRL da CA1 alla condivisione sul server Web WEB1.  
  
8.  Nella CA configurare una copia del modello di certificato server RAS e IAS. L'autorità di certificazione emette certificati basati su un modello di certificato, pertanto è necessario configurare il modello per il certificato del server prima che l'autorità di certificazione può emettere un certificato.  
  
9.  Configurare la registrazione automatica del certificato server in Criteri di gruppo. Quando si configura la registrazione automatica, tutti i server che è stato specificato con l'appartenenza al gruppo di Active Directory automaticamente visualizzato un certificato del server all'aggiornamento di criteri di gruppo in ogni server. Se si aggiungono altri server in un secondo momento, riceverà automaticamente un certificato server troppo.  
  
10. Aggiornare criteri di gruppo nel server. Quando viene aggiornato criteri di gruppo, i server ricevano il certificato del server che si basa sul modello che è stato configurato nel passaggio precedente. Questo certificato viene utilizzato dal server per dimostrare la propria identità ai computer client e altri server durante il processo di autenticazione.  
  
    > [!NOTE]  
    > Tutti i computer membri del dominio riceveranno automaticamente il certificato della CA radice aziendale senza la configurazione della registrazione automatica. Questo certificato è diverso da quello il certificato del server di configurare e distribuire mediante la registrazione automatica. Il certificato della CA viene installato automaticamente nell'archivio certificati Autorità di certificazione radice attendibili per tutti i computer membro di dominio in modo che i certificati rilasciati da questa autorità di certificazione verrà attendibilità.   
  
10. Verificare che tutti i server hanno registrato un certificato server valido.  
  


