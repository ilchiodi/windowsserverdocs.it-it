---
title: Distribuire Accesso remoto in un cluster
description: Questo argomento fa parte della Guida alla distribuzione di accesso remoto in un Cluster in Windows Server 2016.
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 853788f20c452391c802f0681fa23978b4892c6a
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281222"
---
# <a name="deploy-remote-access-in-a-cluster"></a>Distribuire Accesso remoto in un cluster

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Windows Server 2016 e Windows Server 2012 combina DirectAccess e servizio di accesso remoto \(RAS\) VPN in un unico ruolo Accesso remoto. È possibile distribuire accesso remoto in diversi scenari aziendali. Questa panoramica fornisce un'introduzione allo scenario aziendale per la distribuzione di più server di accesso remoto in un cluster con carico bilanciato con Windows Network Load Balancing \(NLB\) o con un servizio di bilanciamento del carico esterno \(ELB \), ad esempio F5 Big\-IP.  

## <a name="BKMK_OVER"></a>Descrizione dello scenario  
Una distribuzione di cluster raccoglie più server di accesso remoto in una singola unità, quindi agisce come un singolo punto di contatto per i computer client remoti connessione tramite DirectAccess o VPN alla rete azienda interna usando l'indirizzo IP virtuale esterno \(VIP\) indirizzo del cluster di accesso remoto.  Traffico al cluster con carico bilanciato tramite bilanciamento carico di rete di Windows o con un riferimento esterno bilanciamento del carico \(, ad esempio F5 Big\-IP\).  

## <a name="prerequisites"></a>Prerequisiti  
Prima di iniziare a distribuire questo scenario, esaminare l'elenco dei requisiti importanti:  

-   Bilanciamento del carico predefinito con Bilanciamento carico di rete di Windows.  

-   Sono supportati i servizi di bilanciamento del carico esterni.  

-   La modalità predefinita e consigliata per Bilanciamento carico di rete di Windows è Unicast.  

-   La modifica dei criteri all'esterno della console di gestione DirectAccess o con cmdlet di PowerShell non è supportata.  

-   Quando si utilizza Bilanciamento carico di rete o un servizio di bilanciamento del carico esterno, il prefisso IPHTTPS non può essere impostato su un valore qualsiasi diverso da \/59.  

-   I nodi con carico bilanciato devono trovarsi nella stessa subnet IPv4.  

-   Nelle distribuzioni ELB, se la gestione è necessaria, quindi non è possibile usare i client DirectAccess&nbsp;Teredo. Solo IPHTTPS può essere usato per l'estremità\-a\-interrompere la comunicazione.  

-   Verificare che tutti i noti bilanciamento carico di rete\/ELB hotfix sono installati.  

-   La tecnologia ISATAP non è supportata nella rete aziendale. Se si usa la tecnologia ISATAP, rimuoverla e usare la connettività IPv6 nativa.  

## <a name="in-this-scenario"></a>In questo scenario  
Lo scenario di distribuzione di cluster include alcuni passaggi:  

1.  [Distribuire un server VPN con le opzioni avanzate di AlwaysOn](../../vpn/always-on-vpn/deploy/always-on-vpn-adv-options.md). Un singolo server di accesso remoto con impostazioni avanzate devono essere distribuiti prima di configurare una distribuzione cluster.  

2.  [Pianificare una distribuzione di Cluster di accesso remoto](plan/Plan-a-Remote-Access-Cluster-Deployment.md). Per creare un cluster da una singola distribuzione server alcuni altri passaggi sono obbligatori, inclusa la preparazione dei certificati per la distribuzione del cluster.  

3.  [Configurare un Cluster di accesso remoto](configure/Configure-a-Remote-Access-Cluster.md). Si tratta di un numero di passaggi di configurazione, compresi preparazione del server singolo per Bilanciamento carico di rete di Windows o un servizio di bilanciamento del carico esterno, la preparazione di altri server da aggiungere al cluster e abilitare il bilanciamento del carico.  

## <a name="BKMK_APP"></a>Applicazioni pratiche  
La raccolta di più server in un cluster di server fornisce quanto segue:  

-   Scalabilità. Un singolo server di accesso remoto offre un livello limitato di scalabilità delle prestazioni e affidabilità dei server. Raggruppando le risorse di due o più server in un unico cluster, si aumenta la capacità per numero di utenti e velocità effettiva.  

-   Disponibilità elevata. Un cluster garantisce un'elevata disponibilità per sempre\-sull'accesso. Se un server nel cluster non riesce, gli utenti remoti possono continuare ad accedere alla rete aziendale usando un altro server nel cluster. Tutti i server del cluster hanno lo stesso set di IP virtuale del cluster \(VIP\) indirizzi, pur mantenendo un valore univoco, agli indirizzi IP dedicati per ogni server.  

-   Facilità\-di\-management. Un cluster consente la gestione di più server come singola entità. Le impostazioni condivise possono essere impostate facilmente in tutti i server cluster. Impostazioni di accesso remoto possono essere gestite da uno qualsiasi dei server in cluster o in remoto mediante strumenti di amministrazione remota del Server \(RSAT\). L'intero cluster può anche essere monitorato da una sola Console di gestione Accesso remoto.  

## <a name="BKMK_NEW"></a>Ruoli e funzionalità incluse in questo scenario  
Nella tabella seguente sono elencati i ruoli e le funzionalità richiesti per lo scenario.  

|Ruolo\/funzionalità|Modalità di supporto dello scenario|  
|---------|-----------------|  
|Ruolo Accesso remoto|Il ruolo viene installato e disinstallato tramite la console di Server Manager. Comprende DirectAccess, che in precedenza era una funzionalità in Windows Server 2008 R2 e servizio Routing e servizi di accesso remoto \(RRAS\), che in precedenza era un servizio ruolo in base ai criteri di rete e servizi di accesso \(NPAS\) ruolo del server. Il ruolo Accesso remoto è costituito da due componenti:<br /><br />-VPN always On e Routing e accesso remoto \(RRAS\) VPN DirectAccess e VPN vengono gestiti insieme nella console di gestione accesso remoto.<br />Funzionalità di routing di Routing RRAS routing e accesso REMOTO vengono gestite nella console di Routing e accesso remoto legacy.<br /><br />Le dipendenze sono le seguenti:<br /><br />-Internet Information Services \(IIS\) Server Web - questa funzionalità è necessaria per configurare il probe web predefinito e server del percorso di rete.<br />-Windows Database-Used interna per l'accounting locale sul server di accesso remoto.|  
|Funzionalità Strumenti di Gestione Accesso remoto|Questa funzionalità viene installata come segue:<br /><br />-Viene installato per impostazione predefinita in un server di accesso remoto quando è installato il ruolo Accesso remoto e supporta l'interfaccia utente della console di gestione remota.<br />-Può essere installata facoltativamente in un server non è in esecuzione il ruolo di server di accesso remoto. In questo caso viene utilizzata per la gestione remota di un computer di Accesso remoto che esegue DirectAccess e VPN.<br /><br />La funzionalità Strumenti di Gestione Accesso remoto è costituita dai seguenti elementi:<br /><br />-Accesso remoto GUI e strumenti da riga di comando<br />-Modulo di accesso remoto per Windows PowerShell<br /><br />Le dipendenze includono:<br /><br />-Console Gestione criteri di gruppo<br />-RAS Connection Manager Administration Kit \(Connection Manager Administration Kit\)<br />-Windows PowerShell 3.0<br />-Infrastruttura e strumenti di gestione grafico|  
|Bilanciamento carico di rete|Questa funzionalità fornisce il bilanciamento del carico in un cluster usando il Bilanciamento carico di rete di Windows.|  

## <a name="BKMK_HARD"></a>Requisiti hardware  
I requisiti hardware per questo scenario includono i seguenti.  

-   Almeno due computer che soddisfano i requisiti hardware per Windows Server 2012.  

-   Per lo scenario di bilanciamento del carico esterno, è necessario hardware dedicato \(ad esempio F5 BigIP\).  

-   Per testare lo scenario, è necessario disporre di almeno un computer che eseguono Windows 10 configurati come client VPN Always On.   

## <a name="BKMK_SOFT"></a>Requisiti software  
Per questo scenario sono presenti numerosi requisiti.  

-   Requisiti software per una distribuzione a server singolo. Per altre informazioni, vedere [distribuire un DirectAccess Server singolo con impostazioni avanzate](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md). Una singola di accesso remoto).  

-   Oltre ai requisiti software per un singolo server esistono una serie di cluster\-requisiti specifici:  

    -   In ogni server cluster IP\-certificato HTTPS nome soggetto deve corrispondere all'indirizzo ConnectTo. Una distribuzione cluster supporta una combinazione dei caratteri jolly e non\-certificati con caratteri jolly nei server cluster.  

    -   Se il server dei percorsi di rete è installato sul server di Accesso remoto, su ogni server cluster dovrà avere lo stesso nome soggetto. Inoltre, il nome del certificato del server dei percorsi di rete non deve essere lo stesso di qualsiasi altro server nella distribuzione di DirectAccess.  

    -   IP\-i certificati del server percorso HTTPS e la rete devono essere emesso con lo stesso metodo con cui è stato emesso il certificato per il singolo server. Ad esempio, se il singolo server usa un'autorità di certificazione pubbliche \(CA\) quindi tutti i server del cluster devono avere un certificato emesso da un'autorità di certificazione pubblica. O se il singolo server usa un self\-certificato autofirmato per IP\-HTTPS quindi tutti i server nel cluster dovranno fare altrettanto.  

    -   Il prefisso IPv6 assegnato ai computer client DirectAccess nei cluster di server deve essere di 59 bit. Se VPN è abilitato, anche il prefisso VPN deve essere di 59 bit.  

## <a name="KnownIssues"></a>Problemi noti  
Di seguito sono riportati alcuni problemi noti durante la configurazione di uno scenario cluster:  

-   Dopo aver configurato DirectAccess in IPv4\-distribuzione solo con una sola scheda di rete e dopo l'impostazione predefinita DNS64 \(l'indirizzo IPv6 che contiene ": 3333::"\) viene automaticamente configurato nella scheda di rete, tentativo di abilitare carico\-bilanciamento del carico tramite la console Gestione accesso remoto comporta una richiesta all'utente di fornire un DIP IPv6. Se viene fornito un DIP IPv6, la configurazione non riesce dopo aver fatto clic su **Commit** con l'errore: Parametro non corretto.  

    Per risolvere il problema:  

    1.  Scaricare gli script di backup e ripristino da [Backup e ripristino della configurazione di Accesso remoto](https://gallery.technet.microsoft.com/Back-up-and-Restore-Remote-e157e6a6).  

    2.  Eseguire il backup il GPO di accesso remoto usando il file di script Backup\-RemoteAccess.ps1  

    3.  Provare ad abilitare il bilanciamento del carico fino al passaggio in cui si verifica un errore. Nella finestra di dialogo Abilita bilanciamento del carico, espandere l'area dei dettagli a destra\-fare clic nell'area dei dettagli e quindi fare clic su **copia Script**.  

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

    8.  Se il cmdlet non riesce in fase di esecuzione \(non a causa di valori di input non corretti\), eseguire il comando Restore\-RemoteAccess.ps1 e seguire le istruzioni per assicurarsi che l'integrità della configurazione originale viene mantenuta .  

    9. È ora possibile riaprire la Console di gestione Accesso remoto.  
