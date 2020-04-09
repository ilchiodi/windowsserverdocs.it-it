---
title: Distribuire un'infrastruttura software defined Network usando gli script
description: Questo argomento illustra come distribuire un'infrastruttura SDN (software defined Network) Microsoft usando gli script in Windows Server 2016. '
manager: grcusanz
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 5ba5bb37-ece0-45cb-971b-f7149f658d19
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/23/2018
ms.openlocfilehash: 522c4c640daf438f122857154fed2b0ec8138bec
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855724"
---
# <a name="deploy-a-software-defined-network-infrastructure-using-scripts"></a>Distribuire un'infrastruttura Software Defined Network usando gli script

>Si applica a: Windows Server (canale semestrale), Windows Server 2016' in questo argomento si distribuisce un'infrastruttura SDN (software defined Network) Microsoft usando gli script. L'infrastruttura include un controller di rete a disponibilità elevata, Load Balancer un SLB/MUX, reti virtuali ed elenchi di controllo di accesso (ACL) associati. Inoltre, un altro script consente di distribuire un carico di lavoro tenant per convalidare l'infrastruttura SDN.  

Se si vuole che i carichi di lavoro del tenant comunicano all'esterno delle reti virtuali, è possibile configurare le regole NAT SLB, i tunnel del gateway da sito a sito o l'inoltro di livello 3 per il routing tra carichi di lavoro virtuali e fisici.  

È anche possibile distribuire un'infrastruttura SDN usando Virtual Machine Manager (VMM). Per altre informazioni, vedere [configurare un'infrastruttura SDN (software defined Network) nell'infrastruttura di VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-overview).  


## <a name="pre-deployment"></a>Pre-distribuzione  

> [!IMPORTANT]  
> Prima di iniziare la distribuzione, è necessario pianificare e configurare gli host e l'infrastruttura di rete fisica. Per altre informazioni, vedi [Pianificare un'infrastruttura Software Defined Network](../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md).  

Per tutti gli host Hyper-V deve essere installato Windows Server 2016.  

## <a name="deployment-steps"></a>Fasi di distribuzione  
Iniziare configurando il Commuter virtuale Hyper-V (server fisici) dell'host Hyper-V e l'assegnazione di indirizzi IP. È possibile utilizzare qualsiasi tipo di archiviazione compatibile con Hyper-V, Shared o local.  

### <a name="install-host-networking"></a>Installare la rete host  

1. Installare i driver di rete più recenti disponibili per l'hardware della scheda di interfaccia di rete.  
2. Installare il ruolo Hyper-V in tutti gli host (per altre informazioni, vedere [Introduzione a Hyper-v in Windows Server 2016](https://docs.microsoft.com/windows-server/virtualization/hyper-v/get-started/Get-started-with-Hyper-V-on-Windows).   

   ```PowerShell
   Install-WindowsFeature -Name Hyper-V -ComputerName <computer_name> -IncludeManagementTools -Restart
   ```  

3. Creare il Commuter virtuale Hyper-V.<p>Usare lo stesso nome Commuter per tutti gli host, ad esempio **sdnSwitch**. Configurare almeno una scheda di rete o, se si utilizza imposta, configurare almeno due schede di rete. Quando si usano due schede di rete, la distribuzione in ingresso massima si verifica.  

   ```PowerShell
   New-VMSwitch "<switch name>" -NetAdapterName "<NetAdapter1>" [, "<NetAdapter2>" -EnableEmbeddedTeaming $True] -AllowManagementOS $True
   ```  
   >[!TIP] 
   >Se sono presenti schede NIC separate, è possibile ignorare i passaggi 4 e 5.

3. Vedere l'argomento relativo alla pianificazione ([pianificare un'infrastruttura software defined Network](../../sdn/plan/../../sdn/plan/../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md)) e collaborare con l'amministratore di rete per ottenere l'ID VLAN della VLAN di gestione. Alleghi il vNIC di gestione del Commuter virtuale appena creato alla VLAN di gestione. Questo passaggio può essere omesso se l'ambiente in uso non utilizza tag VLAN.  

   ```PowerShell
   Set-VMNetworkAdapterIsolation -ManagementOS -IsolationMode Vlan -DefaultIsolationID <Management VLAN> -AllowUntaggedTraffic $True
   ```  

4. Vedere l'argomento relativo alla pianificazione ([pianificare un'infrastruttura software defined Network](../../sdn/plan/../../sdn/plan/../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md)) e collaborare con l'amministratore di rete per usare le assegnazioni IP DHCP o statiche per assegnare un indirizzo IP al VNIC di gestione del vswitch appena creato. Nell'esempio seguente viene illustrato come creare un indirizzo IP statico e assegnarlo alla vNIC di gestione di vSwitch:  

   ```PowerShell
   New-NetIPAddress -InterfaceAlias "vEthernet (<switch name>)" -IPAddress <IP> -DefaultGateway <Gateway IP> -AddressFamily IPv4 -PrefixLength <Length of Subnet Mask - for example: 24>
   ```  

5. Opzionale Distribuire una macchina virtuale per ospitare Active Directory Domain Services ([installare Active Directory Domain Services (livello 100)](https://technet.microsoft.com/library/hh472162.aspx) e un server DNS.  

    a. Connettere la macchina virtuale del server Active Directory/DNS alla VLAN di gestione:

       ```PowerShell
       Set-VMNetworkAdapterIsolation -VMName "<VM Name>" -Access -VlanId <Management VLAN> -AllowUntaggedTraffic $True  
       ```   

   b. Installare Active Directory Domain Services e DNS.  

   >[!NOTE]
   >Il controller di rete supporta sia i certificati Kerberos sia i certificati X. 509 per l'autenticazione. Questa guida USA entrambi i meccanismi di autenticazione per scopi diversi (sebbene sia necessario solo uno).  

6. Aggiungere tutti gli host Hyper-V al dominio. Verificare che la voce relativa al server DNS per la scheda di rete con un indirizzo IP assegnato alla rete di gestione punti a un server DNS in grado di risolvere il nome di dominio. 

   ```PowerShell   
   Set-DnsClientServerAddress -InterfaceAlias "vEthernet (<switch name>)" -ServerAddresses <DNS Server IP>  
   ```

   a. Fare clic con il pulsante destro del mouse su **Start**, scegliere **sistema**, quindi fare clic su **Cambia impostazioni**.  
   b. Fare clic su **Cambia**.  
   c. Fare clic su **dominio** e specificare il nome di dominio.  "" "d. Fare clic su **OK**.  
   e. Digitare le credenziali nome utente e password quando richiesto.  
   f. Riavviare il server.  

### <a name="validation"></a>Validation  
Utilizzare la procedura seguente per verificare che la rete host sia impostata correttamente.  

1. Verificare che l'opzione della macchina virtuale sia stata creata correttamente:  

   ```PowerShell
   Get-VMSwitch "<switch name>"
   ```  

2. Verificare che la vNIC di gestione nel commutire della macchina virtuale sia connessa alla VLAN di gestione:  

   >[!NOTE]
   >Pertinente solo se il traffico di gestione e tenant condivide la stessa scheda di interfaccia di rete.    

   ```PowerShell
   Get-VMNetworkAdapterIsolation -ManagementOS
   ```

3. Convalidare tutti gli host Hyper-V e le risorse di gestione esterne, ad esempio i server DNS.<p>Assicurarsi che siano accessibili tramite ping usando l'indirizzo IP di gestione e/o il nome di dominio completo (FQDN).   

   ``ping <Hyper-V Host IP>``  
   ``ping <Hyper-V Host FQDN>``  

4. Eseguire il comando seguente nell'host di distribuzione e specificare il nome di dominio completo di ogni host Hyper-V per assicurarsi che le credenziali Kerberos utilizzate forniscano l'accesso a tutti i server.  

   ``winrm id -r:<Hyper-V Host FQDN>``  

### <a name="nano-installation-requirements-and-notes"></a>Note e requisiti per l'installazione di nano  

Se si usa nano come host Hyper-V (server fisici) per la distribuzione, sono necessari i requisiti aggiuntivi seguenti:  

1. Per tutti i nodi nano deve essere installato il pacchetto DSC con il Language Pack:  

   - Microsoft-NanoServer-DSC-Package. cab  
   - Microsoft-nanoserver-DSC-Package_en-US. cab

     ``dism /online /add-package /packagepath:<Path> /loglevel:4``  

2. Gli script SDN Express devono essere eseguiti da un host non nano (Windows Server Core o Windows Server w/GUI). I flussi di lavoro di PowerShell non sono supportati in nano.  

3. Richiamando il controller di rete, l'API in Nord usando i wrapper REST di PowerShell o NC (basati su Invoke-WebRequest e Invoke-RestMethod) deve essere eseguita da un host non nano.  


### <a name="run-sdn-express-scripts"></a>Eseguire script SDN Express  

1. Passare al [repository GitHub di Microsoft Sdn](https://github.com/Microsoft/SDN.git) per i file di installazione.

2. Scaricare i file di installazione dal repository nel computer di distribuzione designato. Fare clic su **clona o Scarica** e quindi su **Scarica zip**.  

   >[!NOTE]
   >Il computer di distribuzione designato deve eseguire Windows Server 2016 o versione successiva.

3. Espandere il file zip e copiare la cartella **SDNExpress** nella cartella `C:\` del computer di distribuzione.  

4. Condividere la cartella `C:\SDNExpress` come "**SDNExpress**" con l'autorizzazione per la **lettura/scrittura**da parte di **tutti** .  

5. Passare alla cartella `C:\SDNExpress`.<p>Vengono visualizzate le cartelle seguenti:  


   | Nome cartella |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              Descrizione                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
   |-------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
   |  AgentConf  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   Include copie aggiornate degli schemi OVSDB usati dall'agente host SDN in ogni host Hyper-V di Windows Server 2016 per programmare i criteri di rete.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
   |    Certificati    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         Percorso condiviso temporaneo per il file del certificato NC.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
   |   Immagini    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         Vuota, inserire l'immagine di Windows Server 2016 VHDX qui                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
   |    Strumenti    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            Utilità per la risoluzione dei problemi e il debug.  Copiato negli host e nelle macchine virtuali.  È consigliabile inserire qui Network Monitor o Wireshark, in modo che sia disponibile, se necessario.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
   |   Script   | Script di distribuzione.<p>-   **SDNExpress. ps1**<br />    Consente di distribuire e configurare l'infrastruttura, incluse le macchine virtuali del controller di rete, le macchine virtuali SLB Mux, i pool di gateway e le macchine virtuali del gateway HNV corrispondenti ai pool.<br />-   **FabricConfig. psd1**<br />    Modello di file di configurazione per lo script SDNExpress.  Questa operazione verrà personalizzata per l'ambiente in uso.<br />-   **SDNExpressTenant. ps1**<br />    Distribuisce un carico di lavoro tenant di esempio in una rete virtuale con un indirizzo VIP con bilanciamento del carico.<br />    Esegue anche il provisioning di una o più connessioni di rete (IPSec S2S VPN, GRE, L3) sui gateway perimetrali del provider di servizi connessi al carico di lavoro tenant creato in precedenza. I gateway IPSec e GRE sono disponibili per la connettività tramite l'indirizzo IP VIP corrispondente e il gateway di invio L3 sul pool di indirizzi corrispondente.<br />    Questo script può essere usato per eliminare la configurazione corrispondente anche con un'opzione di annullamento.<br />-   **TenantConfig. psd1**<br />    Un file di configurazione del modello per il carico di lavoro tenant e la configurazione del gateway S2S.<br />-   **SDNExpressUndo. ps1**<br />    Pulisce l'ambiente dell'infrastruttura e lo reimposta sullo stato iniziale.<br />-   **SDNExpressEnterpriseExample. ps1**<br />    Effettua il provisioning di uno o più ambienti del sito aziendale con un gateway di accesso remoto e, facoltativamente, una macchina virtuale aziendale corrispondente per sito. Il Gateway Enterprise IPSec o GRE si connette all'indirizzo IP VIP corrispondente del gateway del provider di servizi per stabilire i tunnel S2S. Il gateway di inoltri L3 si connette all'indirizzo IP peer corrispondente. <br />            Questo script può essere usato per eliminare la configurazione corrispondente anche con un'opzione di annullamento.<br />-   **EnterpriseConfig. psd1**<br />    Un file di configurazione del modello per il gateway da sito a sito aziendale e la configurazione della macchina virtuale client. |
   | TenantApps  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             File usati per distribuire carichi di lavoro tenant di esempio.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |

   ---

6. Verificare che il file VHDX di Windows Server 2016 si trovi nella cartella **Immagini** .  

7. Personalizzare il file SDNExpress\scripts\FabricConfig.psd1 modificando il **< < sostituire > tag >** con valori specifici in modo da adattarli all'infrastruttura Lab, inclusi i nomi host, i nomi di dominio, i nomi utente e le password e le informazioni di rete per le reti elencate nell'argomento sulla rete di pianificazione.  

8. Creare un host a record in DNS per NetworkControllerRestName (FQDN) e NetworkControllerRestIP.  

9. Eseguire lo script come utente con credenziali di amministratore di dominio:  

   ``SDNExpress\scripts\SDNExpress.ps1 -ConfigurationDataFile FabricConfig.psd1 -Verbose``  

10. Per annullare tutte le operazioni, eseguire il comando seguente:  

    ``SDNExpress\scripts\SDNExpressUndo.ps1 -ConfigurationDataFile FabricConfig.psd1 -Verbose``  

#### <a name="validation"></a>Validation  

Supponendo che lo script SDN Express sia stato completato senza segnalare errori, è possibile eseguire il passaggio seguente per assicurarsi che le risorse dell'infrastruttura siano state distribuite correttamente e siano disponibili per la distribuzione dei tenant.  

Usare [strumenti di diagnostica](https://docs.microsoft.com/windows-server/networking/sdn/troubleshoot/troubleshoot-windows-server-software-defined-networking-stack) per assicurarsi che non siano presenti errori nelle risorse di infrastruttura del controller di rete.  

   ``Debug-NetworkControllerConfigurationState -NetworkController <FQDN of Network Controller Rest Name>``  


### <a name="deploy-a-sample-tenant-workload-with-the-software-load-balancer"></a>Distribuire un carico di lavoro tenant di esempio con il servizio di bilanciamento del carico software  

Ora che sono state distribuite le risorse dell'infrastruttura, è possibile convalidare la distribuzione SDN end-to-end distribuendo un esempio di carico di lavoro tenant. Questo carico di lavoro tenant è costituito da due subnet virtuali (livello Web e livello database) protette tramite le regole dell'elenco di controllo di accesso (ACL) che usano il firewall distribuito SDN. La subnet virtuale del livello Web è accessibile tramite SLB/MUX usando un indirizzo IP virtuale (VIP). Lo script distribuisce automaticamente due macchine virtuali di livello Web e una macchina virtuale di livello database e le connette alle subnet virtuali.  

1.  Personalizzare il file SDNExpress\scripts\TenantConfig.psd1 modificando il **< < sostituire > >** tag con valori specifici, ad esempio il nome dell'immagine VHD, il nome REST del controller di rete, il nome vswitch e così via, come definito in precedenza nel file FabricConfig. psd1.  

2.  Eseguire lo script. Ad esempio,  

    ``SDNExpress\scripts\SDNExpressTenant.ps1 -ConfigurationDataFile TenantConfig.psd1 -Verbose``  

3.  Per annullare la configurazione, eseguire lo stesso script con il parametro **Undo** . Ad esempio,  

    ``SDNExpress\scripts\SDNExpressTenant.ps1 -Undo -ConfigurationDataFile TenantConfig.psd1 -Verbose``  

#### <a name="validation"></a>Validation  

Per verificare che la distribuzione del tenant abbia avuto esito positivo, procedere come segue:

1. Accedere alla macchina virtuale di livello database e provare a effettuare il ping dell'indirizzo IP di una delle macchine virtuali del livello Web (assicurarsi che Windows Firewall sia disattivato nelle macchine virtuali di livello Web).  

2. Verificare la presenza di eventuali errori nelle risorse del tenant del controller di rete. Eseguire il comando seguente da qualsiasi host Hyper-V con connettività di livello 3 al controller di rete:  

   ``Debug-NetworkControllerConfigurationState -NetworkController <FQDN of Network Controller REST Name>``

3. Per verificare che il servizio di bilanciamento del carico venga eseguito correttamente, eseguire il comando seguente da qualsiasi host Hyper-V:

   ``wget <VIP IP address>/unique.htm -disablekeepalive -usebasicparsing``

   dove `<VIP IP address>` è l'indirizzo IP VIP del livello Web configurato nel file TenantConfig. psd1. 

   >[!TIP]
   >Cercare la variabile `VIPIP` in TenantConfig. psd1.

   Eseguire questo muliple volte per vedere l'opzione del servizio di bilanciamento del carico tra i dip disponibili. È possibile osservare questo comportamento anche usando un Web browser. Passare a `<VIP IP address>/unique.htm`. Chiudere il browser e aprire una nuova istanza e rieseguire la ricerca. Si noterà la pagina blu e la pagina verde alternativa, tranne quando il browser memorizza nella cache la pagina prima del timeout della cache.

---