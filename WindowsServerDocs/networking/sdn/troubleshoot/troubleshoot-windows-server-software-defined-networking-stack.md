---
title: Risolvere i problemi di Windows Server Software allo Stack Defined Networking
description: Questa Guida di Windows Server esamina i più comuni errori di rete SDN (Software Defined) e gli scenari di errore e illustra un flusso di lavoro sulla risoluzione dei problemi che sfrutta gli strumenti di diagnostica disponibili.
manager: ravirao
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 9be83ed2-9e62-49e8-88e7-f52d3449aac5
ms.author: pashort
author: JMesser81
ms.openlocfilehash: af59ae6746467f9aecf384d1b3cf9af1e8baeb9a
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="troubleshoot-the-windows-server-software-defined-networking-stack"></a>Risolvere i problemi di Windows Server Software allo Stack Defined Networking

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Questa guida esamina i più comuni errori di rete SDN (Software Defined) e gli scenari di errore e descrive un flusso di lavoro sulla risoluzione dei problemi che sfrutta gli strumenti di diagnostica disponibili.  

Per ulteriori informazioni su Microsoft Software Defined Networking, vedere [rete definita dal Software](../../sdn/Software-Defined-Networking--SDN-.md).  

## <a name="error-types"></a>Tipi di errore  
Nell'elenco seguente rappresenta la classe del problemi più spesso visualizzati con virtualizzazione rete Hyper-V (HNVv1) in Windows Server 2012 R2 dalle distribuzioni di produzione di mercato e coincide in molti modi con gli stessi tipi di problemi in Windows Server 2016 HNVv2 con il nuovo Stack di rete SDN (Software Defined).  

La maggior parte degli errori possono essere classificati in un piccolo set di classi:   
* **Configurazione non valida o non supportati**  
   Un utente richiama l'API NorthBound, in modo errato o con criteri non validi.   

* **Errore durante l'applicazione dei criteri**  
     Criteri dal Controller di rete non è stato recapitato a un Host Hyper-V, in modo significativo ritardata e / o non aggiornata in tutti gli host Hyper-V (ad esempio, dopo la migrazione in tempo reale).  
* **Bug di software o la deviazione della configurazione**  
 Problemi di percorso dei dati risultanti in pacchetti ignorati.  

* **Errore esterno relativi all'hardware NIC al driver o Metti di sotto dell'infrastruttura di rete**  
 Malfunzionante offload delle attività (ad esempio VMQ) o l'infrastruttura di rete Metti sotto non configurato correttamente (ad esempio MTU)   

 Questa Guida alla risoluzione dei problemi esamina ognuna di queste categorie di errore e consiglia le procedure consigliate e strumenti di diagnostica disponibili per identificare e correggere l'errore.  

## <a name="diagnostic-tools"></a>Strumenti di diagnostica  

Prima di esaminare i flussi di lavoro sulla risoluzione dei problemi per ognuno di questi tipo di errori, esaminare gli strumenti di diagnostica disponibili.   
  
Per utilizzare gli strumenti di diagnostica del Controller di rete (controllo-path), è necessario installare la funzionalità di amministrazione remota del server NetworkController e importare il ``NetworkControllerDiagnostics`` modulo:  

```  
Add-WindowsFeature RSAT-NetworkController -IncludeManagementTools  
Import-Module NetworkControllerDiagnostics  
```  

Per utilizzare gli strumenti di diagnostica di diagnostica di virtualizzazione rete (data-path), è necessario importare il ``HNVDiagnostics`` modulo:
  
```  
# Assumes RSAT-NetworkController feature has already been installed
Import-Module hnvdiagnostics   
```  

### <a name="network-controller-diagnostics"></a>Diagnostica del controller di rete  
Questi cmdlet sono documentati in TechNet nel [argomento Cmdlet di diagnostica di Controller di rete](https://docs.microsoft.com/en-us/powershell/module/networkcontrollerdiagnostics/). Consentono di identificare i problemi di coerenza dei criteri di rete nel percorso di controllo tra i nodi di Controller di rete e tra il Controller di rete e gli agenti di contesto dei nomi Host in esecuzione negli host Hyper-V.

 Il _Debug ServiceFabricNodeStatus_ e _Get-NetworkControllerReplica_ cmdlet devono essere eseguiti da una delle macchine virtuali nodo Controller di rete. Tutti gli altri cmdlet diagnostica contesto dei nomi possono essere eseguiti da qualsiasi host che disponga della connettività al Controller di rete e si trova in un gruppo di protezione gestione dei Controller di rete (Kerberos) o ha accesso al certificato x. 509 per la gestione dei Controller di rete. 
   
### <a name="hyper-v-host-diagnostics"></a>Diagnostica di host Hyper-V  
Questi cmdlet sono documentati in TechNet nel [argomento Cmdlet di Hyper-V rete virtualizzazione diagnostica](https://docs.microsoft.com/en-us/powershell/module/hnvdiagnostics/). Consentono di identificare i problemi nel percorso dei dati tra le macchine virtuali tenant (EST/ovest) e il traffico in ingresso tramite un indirizzo VIP SLB (nord/sud). 

Il _Debug VirtualMachineQueueOperation_, _Get-CustomerRoute_, _Get-PACAMapping_, _Get-ProviderAddress_, _Get-VMNetworkAdapterPortId_, _Get-VMSwitchExternalPortId_, e _Test EncapOverheadSettings_ sono tutti i test locali che possono essere eseguiti da qualsiasi host Hyper-V. Gli altri cmdlet test-path dati tramite il Controller di rete di richiamare e pertanto è necessario accedere al Controller di rete come descried sopra.
 
### <a name="github"></a>GitHub
Il [repository GitHub Microsoft/SDN](https://github.com/microsoft/sdn) ha un numero di script e flussi di lavoro che si basano su questi cmdlet nella casella. In particolare, script, vedere il [diagnostica](https://github.com/Microsoft/sdn/diagnostics) cartella. Aiutaci a contribuire a questi script tramite l'invio di richieste Pull.

## <a name="troubleshooting-workflows-and-guides"></a>I flussi di lavoro e le guide alla risoluzione dei problemi  

### <a name="hoster-validate-system-health"></a>[Provider] Convalidare l'integrità di sistema
Esiste una risorsa incorporata denominata _lo stato di configurazione_ in diverse delle risorse del Controller di rete. Lo stato di configurazione fornisce informazioni sull'integrità di sistema, tra cui la coerenza tra la configurazione del controller di rete e lo stato effettivo (esecuzione) negli host Hyper-V. 

Per controllare lo stato di configurazione, eseguire il comando seguente da qualsiasi host Hyper-V con connettività al Controller di rete.

>[!NOTE] 
>Il valore per il *NetworkController* parametro deve essere il nome di dominio completo o indirizzo IP in base al nome soggetto di x. 509 > certificato creato per i Controller di rete.
>
>Il *Credential* solo parametro deve essere specificato se il controller di rete utilizza l'autenticazione Kerberos (tipico nelle distribuzioni di VMM). Le credenziali devono essere per un utente nel gruppo di sicurezza di gestione di Controller di rete.

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

Un messaggio di stato di configurazione di esempio è illustrato di seguito:

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
> Esiste un bug nel sistema in cui le risorse di interfaccia di rete per la scheda NIC VM di transito Mux SLB sono in uno stato di errore con l'errore "Virtual Switch – non connesso a Controller Host". Questo errore può essere tranquillamente ignorato se la configurazione IP della risorsa VM NIC è impostata su un indirizzo IP dal Pool IP della rete logica transito. Esiste un bug secondo nel sistema in cui le risorse di interfaccia di rete per le schede NIC VM di Gateway di virtualizzazione rete Provider sono in uno stato di errore con l'errore "Virtual Switch – PortBlocked". Questo errore può essere tranquillamente ignorato anche se la configurazione IP della risorsa VM NIC è impostata su null (per impostazione predefinita).


La tabella seguente mostra l'elenco dei codici di errore, messaggi e azioni di completamento per eseguire in base allo stato di configurazione rilevato.

  
| **Codice**| **Messaggio**| **Azione**|  
|:--------:|:-----------:|----------:|  
| Sconosciuto| Errore sconosciuto| |  
| HostUnreachable                       | Il computer host non è raggiungibile | Controllare la connettività di rete di gestione tra Controller di rete e Host |  
| PAIpAddressExhausted                  | Gli indirizzi Ip PA esaurito | Aumentare la dimensione di Pool IP della subnet logica Provider di virtualizzazione rete |  
| PAMacAddressExhausted                 | Gli indirizzi Mac PA esaurito | Aumentare l'intervallo di Pool Mac |  
| PAAddressConfigurationFailure         | Non è riuscito a eseguire il plumbing gli indirizzi PA all'host | Controllare la connettività di rete di gestione tra Controller di rete e Host. |  
| CertificateNotTrusted                 | Certificato non è attendibile  |Risolvere i certificati usati per la comunicazione con l'host. |  
| CertificateNotAuthorized              | Certificato non autorizzato | Risolvere i certificati usati per la comunicazione con l'host. |  
| PolicyConfigurationFailureOnVfp       | Errore di configurazione dei criteri VFP | Si tratta di un errore di runtime.  Soluzioni non lavoro definita. Raccolta di log. |  
| PolicyConfigurationFailure            | Errore durante il push di criteri per gli host, a causa di errori di comunicazione o altri errori nel NetworkController.| Nessuna azione definita.  Ciò è dovuto a un errore in stato di obiettivo l'elaborazione nei moduli di Controller di rete. Raccolta di log. |  
| HostNotConnectedToController          | L'Host non è ancora connesso al Controller di rete | Profilo di porta non è applicata l'host o l'host non è raggiungibile dal Controller di rete. Convalidare che chiave del Registro di sistema HostID corrisponde all'ID istanza della risorsa server |  
| MultipleVfpEnabledSwitches            | Sono presenti più VFp abilitato commutatori nell'host  | Eliminare una delle opzioni, dato che l'agente Host Controller di rete supporta solo un vSwitch con estensione VFP abilitato |  
| PolicyConfigurationFailure            | Non è riuscito a effettuare il push i criteri di rete virtuale per una scheda a causa di errori del certificato o errori di connettività  | Controllare se sono stati distribuiti i certificati corretti (nome soggetto del certificato deve corrispondere FQDN dell'host). Verificare inoltre la connettività host con il Controller di rete |  
| PolicyConfigurationFailure            | Non è riuscito a effettuare il push vSwitch criteri per un'interfaccia di rete VM a causa di errori del certificato o errori di connettività  | Controllare se sono stati distribuiti i certificati corretti (nome soggetto del certificato deve corrispondere FQDN dell'host). Verificare inoltre la connettività host con il Controller di rete|
| PolicyConfigurationFailure            | Non è riuscito a effettuare il push criteri del Firewall per una scheda a causa di errori del certificato o errori di connettività | Controllare se sono stati distribuiti i certificati corretti (nome soggetto del certificato deve corrispondere FQDN dell'host). Verificare inoltre la connettività host con il Controller di rete|
| DistributedRouterConfigurationFailure | Impossibile configurare le impostazioni di router distribuito in vNic host                          | Errore dello stack TCP/IP. Potrebbe richiedere la pulitura dei scheda Vnic PA e il ripristino di emergenza Host nel server in cui è stato segnalato l'errore |
| DhcpAddressAllocationFailure          | Assegnazione di indirizzi DHCP non è riuscita per un'interfaccia di rete VM                                                    | Controllare se l'attributo di indirizzo IP statico è configurato per la risorsa NIC |  
| CertificateNotTrusted<br>CertificateNotAuthorized | Non è riuscito a connettersi a Mux a causa di errori di rete o del certificato | Controllare il codice numerico fornito nel codice di messaggio di errore: corrisponde al codice di errore winsock. Errori del certificato sono granulari (ad esempio, non possono essere verificati cert, cert non autorizzato, ecc.) |  
| HostUnreachable                       | MUX è danneggiato (caso più comune è disconnesso BGPRouter) | I peer BGP di RRAS (macchina virtuale BGP) o un commutatore Top-of-Rack (ToR) è raggiungibile o non peering correttamente. Controllare le impostazioni di BGP nel Software carico bilanciamento del carico Multiplexer risorse sia peer BGP (macchina virtuale ToR o RRAS) |  
| HostNotConnectedToController          | Agente host SLB non è connesso  | Verificare che sia in esecuzione il servizio agente Host SLB; Per motivi di fare riferimento a registri dell'agente di host SLB (automatico in esecuzione) perché, nel caso in cui SLBM (NC) ha rifiutato il certificato presentato dall'agente host che esegue lo stato verrà visualizzate informazioni particolarmente  |  
| PortBlocked                           | La porta di VFP è bloccata, a causa della mancanza di VNET / criteri ACL | Controllare se sono presenti altri errori, che potrebbero non essere configurato i criteri. |  
| L'overload                            | È in overload MUX Loadbalancer  | Problema di prestazioni con MUX |  
| RoutePublicationFailure               | MUX Loadbalancer non è connesso al router BGP | Controllare se MUX disponga della connettività con il router BGP e che il peering BGP sia configurato correttamente |  
| VirtualServerUnreachable              | MUX Loadbalancer non è connesso alla console di gestione SLB | Verificare la connettività tra SLBM e MUX |  
| QosConfigurationFailure               | Non è riuscito a configurare i criteri QOS | Se è disponibile per tutte le VM se viene utilizzata prenotazione QOS larghezza di banda sufficiente |


#### <a name="check-network-connectivity-between-the-network-controller-and-hyper-v-host-nc-host-agent-service"></a>Verificare la connettività di rete tra il controller di rete e l'Host Hyper-V (servizio agente di contesto dei nomi Host)
Eseguire il *netstat* comando seguente per convalidare che esistono tre stabilite connessioni tra l'agente Host contesto dei nomi e i nodi di Controller di rete e un socket di ascolto nell'Host Hyper-V
- In ascolto sulla porta TCP:6640 in Host Hyper-V (servizio agente di contesto dei nomi Host)
- Due connessioni stabilite da Hyper-V host IP sulla porta 6640 su IP nodo di contesto dei nomi in porte temporanee (> 32000)
- Una connessione stabilita da Hyper-V host IP sulla porta temporanea IP RESTANTI Controller di rete sulla porta 6640

>[!NOTE]
>Potrebbe essere presente solo due connessioni stabilite in un host Hyper-V se non siano presenti macchine virtuali tenant distribuite in tale host specifico.

```none
netstat -anp tcp |findstr 6640

# Successful output
  TCP    0.0.0.0:6640           0.0.0.0:0              LISTENING
  TCP    10.127.132.153:6640    10.127.132.213:50095   ESTABLISHED
  TCP    10.127.132.153:6640    10.127.132.214:62514   ESTABLISHED
  TCP    10.127.132.153:50023   10.127.132.211:6640    ESTABLISHED
```
#### <a name="check-host-agent-services"></a>Verifica servizi agente Host
Il controller di rete comunica con i due servizi agente host negli host Hyper-V: SLB Host agente e contesto dei nomi Host. È possibile che uno o entrambi questi servizi non è in esecuzione. Controllare lo stato e riavviare se non è in esecuzione.

```none
Get-Service SlbHostAgent
Get-Service NcHostAgent

# (Re)start requires -Force flag
Start-Service NcHostAgent -Force
Start-Service SlbHostAgent -Force
```

#### <a name="check-health-of-network-controller"></a>Controllare l'integrità del controller di rete
Se esistono non tre stabilite connessioni o se il Controller di rete sembrano non rispondere, verificare che tutti i nodi e i moduli del servizio sono in esecuzione utilizzando i cmdlet seguenti. 

```none
# Prints a DIFF state (status is automatically updated if state is changed) of a particular service module replica 
Debug-ServiceFabricNodeStatus [-ServiceTypeName] <Service Module>
```
I moduli del servizio controller di rete sono:
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

Verificare che sia ReplicaStatus **pronto** e HealthState **Ok**.

In un ambiente di produzione è la distribuzione con un Controller di rete a più nodi, è inoltre possibile controllare il nodo è primario su ogni servizio e il relativo stato di replica singoli.

```none  
Get-NetworkControllerReplica

# Sample Output for the API service module
Replicas for service: ApiService

ReplicaRole   : Primary
NodeName      : SA18N30NC3.sa18.nttest.microsoft.com
ReplicaStatus : Ready

```
Verificare che lo stato di Replica sia pronto per ogni servizio.
 
#### <a name="check-for-corresponding-hostids-and-certificates-between-network-controller-and-each-hyper-v-host"></a>Controllo per i certificati tra controller di rete e ogni Host Hyper-V e ID host corrispondente 
In un Host Hyper-V, eseguire i comandi seguenti per verificare che l'ID host corrisponde all'Id istanza di una risorsa di server nel Controller di rete

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

*Monitoraggio e aggiornamento* se SDNExpress utilizzando script o la distribuzione manuale, aggiornare la chiave HostId nel Registro di sistema per trovare la corrispondenza l'Id istanza della risorsa server. Riavviare l'agente Host Controller di rete nell'host Hyper-V (server fisico) se tramite VMM, eliminare il Server Hyper-V da VMM e rimuovere la chiave del Registro di sistema HostId. Quindi, aggiungere nuovamente il server tramite VMM.


Verificare che le identificazioni personali dei certificati x. 509 utilizzati dall'host di Hyper-V (il nome host sarà il nome soggetto del certificato) per la comunicazione tra l'Host Hyper-V (servizio agente di contesto dei nomi Host) e i nodi di Controller di rete (SouthBound) sono gli stessi. Controllare inoltre che disponga di nome soggetto del certificato resto del Controller di rete *CN =<FQDN or IP>*.

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

È inoltre possibile controllare i seguenti parametri di ogni certificato per assicurarsi che il nome del soggetto è quello previsto (nome host o FQDN REST contesto dei nomi o IP), e che tutte le autorità di certificazione nella catena di certificati sono inclusi nell'autorità radice attendibile il certificato non è ancora scaduto.

- Nome del soggetto  
- Data di scadenza  
- L'autorità radice attendibili  

*Monitoraggio e aggiornamento* se più certificati hanno lo stesso nome di soggetto nell'host Hyper-V, l'agente Host Controller di rete in modo casuale scegliere uno da presentare al Controller di rete. Questo potrebbe non corrispondere l'identificazione personale della risorsa server nota per il Controller di rete. In questo caso, eliminare uno dei certificati con lo stesso nome soggetto nell'host Hyper-V e quindi riavviare il servizio agente Host Controller di rete. Se ancora può non essere effettuata una connessione, eliminare il certificato altri con lo stesso nome soggetto nell'Host Hyper-V ed eliminare la risorsa server corrispondente in VMM. Quindi, ricreare la risorsa di server in VMM che consente di generare un nuovo certificato x. 509 e installarlo nell'host Hyper-V.
  

#### <a name="check-the-slb-configuration-state"></a>Controllare lo stato di configurazione SLB
È possibile determinare lo stato di configurazione SLB come parte dell'output al cmdlet Debug NetworkController. Questo cmdlet sarà inoltre il set corrente di risorse di Controller di rete nel file JSON, tutte le configurazioni IP da ogni host Hyper-V (server) e criteri di rete locale di tabelle di database dell'agente Host. 

Per impostazione predefinita verranno raccolti tracce aggiuntive. Per non raccogliere tracce, aggiungere IncludeTraces-: parametro $false.

```none
Debug-NetworkController -NetworkController <FQDN or IP> [-Credential <PS Credential>] [-IncludeTraces:$false]

# Don't collect traces
$cred = Get-Credential 
Debug-NetworkController -NetworkController 10.127.132.211 -Credential $cred -IncludeTraces:$false

Transcript started, output file is C:\\NCDiagnostics.log
Collecting Diagnostics data from NC Nodes
```

>[!NOTE]
>Il percorso di output predefinito sarà la directory \NCDiagnostics\ < working_directory >. La directory di output predefinito può essere modificata utilizzando il `-OutputDirectory` parametro. 

Le informazioni sullo stato di configurazione SLB sono reperibile nel _diagnostica slbstateResults.Json_ file in questa directory.

Questo file JSON può essere suddivisi nelle seguenti sezioni:
 * Infrastruttura
   * SlbmVips - in questa sezione elenca l'indirizzo IP dell'indirizzo VIP SLB Manager che viene utilizzato dal Controller di rete di configurazione coodinate e lo stato tra gli agenti Host SLB e SLB Muxes.
   * MuxState - in questa sezione è elencate un valore di ogni Mux SLB distribuito fornendo lo stato di mux
   * Configurazione del router - in questa sezione è elencate Upstream del Router (Peer BGP) il numero di sistema autonomo (ASN), indirizzo IP di transito e ID. Sarà anche elencato il SLB Muxes ASN e IP transito.
   * Connesso Host Info - in questa sezione verrà elenco IP gestione risolvere tutti gli host Hyper-V disponibili per l'esecuzione di carichi di lavoro con carico bilanciato.
   * Intervalli di indirizzi VIP - in questa sezione sono elencati gli intervalli di pool di indirizzi VIP IP pubblici e privati. L'indirizzo VIP SLBM saranno incluso come un indirizzo IP allocato da uno di questi intervalli. 
   * Route MUX - in questa sezione è elencate un valore di ogni Mux SLB distribuito contenente tutti gli annunci della Route per quel particolare mux.
 * Tenant
   * VipConsolidatedState - in questa sezione è elencate lo stato di connettività di indirizzi VIP ogni Tenant con prefisso di route annunciati, Host Hyper-V e gli endpoint DIP.
    
> [!NOTE]
> Stato SLB accessibili direttamente tramite il [DumpSlbRestState](https://github.com/Microsoft/SDN/blob/master/Diagnostics/DumpSlbRestState.ps1) script disponibile nel [repository GitHub SDN Microsoft](https://github.com/microsoft/sdn). 

#### <a name="gateway-validation"></a>Convalida del gateway

**Dal Controller di rete:**
```
Get-NetworkControllerLogicalNetwork
Get-NetworkControllerPublicIPAddress
Get-NetworkControllerGatewayPool
Get-NetworkControllerGateway
Get-NetworkControllerVirtualGateway
Get-NetworkControllerNetworkInterface
```

**Dal Gateway VM:**
```
Ipconfig /allcompartments /all
Get-NetRoute -IncludeAllCompartments -AddressFamily
Get-NetBgpRouter
Get-NetBgpRouter | Get-BgpPeer
Get-NetBgpRouter | Get-BgpRouteInformation
```

**Dal commutatore Top of Rack (ToR):**

`sh ip bgp summary (for 3rd party BGP Routers)`

**Router BGP di Windows**
```
Get-BgpRouter
Get-BgpPeer
Get-BgpRouteInformation
```

Oltre a queste, da problemi che abbiamo visto finora (soprattutto per le distribuzioni SDNExpress base), il motivo più comune per il raggruppamento di Tenant recupero non è configurato nelle macchine virtuali GW sembra essere il fatto che la capacità di GW in FabricConfig.psd1 è inferiore rispetto a quello persone tenta di assegnare a connessioni di rete (tunnel S2S) in TenantConfig.psd1. Questo può essere controllato facilmente confrontando l'output dei comandi seguenti:

```
PS > (Get-NetworkControllerGatewayPool -ConnectionUri $uri).properties.Capacity
PS > (Get-NetworkControllerVirtualgatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "TenantName").properties.OutboundKiloBitsPerSecond
PS > (Get-NetworkControllerVirtualgatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "TenantName").property
```

### <a name="hoster-validate-data-plane"></a>[Provider] Convalida del piano dati
Dopo che è stato distribuito il Controller di rete, subnet e reti virtuali tenant sono state create e le macchine virtuali siano state associate alle subnet virtuali, i test di livello di infrastruttura aggiuntive possono essere eseguite dal provider di servizi di hosting per verificare la connettività tenant.

#### <a name="check-hnv-provider-logical-network-connectivity"></a>Verificare la connettività di rete logica Provider di virtualizzazione rete
Dopo il primo guest macchina virtuale in esecuzione in un host Hyper-V sia stato connesso a una rete virtuale del tenant, il Controller di rete assegnerà due indirizzi IP di Provider di virtualizzazione rete (gli indirizzi IP PA) per l'Host Hyper-V. Questi indirizzi IP viene offerto dal Pool IP della rete logica Provider di virtualizzazione rete e gestite dal Controller di rete.  Per scoprire quali sono i due indirizzi IP di virtualizzazione rete di

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

Questi indirizzi IP Provider di virtualizzazione rete (indirizzi IP PA) vengono assegnati a schede Ethernet creato in un raggruppamento di rete TCP/IP separato in modo che il nome di una scheda di _VLANX_ dove X è la VLAN assegnata alla rete logica Provider di virtualizzazione rete (trasporto).

La connettività tra due host Hyper-V utilizzando il Provider di virtualizzazione rete logica può essere eseguita da un ping con un raggruppamento aggiuntiva (-c Y) parametro in cui il raggruppamento di rete TCP/IP in cui vengono creati i PAhostVNICs Y. Questo raggruppamento può essere determinato eseguendo:

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
> Schede vNIC dell'Host PA non vengono utilizzate nel percorso dei dati e pertanto non sono un indirizzo IP assegnato alla scheda"vEthernet (PAhostVNic)".

Ad esempio, presuppongono che la host Hyper-V 1 e 2 indirizzi IP di virtualizzazione rete Provider (PA) di:

|Host hyper-V-|-Indirizzo IP PA 1|-Indirizzo IP PA 2|
|---             |---            |---             |
|Host 1 | 10.10.182.64 | 10.10.182.65 |
|Host 2 | 10.10.182.66 | 10.10.182.67 |

è possibile effettuare il ping tra i due usando il comando seguente per verificare la connettività di rete logica Provider di virtualizzazione rete.

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

*Monitoraggio e aggiornamento* ping Provider di virtualizzazione rete se non riesce, controllare la connettività di rete fisica tra la configurazione VLAN. Le schede NIC fisiche in ogni host Hyper-V deve essere in modalità trunk con alcuna VLAN specifico assegnato. VNIC Host di gestione deve essere isolato per VLAN della gestione rete logica.

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
 
#### <a name="check-mtu-and-jumbo-frame-support-on-hnv-provider-logical-network"></a>Controllare il supporto MTU e Frame Jumbo sulla rete logica Provider di virtualizzazione rete

Un altro problema comune nella rete logica Provider di virtualizzazione rete è che le porte di rete fisica e/o una scheda Ethernet non dispongono di una MTU di grandi dimensioni abbastanza configurato per gestire l'overhead di incapsulamento (VXLAN o NVGRE). 
>[!NOTE]
> Alcune schede Ethernet e i driver supportano il nuovo * EncapOverhead parola chiave che verrà impostata automaticamente dall'agente Host Controller di rete su un valore di 160. Quindi questo valore verrà aggiunto il valore di * parola chiave JumboPacket cui riepilogo viene utilizzato come il valore MTU annunciato.
> ad esempio, * EncapOverhead = 160 e * JumboPacket = 1514 = > MTU = 1674B

```none
# Check whether or not your Ethernet card and driver support *EncapOverhead
PS C:\ > Test-EncapOverheadSettings

Verifying Physical Nic : <NIC> Ethernet Adapter #2
Physical Nic  <NIC> Ethernet Adapter #2 can support SDN traffic. Encapoverhead value set on the nic is  160
Verifying Physical Nic : <NIC> Ethernet Adapter
Physical Nic  <NIC> Ethernet Adapter can support SDN traffic. Encapoverhead value set on the nic is  160
```

Per verificare se la rete logica Provider di virtualizzazione rete supporta il più grande MTU dimensioni end-to-end, utilizzare il _Test LogicalNetworkSupportsJumboPacket_ cmdlet:
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

# TODO: Success Results aftering updating MTU on physical switch ports

```

*Monitoraggio e aggiornamento*
* Regolare le dimensioni MTU su porte del commutatore fisico di almeno 1674B (inclusi intestazione Ethernet 14B e trailer)
* Se la scheda NIC non supporta la parola chiave EncapOverhead, modificare la parola chiave JumboPacket per essere almeno 1674B


#### <a name="check-tenant-vm-nic-connectivity"></a>Verificare la connettività NIC macchina virtuale Tenant
Ogni scheda di rete della macchina virtuale assegnato a una macchina virtuale guest dispone di un mapping di CA-PA tra il privata indirizzo cliente (CA) e lo spazio indirizzo Provider virtualizzazione rete (PA). Questi mapping vengono mantenuti nelle tabelle server OVSDB in ogni host Hyper-V e sono reperibili eseguendo il cmdlet seguente.

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
> Se si prevede che i mapping di CA-PA non vengono emessi per un determinato tenant macchina virtuale, controllare le risorse di configurazione IP e VM NIC sul Controller di rete utilizzando il _Get-NetworkControllerNetworkInterface_ cmdlet. Controllare inoltre le connessioni stabilite tra i nodi del contesto dei nomi Host agente e Controller di rete.

Con queste informazioni, un ping di macchina virtuale tenant ora può essere avviato da provider di servizi di hosting dal Controller di rete utilizzando il _Test VirtualNetworkConnection_ cmdlet.

## <a name="specific-troubleshooting-scenarios"></a>Scenari di risoluzione dei problemi specifici

Le sezioni seguenti forniscono indicazioni per la risoluzione dei problemi relativi a scenari specifici.

### <a name="no-network-connectivity-between-two-tenant-virtual-machines"></a>Nessuna connettività di rete tra due macchine virtuali tenant

1.  [Tenant] Assicurarsi che Windows Firewall in macchine virtuali tenant non blocca il traffico.  
2.  [Tenant] Verificare che gli indirizzi IP sono stati assegnati alla macchina virtuale tenant eseguendo _ipconfig_. 
3.  [Provider] Eseguire **Test VirtualNetworkConnection** dall'host Hyper-V per convalidare la connettività tra le macchine virtuali due tenant in questione. 

>[!NOTE]
>Il VSID si intende l'ID Subnet virtuale. Nel caso VXLAN, questo è l'identificatore rete VXLAN (VNI). È possibile trovare questo valore eseguendo il **Get-PACAMapping** cmdlet.

#### <a name="example"></a>Esempio

    $password = ConvertTo-SecureString -String "password" -AsPlainText -Force
    $cred = New-Object pscredential -ArgumentList (".\administrator", $password) 

Creare "sa18n30-2.sa18.nttest.microsoft.com" con IP Mgmt di 10.127.132.153 ListenerCA IP di 192.168.1.5 che sia collegati a Subnet virtuale (VSID) 4114 di CA-ping tra "Verde Web VM 1" con IP SenderCA di 192.168.1.4 nell'Host.

    Test-VirtualNetworkConnection -OperationId 27 -HostName sa18n30-2.sa18.nttest.microsoft.com -MgmtIp 10.127.132.153 -Creds $cred -VMName "Green Web VM 1" -VMNetworkAdapterName "Green Web VM 1" -SenderCAIP 192.168.1.4 -SenderVSID 4114 -ListenerCAIP 192.168.1.5 -ListenerVSID 4114

    Test-VirtualNetworkConnection at command pipeline position 1

Test di ping spazio CA avvio avvio sessione di traccia Ping per 192.168.1.5 ha avuto esito positivo dall'indirizzo 192.168.1.4 Rtt = 0 ms


Informazioni di Routing CA:

    Local IP: 192.168.1.4
    Local VSID: 4114
    Remote IP: 192.168.1.5
    Remote VSID: 4114
    Distributed Router Local IP: 192.168.1.1
    Distributed Router Local MAC: 40-1D-D8-B7-1C-06
    Local CA MAC: 00-1D-D8-B7-1C-05
    Remote CA MAC: 00-1D-D8-B7-1C-07
    Next Hop CA MAC Address: 00-1D-D8-B7-1C-07

Informazioni di Routing PA:

    Local PA IP: 10.10.182.66
    Remote PA IP: 10.10.182.65
 
 <snip> ...

4.  [Tenant] Verificare che non esiste alcun criterio di firewall distribuito specificato nella subnet virtuale o interfacce di rete VM che bloccano il traffico.    

Eseguire una query nell'ambiente di demo sa18n30nc nel dominio sa18.nttest.microsoft.com API REST di Controller di rete.

    $uri = "https://sa18n30nc.sa18.nttest.microsoft.com"
    Get-NetworkControllerAccessControlList -ConnectionUri $uri 

# <a name="look-at-ip-configuration-and-virtual-subnets-which-are-referencing-this-acl"></a>Esaminare la configurazione IP e le subnet virtuali che fanno riferimento l'ACL

1. [Provider] Eseguire ``Get-ProviderAddress`` in entrambi Hyper-V che ospita i due host tenant di macchine virtuali in questione e quindi eseguire ``Test-LogicalNetworkConnection`` o ``ping -c <compartment>`` dall'host per convalidare la connettività nella rete logica Provider di virtualizzazione rete Hyper-V
2.  [Provider] Assicurarsi che le impostazioni di MTU siano corrette negli host Hyper-V e qualsiasi livello 2 dispositivi tra gli host Hyper-V di commutazione. Eseguire ``Test-EncapOverheadValue`` in tutti gli host Hyper-V in questione. Controllare anche che tutti i commutatori di livello 2 in tra MTU impostato su almeno 1674 byte per conto di carico massimo di byte 160.  
3.  [Provider] Se non sono presenti indirizzi IP PA e/o CA connettività viene interrotta, verificare i criteri di rete sono stato ricevuto. Eseguire ``Get-PACAMapping`` per vedere se le regole di incapsulamento e mapping CA-PA necessari per la creazione di overlay della rete virtuale siano correttamente impostati.  
4.  [Provider] Verificare che l'agente Host Controller di rete sia collegato al Controller di rete. Eseguire ``netstat -anp tcp |findstr 6640`` per verificare se il   
5.  [Provider] Verificare che l'Host ID in HKLM / corrisponde all'ID istanza delle risorse del server che ospita le macchine virtuali tenant.  
6. [Provider] Verificare che l'ID del profilo di porta corrisponde all'ID istanza delle interfacce di rete VM delle macchine virtuali tenant.  

## <a name="logging-tracing-and-advanced-diagnostics"></a>La registrazione, traccia e diagnostica avanzata

Le sezioni seguenti forniscono informazioni sulla diagnostica avanzata, registrazione e traccia.

### <a name="network-controller-centralized-logging"></a>Controller di rete centralizzata registrazione 
 
Il Controller di rete può raccogliere registri debugger e archiviarli in una posizione centralizzata automaticamente. Raccolta di log può essere abilitata quando quando si distribuisce il Controller di rete per la prima volta o in qualsiasi momento in un secondo momento. Vengono raccolti dal Controller di rete, i registri e gli elementi gestiti dal Controller di rete di rete: host di macchine, servizi di bilanciamento del carico software (SLB) e gateway. 

Questi registri includono i registri di debug per il cluster di Controller di rete, l'applicazione Controller di rete, i registri di gateway, SLB, le reti virtuali e firewall distribuito. Ogni volta che viene aggiunto un nuovo host/SLB/gateway per il Controller di rete, la registrazione è stata avviata su tali macchine. Analogamente, quando un host, SLB/gateway viene rimosso dal Controller di rete, la registrazione è stata arrestata su tali macchine.

#### <a name="enable-logging"></a>Abilitare la registrazione

La registrazione è abilitata automaticamente quando si installa il cluster di Controller di rete mediante il **installazione NetworkControllerCluster** cmdlet. Per impostazione predefinita, i registri vengono raccolti localmente sui nodi di Controller di rete in *%systemdrive%\SDNDiagnostics*. È **consigliabile** che modificare questo percorso per la condivisione di file remota (non locale). 

I registri di Controller di rete del cluster vengono archiviati in *%programData%\Windows fabric\log\traces locale*. È possibile specificare una posizione centralizzata per la raccolta di log con il **DiagnosticLogLocation** parametro con l'indicazione che è anche possibile una condivisione file remota. 

Se si desidera limitare l'accesso a questo percorso, è possibile fornire le credenziali di accesso con il **LogLocationCredential** parametro. Se si forniscono le credenziali per accedere al percorso del registro, è inoltre necessario fornire il **CredentialEncryptionCertificate** parametro, che viene usata per crittografare le credenziali archiviate in locale nei nodi del Controller di rete.  

Con le impostazioni predefinite, si consiglia di disporre almeno 75 GB di spazio libero in posizione centrale e 25 GB nei nodi locali (se non si utilizza una posizione centrale) per un cluster di Controller di rete di 3 nodi.

#### <a name="change-logging-settings"></a>Modificare le impostazioni di registrazione

È possibile modificare le impostazioni di registrazione in qualsiasi momento utilizzando il ``Set-NetworkControllerDiagnostic`` cmdlet. Possono essere modificate le impostazioni seguenti:

- **Percorso log centralizzata**.  È possibile modificare il percorso per archiviare tutti i registri, con la ``DiagnosticLogLocation`` parametro.  
- **Le credenziali per accedere alla posizione registro**.  È possibile modificare le credenziali per accedere al percorso del registro, con la ``LogLocationCredential`` parametro.  
- **Spostare la registrazione locale**.  Se la posizione centralizzata per l'archiviazione dei registri, è possibile riportare l'accesso localmente i nodi di Controller di rete con il ``UseLocalLogLocation`` parametro (scelta non consigliata a causa di requisiti di spazio su disco di grandi dimensioni).  
- **Registrazione ambito**.  Per impostazione predefinita, vengono raccolti tutti i registri. È possibile modificare l'ambito per raccogliere solo i registri di cluster di Controller di rete.  
- **Livello di registrazione**.  Il livello di registrazione predefinito è informativo. È possibile modificarlo per errore, avviso o Verbose.  
- **Log del periodo di tempo di permanenza**.  I log vengono archiviati in modo circolare. Per impostazione predefinita, se si usa registrazione centralizzata o locale, sarà necessario 3 giorni di registrazione dei dati. È possibile modificare questo limite di tempo con **LogTimeLimitInDays** parametro.  
- **Dimensioni di durata registro**.  Per impostazione predefinita, se si utilizza la registrazione centralizzata e 25 GB se utilizza la registrazione locale sarà necessario un massimo 75 GB di dati di registrazione. È possibile modificare questo limite con il **LogSizeLimitInMBs** parametro.

#### <a name="collecting-logs-and-traces"></a>La raccolta dei registri e tracce

Per impostazione predefinita, le distribuzioni di VMM utilizzano registrazione centralizzato per il Controller di rete. Il percorso di condivisione file per questi registri viene specificato quando si distribuisce il modello di servizio Controller di rete.

Se un percorso del file non è specificato, locale registrazione verrà utilizzata in ogni nodo di Controller di rete con i registri salvati in C:\Windows\tracing\SDNDiagnostics. Questi log vengono salvati con la gerarchia:

- CrashDumps
- NCApplicationCrashDumps
- NCApplicationLogs
- Contatori delle prestazioni del
- SDNDiagnostics
- Tracce

Il Controller di rete utilizza Service Fabric (Azure). Service Fabric registri potrebbero essere necessari durante la risoluzione di alcuni problemi. Questi registri è reperibile in ogni nodo di Controller di rete all'infrastruttura C:\ProgramData\Microsoft\Service.

Se un utente è stato eseguito il _NetworkController Debug_ cmdlet, altri registri saranno disponibili in ogni host Hyper-V che è stato specificato con una risorsa del server nel Controller di rete. Questi registri (e le tracce se abilitata) vengono mantenute in C:\NCDiagnostics

### <a name="slb-diagnostics"></a>Diagnostica SLB

#### <a name="slbm-fabric-errors-hosting-service-provider-actions"></a>Errori di infrastruttura SLBM (azioni di provider di Hosting del servizio)

1.  Verificare che funzioni Software carico bilanciamento Manager (SLBM) e che i livelli di orchestrazione possono comunicare: SLBM -> Mux SLB e SLBM -> SLB Host agenti. Eseguire [DumpSlbRestState](https://github.com/Microsoft/SDN/blob/master/Diagnostics/DumpSlbRestState.ps1) da qualsiasi nodo con accesso all'Endpoint REST Controller di rete.  
2.  Convalidare la *SDNSLBMPerfCounters* Perfmon in uno del nodo di Controller di rete di macchine virtuali (preferibilmente il Controller di rete nodo primario - Get-NetworkControllerReplica):
    1.  Motore servizio di bilanciamento carico (kg) è connesso a SLBM? (*SLBM LBEngine configurazioni totale* > 0)  
    2.  SLBM almeno conoscere il proprio endpoint? (*VIP endpoint totale* > = 2)  
    3.  Gli host Hyper-V (DIP) connessi a SLBM? (*HP client connessi* = = num server)   
    4.  SLBM è connesso a Muxes? (*Muxes connesso* == *Muxes integro su SLBM* == *Muxes reporting integro* = # SLB Muxes VM).  
3.  Assicurarsi che il router BGP configurato correttamente è peering con MUX SLB  
    1.  Se si usa RRAS con accesso remoto (ad esempio, macchina virtuale BGP):  
        1.  Get-BgpPeer dovrebbe venire visualizzata connesso  
        2.  Get-BgpRouteInformation deve visualizzare almeno una route per il SLBM self VIP  
    2.  Se con fisico Top-of-Rack (ToR) come Peer BGP, consultare la documentazione  
        1.  Ad esempio: # Mostra istanza bgp  
4.  Convalidare la *SlbMuxPerfCounters* e *SLBMUX* i contatori di PerfMon nella macchina virtuale Mux SLB
5.  Controllare lo stato di configurazione e gli intervalli di indirizzi VIP nella risorsa di gestione di bilanciamento del carico Software  
    1.  Get-NetworkControllerLoadBalancerConfiguration - ConnectionUri < https://<FQDN or IP>| ConvertTo-json-depth 8 (controllare gli intervalli di indirizzi VIP in pool IP e verificare SLBM self-VIP (*LoadBalanacerManagerIPAddress*) e tutti gli indirizzi VIP con connessione tenant sono all'interno di questi intervalli)  
        1. Get-NetworkControllerIpPool - NetworkId "< pubblica e privata VIP logica risorsa ID di rete >" - SubnetId "< pubblica e privata VIP logica risorsa ID Subnet >" - ResourceId "<IP Pool Resource Id>" - ConnectionUri $uri | convertto-json-depth 8 
    2.  Debug-NetworkControllerConfigurationState-  

Se uno dei controlli sopra hanno esito negativo, il tenant stato SLB sarà anche in una modalità di errore.  

**Monitoraggio e aggiornamento**   
In base alle seguenti informazioni diagnostiche presentate, correggere le operazioni seguenti:  
* Accertarsi che siano connessi multiplexer SLB  
  * Risolvere i problemi di certificato  
  * Risolvere i problemi di connettività di rete  
* Verificare le informazioni di peering BGP sia configurate correttamente  
* Assicurarsi ID Host nel Registro di sistema corrisponda ID dell'istanza di Server nel Server di risorse (fare riferimento a appendice per *HostNotConnected* codice di errore)  
* Raccolta di log  

#### <a name="slbm-tenant-errors-hosting-service-provider--and-tenant-actions"></a>Errori SLBM Tenant (azioni tenant e i provider del servizio di Hosting)

1.  [Provider] Controllare *Debug NetworkControllerConfigurationState* per vedere se tutte le risorse bilanciatore del carico sono in stato di errore. Provare a ridurre seguendo le azioni tabella nell'appendice.   
    1.  Verificare che un endpoint VIP sia presente e route pubblicità  
    2.  Controllare il numero degli endpoint DIP sono stati individuati per l'endpoint di indirizzi VIP  
2.  [Tenant] Convalidare le risorse di sistema di bilanciamento del carico sono specificate correttamente  
    1.  Convalidare DIP endpoint che sono registrati in SLBM ospitano macchine virtuali tenant che corrispondono alle configurazioni IP del pool di indirizzi LoadBalancer Back-end  
3.  [Provider] Se gli endpoint DIP non vengono individuati o connesso:   
    1.  Controllare *NetworkControllerConfigurationState di Debug*  
        1.  Convalidare tale contesto dei nomi e l'agente Host SLB correttamente è connesso al coordinatore di eventi di Controller di rete usando ``netstat -anp tcp |findstr 6640)``  
    2.  Controllare *HostId* in *nchostagent* chiave di registro del servizio (riferimento *HostNotConnected* codice di errore nell'appendice) corrisponde a Id istanza della risorsa server corrispondente (``Get-NCServer |convertto-json -depth 8``)  
    3.  Verificare id profilo di porta per la macchina virtuale porta corrisponda all'Id istanza corrispondente macchina virtuale NIC della risorsa   
4.  [Provider di hosting] Raccolta di log   

#### <a name="slb-mux-tracing"></a>Traccia Mux SLB

Informazioni di Muxes servizio di bilanciamento del carico Software possono essere determinate anche tramite il Visualizzatore eventi. 
1. Fare clic su "Mostra analitico e Debug registri" nel menu Visualizza Visualizzatore eventi
2. Passare a "Registri applicazioni e servizi" > Microsoft > Windows > SlbMuxDriver > nel Visualizzatore eventi di traccia 
3. Fare clic su di esso e selezionare "Attiva registro"

>[!NOTE]
>È consigliabile avere solo la registrazione abilitata per un breve periodo di tempo, anche se si sta tentando di riprodurre un problema

### <a name="vfp-and-vswitch-tracing"></a>VFP di vSwitch tracciatura

Da qualsiasi Hyper-V host che ospita una macchina virtuale associata a una rete virtuale tenant guest, è possibile raccolti una traccia VFP per determinare quale potrebbero essere problemi.

```
netsh trace start provider=Microsoft-Windows-Hyper-V-VfpExt overwrite=yes tracefile=vfp.etl report=disable provider=Microsoft-Windows-Hyper-V-VmSwitch 
netsh trace stop
netsh trace convert .\vfp.etl ov=yes 
```
