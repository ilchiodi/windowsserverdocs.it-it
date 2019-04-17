---
title: Distribuire un'infrastruttura Software Defined Network usando gli script
description: In questo argomento descrive come implementare un'infrastruttura di Microsoft rete SDN (Software Defined) utilizzando gli script in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.service: virtual-network
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 5ba5bb37-ece0-45cb-971b-f7149f658d19
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4428ad73ab8933510d5a759ec4fa7377ea222ebd
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="deploy-a-software-defined-network-infrastructure-using-scripts"></a>Distribuire un'infrastruttura Software Defined Network usando gli script

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

In questo argomento descrive come implementare un'infrastruttura di Microsoft rete SDN (Software Defined) utilizzando gli script. L'infrastruttura include un controller di rete (HA) a disponibilità elevata, un a disponibilità elevata Software carico bilanciamento (SLB) / MUX, reti virtuali e associati a elenchi di controllo di accesso (ACL). Inoltre, un altro script consente di distribuire un carico di lavoro tenant che permette di convalidare l'infrastruttura SDN.  
  
Se vuoi che i carichi di lavoro tenant per comunicare all'esterno di loro reti virtuali, è possibile impostare le regole SLB NAT, tunnel Gateway da sito a sito o l'inoltro di livello 3 per il routing tra i carichi di lavoro fisici e virtuali.  
  
È inoltre possibile distribuire un'infrastruttura SDN con Virtual Machine Manager (VMM). Per ulteriori informazioni, vedere [configurare un'infrastruttura di rete SDN (Software Defined) nell'infrastruttura VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-overview).  
  
## <a name="pre-deployment"></a>Pre-distribuzione  
  
> [!IMPORTANT]  
> Prima di iniziare la distribuzione, è necessario pianificare e configurare l'host e l'infrastruttura di rete fisica. Per ulteriori informazioni, vedere [pianificare un'infrastruttura di rete Software definito](../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md).  
  
Tutti gli host Hyper-V devono disporre di Windows Server 2016 già installato.  
  
## <a name="deployment-steps"></a>Passaggi di distribuzione  
Avviare la configurazione di commutatore virtuale Hyper-V (server fisico) e l'assegnazione di indirizzi IP dell'host Hyper-V. È possibile utilizzare qualsiasi tipo di archiviazione compatibile con Hyper-V, locale o condivisa.  
### <a name="install-host-networking"></a>Installazione di host di rete  
1. Installare i driver più recenti di rete disponibili per l'hardware NIC.  
2. Installare il ruolo Hyper-V in tutti gli host (per ulteriori informazioni, vedere [Introduzione a Hyper-V in Windows Server 2016](https://technet.microsoft.com/en-us/library/mt126159.aspx).   
  
   Da un prompt di Windows PowerShellcommand con privilegi elevato:  
   ``Install-WindowsFeature -Name Hyper-V -ComputerName <computer_name> -IncludeManagementTools -Restart``  
    
    un. Creare il commutatore virtuale Hyper-V (utilizzare lo stesso nome di commutatore per tutti gli host. Ad esempio: **sdnSwitch**). Configurare almeno una scheda di rete oppure, se utilizza Switch Embedded Teaming, configurare almeno due schede di rete. Massima diffusione in entrata si verifica quando si utilizzano due schede di rete.  
 `` New-VMSwitch "<switch name>" -NetAdapterName "<NetAdapter1>" [, "<NetAdapter2>" -EnableEmbeddedTeaming $True] -AllowManagementOS $True``  
 
 >[!NOTE] 
 >Se si utilizzano schede di rete di gestione separata, è possibile ignorare i passaggi 3 e 4.

3. Vedere l'argomento sulla pianificazione ([pianificare un'infrastruttura di rete Software definito](../../sdn/plan/../../sdn/plan/../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md)) e il corretto funzionamento con l'amministratore di rete per ottenere l'ID VLAN della VLAN gestione. Collegare la scheda di rete di gestione del commutatore virtuale appena creato per la gestione di VLAN. Questo passaggio può essere omesso se l'ambiente non vengono utilizzati tag VLAN.  
 `` Set-VMNetworkAdapterIsolation -ManagementOS -IsolationMode Vlan -DefaultIsolationID <Management VLAN> -AllowUntaggedTraffic $True``  
 
4. Vedere l'argomento sulla pianificazione ([pianificare un'infrastruttura di rete Software definito](../../sdn/plan/../../sdn/plan/../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md)) e il corretto funzionamento con l'amministratore di rete da utilizzare in DHCP o le assegnazioni di IP statiche per assegnare un indirizzo IP per la gestione vNIC di vSwitch appena creato. L'esempio seguente mostra come creare un indirizzo IP statico e assegnarlo a una scheda di rete di gestione di vSwitch:  
 ``New-NetIPAddress -InterfaceAlias "vEthernet (<switch name>)" -IPAddress <IP> -DefaultGateway <Gateway IP> -AddressFamily IPv4 -PrefixLength <Length of Subnet Mask - for example: 24>``  
      
5. [Facoltativo] Distribuire una macchina virtuale per ospitare servizi di dominio Active Directory ([installare Active Directory Domain Services (livello 100)](https://technet.microsoft.com/library/hh472162.aspx) e un Server DNS.  
   
    un. Connettere la macchina virtuale di Active Directory/Server DNS per la gestione di VLAN:
    
            Set-VMNetworkAdapterIsolation -VMName "<VM Name>" -Access -VlanId <Management VLAN> -AllowUntaggedTraffic $True  
   
   b. Installare servizi di dominio Active Directory e DNS.  
      >[!NOTE]
      >Il controller di rete supporta sia Kerberos e x. 509 certificati per l'autenticazione. Questa Guida Usa entrambi i meccanismi di autenticazione per scopi diversi (anche se è necessario solo uno).  
        
6. Aggiungere tutti gli host Hyper-V al dominio. Assicurarsi che la voce di server DNS per la scheda di rete con un indirizzo IP assegnato ai punti di rete di gestione a un server DNS in grado di risolvere il nome di dominio. Per esempio:

        Set-DnsClientServerAddress -InterfaceAlias "vEthernet (<switch name>)" -ServerAddresses <DNS Server IP>  
   
   un. Fare doppio clic su **Start**, fare clic su **sistema**, quindi fare clic su **modificare le impostazioni**.  
   b. Fare clic su **modifica**.  
   c. Fare clic su **dominio** e specificare il nome di dominio.  
   d. Fare clic su **OK**.  
   e. Digitare le credenziali di nome e la password dell'utente quando viene richiesto.  
   f. Riavviare il server.  
  
### <a name="validation"></a>Convalida  
Utilizzare la procedura seguente per convalidare l'host di rete sia configurato correttamente.  
1. Assicurarsi che il commutatore di macchina virtuale sia stato creato correttamente:  
      
    ``Get-VMSwitch "<switch name>"``  
2. Verificare che la scheda di rete di gestione del commutatore di macchina virtuale è connessa alla VLAN gestione:  
    >[!NOTE]
    >Pertinenti solo se il traffico di gestione e Tenant condivisione la stessa scheda di rete.    
      
    ``Get-VMNetworkAdapterIsolation -ManagementOS``  
3. Verificare che tutti gli host Hyper-V (e le risorse di gestione esterno, ad esempio: server DNS) sono accessibili tramite il comando ping con loro indirizzo IP di gestione e/o il nome di dominio completo (FQDN).   
      
   ``ping <Hyper-V Host IP>``  
   ``ping <Hyper-V Host FQDN>``  
4. Eseguire il comando seguente nell'host distribuzione e specificare il nome di dominio completo di ogni host Hyper-V per verificare le credenziali Kerberos utilizzate fornisce l'accesso a tutti i server.  
      
   ``winrm id -r:<Hyper-V Host FQDN>``  
      
### <a name="nano-installation-requirements-and-notes"></a>Note e i requisiti di installazione Nano  
Se si utilizza Nano come gli host Hyper-V (server fisici) per la distribuzione, di seguito sono requisiti aggiuntivi:  
1. Tutti i nodi di Nano è necessario che il pacchetto di DSC installato con il language pack:  
   
   * Microsoft-NanoServer-DSC-Package.cab  
   * Microsoft-NanoServer-DSC-Package_en-us.cab
   
        ``dism /online /add-package /packagepath:<Path> /loglevel:4``  
2. Gli script SDN Express devono essere eseguiti da un host non Nano (Windows Server Core o Windows Server con interfaccia utente grafica). I flussi di lavoro di PowerShell non sono supportate in Nano.  
3.  Chiamare l'API NorthBound del Controller di rete con PowerShell o i wrapper REST contesto dei nomi (che si basano su Invoke-WebRequest e Invoke-RestMethod) deve essere eseguita da un host non Nano.  
   
         
### <a name="run-sdn-express-scripts"></a>Eseguire gli script Express SDN  
  
1.  I file di installazione si trovano su GitHub. Scaricare il file zip dal [Microsoft GitHub Repository SDN](https://github.com/Microsoft/SDN.git). Nella pagina Microsoft SDN repository, fare clic su **Clone o download** e quindi fare clic su **Download ZIP**.  
  
2.  Designare un computer come computer di distribuzione.  Questo computer deve essere in esecuzione Windows Server 2016. Espandere il file zip e copia il **SDNExpress** cartella sul computer di distribuzione `C:\`cartella.  
  
3.  Condivisione di `C:\SDNExpress`cartella "**SDNExpress**" con l'autorizzazione per **Everyone** a **lettura/scrittura **.  
  
4.  Passare al `C:\SDNExpress`cartella.

 Vedrai le cartelle seguenti:  

|Nome cartella|Descrizione|  
|---------------|---------------|  
|AgentConf|Contiene le copie di schemi OVSDB utilizzati dall'agente Host SDN in ogni host Windows Server 2016 Hyper-V per i criteri di rete del programma.|  
|Certificati|Percorso temporaneo condivisa per il file di certificato del contesto dei nomi.|  
|Immagini|Svuotare, inserire qui l'immagine del vhdx di Windows Server 2016|  
|Strumenti|Utilità per la risoluzione dei problemi e il debug.  Copiare gli host e macchine virtuali.  È consigliabile che inserire Network Monitor Wireshark qui o per renderla disponibile se necessario.|  
|Script|Script di distribuzione.<br /><br />-   **SDNExpress.ps1**<br />    Distribuisce e configura l'infrastruttura, tra cui le macchine virtuali controller di rete, le macchine virtuali SLB Mux, pool di gateway e le macchine virtuali gateway di virtualizzazione rete corrispondente per il pool.<br />-   **FabricConfig.psd1**<br />    Un modello di file di configurazione per lo script SDNExpress.  Si personalizzerà per l'ambiente.<br />-   **SDNExpressTenant.ps1**<br />    Consente di distribuire un carico di lavoro tenant esempio in una rete virtuale con un indirizzo VIP con carico bilanciato.<br />    Esegue il provisioning anche uno o più connessioni di rete (GRE VPN S2S IPSec, L3) nel gateway edge provider del servizio che sono connessi al carico di lavoro tenant creato in precedenza. I gateway IPSec e GRE sono disponibili per la connettività tramite l'indirizzo IP corrispondente di indirizzi VIP e il gateway di inoltro L3 tramite il pool di indirizzi corrispondente.<br />    Questo script consente di eliminare la configurazione corrispondente con un'opzione Undo anche.<br />-   **TenantConfig.psd1**<br />    Un file di configurazione modello per la configurazione di gateway S2S e carico di lavoro tenant.<br />-   **SDNExpressUndo.ps1**<br />    La pulizia dell'ambiente di infrastruttura e viene ripristinato uno stato iniziale.<br />-   **SDNExpressEnterpriseExample.ps1**<br />    Esegue il provisioning uno o più ambienti di sito aziendali con un Gateway di accesso remoto e, facoltativamente, una macchina virtuale enterprise corrispondenti per ogni sito. I gateway aziendali IPSec o GRE si connette all'indirizzo VIP IP corrispondente del gateway del provider del servizio per stabilire i tunnel S2S. Il Gateway di inoltro L3 si connette tramite l'indirizzo IP del Peer corrispondente. <br />            Questo script consente di eliminare la configurazione corrispondente con un'opzione Undo anche.<br />-   **EnterpriseConfig.psd1**<br />    Un file di modello di configurazione per il gateway da sito a sito aziendale e la configurazione della macchina virtuale Client.|  
|TenantApps|File utilizzati per distribuire i carichi di lavoro di esempio tenant.|  
  
5.  Verificare il file VHDX di Windows Server 2016 è la **immagini** cartella.  
  
6. Personalizzare il file SDNExpress\scripts\FabricConfig.psd1 modificando il **<< sostituire >>** tag con valori specifici in base l'infrastruttura di laboratorio, compresi i nomi host, i nomi di dominio, nomi utente e password e informazioni sulla rete per le reti elencate nell'argomento pianificazione della rete.  
7. Creare un record Host DNS per il NetworkControllerRestName (FQDN) e NetworkControllerRestIP.  
8. Eseguire lo script come utente con credenziali di amministratore di dominio:  
      
    ``SDNExpress\scripts\SDNExpress.ps1 -ConfigurationDataFile FabricConfig.psd1 -Verbose``  
      
9.  Per annullare tutte le operazioni, eseguire il comando seguente:  
      
    ``SDNExpress\scripts\SDNExpressUndo.ps1 -ConfigurationDataFile FabricConfig.psd1 -Verbose``  
      
#### <a name="validation"></a>Convalida  
Supponendo che lo script SDN Express eseguita fino al completamento senza segnalare eventuali errori, è possibile eseguire il passaggio seguente per verificare le risorse di infrastruttura sono state distribuite correttamente e siano disponibili per la distribuzione di tenant.  

- Utilizzare [gli strumenti di diagnostica](https://docs.microsoft.com/windows-server/networking/sdn/troubleshoot/troubleshoot-windows-server-software-defined-networking-stack) per assicurarsi che non siano presenti errori in tutte le risorse di infrastruttura nel controller di rete.  
      
    ``Debug-NetworkControllerConfigurationState -NetworkController <FQDN of Network Controller Rest Name>``  
        
   
### <a name="deploy-a-sample-tenant-workload-with-the-software-load-balancer"></a>Distribuire un carico di lavoro tenant esempio con il bilanciamento del carico software  
    
Ora che sono state distribuite risorse di infrastruttura, è possibile convalidare il SDN distribuzione end-to-end tramite la distribuzione di un carico di lavoro di esempio tenant. Questo carico di lavoro tenant è costituito da due le subnet virtuali (livello web e il livello di database) protette tramite regole di elenco di controllo di accesso (ACL) utilizzando il firewall distribuito SDN. Subnet virtuale del livello web è accessibile tramite SLB/MUX utilizzando un indirizzo IP virtuale (VIP). Lo script automaticamente distribuisce due macchine virtuali di livello web e una macchina virtuale di livello un database e si connette per le subnet virtuali.  
  
1.  Personalizzare il file SDNExpress\scripts\TenantConfig.psd1 modificando il **<< sostituire >>** tag con valori specifici (ad esempio: l'immagine VHD nome, nome di altri controller di rete, vSwitch nome, definito in precedenza nel file FabricConfig.psd1 e così via)  
2.  Eseguire lo script. Per esempio:  
``SDNExpress\scripts\SDNExpressTenant.ps1 -ConfigurationDataFile TenantConfig.psd1 -Verbose``  
3.  Per annullare la configurazione, eseguire lo stesso script con il **Annulla** parametro. Per esempio:  
``SDNExpress\scripts\SDNExpressTenant.ps1 -Undo -ConfigurationDataFile TenantConfig.psd1 -Verbose``  

#### <a name="validation"></a>Convalida  
Per verificare che la distribuzione di tenant sia stata eseguita correttamente, eseguire le operazioni seguenti:
1.  Accedere alla macchina virtuale di livello del database e provare a effettuare il ping l'indirizzo IP di una delle macchine virtuali livello web (assicurarsi di che Windows Firewall è disattivato nelle macchine virtuali di livello web).  
2.  Controllare le risorse di tenant di controller di rete per individuare eventuali errori. Eseguire il comando seguente da qualsiasi host Hyper-V con connettività di livello 3 al controller di rete:  
      
    ``Debug-NetworkControllerConfigurationState -NetworkController <FQDN of Network Controller REST Name>``
3. Per verificare che il servizio di bilanciamento del carico venga eseguito correttamente, eseguire il comando seguente da qualsiasi host Hyper-V:
    
        wget <VIP IP address>/unique.htm -disablekeepalive -usebasicparsing
   
   Dove `<VIP IP address>`è il livello web è configurato nel file TenantConfig.psd1 indirizzo VIP IP. Cercare il `VIPIP`variabile in TenantConfig.psd1.

   Eseguire questo volte più per visualizzare il bilanciamento del carico di alternare il DIP disponibili. È anche possibile osservare questo comportamento usando un web browser. Passare a `<VIP IP address>/unique.htm`. Chiudere il browser apre una nuova istanza e passare nuovamente. Vedrai la pagina blu e la pagina verde alternativa, ad eccezione di quando il browser memorizza nella cache della pagina prima del timeout della cache.