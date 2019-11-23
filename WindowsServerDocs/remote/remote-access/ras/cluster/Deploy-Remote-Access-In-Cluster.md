---
title: Distribuire Accesso remoto in un cluster
description: Questo argomento fa parte della Guida deploy Remote Access in a cluster in Windows Server 2016.
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5e4108eee0c62ae4d4db31560b31a6f90751c6b8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404645"
---
# <a name="deploy-remote-access-in-a-cluster"></a>Distribuire Accesso remoto in un cluster

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Windows Server 2016 e Windows Server 2012 combinano DirectAccess e il servizio di accesso remoto \(RAS\) VPN in un singolo ruolo accesso remoto. È possibile distribuire accesso remoto in diversi scenari aziendali. Questa panoramica offre un'introduzione allo scenario aziendale per la distribuzione di più server di accesso remoto in un cluster con carico bilanciato con bilanciamento carico di rete di Windows \(NLB\) o con un servizio di bilanciamento del carico esterno \(ELB\), ad esempio F5 Big\-IP.  

## <a name="BKMK_OVER"></a>Descrizione dello scenario  
Una distribuzione di cluster raccoglie più server di accesso remoto in una singola unità, che funge quindi da singolo punto di contatto per i computer client remoti che si connettono tramite DirectAccess o VPN alla rete aziendale interna usando l'indirizzo IP virtuale esterno \(indirizzo VIP\) del cluster di accesso remoto.  Il traffico verso il cluster viene sottoposta a bilanciamento del carico usando il bilanciamento del carico di Windows o un servizio di bilanciamento del carico esterno \(come F5 Big\-IP\).  

## <a name="prerequisites"></a>Prerequisiti  
Prima di iniziare a distribuire questo scenario, esaminare l'elenco dei requisiti importanti:  

-   Bilanciamento del carico predefinito con Bilanciamento carico di rete di Windows.  

-   Sono supportati i servizi di bilanciamento del carico esterni.  

-   La modalità predefinita e consigliata per Bilanciamento carico di rete di Windows è Unicast.  

-   La modifica dei criteri all'esterno della console di gestione DirectAccess o con cmdlet di PowerShell non è supportata.  

-   Quando si usa NLB o un servizio di bilanciamento del carico esterno, il prefisso IPHTTPS non può essere modificato in un valore diverso da \/59.  

-   I nodi con carico bilanciato devono trovarsi nella stessa subnet IPv4.  

-   Nelle distribuzioni ELB, se è necessaria la gestione in uscita, i client DirectAccess non possono usare&nbsp;Teredo. È possibile utilizzare solo IPHTTPS per terminare la comunicazione\-end\-.  

-   Verificare che siano installati tutti gli hotfix noti\/ELB.  

-   La tecnologia ISATAP non è supportata nella rete aziendale. Se si usa la tecnologia ISATAP, rimuoverla e usare la connettività IPv6 nativa.  

## <a name="in-this-scenario"></a>In questo scenario  
Lo scenario di distribuzione di cluster include alcuni passaggi:  

1.  [Distribuire un server VPN always on con le opzioni avanzate](../../vpn/always-on-vpn/deploy/always-on-vpn-adv-options.md). Prima di configurare la distribuzione di un cluster, è necessario distribuire un singolo server di accesso remoto con impostazioni avanzate.  

2.  [Pianificare una distribuzione di cluster di accesso remoto](plan/Plan-a-Remote-Access-Cluster-Deployment.md). Per compilare un cluster da una distribuzione a server singolo sono necessari alcuni passaggi aggiuntivi, inclusa la preparazione dei certificati per la distribuzione del cluster.  

3.  [Configurare un cluster di accesso remoto](configure/Configure-a-Remote-Access-Cluster.md). Questa operazione è costituita da una serie di passaggi di configurazione, tra cui la preparazione del singolo server per NLB di Windows o il servizio di bilanciamento del carico esterno, la preparazione di server aggiuntivi per l'aggiunta al cluster e l'abilitazione del bilanciamento del carico.  

## <a name="BKMK_APP"></a>Applicazioni pratiche  
La raccolta di più server in un cluster di server fornisce quanto segue:  

-   Scalabilità. Un singolo server di accesso remoto offre un livello limitato di affidabilità del server e prestazioni scalabili. Raggruppando le risorse di due o più server in un unico cluster, si aumenta la capacità per numero di utenti e velocità effettiva.  

-   Disponibilità elevata. Un cluster fornisce disponibilità elevata per Always\-all'accesso. Se un server nel cluster non riesce, gli utenti remoti possono continuare ad accedere alla rete aziendale usando un altro server nel cluster. Tutti i server del cluster hanno lo stesso set di indirizzi IP virtuali del cluster \(indirizzi VIP\), mantenendo comunque un indirizzo IP dedicato e univoco per ogni server.  

-   Semplifica la\-della gestione\-. Un cluster consente la gestione di più server come singola entità. Le impostazioni condivise possono essere impostate facilmente in tutti i server cluster. Le impostazioni di accesso remoto possono essere gestite da qualsiasi server nel cluster oppure in remoto usando Strumenti di amministrazione remota del server \(strumenti di amministrazione remota dei\). L'intero cluster può anche essere monitorato da una sola Console di gestione Accesso remoto.  

## <a name="BKMK_NEW"></a>Ruoli e funzionalità inclusi in questo scenario  
Nella tabella seguente sono elencati i ruoli e le funzionalità richiesti per lo scenario.  

|Funzionalità\/ruoli|Modalità di supporto dello scenario|  
|---------|-----------------|  
|Ruolo Accesso remoto|Il ruolo viene installato e disinstallato tramite la console di Server Manager. Include sia DirectAccess, che in precedenza era una funzionalità di Windows Server 2008 R2, che servizi routing e accesso remoto \(RRAS\), che in precedenza era un servizio ruolo in servizi di accesso e criteri di rete \(NPAS\) ruolo del server. Il ruolo Accesso remoto è costituito da due componenti:<br /><br />-Always On VPN e servizi di routing e accesso remoto \(RRAS\) VPN: DirectAccess e VPN vengono gestiti insieme nella console di gestione accesso remoto.<br />Funzionalità di routing di Routing RRAS routing e accesso REMOTO vengono gestite nella console di Routing e accesso remoto legacy.<br /><br />Le dipendenze sono le seguenti:<br /><br />-Internet Information Services \(server Web IIS\): questa funzionalità è necessaria per configurare il server dei percorsi di rete e il probe Web predefinito.<br />-Windows Database-Used interna per l'accounting locale sul server di accesso remoto.|  
|Funzionalità Strumenti di Gestione Accesso remoto|Questa funzionalità viene installata come segue:<br /><br />-Viene installato per impostazione predefinita in un server di accesso remoto quando è installato il ruolo Accesso remoto e supporta l'interfaccia utente della console di gestione remota.<br />-Può essere installata facoltativamente in un server non è in esecuzione il ruolo di server di accesso remoto. In questo caso viene utilizzata per la gestione remota di un computer di Accesso remoto che esegue DirectAccess e VPN.<br /><br />La funzionalità Strumenti di Gestione Accesso remoto è costituita dai seguenti elementi:<br /><br />-Accesso remoto GUI e strumenti da riga di comando<br />-Modulo di accesso remoto per Windows PowerShell<br /><br />Le dipendenze includono:<br /><br />-Console Gestione criteri di gruppo<br />-RAS Connection Manager Administration Kit \(CMAK\)<br />-Windows PowerShell 3.0<br />-Infrastruttura e strumenti di gestione grafico|  
|Bilanciamento carico di rete|Questa funzionalità fornisce il bilanciamento del carico in un cluster usando il Bilanciamento carico di rete di Windows.|  

## <a name="BKMK_HARD"></a>Requisiti hardware  
I requisiti hardware per questo scenario includono i seguenti.  

-   Almeno due computer che soddisfino i requisiti hardware per Windows Server 2012.  

-   Per lo scenario di Load Balancer esterno, è necessario hardware dedicato \(ad esempio F5 BigIP\).  

-   Per testare lo scenario, è necessario disporre di almeno un computer che esegue Windows 10 configurato come client VPN Always On.   

## <a name="BKMK_SOFT"></a>Requisiti software  
Per questo scenario sono presenti numerosi requisiti.  

-   Requisiti software per una distribuzione a server singolo. Per ulteriori informazioni, vedere la pagina relativa alla [distribuzione di un server DirectAccess singolo con impostazioni avanzate](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md). Un singolo accesso remoto).  

-   Oltre ai requisiti software per un server singolo, esistono diversi cluster\-requisiti specifici:  

    -   In ogni server cluster il nome del soggetto del certificato HTTPS IP\-deve corrispondere all'indirizzo ConnectTo. Una distribuzione cluster supporta una combinazione di certificati con caratteri jolly e non\-nei server del cluster.  

    -   Se il server dei percorsi di rete è installato sul server di Accesso remoto, su ogni server cluster dovrà avere lo stesso nome soggetto. Inoltre, il nome del certificato del server dei percorsi di rete non deve essere lo stesso di qualsiasi altro server nella distribuzione di DirectAccess.  

    -   I certificati IP\-HTTPS e del server dei percorsi di rete devono essere emessi utilizzando lo stesso metodo con cui è stato emesso il certificato per il singolo server. Ad esempio, se il singolo server usa un'autorità di certificazione pubblica \(CA\), tutti i server nel cluster devono disporre di un certificato emesso da una CA pubblica. In alternativa, se il singolo server usa un certificato auto\-firmato per IP\-HTTPS, tutti i server nel cluster devono eseguire le stesse operazioni.  

    -   Il prefisso IPv6 assegnato ai computer client DirectAccess nei cluster di server deve essere di 59 bit. Se VPN è abilitato, anche il prefisso VPN deve essere di 59 bit.  

## <a name="KnownIssues"></a>Problemi noti  
Di seguito sono riportati alcuni problemi noti durante la configurazione di uno scenario cluster:  

-   Dopo la configurazione di DirectAccess in un\-IPv4 solo la distribuzione con una singola scheda di rete e dopo il DNS64 predefinito \(l'indirizzo IPv6 che contiene ": 3333::"\) viene configurato automaticamente nella scheda di rete, il tentativo di abilitare il bilanciamento del carico\-tramite la console di gestione accesso remoto comporta la richiesta all'utente di fornire un DIP IPv6. Se si specifica un DIP IPv6, la configurazione non riesce dopo aver fatto clic su **Commit** e viene visualizzato l'errore Parametro non corretto.  

    Per risolvere il problema:  

    1.  Scaricare gli script di backup e ripristino da [Backup e ripristino della configurazione di Accesso remoto](https://gallery.technet.microsoft.com/Back-up-and-Restore-Remote-e157e6a6).  

    2.  Eseguire il backup degli oggetti Criteri di gruppo di accesso remoto usando lo script di backup scaricato\-RemoteAccess. ps1  

    3.  Provare ad abilitare il bilanciamento del carico fino al passaggio in cui si verifica un errore. Nella finestra di dialogo Abilita bilanciamento del carico, espandere l'area dei dettagli, fare clic con il pulsante destro del mouse\-fare clic nell'area dei dettagli e quindi fare clic su **copia script**.  

    4.  Aprire Blocco note e incollare il contenuto degli Appunti. Ad esempio:  

        ```  
        Set-RemoteAccessLoadBalancer -InternetDedicatedIPAddress @('10.244.4.19 /255.255.255.0','fdc4:29bd:abde:3333::2/128') -InternetVirtualIPAddress @('fdc4:29bd:abde:3333::1/128', '10.244.4.21 /255.255.255.0') -ComputerName 'DA1.domain1.corp.contoso.com' -Verbose  
        ```  

    5.  Chiudere eventuali finestre di dialogo di Accesso remoto aperte e chiudere la Console di gestione Accesso remoto.  

    6.  Modificare il testo incollato e rimuovere gli indirizzi IPv6. Ad esempio:  

        ```  
        Set-RemoteAccessLoadBalancer -InternetDedicatedIPAddress @('10.244.4.19 /255.255.255.0') -InternetVirtualIPAddress @('10.244.4.21 /255.255.255.0') -ComputerName 'DA1.domain1.corp.contoso.com' -Verbose  
        ```  

    7.  In una finestra di PowerShell con privilegi elevati, eseguire il comando del passaggio precedente.  

    8.  Se il cmdlet ha esito negativo mentre è in esecuzione \(non a causa di valori di input non corretti\), eseguire il comando Restore\-RemoteAccess. ps1 e seguire le istruzioni per assicurarsi che l'integrità della configurazione originale venga mantenuta.  

    9. È ora possibile riaprire la Console di gestione Accesso remoto.  
