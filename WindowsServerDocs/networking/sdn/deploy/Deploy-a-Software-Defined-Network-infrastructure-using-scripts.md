---
title: Distribuire un'infrastruttura Software Defined Network usando gli script
description: Questo argomento illustra come distribuire un'infrastruttura di Microsoft rete SDN (Software Defined) utilizzando gli script in Windows Server 2016.
manager: dougkim
ms.prod: windows-server-threshold
ms.service: virtual-network
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 5ba5bb37-ece0-45cb-971b-f7149f658d19
ms.author: pashort
author: shortpatti
ms.date: 08/23/2018
ms.openlocfilehash: ce88659f6065ddd0957b95831a4e3065f09dc9e9
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446371"
---
# <a name="deploy-a-software-defined-network-infrastructure-using-scripts"></a>Distribuire un'infrastruttura Software Defined Network usando gli script

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

In questo argomento, si distribuisce un'infrastruttura di Microsoft rete SDN (Software Defined) tramite script. L'infrastruttura include un controller di rete (disponibilità elevata) a disponibilità elevata, un a disponibilità elevata Software Load Balancer (SLB) / MUX, reti virtuali e associati elenchi di controllo di accesso (ACL). Inoltre, un altro script consente di distribuire un carico di lavoro tenant per confermare l'infrastruttura SDN.  

Se si desidera che i carichi di lavoro tenant per la comunicazione all'esterno le proprie reti virtuali, è possibile configurare le regole NAT di SLB, tunnel Gateway da sito a sito o l'inoltro di livello 3 per il routing tra i carichi di lavoro fisici e virtuali.  

È anche possibile distribuire un'infrastruttura SDN usando Virtual Machine Manager (VMM). Per altre informazioni, vedere [configurare un'infrastruttura di rete SDN (Software Defined) nell'infrastruttura di VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-overview).  


## <a name="pre-deployment"></a>Pre-distribuzione  

> [!IMPORTANT]  
> Prima di iniziare la distribuzione, è necessario pianificare e configurare gli host e l'infrastruttura di rete fisica. Per altre informazioni, vedi [Pianificare un'infrastruttura Software Defined Network](../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md).  

Tutti gli host Hyper-V devono avere Windows Server 2016 già installato.  

## <a name="deployment-steps"></a>Fasi di distribuzione  
Avviare la configurazione di commutatore virtuale Hyper-V (server fisico) e assegnazione di indirizzi IP dell'host Hyper-V. È possibile utilizzare qualsiasi tipo di archiviazione compatibile con Hyper-V, condivisa o locale.  

### <a name="install-host-networking"></a>Installazione di host di rete  

1. Installare i driver di rete più recenti disponibili per l'hardware di interfaccia di rete.  
2. Installare il ruolo Hyper-V in tutti gli host (per altre informazioni, vedere [Introduzione a Hyper-V in Windows Server 2016](https://docs.microsoft.com/windows-server/virtualization/hyper-v/get-started/Get-started-with-Hyper-V-on-Windows).   

   ```PowerShell
   Install-WindowsFeature -Name Hyper-V -ComputerName <computer_name> -IncludeManagementTools -Restart
   ```  

3. Creare il commutatore virtuale Hyper-V.<p>Usare lo stesso nome di opzione per tutti gli host, ad esempio, **sdnSwitch**. Configurare almeno una scheda di rete o, se utilizzati insieme, configurare almeno due schede di rete. La distribuzione in ingresso massimo si verifica quando si usano due schede di rete.  

   ```PowerShell
   New-VMSwitch "<switch name>" -NetAdapterName "<NetAdapter1>" [, "<NetAdapter2>" -EnableEmbeddedTeaming $True] -AllowManagementOS $True
   ```  
   >[!TIP] 
   >Se si dispone di schede di rete di gestione separata, è possibile ignorare i passaggi 4 e 5.

3. Vedere l'argomento pianificazione ([pianificare un'infrastruttura Software Defined Network](../../sdn/plan/../../sdn/plan/../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md)) e funzionano con l'amministratore di rete per ottenere l'ID VLAN della VLAN Management. Collegare la scheda di rete virtuale di gestione del commutatore virtuale appena creata per la gestione di VLAN. Questo passaggio può essere omesso se l'ambiente non vengono utilizzati i tag VLAN.  

   ```PowerShell
   Set-VMNetworkAdapterIsolation -ManagementOS -IsolationMode Vlan -DefaultIsolationID <Management VLAN> -AllowUntaggedTraffic $True
   ```  

4. Vedere l'argomento pianificazione ([pianificare un'infrastruttura Software Defined Network](../../sdn/plan/../../sdn/plan/../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md)) e funzionano con l'amministratore di rete per usare DHCP sia o assegnazioni di IP statiche per assegnare un indirizzo IP per la scheda di rete virtuale di gestione dell'oggetto appena creato commutatore virtuale. Nell'esempio seguente viene illustrato come creare un indirizzo IP statico e assegnarlo alla scheda di rete virtuale di gestione del commutatore virtuale:  

   ```PowerShell
   New-NetIPAddress -InterfaceAlias "vEthernet (<switch name>)" -IPAddress <IP> -DefaultGateway <Gateway IP> -AddressFamily IPv4 -PrefixLength <Length of Subnet Mask - for example: 24>
   ```  

5. [Facoltativo] Distribuire una macchina virtuale per ospitare servizi di dominio Active Directory ([installare Active Directory Domain Services (livello 100)](https://technet.microsoft.com/library/hh472162.aspx) e un Server DNS.  

    a. Connettere la macchina virtuale di Active Directory/DNS Server per la gestione delle VLAN:

       ```PowerShell
       Set-VMNetworkAdapterIsolation -VMName "<VM Name>" -Access -VlanId <Management VLAN> -AllowUntaggedTraffic $True  
       ```   

   b. Installare servizi di dominio Active Directory e DNS.  

   >[!NOTE]
   >Il controller di rete supporta i certificati sia Kerberos e X.509 per l'autenticazione. Questa Guida Usa entrambi i meccanismi di autenticazione per scopi diversi (anche se è necessario solo uno).  

6. Aggiungere tutti gli host Hyper-V al dominio. Verificare che la voce di server DNS per la scheda di rete che abbia un indirizzo IP assegnato ai punti della rete di gestione a un server DNS in grado di risolvere il nome di dominio. 

   ```PowerShell   
   Set-DnsClientServerAddress -InterfaceAlias "vEthernet (<switch name>)" -ServerAddresses <DNS Server IP>  
   ```

   a. Fare doppio clic su **avviare**, fare clic su **System**, quindi fare clic su **Cambia impostazioni**.  
   b. Fare clic su **Cambia**.  
   c. Fare clic su **dominio** e specificare il nome di dominio.  
   d. Fare clic su **OK**.  
   e. Digitare le credenziali nome utente e password quando richiesto.  
   f. Riavviare il server.  

### <a name="validation"></a>Convalida  
Utilizzare la procedura seguente per convalidare tale rete host è configurare correttamente.  

1. Assicurarsi che il commutatore di macchina virtuale è stato creato correttamente:  

   ```PowerShell
   Get-VMSwitch "<switch name>"
   ```  

2. Verificare che la scheda di rete virtuale di gestione del commutatore di macchina virtuale sia connessa alla VLAN Management:  

   >[!NOTE]
   >Si applica solo se il traffico di gestione e Tenant condivide la stessa interfaccia di rete.    

   ```PowerShell
   Get-VMNetworkAdapterIsolation -ManagementOS
   ```

3. Convalidare tutti gli host Hyper-V e le risorse di gestione esterno, ad esempio, i server DNS.<p>Controllare che siano accessibili tramite il ping con loro indirizzo IP di gestione e/o nome di dominio completo (FQDN).   

   ``ping <Hyper-V Host IP>``  
   ``ping <Hyper-V Host FQDN>``  

4. Eseguire il comando seguente nell'host di distribuzione e specificare il nome di dominio completo di ogni host Hyper-V per verificare che le credenziali Kerberos utilizzate fornisce l'accesso a tutti i server.  

   ``winrm id -r:<Hyper-V Host FQDN>``  

### <a name="nano-installation-requirements-and-notes"></a>Note e i requisiti di installazione Nano  

Se si usa Nano come host Hyper-V (server fisici) per la distribuzione, di seguito sono requisiti aggiuntivi:  

1. Tutti i nodi di Nano è necessario che il pacchetto DSC installato con il language pack:  

   - Microsoft-NanoServer-DSC-Package.cab  
   - Microsoft-NanoServer-DSC-Package_en-us.cab

     ``dism /online /add-package /packagepath:<Path> /loglevel:4``  

2. Gli script SDN Express devono essere eseguiti da un host non Nano (Windows Server Core o Windows Server con GUI). I flussi di lavoro di PowerShell non sono supportati in Nano.  

3. Richiamare l'API NorthBound del Controller di rete usando PowerShell o wrapper REST NC (che si basano su Invoke-WebRequest e Invoke-RestMethod) deve essere eseguita da un host non Nano.  


### <a name="run-sdn-express-scripts"></a>Eseguire gli script SDN Express  

1. Andare alla [GitHub Repository SDN di Microsoft](https://github.com/Microsoft/SDN.git) per i file di installazione.

2. Scaricare i file di installazione dal repository nel computer di distribuzione designata. Fare clic su **Clona o Scarica** e quindi fare clic su **Download ZIP**.  

   >[!NOTE]
   >Il computer designato distribuzione deve eseguire Windows Server 2016 o versione successiva.

3. Espandere il file con estensione zip e copiare il **SDNExpress** cartella nel computer di distribuzione `C:\` cartella.  

4. Condivisione il `C:\SDNExpress` cartella come "**SDNExpress**" con l'autorizzazione per **Everyone** a **lettura/scrittura**.  

5. Passare al `C:\SDNExpress` cartella.<p>Vengono visualizzate le cartelle seguenti:  


   | Nome cartella |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              Descrizione                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
   |-------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
   |  AgentConf  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   Contiene nuove copie degli schemi OVSDB utilizzati dall'agente Host SDN in ogni host di Windows Server 2016 Hyper-V per i criteri di rete del programma.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
   |    Certificati    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         Percorso temporaneo condiviso per il file del certificato dal controller di rete.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
   |   Immagini    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         Vuoto, inserire l'immagine di Windows Server 2016 vhdx qui                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
   |    Strumenti    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            Utilità per la risoluzione dei problemi e il debug.  Copiata negli host e macchine virtuali.  Si consiglia di che posizionare monitoraggio rete o Wireshark qui in modo che è disponibile se necessario.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
   |   Script   | Script di distribuzione.<br /><br />-   **SDNExpress.ps1**<br />    Distribuisce e configura l'infrastruttura, tra cui macchine virtuali del controller di rete, macchine virtuali Mux di bilanciamento del carico software, i pool di gateway e macchine virtuali gateway HNV corrispondente per il pool.<br />-   **FabricConfig.psd1**<br />    Un modello di file di configurazione per lo script SDNExpress.  Si personalizzerà per l'ambiente.<br />-   **SDNExpressTenant.ps1**<br />    Consente di distribuire un carico di lavoro tenant di esempio in una rete virtuale con un indirizzo VIP con carico bilanciato.<br />    Inoltre effettua il provisioning di una o più connessioni di rete (VPN S2S IPSec, GRE, L3) nei gateway edge provider del servizio che sono connessi al carico di lavoro tenant creato in precedenza. I gateway IPSec e GRE sono disponibili per la connettività tramite l'indirizzo IP VIP corrispondente e il gateway di inoltro L3 tramite pool di indirizzi corrispondente.<br />    Questo script è utilizzabile per eliminare la configurazione corrispondente con anche un'opzione di annullamento.<br />-   **TenantConfig.psd1**<br />    Un file di configurazione di modelli per la configurazione di gateway S2S e carico di lavoro tenant.<br />-   **SDNExpressUndo.ps1**<br />    Pulisce l'ambiente di infrastruttura e di ripristinarne uno stato iniziale.<br />-   **SDNExpressEnterpriseExample.ps1**<br />    Effettua il provisioning di una o più ambienti di sito aziendali con un Gateway di accesso remoto e (facoltativamente) una macchina virtuale enterprise corrispondente per ogni sito. I gateway aziendali IPSec o GRE si connette all'indirizzo IP VIP corrispondente del gateway del provider del servizio per stabilire i tunnel S2S. Il Gateway L3 di inoltro si connette tramite l'indirizzo IP del Peer corrispondente. <br />            Questo script è utilizzabile per eliminare la configurazione corrispondente con anche un'opzione di annullamento.<br />-   **EnterpriseConfig.psd1**<br />    Un file di configurazione di modello per il gateway aziendale site-to-site e la configurazione della macchina virtuale Client. |
   | TenantApps  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             File utilizzati per distribuire i carichi di lavoro di esempio tenant.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |

   ---

6. Verificare il file VHDX di Windows Server 2016 è la **immagini** cartella.  

7. Personalizzare il file SDNExpress\scripts\FabricConfig.psd1 modificando la **<< sostituire >>** tag con valori specifici per adattare l'infrastruttura di lab, compresi i nomi host, i nomi di dominio, i nomi utente e password, e informazioni di rete per le reti elencate nell'argomento pianificazione della rete.  

8. Creare un record Host A in DNS per le NetworkControllerRestIP NetworkControllerRestName (FQDN).  

9. Eseguire lo script come utente con le credenziali di amministratore di dominio:  

   ``SDNExpress\scripts\SDNExpress.ps1 -ConfigurationDataFile FabricConfig.psd1 -Verbose``  

10. Per annullare tutte le operazioni, eseguire il comando seguente:  

    ``SDNExpress\scripts\SDNExpressUndo.ps1 -ConfigurationDataFile FabricConfig.psd1 -Verbose``  

#### <a name="validation"></a>Convalida  

Supponendo che lo script SDN Express è stato eseguito fino al completamento senza segnalare eventuali errori, è possibile eseguire il passaggio seguente per verificare che le risorse di infrastruttura sono state distribuite correttamente e sono disponibili per la distribuzione di tenant.  

Uso [strumenti di diagnostica](https://docs.microsoft.com/windows-server/networking/sdn/troubleshoot/troubleshoot-windows-server-software-defined-networking-stack) per assicurarsi che non siano presenti errori in tutte le risorse di infrastruttura nel controller di rete.  

   ``Debug-NetworkControllerConfigurationState -NetworkController <FQDN of Network Controller Rest Name>``  


### <a name="deploy-a-sample-tenant-workload-with-the-software-load-balancer"></a>Distribuire un carico di lavoro tenant di esempio con il servizio di bilanciamento del carico software  

Ora che le risorse di infrastruttura sono state distribuite, è possibile convalidare la rete SDN distribuzione end-to-end tramite la distribuzione di un carico di lavoro tenant di esempio. Questo carico di lavoro tenant è costituito da due subnet virtuali (livello web e il livello di database) tramite le regole di elenco di controllo di accesso (ACL) tramite il firewall di rete SDN distribuito. Subnet virtuali del livello web sono accessibili tramite l'utilizzo di un indirizzo IP virtuale (VIP) SLB/MUX. Lo script distribuisce due macchine virtuali del livello web e macchina virtuale di livello un database automaticamente e si connette per le subnet virtuali.  

1.  Personalizzare il file SDNExpress\scripts\TenantConfig.psd1 modificando la **<< sostituire >>** tag con valori specifici (ad esempio: Nome immagine di disco rigido virtuale, nome REST controller di rete, nome del commutatore virtuale, come precedentemente definito nel file FabricConfig.psd1 e così via)  

2.  Eseguire lo script. Ad esempio:  

    ``SDNExpress\scripts\SDNExpressTenant.ps1 -ConfigurationDataFile TenantConfig.psd1 -Verbose``  

3.  Per annullare la configurazione, eseguire lo stesso script con il **annullare** parametro. Ad esempio:  

    ``SDNExpress\scripts\SDNExpressTenant.ps1 -Undo -ConfigurationDataFile TenantConfig.psd1 -Verbose``  

#### <a name="validation"></a>Convalida  

Per convalidare che la distribuzione di tenant è riuscita, eseguire le operazioni seguenti:

1. Accedere alla macchina virtuale di livello database e provare a il ping dell'indirizzo IP di una delle macchine virtuali del livello web (assicurarsi che Windows Firewall è disattivato in macchine virtuali del livello web).  

2. Controllare le risorse di tenant di controller di rete per individuare eventuali errori. Eseguire il comando seguente da qualsiasi host Hyper-V con connettività di livello 3 al controller di rete:  

   ``Debug-NetworkControllerConfigurationState -NetworkController <FQDN of Network Controller REST Name>``

3. Per verificare che il servizio di bilanciamento del carico venga eseguito correttamente, eseguire il comando seguente da qualsiasi host Hyper-V:

   ``wget <VIP IP address>/unique.htm -disablekeepalive -usebasicparsing``

   in cui `<VIP IP address>` è l'indirizzo IP VIP configurato nel file TenantConfig.psd1 di livello web. 

   >[!TIP]
   >Cercare il `VIPIP` variabile TenantConfig.psd1.

   Eseguire questo più volte per visualizzare il bilanciamento del carico passa tra la disponibilità DIP. È anche possibile osservare questo comportamento usando un web browser. Passare a `<VIP IP address>/unique.htm`. Chiudere il browser e aprire una nuova istanza e passare di nuovo. Si noterà la pagina blu e verde pagina alternativa, ad eccezione di quando il browser vengono memorizzati nella cache di pagina prima del timeout della cache.

---