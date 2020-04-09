---
title: Risolvere i problemi dello stack di SDN (Software Defined Networking) di Windows Server
description: Questa guida di Windows Server esamina gli scenari comuni di errore e di errore di rete SDN (Software Defined Networking) e illustra un flusso di lavoro per la risoluzione dei problemi che sfrutta gli strumenti di diagnostica disponibili.
manager: grcusanz
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 9be83ed2-9e62-49e8-88e7-f52d3449aac5
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/14/2018
ms.openlocfilehash: 90e3fd4bde06107871cc3a6b31939ca6b30f2473
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853614"
---
# <a name="troubleshoot-the-windows-server-software-defined-networking-stack"></a>Risolvere i problemi dello stack di SDN (Software Defined Networking) di Windows Server

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

In questa guida vengono esaminati gli scenari di errore e di errore comuni di SDN (Software Defined Networking) e viene descritto un flusso di lavoro per la risoluzione dei problemi che utilizza gli strumenti di diagnostica disponibili.  

Per ulteriori informazioni su Software Defined Networking di Microsoft, vedere [Software Defined Networking](../../sdn/Software-Defined-Networking--SDN-.md).  

## <a name="error-types"></a>Tipi di errore  
L'elenco seguente rappresenta la classe di problemi più spesso riscontrati con la virtualizzazione rete Hyper-V (HNVv1) in Windows Server 2012 R2 dalle distribuzioni di produzione nel mercato e coincide con molti modi con gli stessi tipi di problemi riscontrati in Windows Server 2016 HNVv2 con il nuovo stack SDN (software defined Network).  

La maggior parte degli errori può essere classificata in un set ridotto di classi:   
* **Configurazione non valida o non supportata**  
   Un utente richiama l'API in Nord in modo errato o con criteri non validi.   

* **Errore nell'applicazione dei criteri**  
     Il criterio del controller di rete non è stato recapitato a un host Hyper-V, ritardato in modo significativo e/o non aggiornato in tutti gli host Hyper-V, ad esempio dopo un Live Migration.  
* **Drift di configurazione o bug del software**  
  Problemi relativi al percorso dati che hanno causato l'eliminazione di pacchetti.  

* **Errore esterno relativo a hardware/driver NIC o all'infrastruttura di rete sottostante**  
  Offload delle attività con comportamento errato (ad esempio VMQ) o infrastruttura di rete sottoposto a configurazione non configurata (ad esempio MTU)   

  In questa guida alla risoluzione dei problemi vengono esaminate tutte le categorie di errore e vengono consigliate le procedure consigliate e gli strumenti di diagnostica disponibili per identificare e correggere l'errore.  

## <a name="diagnostic-tools"></a>Strumenti di diagnostica  

Prima di illustrare i flussi di lavoro per la risoluzione dei problemi per ognuno di questi tipi di errori, esaminare gli strumenti di diagnostica disponibili.   

Per usare gli strumenti di diagnostica del controller di rete (percorso), è necessario prima installare la funzionalità NetworkController e importare il modulo ``NetworkControllerDiagnostics``:  

```  
Add-WindowsFeature RSAT-NetworkController -IncludeManagementTools  
Import-Module NetworkControllerDiagnostics  
```  

Per usare gli strumenti di diagnostica HNV Diagnostics (percorso dati), è necessario importare il modulo ``HNVDiagnostics``:

```  
# Assumes RSAT-NetworkController feature has already been installed
Import-Module hnvdiagnostics   
```  

### <a name="network-controller-diagnostics"></a>Diagnostica del controller di rete  
Questi cmdlet sono documentati in TechNet nell' [argomento cmdlet di diagnostica del controller di rete](https://docs.microsoft.com/powershell/module/networkcontrollerdiagnostics/). Consentono di identificare i problemi di coerenza dei criteri di rete nel percorso di controllo tra i nodi del controller di rete e tra il controller di rete e gli agenti host NC in esecuzione negli host Hyper-V.

 I cmdlet _debug-ServiceFabricNodeStatus_ e _Get-NetworkControllerReplica_ devono essere eseguiti da una delle macchine virtuali del nodo del controller di rete. Tutti gli altri cmdlet di diagnostica NC possono essere eseguiti da qualsiasi host che disponga di connettività al controller di rete e si trova nel gruppo di sicurezza di gestione del controller di rete (Kerberos) oppure può accedere al certificato X. 509 per la gestione del controller di rete. 

### <a name="hyper-v-host-diagnostics"></a>Diagnostica host Hyper-V  
Questi cmdlet sono documentati in TechNet nell' [argomento cmdlet di diagnostica di virtualizzazione rete Hyper-V (HNV)](https://docs.microsoft.com/powershell/module/hnvdiagnostics/). Consentono di identificare i problemi del percorso dati tra le macchine virtuali tenant (est/ovest) e il traffico in ingresso tramite un indirizzo VIP SLB (Nord/Sud). 

I test di _debug-VirtualMachineQueueOperation_, _Get-CustomerRoute_, _Get-PACAMapping_, _Get-ProviderAddress_, _Get-VMNetworkAdapterPortId_, _Get-VMSwitchExternalPortId_e _test-EncapOverheadSettings_ sono tutti test locali che possono essere eseguiti da qualsiasi host Hyper-V. Gli altri cmdlet richiamano i test del percorso dati tramite il controller di rete e pertanto necessitano dell'accesso al controller di rete come descried sopra.

### <a name="github"></a>GitHub
Il [repository GitHub Microsoft/Sdn](https://github.com/microsoft/sdn) include una serie di script di esempio e flussi di lavoro che si basano su questi cmdlet predefiniti. In particolare, gli script di diagnostica sono reperibili nella cartella [diagnostica](https://github.com/Microsoft/sdn/diagnostics) . Per contribuire a questi script, inviare richieste pull.

## <a name="troubleshooting-workflows-and-guides"></a>Risoluzione dei problemi relativi a flussi di lavoro e guide  

### <a name="hoster-validate-system-health"></a>Hosting Convalida integrità sistema
In molte risorse del controller di rete è presente una risorsa incorporata denominata _stato di configurazione_ . Lo stato di configurazione fornisce informazioni sull'integrità del sistema, inclusa la coerenza tra la configurazione del controller di rete e lo stato effettivo (in esecuzione) negli host Hyper-V. 

Per controllare lo stato della configurazione, eseguire il comando seguente da qualsiasi host Hyper-V con connettività al controller di rete.

>[!NOTE] 
>Il valore del parametro *NetworkController* deve essere il nome di dominio completo o l'indirizzo IP in base al nome del soggetto del certificato X. 509 > creato per il controller di rete.
>
>Il parametro *Credential* deve essere specificato solo se il controller di rete usa l'autenticazione Kerberos (tipica nelle distribuzioni VMM). Le credenziali devono essere relative a un utente che si trova nel gruppo di sicurezza di gestione del controller di rete.

```none
Debug-NetworkControllerConfigurationState -NetworkController <FQDN or NC IP> [-Credential <PS Credential>]

# Healthy State Example - no status reported
$cred = Get-Credential
Debug-NetworkControllerConfigurationState -NetworkController 10.127.132.211 -Credential $cred

Fetching ResourceType:     accessControlLists
Fetching ResourceType:     servers
Fetching ResourceType:     virtualNetworks
Fetching ResourceType:     networkInterfaces
Fetching ResourceType:     virtualGateways
Fetching ResourceType:     loadbalancerMuxes
Fetching ResourceType:     Gateways
```

Di seguito è riportato un messaggio di stato di configurazione di esempio:

```none
Fetching ResourceType:     servers
---------------------------------------------------------------------------------------------------------
ResourcePath:     https://10.127.132.211/Networking/v1/servers/4c4c4544-0056-4b10-8058-b8c04f395931
Status:           Warning

Source:           SoftwareLoadBalancerManager
Code:             HostNotConnectedToController
Message:          Host is not Connected.
----------------------------------------------------------------------------------------------------------
```

>[!NOTE]
> Nel sistema è presente un bug in cui le risorse dell'interfaccia di rete per la scheda di interfaccia di rete della macchina virtuale SLB Mux Transit sono in stato di errore con errore "switch virtuale-host non connesso al controller". Questo errore può essere ignorato se la configurazione IP nella risorsa NIC della macchina virtuale è impostata su un indirizzo IP del pool IP della rete logica di transito. Nel sistema è presente un secondo bug in cui le risorse dell'interfaccia di rete per le schede NIC della VM del provider HNV del gateway sono in stato di errore con errore "Virtual Switch-PortBlocked". Questo errore può anche essere ignorato se la configurazione IP nella risorsa NIC della macchina virtuale è impostata su null (da progettazione).


La tabella seguente mostra l'elenco dei codici di errore, dei messaggi e delle azioni di completamento da eseguire in base allo stato di configurazione osservato.


| **Codice**| **Messaggio**| **Azione**|  
|--------|-----------|----------|  
| Sconosciuto| Errore sconosciuto| |  
| HostUnreachable                       | Il computer host non è raggiungibile | Controllare la connettività di rete di gestione tra il controller di rete e l'host |  
| PAIpAddressExhausted                  | Indirizzi IP PA esauriti | Aumentare le dimensioni del pool IP della subnet logica del provider HNV |  
| PAMacAddressExhausted                 | Indirizzi Mac PA esauriti | Aumentare l'intervallo del pool Mac |  
| PAAddressConfigurationFailure         | Non è stato possibile plumbare gli indirizzi PA nell'host | Controllare la connettività di rete di gestione tra il controller di rete e l'host. |  
| CertificateNotTrusted                 | Il certificato non è attendibile  |Correggere i certificati utilizzati per la comunicazione con l'host. |  
| CertificateNotAuthorized              | Certificato non autorizzato | Correggere i certificati utilizzati per la comunicazione con l'host. |  
| PolicyConfigurationFailureOnVfp       | Errore durante la configurazione di criteri VFP | Si tratta di un errore di Runtime.  Nessuna soluzione definitiva. Raccogliere i log. |  
| PolicyConfigurationFailure            | Errore durante il push dei criteri agli host a causa di errori di comunicazione o di altri errori in NetworkController.| Nessuna azione definita.  Ciò è dovuto a un errore nell'elaborazione dello stato degli obiettivi nei moduli del controller di rete. Raccogliere i log. |  
| HostNotConnectedToController          | L'host non è ancora connesso al controller di rete | Il profilo di porta non è applicato nell'host o l'host non è raggiungibile dal controller di rete. Verificare che la chiave del registro di sistema HostID corrisponda all'ID istanza della risorsa server |  
| MultipleVfpEnabledSwitches            | Nell'host sono presenti più commutatori abilitati per VFp  | Eliminare una delle opzioni, poiché l'agente host del controller di rete supporta solo una vSwitch con l'estensione VFP abilitata |  
| PolicyConfigurationFailure            | Non è stato possibile eseguire il push dei criteri VNet per un scheda a causa di errori del certificato o di connettività  | Controllare se sono stati distribuiti certificati appropriati (il nome del soggetto del certificato deve corrispondere al nome di dominio completo dell'host). Verificare inoltre la connettività dell'host con il controller di rete |  
| PolicyConfigurationFailure            | Non è stato possibile eseguire il push dei criteri vSwitch per un scheda a causa di errori del certificato o di connettività  | Controllare se sono stati distribuiti certificati appropriati (il nome del soggetto del certificato deve corrispondere al nome di dominio completo dell'host). Verificare inoltre la connettività dell'host con il controller di rete|
| PolicyConfigurationFailure            | Non è stato possibile eseguire il push dei criteri firewall per un scheda a causa di errori del certificato o di connettività | Controllare se sono stati distribuiti certificati appropriati (il nome del soggetto del certificato deve corrispondere al nome di dominio completo dell'host). Verificare inoltre la connettività dell'host con il controller di rete|
| DistributedRouterConfigurationFailure | Impossibile configurare le impostazioni del router distribuito nell'host vNic                          | Errore dello stack TCPIP. Potrebbe richiedere la pulizia dell'host PA e del ripristino di emergenza schede nel server in cui è stato segnalato questo errore |
| DhcpAddressAllocationFailure          | Allocazione indirizzi DHCP non riuscita per un scheda                                                    | Controllare se l'attributo dell'indirizzo IP statico è configurato nella risorsa NIC |  
| CertificateNotTrusted<br>CertificateNotAuthorized | Non è stato possibile connettersi a Mux a causa di errori di rete o di certificato | Controllare il codice numerico fornito nel codice del messaggio di errore: corrisponde al codice di errore Winsock. Gli errori del certificato sono granulari (ad esempio, non è possibile verificare il certificato, il certificato non è autorizzato e così via) |  
| HostUnreachable                       | MUX non è integro (il caso comune è BGPRouter disconnesso) | Il peer BGP su RRAS (macchina virtuale BGP) o il commutatore Top-of-rack (ToR) non è raggiungibile o il peering non è riuscito. Controllare le impostazioni BGP nella risorsa software Load Balancer multiplexer e nel peer BGP (macchina virtuale ToR o RRAS) |  
| HostNotConnectedToController          | L'agente host SLB non è connesso  | Verificare che il servizio agente host SLB sia in esecuzione. Vedere i log dell'agente host SLB (esecuzione automatica) per i motivi per cui, nel caso in cui SLBM (NC) ha rifiutato il certificato presentato dallo stato di esecuzione dell'agente host, visualizzerà informazioni sfumate  |  
| PortBlocked                           | La porta VFP è bloccata, a causa della mancanza di criteri VNET/ACL | Controllare se sono presenti altri errori che potrebbero causare la mancata configurazione dei criteri. |  
| Overloaded                            | LoadBalancer MUX è sovraccarico  | Problemi di prestazioni con MUX |  
| RoutePublicationFailure               | MUX LoadBalancer non è connesso a un router BGP | Controllare se la MUX ha connettività con i router BGP e che il peering BGP sia configurato correttamente |  
| VirtualServerUnreachable              | LoadBalancer MUX non è connesso a SLB Manager | Controllare la connettività tra SLBM e MUX |  
| QosConfigurationFailure               | Non è stato possibile configurare i criteri QOS | Controllare se è disponibile una larghezza di banda sufficiente per tutte le VM se si usa la prenotazione QOS |


#### <a name="check-network-connectivity-between-the-network-controller-and-hyper-v-host-nc-host-agent-service"></a>Controllare la connettività di rete tra il controller di rete e l'host Hyper-V (servizio agente host NC)
Eseguire il comando *netstat* seguente per verificare che siano presenti tre connessioni stabilite tra l'agente host NC e i nodi del controller di rete e un socket di ascolto nell'host Hyper-V
- ASCOLTO sulla porta TCP: 6640 nell'host Hyper-V (servizio agente host NC)
- Due connessioni stabilite dall'IP dell'host Hyper-V sulla porta 6640 all'indirizzo IP del nodo NC sulle porte temporanee (> 32000)
- Una connessione stabilita dall'IP dell'host Hyper-V sulla porta temporanea all'IP REST del controller di rete sulla porta 6640

>[!NOTE]
>In un host Hyper-V possono essere presenti solo due connessioni stabilite se non sono presenti macchine virtuali tenant distribuite in quel particolare host.

```none
netstat -anp tcp |findstr 6640

# Successful output
  TCP    0.0.0.0:6640           0.0.0.0:0              LISTENING
  TCP    10.127.132.153:6640    10.127.132.213:50095   ESTABLISHED
  TCP    10.127.132.153:6640    10.127.132.214:62514   ESTABLISHED
  TCP    10.127.132.153:50023   10.127.132.211:6640    ESTABLISHED
```
#### <a name="check-host-agent-services"></a>Controllare i servizi dell'agente host
Il controller di rete comunica con due servizi dell'agente host negli host Hyper-V: agente host SLB e agente host NC. È possibile che uno o entrambi i servizi non siano in esecuzione. Controllare lo stato e riavviare se non sono in esecuzione.

```none
Get-Service SlbHostAgent
Get-Service NcHostAgent

# (Re)start requires -Force flag
Start-Service NcHostAgent -Force
Start-Service SlbHostAgent -Force
```

#### <a name="check-health-of-network-controller"></a>Controllare l'integrità del controller di rete
Se non sono presenti tre connessioni STABILIte o se il controller di rete non risponde, verificare che tutti i nodi e i moduli del servizio siano operativi con i cmdlet seguenti. 

```none
# Prints a DIFF state (status is automatically updated if state is changed) of a particular service module replica 
Debug-ServiceFabricNodeStatus [-ServiceTypeName] <Service Module>
```
I moduli del servizio di controller di rete sono:
- ControllerService
- ApiService
- SlbManagerService
- ServiceInsertion
- FirewallService
- VSwitchService
- GatewayManager
- FnmService
- HelperService
- UpdateService

Verificare che ReplicaStatus sia **pronto** e che HealthState sia **OK**.

In una distribuzione di produzione è con un controller di rete a più nodi, è anche possibile controllare il nodo su cui è primario ogni servizio e il relativo stato di replica.

```none  
Get-NetworkControllerReplica

# Sample Output for the API service module 
Replicas for service: ApiService

ReplicaRole   : Primary
NodeName      : SA18N30NC3.sa18.nttest.microsoft.com
ReplicaStatus : Ready

```
Verificare che lo stato della replica sia pronto per ogni servizio.

#### <a name="check-for-corresponding-hostids-and-certificates-between-network-controller-and-each-hyper-v-host"></a>Verificare la presenza di HostIDs e certificati corrispondenti tra il controller di rete e ogni host Hyper-V 
In un host Hyper-V eseguire i comandi seguenti per verificare che HostID corrisponda all'ID istanza di una risorsa server nel controller di rete

```none
Get-ItemProperty "hklm:\system\currentcontrolset\services\nchostagent\parameters" -Name HostId |fl HostId

HostId : **162cd2c8-08d4-4298-8cb4-10c2977e3cfe**

Get-NetworkControllerServer -ConnectionUri $uri |where { $_.InstanceId -eq "162cd2c8-08d4-4298-8cb4-10c2977e3cfe"}

Tags             :
ResourceRef      : /servers/4c4c4544-0056-4a10-8059-b8c04f395931
InstanceId       : **162cd2c8-08d4-4298-8cb4-10c2977e3cfe**
Etag             : W/"50f89b08-215c-495d-8505-0776baab9cb3"
ResourceMetadata : Microsoft.Windows.NetworkController.ResourceMetadata
ResourceId       : 4c4c4544-0056-4a10-8059-b8c04f395931
Properties       : Microsoft.Windows.NetworkController.ServerProperties
```

*Correzione* Se si usano script SDNExpress o la distribuzione manuale, aggiornare la chiave HostId nel registro di sistema in modo che corrisponda all'ID istanza della risorsa server. Riavviare l'agente host del controller di rete nell'host Hyper-V (server fisico) se si utilizza VMM, eliminare il server Hyper-V da VMM e rimuovere la chiave del registro di sistema HostId. Quindi, aggiungere nuovamente il server tramite VMM.


Verificare che le identificazioni personali dei certificati X. 509 utilizzate dall'host Hyper-V (il nome host sarà il nome del soggetto del certificato) per la comunicazione (a sud) tra l'host Hyper-V (servizio agente host NC) e i nodi del controller di rete siano uguali. Verificare inoltre che il certificato REST del controller di rete abbia il nome del soggetto *CN =<FQDN or IP>* .

```  
# On Hyper-V Host
dir cert:\\localmachine\my  

Thumbprint                                Subject
----------                                -------
2A3A674D07D8D7AE11EBDAC25B86441D68D774F9  CN=SA18n30-4.sa18.nttest.microsoft.com
...

dir cert:\\localmachine\root

Thumbprint                                Subject
----------                                -------
30674C020268AA4E40FD6817BA6966531FB9ADA4  CN=10.127.132.211 **# NC REST IP ADDRESS**

# On Network Controller Node VM
dir cert:\\localmachine\root  

Thumbprint                                Subject
----------                                -------
2A3A674D07D8D7AE11EBDAC25B86441D68D774F9  CN=SA18n30-4.sa18.nttest.microsoft.com
30674C020268AA4E40FD6817BA6966531FB9ADA4  CN=10.127.132.211 **# NC REST IP ADDRESS**
...
```  

È anche possibile controllare i parametri seguenti di ogni certificato per assicurarsi che il nome del soggetto sia quello previsto (nome host o FQDN REST NC o IP), il certificato non è ancora scaduto e che tutte le autorità di certificazione nella catena di certificati siano incluse nella radice attendibile autorità.

- Nome soggetto  
- Data di scadenza  
- Attendibile per autorità radice  

*Correzione* Se più certificati hanno lo stesso nome soggetto nell'host Hyper-V, l'agente host del controller di rete sceglierà in modo casuale uno per presentare il controller di rete. Questo potrebbe non corrispondere all'identificazione personale della risorsa server nota al controller di rete. In tal caso, eliminare uno dei certificati con lo stesso nome soggetto nell'host Hyper-V e quindi riavviare il servizio agente host del controller di rete. Se non è ancora possibile effettuare una connessione, eliminare l'altro certificato con lo stesso nome soggetto nell'host Hyper-V ed eliminare la risorsa server corrispondente in VMM. Quindi, ricreare la risorsa server in VMM, che genererà un nuovo certificato X. 509 e lo installerà nell'host Hyper-V.


#### <a name="check-the-slb-configuration-state"></a>Controllare lo stato di configurazione di SLB
Lo stato di configurazione SLB può essere determinato come parte dell'output del cmdlet debug-NetworkController. Questo cmdlet restituirà anche il set corrente di risorse del controller di rete nei file JSON, tutte le configurazioni IP da ogni host Hyper-V (Server) e criteri di rete locale dalle tabelle di database dell'agente host. 

Per impostazione predefinita, verranno raccolte tracce aggiuntive. Per non raccogliere tracce, aggiungere il parametro-IncludeTraces: $false.

```none
Debug-NetworkController -NetworkController <FQDN or IP> [-Credential <PS Credential>] [-IncludeTraces:$false]

# Don't collect traces
$cred = Get-Credential 
Debug-NetworkController -NetworkController 10.127.132.211 -Credential $cred -IncludeTraces:$false

Transcript started, output file is C:\\NCDiagnostics.log
Collecting Diagnostics data from NC Nodes
```

>[!NOTE]
>Il percorso di output predefinito sarà il < working_directory > Directory \NCDiagnostics\. La directory di output predefinita può essere modificata tramite il parametro `-OutputDirectory`. 

Le informazioni sullo stato di configurazione di SLB sono disponibili nel file _Diagnostics-slbstateResults. JSON_ in questa directory.

Questo file JSON può essere suddiviso nelle sezioni seguenti:
 * Infrastruttura
   * SlbmVips: questa sezione elenca l'indirizzo IP dell'indirizzo VIP di SLB Manager usato dal controller di rete per la configurazione e lo stato di integrità tra gli agenti host SLB mux e SLB.
   * MuxState-questa sezione elenca un valore per ogni Mux SLB distribuita che fornisce lo stato di MUX
   * Configurazione router: questa sezione elenca il numero di sistema autonomo (ASN) del router upstream, l'indirizzo IP di transito e l'ID. Vengono inoltre elencati i SLB Mux ASN e Transit IP.
   * Informazioni sull'host connesso: questa sezione elenca gli indirizzi IP di gestione di tutti gli host Hyper-V disponibili per l'esecuzione di carichi di lavoro con carico bilanciato.
   * Intervalli VIP: in questa sezione vengono elencati gli intervalli di pool di indirizzi IP VIP pubblici e privati. Il VIP SLBM verrà incluso come IP allocato da uno di questi intervalli. 
   * Route Mux: questa sezione elenca un valore per ogni Mux SLB distribuito contenente tutti gli annunci di route per quel particolare MUX.
 * Tenant
   * VipConsolidatedState: questa sezione elenca lo stato di connettività per ogni indirizzo VIP del tenant, inclusi il prefisso di route annunciato, l'host Hyper-V e gli endpoint DIP.

> [!NOTE]
> Lo stato SLB può essere verificato direttamente usando lo script [DumpSlbRestState](https://github.com/Microsoft/SDN/blob/master/Diagnostics/DumpSlbRestState.ps1) disponibile nel [repository GITHUB di Microsoft Sdn](https://github.com/microsoft/sdn). 

#### <a name="gateway-validation"></a>Convalida del gateway

**Dal controller di rete:**
```
Get-NetworkControllerLogicalNetwork
Get-NetworkControllerPublicIPAddress
Get-NetworkControllerGatewayPool
Get-NetworkControllerGateway
Get-NetworkControllerVirtualGateway
Get-NetworkControllerNetworkInterface
```

**Dalla macchina virtuale del gateway:**
```
Ipconfig /allcompartments /all
Get-NetRoute -IncludeAllCompartments -AddressFamily
Get-NetBgpRouter
Get-NetBgpRouter | Get-BgpPeer
Get-NetBgpRouter | Get-BgpRouteInformation
```

**Dal commutatore Top of rack (ToR):**

`sh ip bgp summary (for 3rd party BGP Routers)`

**Router BGP Windows**
```
Get-BgpRouter
Get-BgpPeer
Get-BgpRouteInformation
```

Oltre a questi, dai problemi che abbiamo visto finora (soprattutto nelle distribuzioni basate su SDNExpress), il motivo più comune per cui il raggruppamento dei tenant non è configurato nelle VM GW sembra essere il fatto che la capacità GW in FabricConfig. psd1 è inferiore rispetto a quanto Gli utenti provano ad assegnare le connessioni di rete (Tunnel da sito a sito) in TenantConfig. psd1. Questa operazione può essere verificata facilmente confrontando gli output dei comandi seguenti:

```
PS > (Get-NetworkControllerGatewayPool -ConnectionUri $uri).properties.Capacity
PS > (Get-NetworkControllerVirtualgatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "TenantName").properties.OutboundKiloBitsPerSecond
PS > (Get-NetworkControllerVirtualgatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "TenantName").property
```

### <a name="hoster-validate-data-plane"></a>Hosting Convalida dati-piano
Dopo aver distribuito il controller di rete, sono state create le reti virtuali e le subnet tenant e le macchine virtuali sono state collegate alle subnet virtuali, i test del livello di infrastruttura aggiuntivi possono essere eseguiti dal provider di servizi di hosting per verificare la connettività del tenant.

#### <a name="check-hnv-provider-logical-network-connectivity"></a>Controllare la connettività di rete logica del provider HNV
Dopo che la prima VM guest in esecuzione in un host Hyper-V è stata connessa a una rete virtuale tenant, il controller di rete assegnerà due indirizzi IP del provider HNV (indirizzi IP PA) all'host Hyper-V. Questi indirizzi IP provengono dal pool IP della rete logica del provider HNV e vengono gestiti dal controller di rete.  Per scoprire quali sono i due indirizzi IP HNV

```none
PS > Get-ProviderAddress

# Sample Output
ProviderAddress : 10.10.182.66
MAC Address     : 40-1D-D8-B7-1C-04
Subnet Mask     : 255.255.255.128
Default Gateway : 10.10.182.1
VLAN            : VLAN11

ProviderAddress : 10.10.182.67
MAC Address     : 40-1D-D8-B7-1C-05
Subnet Mask     : 255.255.255.128
Default Gateway : 10.10.182.1
VLAN            : VLAN11
```

Questi indirizzi IP del provider HNV sono assegnati a schede Ethernet create in un raggruppamento di rete TCPIP separato e hanno un nome di adapter _VLANX_ dove X è la VLAN assegnata alla rete logica del provider HNV (trasporto).

La connettività tra due host Hyper-V tramite la rete logica del provider HNV può essere eseguita tramite un ping con un parametro di raggruppamento (-c Y) aggiuntivo in cui Y è il raggruppamento di rete TCPIP in cui vengono creati i PAhostVNICs. Questo raggruppamento può essere determinato eseguendo:

```none
C:\> ipconfig /allcompartments /all

<snip> ...
==============================================================================
Network Information for *Compartment 3*
==============================================================================
   Host Name . . . . . . . . . . . . : SA18n30-2
<snip> ...

Ethernet adapter VLAN11:

   Connection-specific DNS Suffix  . :
   Description . . . . . . . . . . . : Microsoft Hyper-V Network Adapter
   Physical Address. . . . . . . . . : 40-1D-D8-B7-1C-04
   DHCP Enabled. . . . . . . . . . . : No
   Autoconfiguration Enabled . . . . : Yes
   Link-local IPv6 Address . . . . . : fe80::5937:a365:d135:2899%39(Preferred)
   IPv4 Address. . . . . . . . . . . : 10.10.182.66(Preferred)
   Subnet Mask . . . . . . . . . . . : 255.255.255.128
   Default Gateway . . . . . . . . . : 10.10.182.1
   NetBIOS over Tcpip. . . . . . . . : Disabled

Ethernet adapter VLAN11:

   Connection-specific DNS Suffix  . :
   Description . . . . . . . . . . . : Microsoft Hyper-V Network Adapter
   Physical Address. . . . . . . . . : 40-1D-D8-B7-1C-05
   DHCP Enabled. . . . . . . . . . . : No
   Autoconfiguration Enabled . . . . : Yes
   Link-local IPv6 Address . . . . . : fe80::28b3:1ab1:d9d9:19ec%44(Preferred)
   IPv4 Address. . . . . . . . . . . : 10.10.182.67(Preferred)
   Subnet Mask . . . . . . . . . . . : 255.255.255.128
   Default Gateway . . . . . . . . . : 10.10.182.1
   NetBIOS over Tcpip. . . . . . . . : Disabled

*Ethernet adapter vEthernet (PAhostVNic):*
<snip> ...
```

>[!NOTE]
> Le schede vNIC host PA non vengono utilizzate nel percorso dati e pertanto non dispongono di un indirizzo IP assegnato all'adapter "vEthernet (PAhostVNic)".

Si supponga, ad esempio, che gli host Hyper-V 1 e 2 dispongano di indirizzi IP del provider HNV (PA):

|-Host Hyper-V-|-Indirizzo IP PA 1|-Indirizzo IP PA 2|
|---             |---            |---             |
|Host 1 | 10.10.182.64 | 10.10.182.65 |
|Host 2 | 10.10.182.66 | 10.10.182.67 |

è possibile eseguire il ping tra i due usando il comando seguente per controllare la connettività di rete logica del provider HNV.

```none
# Ping the first PA IP Address on Hyper-V Host 2 from the first PA IP address on Hyper-V Host 1 in compartment (-c) 3
C:\> ping -c 3 10.10.182.66 -S 10.10.182.64

# Ping the second PA IP Address on Hyper-V Host 2 from the first PA IP address on Hyper-V Host 1 in compartment (-c) 3
C:\> ping -c 3 10.10.182.67 -S 10.10.182.64

# Ping the first PA IP Address on Hyper-V Host 2 from the second PA IP address on Hyper-V Host 1 in compartment (-c) 3
C:\> ping -c 3 10.10.182.66 -S 10.10.182.65

# Ping the second PA IP Address on Hyper-V Host 2 from the second PA IP address on Hyper-V Host 1 in compartment (-c) 3
C:\> ping -c 3 10.10.182.67 -S 10.10.182.65
```

*Correzione* Se il ping del provider di HNV non funziona, controllare la connettività di rete fisica, inclusa la configurazione VLAN. Le schede di rete fisiche in ogni host Hyper-V devono essere in modalità trunk senza alcuna VLAN specifica assegnata. Il vNIC host di gestione deve essere isolato alla VLAN della rete logica di gestione.

```none
PS C:\> Get-NetAdapter "Ethernet 4" |fl

Name                       : Ethernet 4
InterfaceDescription       : <NIC> Ethernet Adapter
InterfaceIndex             : 2
MacAddress                 : F4-52-14-55-BC-21
MediaType                  : 802.3
PhysicalMediaType          : 802.3
InterfaceOperationalStatus : Up
AdminStatus                : Up
LinkSpeed(Gbps)            : 10
MediaConnectionState       : Connected
ConnectorPresent           : True
*VlanID                     : 0*
DriverInformation          : Driver Date 2016-08-28 Version 5.25.12665.0 NDIS 6.60

# VMM uses the older PowerShell cmdlet <Verb>-VMNetworkAdapterVlan to set VLAN isolation
PS C:\> Get-VMNetworkAdapterVlan -ManagementOS -VMNetworkAdapterName <Mgmt>

VMName VMNetworkAdapterName Mode     VlanList
------ -------------------- ----     --------
<snip> ...        
       Mgmt                 Access   7
<snip> ...

# SDNExpress deployments use the newer PowerShell cmdlet <Verb>-VMNetworkAdapterIsolation to set VLAN isolation
PS C:\> Get-VMNetworkAdapterIsolation -ManagementOS

<snip> ...

IsolationMode        : Vlan
AllowUntaggedTraffic : False
DefaultIsolationID   : 7
MultiTenantStack     : Off
ParentAdapter        : VMInternalNetworkAdapter, Name = 'Mgmt'
IsTemplate           : True
CimSession           : CimSession: .
ComputerName         : SA18N30-2
IsDeleted            : False

<snip> ...
```

#### <a name="check-mtu-and-jumbo-frame-support-on-hnv-provider-logical-network"></a>Verificare il supporto di MTU e Jumbo frame nella rete logica del provider HNV

Un altro problema comune nella rete logica del provider HNV è che le porte di rete fisica e/o la scheda Ethernet non dispongono di un valore MTU sufficientemente grande configurato per gestire l'overhead dall'incapsulamento VXLAN (o NVGRE). 
>[!NOTE]
> Alcune schede e driver Ethernet supportano la nuova parola chiave * EncapOverhead, che verrà automaticamente impostata dall'agente host del controller di rete su un valore di 160. Questo valore verrà quindi aggiunto al valore della parola chiave * JumboPacket la cui sommatoria viene usata come MTU annunciato.
> ad esempio * EncapOverhead = 160 e * JumboPacket = 1514 = > MTU = 1674B

```none
# Check whether or not your Ethernet card and driver support *EncapOverhead
PS C:\ > Test-EncapOverheadSettings

Verifying Physical Nic : <NIC> Ethernet Adapter #2
Physical Nic  <NIC> Ethernet Adapter #2 can support SDN traffic. Encapoverhead value set on the nic is  160
Verifying Physical Nic : <NIC> Ethernet Adapter
Physical Nic  <NIC> Ethernet Adapter can support SDN traffic. Encapoverhead value set on the nic is  160
```

Per verificare se la rete logica del provider HNV supporta le dimensioni MTU più grandi end-to-end, usare il cmdlet _test-LogicalNetworkSupportsJumboPacket_ :
```none
# Get credentials for both source host and destination host (or use the same credential if in the same domain)
$sourcehostcred = Get-Credential
$desthostcred = Get-Credential

# Use the Management IP Address or FQDN of the Source and Destination Hyper-V hosts
Test-LogicalNetworkSupportsJumboPacket -SourceHost sa18n30-2 -DestinationHost sa18n30-3 -SourceHostCreds $sourcehostcred -DestinationHostCreds $desthostcred

# Failure Results
SourceCompartment : 3
pinging Source PA: 10.10.182.66 to Destination PA: 10.10.182.64 with Payload: 1632
pinging Source PA: 10.10.182.66 to Destination PA: 10.10.182.64 with Payload: 1472
Checking if physical nics support jumbo packets on host
Physical Nic  <NIC> Ethernet Adapter #2 can support SDN traffic. Encapoverhead value set on the nic is  160
Cannot send jumbo packets to the destination. Physical switch ports may not be configured to support jumbo packets.
Checking if physical nics support jumbo packets on host
Physical Nic  <NIC> Ethernet Adapter #2 can support SDN traffic. Encapoverhead value set on the nic is  160
Cannot send jumbo packets to the destination. Physical switch ports may not be configured to support jumbo packets.


```

*Correzione*
* Regolare le dimensioni MTU sulle porte del Commuter fisico in modo che siano almeno 1674B (incluse l'intestazione e il trailer di 14B Ethernet)
* Se la scheda NIC non supporta la parola chiave EncapOverhead, modificare la parola chiave JumboPacket in modo da essere almeno 1674B


#### <a name="check-tenant-vm-nic-connectivity"></a>Controllare la connettività NIC della VM tenant
Ogni scheda di interfaccia di rete della macchina virtuale assegnata a una macchina virtuale Guest dispone di un mapping CA-PA tra l'indirizzo del cliente privato (CA) e lo spazio dell'indirizzo del provider di HNV (PA). Questi mapping vengono mantenuti nelle tabelle del server OVSDB in ogni host Hyper-V ed è possibile trovarli eseguendo il cmdlet seguente.

```none
# Get all known PA-CA Mappings from this particular Hyper-V Host
PS > Get-PACAMapping

CA IP Address CA MAC Address    Virtual Subnet ID PA IP Address
------------- --------------    ----------------- -------------
10.254.254.2  00-1D-D8-B7-1C-43              4115 10.10.182.67
10.254.254.3  00-1D-D8-B7-1C-43              4115 10.10.182.67
192.168.1.5   00-1D-D8-B7-1C-07              4114 10.10.182.65
10.254.254.1  40-1D-D8-B7-1C-06              4115 10.10.182.66
192.168.1.1   40-1D-D8-B7-1C-06              4114 10.10.182.66
192.168.1.4   00-1D-D8-B7-1C-05              4114 10.10.182.66
```
>[!NOTE]
> Se i mapping della CA-PA previsti non sono restituiti per una determinata macchina virtuale del tenant, controllare le risorse di configurazione IP e NIC della macchina virtuale nel controller di rete usando il cmdlet _Get-NetworkControllerNetworkInterface_ . Controllare inoltre le connessioni stabilite tra l'agente host NC e i nodi del controller di rete.

Con queste informazioni, è ora possibile avviare un ping della macchina virtuale tenant dal controller di rete usando il cmdlet _test-VirtualNetworkConnection_ .

## <a name="specific-troubleshooting-scenarios"></a>Scenari specifici per la risoluzione dei problemi

Nelle sezioni seguenti vengono fornite indicazioni per la risoluzione di scenari specifici.

### <a name="no-network-connectivity-between-two-tenant-virtual-machines"></a>Nessuna connettività di rete tra due macchine virtuali tenant

1.  Inquilino Assicurarsi che Windows Firewall nelle macchine virtuali tenant non blocchi il traffico.  
2.  Inquilino Verificare che gli indirizzi IP siano stati assegnati alla macchina virtuale tenant eseguendo _ipconfig_. 
3.  Hosting Eseguire **test-VirtualNetworkConnection** dall'host Hyper-V per convalidare la connettività tra le due macchine virtuali tenant in questione. 

>[!NOTE]
>VSID fa riferimento all'ID della subnet virtuale. Nel caso di VXLAN, si tratta dell'identificatore di rete VXLAN (VNI). È possibile trovare questo valore eseguendo il cmdlet **Get-PACAMapping** .

#### <a name="example"></a>Esempio

    $password = ConvertTo-SecureString -String "password" -AsPlainText -Force
    $cred = New-Object pscredential -ArgumentList (".\administrator", $password) 

Creare una CA-ping tra "Green Web VM 1" con SenderCA IP di 192.168.1.4 nell'host "sa18n30-2.sa18.nttest.microsoft.com" con l'IP Mgmt di 10.127.132.153 a ListenerCA IP di 192.168.1.5 entrambi collegati alla subnet virtuale (VSID) 4114.

    Test-VirtualNetworkConnection -OperationId 27 -HostName sa18n30-2.sa18.nttest.microsoft.com -MgmtIp 10.127.132.153 -Creds $cred -VMName "Green Web VM 1" -VMNetworkAdapterName "Green Web VM 1" -SenderCAIP 192.168.1.4 -SenderVSID 4114 -ListenerCAIP 192.168.1.5 -ListenerVSID 4114

    Test-VirtualNetworkConnection at command pipeline position 1

Avvio del test di ping dello spazio CA avvio della sessione di traccia ping per 192.168.1.5 riuscito dall'indirizzo 192.168.1.4 RTT = 0 ms


Informazioni sul routing della CA:

    Local IP: 192.168.1.4
    Local VSID: 4114
    Remote IP: 192.168.1.5
    Remote VSID: 4114
    Distributed Router Local IP: 192.168.1.1
    Distributed Router Local MAC: 40-1D-D8-B7-1C-06
    Local CA MAC: 00-1D-D8-B7-1C-05
    Remote CA MAC: 00-1D-D8-B7-1C-07
    Next Hop CA MAC Address: 00-1D-D8-B7-1C-07

Informazioni sul routing PA:

    Local PA IP: 10.10.182.66
    Remote PA IP: 10.10.182.65

 <snip>...

4. Inquilino Verificare che non siano stati specificati criteri firewall distribuiti nella subnet virtuale o nelle interfacce di rete VM che bloccano il traffico.    

Eseguire una query sull'API REST del controller di rete disponibile nell'ambiente demo in sa18n30nc nel dominio sa18.nttest.microsoft.com.

    $uri = "https://sa18n30nc.sa18.nttest.microsoft.com"
    Get-NetworkControllerAccessControlList -ConnectionUri $uri 

## <a name="look-at-ip-configuration-and-virtual-subnets-which-are-referencing-this-acl"></a>Esaminare la configurazione IP e le subnet virtuali che fanno riferimento a questo ACL

1. Hosting Eseguire ``Get-ProviderAddress`` in entrambi gli host Hyper-V che ospitano le due macchine virtuali tenant in questione, quindi eseguire ``Test-LogicalNetworkConnection`` o ``ping -c <compartment>`` dall'host Hyper-V per convalidare la connettività sulla rete logica del provider HNV
2.  Hosting Verificare che le impostazioni MTU siano corrette negli host Hyper-V e in qualsiasi dispositivo di cambio di livello 2 tra gli host Hyper-V. Eseguire ``Test-EncapOverheadValue`` in tutti gli host Hyper-V in questione. Verificare inoltre che tutti gli switch di livello 2 tra siano impostati su un valore MTU pari a almeno 1674 byte per tenere conto del sovraccarico massimo di 160 byte.  
3.  Hosting Se gli indirizzi IP PA non sono presenti e/o la connettività CA è interruppe, verificare che il criterio di rete sia stato ricevuto. Eseguire ``Get-PACAMapping`` per verificare se le regole di incapsulamento e i mapping CA-PA necessari per la creazione di reti virtuali sovrapposte sono stabiliti correttamente.  
4.  Hosting Verificare che l'agente host del controller di rete sia connesso al controller di rete. Eseguire ``netstat -anp tcp |findstr 6640`` per verificare se il   
5.  Hosting Verificare che l'ID host in HKLM/corrisponda all'ID istanza delle risorse server che ospitano le macchine virtuali tenant.  
6. Hosting Verificare che l'ID del profilo di porta corrisponda all'ID istanza delle interfacce di rete VM delle macchine virtuali tenant.  

## <a name="logging-tracing-and-advanced-diagnostics"></a>Registrazione, traccia e diagnostica avanzata

Nelle sezioni seguenti vengono fornite informazioni su diagnostica avanzata, registrazione e traccia.

### <a name="network-controller-centralized-logging"></a>Registrazione centralizzata del controller di rete 

Il controller di rete può raccogliere automaticamente i log del debugger e archiviarli in una posizione centralizzata. La raccolta di log può essere abilitata quando si distribuisce il controller di rete per la prima volta o in un secondo momento. I log vengono raccolti dal controller di rete e dagli elementi di rete gestiti dal controller di rete: computer host, servizio di bilanciamento del carico software (SLB) e computer gateway. 

Questi log includono i log di debug per il cluster di controller di rete, l'applicazione del controller di rete, i log del gateway, SLB, la rete virtuale e il firewall distribuito. Ogni volta che un nuovo host/SLB/gateway viene aggiunto al controller di rete, la registrazione viene avviata su tali computer. Analogamente, quando un host/SLB/gateway viene rimosso dal controller di rete, la registrazione viene arrestata in tali computer.

#### <a name="enable-logging"></a>Enable logging (Abilita accesso)

La registrazione viene abilitata automaticamente quando si installa il cluster di controller di rete usando il cmdlet **Install-NetworkControllerCluster** . Per impostazione predefinita, i log vengono raccolti localmente nei nodi del controller di rete in *%SystemDrive%\SDNDiagnostics*. Si **consiglia vivamente** di modificare il percorso in modo che sia una condivisione file remota (non locale). 

I log del cluster del controller di rete vengono archiviati in *%ProgramData%\Windows Fabric\log\Traces*. È possibile specificare una posizione centralizzata per la raccolta di log con il parametro **DiagnosticLogLocation** con la raccomandazione che questa sia anche una condivisione file remota. 

Se si vuole limitare l'accesso a questo percorso, è possibile specificare le credenziali di accesso con il parametro **LogLocationCredential** . Se si specificano le credenziali per accedere al percorso del log, è necessario specificare anche il parametro **CredentialEncryptionCertificate** , che consente di crittografare le credenziali archiviate localmente nei nodi del controller di rete.  

Con le impostazioni predefinite, è consigliabile disporre di almeno 75 GB di spazio disponibile nella posizione centrale e 25 GB nei nodi locali (se non si usa una posizione centrale) per un cluster di controller di rete a 3 nodi.

#### <a name="change-logging-settings"></a>Modificare le impostazioni di registrazione

È possibile modificare le impostazioni di registrazione in qualsiasi momento usando il cmdlet ``Set-NetworkControllerDiagnostic``. È possibile modificare le impostazioni seguenti:

- **Posizione del log centralizzata**.  È possibile modificare il percorso per archiviare tutti i log, con il parametro ``DiagnosticLogLocation``.  
- **Credenziali per accedere al percorso del log**.  È possibile modificare le credenziali per accedere al percorso del log, con il parametro ``LogLocationCredential``.  
- **Passa alla registrazione locale**.  Se è stata specificata la posizione centralizzata per archiviare i log, è possibile tornare alla registrazione localmente nei nodi del controller di rete con il parametro ``UseLocalLogLocation`` (scelta non consigliata a causa di requisiti di spazio su disco elevati).  
- **Ambito di registrazione**.  Per impostazione predefinita, vengono raccolti tutti i log. È possibile modificare l'ambito per raccogliere solo i registri del cluster del controller di rete.  
- **Livello di registrazione**.  Il livello di registrazione predefinito è informativo. È possibile modificarlo in errore, avviso o dettagliato.  
- **Tempo di invecchiamento del log**.  I log vengono archiviati in modo circolare. Per impostazione predefinita, si disporrà di 3 giorni di registrazione dei dati, sia che si usi la registrazione locale o la registrazione centralizzata. È possibile modificare questo limite di tempo con il parametro **LogTimeLimitInDays** .  
- **Dimensioni di invecchiamento del log**.  Per impostazione predefinita, si disporrà di un massimo di 75 GB di dati di registrazione se si utilizza la registrazione centralizzata e 25 GB se si utilizza la registrazione locale. È possibile modificare questo limite con il parametro **LogSizeLimitInMBs** .

#### <a name="collecting-logs-and-traces"></a>Raccolta di log e tracce

Per impostazione predefinita, le distribuzioni VMM utilizzano la registrazione centralizzata per il controller di rete. Il percorso di condivisione file per questi log viene specificato quando si distribuisce il modello di servizio di controller di rete.

Se non è stato specificato un percorso di file, verrà usata la registrazione locale in ogni nodo del controller di rete con i log salvati in C:\Windows\tracing\SDNDiagnostics. Questi log vengono salvati usando la gerarchia seguente:

- CrashDumps
- NCApplicationCrashDumps
- NCApplicationLogs
- PerfCounters
- SDNDiagnostics
- Traces

Il controller di rete utilizza (Azure) Service Fabric. Quando si risolvono determinati problemi, potrebbe essere necessario Service Fabric log. Questi log sono disponibili in ogni nodo del controller di rete in C:\ProgramData\Microsoft\Service Fabric.

Se un utente ha eseguito il cmdlet _debug-NetworkController_ , saranno disponibili log aggiuntivi in ogni host Hyper-V specificato con una risorsa server nel controller di rete. Questi log (e TRACES se abilitati) vengono conservati in C:\NCDiagnostics

### <a name="slb-diagnostics"></a>Diagnostica SLB

#### <a name="slbm-fabric-errors-hosting-service-provider-actions"></a>Errori dell'infrastruttura SLBM (azioni del provider di servizi di hosting)

1.  Verificare che il software Load Balancer Manager (SLBM) sia funzionante e che i livelli di orchestrazione siano in grado di comunicare tra loro: SLBM-> SLB mux e SLBM-> SLB agenti host. Eseguire [DumpSlbRestState](https://github.com/Microsoft/SDN/blob/master/Diagnostics/DumpSlbRestState.ps1) da qualsiasi nodo con accesso all'endpoint REST del controller di rete.  
2.  Convalidare il *SDNSLBMPerfCounters* in PerfMon in una delle VM del nodo del controller di rete (preferibilmente il nodo del controller di rete primario-Get-NetworkControllerReplica):
    1.  Il motore è Load Balancer (LB) connesso a SLBM? (*SLBM LBEngine configurazioni totali* > 0)  
    2.  SLBM conosce almeno i propri endpoint? (*Endpoint VIP totali* > = 2)  
    3.  Gli host Hyper-V (DIP) sono connessi a SLBM? (*Client HP connessi* = = num Server)   
    4.  SLBM è connesso a Mux? (*Mux connesso* == *Mux integro in SLBM* == *Mux Reporting integro* = # SLB Mux VM).  
3.  Verificare che il router BGP configurato sia in grado di eseguire correttamente il peering con SLB MUX  
    1.  Se si usa RRAS con accesso remoto (ad esempio, la macchina virtuale BGP):  
        1.  Get-BgpPeer dovrebbe visualizzare Connected  
        2.  Get-BgpRouteInformation dovrebbe visualizzare almeno una route per il VIP self SLBM  
    2.  Se si usa il commutatore Top-of-rack (ToR) fisico come peer BGP, consultare la documentazione  
        1.  Ad esempio: # Mostra istanza BGP  
4.  Convalidare i contatori *SlbMuxPerfCounters* e *SLBMUX* in Perfmon nella VM SLB Mux
5.  Controllare lo stato della configurazione e gli intervalli VIP nella risorsa software Load Balancer Manager  
    1.  Get-NetworkControllerLoadBalancerConfiguration-ConnectionUri < https://<FQDN or IP>| ConvertTo-JSON-Depth 8 (controllare gli intervalli VIP nei pool IP e verificare che SLBM self-VIP (*LoadBalanacerManagerIPAddress*) e tutti gli indirizzi VIP per i tenant siano compresi in questi intervalli)  
        1. Get-NetworkControllerIpPool-NetworkId "< ID di risorsa di rete logica VIP per indirizzi IP pubblici/privati >"-SubnetId "< ID di risorsa della subnet logica VIP per i VIP pubblici/privati >"-ResourceId "<IP Pool Resource Id>"-ConnectionUri $uri | ConvertTo-JSON-Depth 8 
    2.  Debug-NetworkControllerConfigurationState-  

Se uno dei controlli precedenti ha esito negativo, anche lo stato del tenant SLB sarà in modalità di errore.  

  di **monitoraggio e aggiornamento**  
In base alle seguenti informazioni di diagnostica presentate, correggere quanto segue:  
* Verificare che i multiplexer SLB siano connessi  
  * Risolvere i problemi relativi ai certificati  
  * Correggere i problemi di connettività di rete  
* Verificare che le informazioni sul peering BGP siano configurate correttamente  
* Verificare che l'ID host nel registro di sistema corrisponda all'ID istanza del server nella risorsa server (appendice di riferimento per il codice di errore *HostNotConnected* )  
* Raccogli log  

#### <a name="slbm-tenant-errors-hosting-service-provider--and-tenant-actions"></a>Errori del tenant SLBM (azioni del provider di servizi di hosting e del tenant)

1.  Hosting Controllare *debug-NetworkControllerConfigurationState* per verificare se le risorse di LoadBalancer sono in stato di errore. Provare a attenuare seguendo la tabella elementi di azione nell'appendice.   
    1.  Verificare che sia presente un endpoint VIP e che gli itinerari pubblicitari  
    2.  Verificare il numero di endpoint DIP individuati per l'endpoint VIP  
2.  Inquilino Convalida Load Balancer le risorse sono specificate correttamente  
    1.  Convalida gli endpoint DIP registrati in SLBM ospitano macchine virtuali tenant che corrispondono alle configurazioni IP del pool di indirizzi back-end LoadBalancer  
3.  Hosting Se gli endpoint DIP non vengono individuati o connessi:   
    1.  Controllare *debug-NetworkControllerConfigurationState*  
        1.  Verificare che l'agente host NC e SLB sia connesso correttamente al coordinatore di eventi del controller di rete utilizzando ``netstat -anp tcp |findstr 6640)``  
    2.  Controllare *HostID* nel servizio *nchostagent* chiave (fare riferimento al codice di errore *HostNotConnected* nell'appendice) corrisponde all'ID istanza della risorsa server corrispondente (``Get-NCServer |convertto-json -depth 8``)  
    3.  Verificare che l'ID del profilo di porta per la porta della macchina virtuale corrisponda all'ID istanza corrispondente della risorsa NIC della macchina virtuale   
4.  [Provider di hosting] Raccogli log   

#### <a name="slb-mux-tracing"></a>Traccia SLB Mux

Le informazioni del software Load Balancer MUX possono essere determinate anche tramite Visualizzatore eventi. 
1. Fare clic su "Mostra log analitici e di debug" nel menu visualizzazione Visualizzatore eventi
2. Passare a "registri applicazioni e servizi" > Microsoft > Windows > SlbMuxDriver > Trace in Visualizzatore eventi 
3. Fare clic con il pulsante destro del mouse e scegliere "Abilita log".

>[!NOTE]
>Si consiglia di abilitare questa registrazione solo per un breve periodo di tempo durante il tentativo di riprodurre un problema

### <a name="vfp-and-vswitch-tracing"></a>Traccia di VFP e vSwitch

Da qualsiasi host Hyper-V che ospita una VM guest collegata a una rete virtuale tenant, è possibile raccogliere una traccia VFP per determinare dove potrebbero verificarsi problemi.

```
netsh trace start provider=Microsoft-Windows-Hyper-V-VfpExt overwrite=yes tracefile=vfp.etl report=disable provider=Microsoft-Windows-Hyper-V-VmSwitch 
netsh trace stop
netsh trace convert .\vfp.etl ov=yes 
```
