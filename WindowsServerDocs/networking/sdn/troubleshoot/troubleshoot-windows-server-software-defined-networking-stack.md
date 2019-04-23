---
title: Risolvere i problemi dello stack di SDN (Software Defined Networking) di Windows Server
description: Questa Guida di Windows Server esamina i comuni errori di rete SDN (Software Defined) e gli scenari di errore e viene illustrato un flusso di lavoro risoluzione dei problemi che sfrutta gli strumenti di diagnostica disponibili.
manager: ravirao
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 9be83ed2-9e62-49e8-88e7-f52d3449aac5
ms.author: pashort
author: JMesser81
ms.date: 08/14/2018
ms.openlocfilehash: b6d4ff37186e66bec54794f8d6c9fd8a83e23e7d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845392"
---
# <a name="troubleshoot-the-windows-server-software-defined-networking-stack"></a>Risolvere i problemi dello stack di SDN (Software Defined Networking) di Windows Server

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Questa guida esamina i comuni errori di rete SDN (Software Defined) e gli scenari di errore e viene illustrato un flusso di lavoro risoluzione dei problemi che sfrutta gli strumenti di diagnostica disponibili.  

Per altre informazioni su Microsoft Software Defined Networking, vedere [Software Defined Networking](../../sdn/Software-Defined-Networking--SDN-.md).  

## <a name="error-types"></a>Tipi di errore  
Nell'elenco seguente rappresenta la classe di problemi che con la virtualizzazione rete Hyper-V (HNVv1) si vede più spesso in Windows Server 2012 R2 da distribuzioni di produzione in commercio e coincide in molti modi con gli stessi tipi di problemi che possono verificarsi in Windows Server 2016 HNVv2 con il nuovo Stack di rete SDN (Software Defined).  

La maggior parte degli errori possono essere classificati in un piccolo set di classi:   
* **Configurazione non valida o non supportata**  
   Un utente richiama l'API NorthBound, in modo non corretto o con i criteri non è valido.   

* **Errore nell'applicazione dei criteri**  
     Criteri da Controller di rete non è stato recapitato a un Host Hyper-V, in modo significativo ritardata e / o non aggiornati in tutti gli host Hyper-V (ad esempio, dopo una migrazione in tempo reale).  
* **Bug di software o gli orientamenti di configurazione**  
 Problemi di percorso dati risultante in pacchetti ignorati.  

* **Errore esterno correlati all'hardware di interfaccia di rete / driver o Metti di sotto dell'infrastruttura di rete**  
 Presentare un comportamento errato offload delle attività (ad esempio VMQ) o infrastruttura di rete Metti sotto configurato correttamente (ad esempio, il valore MTU)   

 Questa Guida alla risoluzione dei problemi esamina ognuna di queste categorie di errore e suggerisce le procedure consigliate e strumenti di diagnostica disponibili per identificare e correggere l'errore.  

## <a name="diagnostic-tools"></a>Strumenti di diagnostica  

Prima di illustrare la risoluzione dei problemi dei flussi di lavoro per ognuno di questi tipi di errori, verrà ora esaminato gli strumenti di diagnostica disponibili.   
  
Per usare gli strumenti di diagnostica del Controller di rete (percorso di controllo), è necessario installare la funzionalità di amministrazione remota del server NetworkController e importare il ``NetworkControllerDiagnostics`` modulo:  

```  
Add-WindowsFeature RSAT-NetworkController -IncludeManagementTools  
Import-Module NetworkControllerDiagnostics  
```  

Per usare gli strumenti di diagnostica HNV diagnostica (data-path), è necessario importare il ``HNVDiagnostics`` modulo:
  
```  
# Assumes RSAT-NetworkController feature has already been installed
Import-Module hnvdiagnostics   
```  

### <a name="network-controller-diagnostics"></a>Diagnostica del controller di rete  
Questi cmdlet sono documentati nel sito Web TechNet nel [argomento Cmdlet di diagnostica di Controller di rete](https://docs.microsoft.com/powershell/module/networkcontrollerdiagnostics/). Consentono di identificare i problemi con la coerenza dei criteri di rete nel controllo percorso tra i nodi del Controller di rete e tra il Controller di rete e gli agenti di Host dal controller di rete in esecuzione negli host Hyper-V.

 Il _Debug ServiceFabricNodeStatus_ e _Get-NetworkControllerReplica_ cmdlet devono essere eseguiti da una delle macchine virtuali del nodo di Controller di rete. Tutti gli altri cmdlet di diagnostica di controller di rete può essere eseguito da qualsiasi host che disponga della connettività al Controller di rete e si trova in un gruppo di protezione gestione Controller di rete (Kerberos) o può accedere al certificato X.509 per la gestione dei Controller di rete. 
   
### <a name="hyper-v-host-diagnostics"></a>Diagnostica di host Hyper-V  
Questi cmdlet sono documentati nel sito Web TechNet nel [argomento di Cmdlet di Hyper-V rete virtualizzazione diagnostica](https://docs.microsoft.com/powershell/module/hnvdiagnostics/). Essi consentono di identificare problemi nel percorso dei dati tra le macchine virtuali tenant (EST/ovest) e il traffico in ingresso tramite un indirizzo VIP SLB (nord/sud). 

Il _Debug VirtualMachineQueueOperation_, _Get-CustomerRoute_, _Get-PACAMapping_, _Get-ProviderAddress_, _Get-VMNetworkAdapterPortId_, _Get-VMSwitchExternalPortId_, e _Test EncapOverheadSettings_ sono tutti i test locali che possono essere eseguiti da ogni host Hyper-V. Gli altri cmdlet test-path i dati tramite il Controller di rete di richiamare e devono pertanto accedere al Controller di rete come descried sopra.
 
### <a name="github"></a>GitHub
Il [repository GitHub Microsoft/SDN](https://github.com/microsoft/sdn) ha un numero di flussi di lavoro che si basano su questi cmdlet in arrivo e gli script di esempio. In particolare, gli script di diagnostica sono reperibile nella [diagnostica](https://github.com/Microsoft/sdn/diagnostics) cartella. Aiutaci a contribuire a questi script tramite l'invio di richieste Pull.

## <a name="troubleshooting-workflows-and-guides"></a>Risoluzione dei problemi dei flussi di lavoro e guide  

### <a name="hoster-validate-system-health"></a>[Host] Convalidare l'integrità del sistema
È una risorsa incorporata denominata _lo stato della configurazione_ in diversi delle risorse del Controller di rete. Lo stato della configurazione fornisce informazioni sull'integrità di sistema, inclusa la coerenza tra la configurazione del controller di rete e lo stato effettivo (in esecuzione) negli host Hyper-V. 

Per controllare lo stato della configurazione, eseguire il comando seguente da qualsiasi host Hyper-V con la connettività al Controller di rete.

>[!NOTE] 
>Il valore per il *NetworkController* parametro deve essere il nome di dominio completo o indirizzo IP in base al nome soggetto di x. 509 > certificato creato per il Controller di rete.
>
>Il *credenziale* solo parametro deve essere specificato se il controller di rete viene utilizzata l'autenticazione Kerberos (tipica nelle distribuzioni di VMM). Le credenziali devono corrispondere per gli utenti che sono nel gruppo di sicurezza di gestione di Controller di rete.

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

Un messaggio di stato della configurazione di esempio è illustrato di seguito:

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
> È presente un bug nel sistema in cui le risorse di interfaccia di rete per la scheda di rete VM di transito Mux di bilanciamento del carico software sono in uno stato di errore con l'errore "Virtual Switch – non connessi a Controller Host". Questo errore può essere ignorato se la configurazione IP della risorsa VM NIC è impostata su un indirizzo IP dal Pool di indirizzi IP della rete logica di transito. È presente un secondo bug nel sistema in cui le risorse di interfaccia di rete per le schede NIC della macchina virtuale di Gateway HNV Provider sono in uno stato di errore con l'errore "Virtual Switch – PortBlocked". Questo errore può essere tranquillamente ignorato anche se la configurazione IP della risorsa VM NIC sia impostata su null (per impostazione predefinita).


Nella tabella seguente mostra l'elenco dei codici di errore, messaggi e azioni di completamento da eseguire in base sullo stato della configurazione osservato.

  
| **Codice**| **Messaggio**| **Azione**|  
|--------|-----------|----------|  
| Sconosciuta| Errore sconosciuto| |  
| HostUnreachable                       | Il computer host non è raggiungibile | Verificare la connettività di rete di gestione tra Controller di rete e Host |  
| PAIpAddressExhausted                  | Gli indirizzi Ip PA esaurita | Aumentare la dimensione del Pool IP della subnet logica Provider HNV |  
| PAMacAddressExhausted                 | Gli indirizzi PA Mac esaurita | Aumentare l'intervallo di Pool Mac |  
| PAAddressConfigurationFailure         | Non è riuscita ottimizzare gli indirizzi PA per host | Verificare la connettività di rete di gestione tra Controller di rete e l'Host. |  
| CertificateNotTrusted                 | Certificato non attendibile  |Risolvere i certificati usati per la comunicazione con l'host. |  
| CertificateNotAuthorized              | Certificato non autorizzato | Risolvere i certificati usati per la comunicazione con l'host. |  
| PolicyConfigurationFailureOnVfp       | Errore di configurazione dei criteri di VFP | Si tratta di un errore di runtime.  Nessun alternative di lavoro definita. Raccogliere i log. |  
| PolicyConfigurationFailure            | Errore durante il push dei criteri per gli host, a causa di errori di comunicazione o altri errori nel NetworkController.| Nessuna azione definita.  Ciò è dovuto a errore di stato dell'obiettivo di elaborazione nei moduli del Controller di rete. Raccogliere i log. |  
| HostNotConnectedToController          | L'Host non è ancora connesso al Controller di rete | Non applicato all'host o il profilo di porta non è raggiungibile dal Controller di rete. Convalidare che chiave del Registro di sistema HostID corrisponde all'ID istanza della risorsa di server |  
| MultipleVfpEnabledSwitches            | Sono presenti nell'host più VFp abilitata commutatori  | Eliminare una delle opzioni, poiché l'agente Host di Controller di rete supporta solo un commutatore virtuale con l'estensione VFP abilitata |  
| PolicyConfigurationFailure            | Non è riuscito a effettuare il push dei criteri di rete virtuale per a sua volta a causa di errori relativi al certificato o errori di connettività  | Controllare se sono stati distribuiti certificati appropriati (nome soggetto del certificato deve corrispondere FQDN dell'host). Verificare inoltre la connettività host con il Controller di rete |  
| PolicyConfigurationFailure            | Non è riuscito a effettuare il push dei criteri di vSwitch per a sua volta a causa di errori relativi al certificato o errori di connettività  | Controllare se sono stati distribuiti certificati appropriati (nome soggetto del certificato deve corrispondere FQDN dell'host). Verificare inoltre la connettività host con il Controller di rete|
| PolicyConfigurationFailure            | Non è riuscito a effettuare il push dei criteri Firewall per a sua volta a causa di errori relativi al certificato o errori di connettività | Controllare se sono stati distribuiti certificati appropriati (nome soggetto del certificato deve corrispondere FQDN dell'host). Verificare inoltre la connettività host con il Controller di rete|
| DistributedRouterConfigurationFailure | Non è riuscito a configurare le impostazioni del router distribuito su vNic host                          | Errore dello stack TCP/IP. Possono richiedere pulizia la Vnic Host di ripristino di emergenza e PA nel server in cui è stato segnalato l'errore |
| DhcpAddressAllocationFailure          | Assegnazione indirizzi DCHP non è riuscita per un VMNic                                                    | Controllare se l'attributo dell'indirizzo IP statico è configurato per la risorsa interfaccia di rete |  
| CertificateNotTrusted<br>CertificateNotAuthorized | Non è riuscito a connettersi a Mux a causa di errori di rete o del certificato | Controllare il codice numerico nel codice del messaggio di errore fornito: corrisponde al codice di errore winsock. Errori del certificato sono granulari (ad esempio, non può essere verificata cert, cert non autorizzato, ecc.) |  
| HostUnreachable                       | MUX non è integro (caso comune è BGPRouter disconnesso) | Peer BGP di RRAS (BGP una macchina virtuale) o commutatore Top-of-Rack (ToR) non è raggiungibile o peering non correttamente. Controllare le impostazioni BGP nel Software Load Balancer Multiplexer risorse e di peer BGP (ToR o RRAS in macchina virtuale) |  
| HostNotConnectedToController          | Agente host di bilanciamento del carico software non è connesso  | Verificare che sia in esecuzione il servizio agente Host di bilanciamento del carico software; Per motivi di fare riferimento a registri dell'agente host di bilanciamento del carico software (esecuzione automatica) perché, nel caso in cui SLBM (NC) ha rifiutato il certificato presentato dall'agente host che esegue lo stato verrà visualizzate informazioni sul  |  
| PortBlocked                           | La porta VFP è bloccata, a causa della mancanza di rete virtuale / i criteri di ACL | Controllare se sono presenti altri errori, che potrebbero causare non è necessario configurare i criteri. |  
| Overloaded                            | Loadbalancer MUX è in overload  | Problema di prestazioni con MUX |  
| RoutePublicationFailure               | Loadbalancer MUX non è connesso a un router BGP | Verificare che il MUX disponga della connettività con il router BGP e il peering BGP è configurato correttamente |  
| VirtualServerUnreachable              | Loadbalancer MUX non è connesso alla gestione SLB | Controllare la connettività tra SLBM e MUX |  
| QosConfigurationFailure               | Non è riuscito a configurare i criteri QOS | Se la larghezza di banda sufficiente è disponibile per tutte le macchine Virtuali se viene utilizzata la prenotazione QOS |


#### <a name="check-network-connectivity-between-the-network-controller-and-hyper-v-host-nc-host-agent-service"></a>Verificare la connettività di rete tra il controller di rete e l'Host Hyper-V (servizio agente Host dal controller di rete)
Eseguire la *netstat* comando seguente per verificare che siano presenti tre connessioni stabilite tra l'agente Host dal controller di rete e dei nodi di Controller di rete e un socket in ascolto sull'Host Hyper-V
- In attesa sulla porta TCP:6640 in Host Hyper-V (servizio agente Host di controller di rete)
- Due connessioni stabilite da Hyper-V host IP sulla porta 6640 all'indirizzo IP del nodo controller di rete sulle porte temporanee (32000 >)
- Una connessione stabilita dall'indirizzo IP dell'host Hyper-V su porte temporanee all'indirizzo IP REST di Controller di rete sulla porta 6640

>[!NOTE]
>Può esistere solo due connessioni stabilite in un host Hyper-V se non sono presenti macchine virtuali tenant distribuite in tale host specifico.

```none
netstat -anp tcp |findstr 6640

# Successful output
  TCP    0.0.0.0:6640           0.0.0.0:0              LISTENING
  TCP    10.127.132.153:6640    10.127.132.213:50095   ESTABLISHED
  TCP    10.127.132.153:6640    10.127.132.214:62514   ESTABLISHED
  TCP    10.127.132.153:50023   10.127.132.211:6640    ESTABLISHED
```
#### <a name="check-host-agent-services"></a>Controllare i servizi dell'agente Host
Il controller di rete comunica con i due servizi dell'agente host negli host Hyper-V: Agente Host di bilanciamento del carico software e dell'agente Host dal controller di rete. È possibile che uno o entrambi i servizi non è in esecuzione. Verificare il proprio stato e riavviare se non vengono eseguiti.

```none
Get-Service SlbHostAgent
Get-Service NcHostAgent

# (Re)start requires -Force flag
Start-Service NcHostAgent -Force
Start-Service SlbHostAgent -Force
```

#### <a name="check-health-of-network-controller"></a>Verificare l'integrità del controller di rete
Se sono presenti non tre stabilite le connessioni o se risponde il Controller di rete, verificare che tutti i nodi e i moduli del servizio siano in esecuzione usando i cmdlet seguenti. 

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

Verificare che lo stato della replica si trovi **pronti** ed è stato di integrità **Ok**.

In un ambiente di produzione è la distribuzione con un Controller di rete multinodo, è inoltre possibile controllare quale nodo è primaria in ogni servizio e il relativo stato di replica individuali.

```none  
Get-NetworkControllerReplica

# Sample Output for the API service module 
Replicas for service: ApiService

ReplicaRole   : Primary
NodeName      : SA18N30NC3.sa18.nttest.microsoft.com
ReplicaStatus : Ready

```
Verificare che lo stato della Replica è pronta per ogni servizio.
 
#### <a name="check-for-corresponding-hostids-and-certificates-between-network-controller-and-each-hyper-v-host"></a>Verificare la presenza di ID corrispondenti degli host e i certificati tra i controller di rete e ogni Host Hyper-V 
In un Host Hyper-V, eseguire i comandi seguenti per verificare che l'ID host corrispondente all'Id dell'istanza di una risorsa server nel Controller di rete

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

*Monitoraggio e aggiornamento* se SDNExpress usando script o la distribuzione manuale, aggiornare la chiave ID host nel Registro di sistema corrisponde all'Id istanza della risorsa server. Se tramite VMM, eliminare il Server Hyper-V da VMM, riavviare l'agente Host di Controller di rete nell'host Hyper-V (server fisico) e rimuovere la chiave del Registro di sistema HostId. Quindi, aggiungere nuovamente il server tramite VMM.


Verificare che le identificazioni personali dei certificati X.509 utilizzati dall'host Hyper-V (il nome host corrisponderà al nome soggetto del certificato) per la comunicazione tra l'Host Hyper-V (servizio agente Host dal controller di rete) e nodi di Controller di rete (SouthBound) sono uguali. Controllare anche che il certificato REST del Controller di rete ha il nome soggetto del *CN =<FQDN or IP>*.

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

È inoltre possibile controllare i parametri seguenti di ogni certificato per assicurarsi che il nome del soggetto è ciò che è previsto (nome host o FQDN di REST di controller di rete o IP), il certificato non è ancora scaduto, e che tutte le autorità di certificazione nella catena di certificati sono inclusi nella radice attendibile autorità.

- Nome soggetto  
- Data di scadenza  
- L'autorità radice attendibile  

*Monitoraggio e aggiornamento* se più certificati con lo stesso nome di soggetto nell'host Hyper-V, l'agente Host di Controller di rete in modo casuale sceglierà uno da presentare al Controller di rete. Ciò potrebbe non corrispondere l'identificazione personale della risorsa di server nota al Controller di rete. In questo caso, eliminare uno dei certificati con lo stesso nome di soggetto nell'host Hyper-V e quindi avviare nuovamente il servizio dell'agente Host di Controller di rete. Se una connessione non può ancora essere resa, eliminare l'altro certificato con lo stesso nome di soggetto nell'Host Hyper-V ed eliminare la risorsa server corrispondente in VMM. Quindi, creare nuovamente la risorsa server in VMM che genererà un nuovo certificato X.509 e installarlo nell'host Hyper-V.
  

#### <a name="check-the-slb-configuration-state"></a>Controllare lo stato di configurazione di bilanciamento del carico software
Lo stato di configurazione di bilanciamento del carico software possono essere determinato come parte dell'output al cmdlet Debug NetworkController. Questo cmdlet restituirà anche il set di risorse di Controller di rete nel file JSON, tutte le configurazioni IP da ogni host Hyper-V (server) e criteri di rete locale da tabelle di database dell'agente Host corrente. 

Le tracce aggiuntive verranno raccolti per impostazione predefinita. Per non raccogliere le tracce, aggiungere il IncludeTraces-: $false parametro.

```none
Debug-NetworkController -NetworkController <FQDN or IP> [-Credential <PS Credential>] [-IncludeTraces:$false]

# Don't collect traces
$cred = Get-Credential 
Debug-NetworkController -NetworkController 10.127.132.211 -Credential $cred -IncludeTraces:$false

Transcript started, output file is C:\\NCDiagnostics.log
Collecting Diagnostics data from NC Nodes
```

>[!NOTE]
>Il percorso di output predefinita sarà la directory di \NCDiagnostics\ < working_directory >. La directory di output predefinita può essere modificata tramite la `-OutputDirectory` parametro. 

Le informazioni sullo stato di configurazione di bilanciamento del carico software sono reperibili nel _diagnostica slbstateResults.Json_ file in questa directory.

Questo file JSON può essere suddivisi nelle seguenti sezioni:
 * Infrastruttura
   * SlbmVips - questa sezione sono elencati l'indirizzo IP dell'indirizzo VIP SLB Manager che viene usato dal Controller di rete per coodinate configurazione e l'integrità tra gli agenti di Host di bilanciamento del carico software e le istanze MUX SLB.
   * MuxState - in questa sezione elenca un valore per ogni Mux SLB offrendo lo stato del mux distribuita
   * Configurazione del router: in questa sezione elenca Upstream del Router (Peer BGP) numero sistema autonomo (ASN), l'indirizzo IP di transito e ID. Verranno visualizzate anche il numero ASN i MUX SLB e l'IP di transito.
   * Connesso informazioni sull'Host - in questa sezione si occuperanno di elenco di IP di gestione tutti gli host Hyper-V disponibili per l'esecuzione dei carichi di lavoro con bilanciamento del carico.
   * Gli intervalli di indirizzi VIP: in questa sezione elenca gli intervalli di pool IP VIP pubblici e privati. L'indirizzo VIP SLBM verrà incluso come un indirizzo IP allocato da uno di questi intervalli. 
   * Le route MUX - in questa sezione elenca un valore per ogni Mux SLB distribuita contenente tutti gli annunci di Route per quel particolare mux.
 * Tenant
   * VipConsolidatedState - in questa sezione elenca lo stato di connettività per ogni indirizzo IP virtuale del Tenant tra prefisso di route annunciati, Host Hyper-V e gli endpoint DIP.
    
> [!NOTE]
> Che è possibile ottenere lo stato di bilanciamento del carico software direttamente tramite il [DumpSlbRestState](https://github.com/Microsoft/SDN/blob/master/Diagnostics/DumpSlbRestState.ps1) script disponibile nel [repository GitHub Microsoft SDN](https://github.com/microsoft/sdn). 

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

**Dalla VM del Gateway:**
```
Ipconfig /allcompartments /all
Get-NetRoute -IncludeAllCompartments -AddressFamily
Get-NetBgpRouter
Get-NetBgpRouter | Get-BgpPeer
Get-NetBgpRouter | Get-BgpRouteInformation
```

**Dal commutatore Top of Rack (ToR):**

`sh ip bgp summary (for 3rd party BGP Routers)`

**Windows BGP Router**
```
Get-BgpRouter
Get-BgpPeer
Get-BgpRouteInformation
```

Oltre a queste, da problemi abbiamo visto finora (specialmente sul SDNExpress basati su distribuzioni), il motivo più comune per il raggruppamento di Tenant non introduzione configurato nelle macchine virtuali gateway sembra essere il fatto che la capacità di GW in FabricConfig.psd1 è minore rispetto alle operazioni svolte tenta di assegnare alle connessioni di rete (tunnel S2S) in TenantConfig.psd1 persone. Ciò può essere controllato con facilità tramite il confronto tra gli output dei comandi seguenti:

```
PS > (Get-NetworkControllerGatewayPool -ConnectionUri $uri).properties.Capacity
PS > (Get-NetworkControllerVirtualgatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "TenantName").properties.OutboundKiloBitsPerSecond
PS > (Get-NetworkControllerVirtualgatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "TenantName").property
```

### <a name="hoster-validate-data-plane"></a>[Host] Convalidare piano dati
Dopo aver distribuito il Controller di rete, subnet e reti virtuali dei tenant sono state create e le macchine virtuali sono state associate alle subnet virtuali, i test a livello di infrastruttura aggiuntive possono essere eseguiti dall'host per verificare la connettività di tenant.

#### <a name="check-hnv-provider-logical-network-connectivity"></a>Controllare la connettività di rete logica Provider HNV
Dopo il primo guest macchina virtuale in esecuzione in un host Hyper-V è stata connessa a una rete virtuale del tenant, il Controller di rete assegnerà due indirizzi IP di Provider HNV (gli indirizzi IP PA) per l'Host Hyper-V. Questi indirizzi IP proverrà dal Pool di indirizzi IP della rete logica Provider HNV ed essere gestito dal Controller di rete.  Per scoprire che cosa sono questi due indirizzi IP HNV del

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

Questi indirizzi IP di Provider HNV (indirizzi IP PA) vengono assegnati alle schede Ethernet creato in un raggruppamento di rete TCP/IP separato e hanno un nome dell'adapter del _VLANX_ dove X è la VLAN assegnata alla rete logica Provider HNV (trasporto).

Connettività tra due host Hyper-V utilizzando il Provider HNV rete logica è possibile un ping con un raggruppamento aggiuntiva (-c Y) dove Y è il raggruppamento di rete TCP/IP in cui vengono creati i PAhostVNICs parametro. Questo raggruppamento è possibile determinare eseguendo:

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
> Schede vNIC dell'Host PA non vengono utilizzate nel percorso dei dati e pertanto non è un indirizzo IP assegnato all'adapter"vEthernet (PAhostVNic)".

Ad esempio, presuppone che gli host Hyper-V 1 e 2 indirizzi IP HNV Provider (PA) di:

|Host hyper-V-|-Indirizzo IP PA 1|-Indirizzo IP PA 2|
|---             |---            |---             |
|Host 1 | 10.10.182.64 | 10.10.182.65 |
|Host 2 | 10.10.182.66 | 10.10.182.67 |

è possibile effettuare il ping tra i due usando il comando seguente per verificare la connettività di rete logica Provider HNV.

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

*Monitoraggio e aggiornamento* ping Provider HNV se non riesce, controllare la connettività di rete fisica, inclusa la configurazione VLAN. Le schede NIC fisiche in ogni host Hyper-V deve essere in modalità trunk con alcuna VLAN specifico assegnato. La scheda di rete virtuale Host di gestione deve essere isolato a VLAN della rete logica di gestione.

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
 
#### <a name="check-mtu-and-jumbo-frame-support-on-hnv-provider-logical-network"></a>Verificare il supporto MTU e Frame Jumbo sulla rete logica Provider HNV

Un altro problema comune nella rete logica Provider HNV è che le porte di rete fisica e/o la scheda Ethernet non hanno una MTU di grandi dimensioni sufficientemente è configurato per gestire il sovraccarico di incapsulamento (VXLAN o NVGRE). 
>[!NOTE]
> Alcune schede Ethernet e i driver di supportano la nuova * EncapOverhead parola chiave che verrà impostato automaticamente dall'agente Host di Controller di rete su un valore pari a 160. Questo valore verrà quindi aggiunto al valore della * JumboPacket parola chiave la cui somma viene utilizzato come il valore MTU annunciato.
> ad esempio * EncapOverhead = 160 e * JumboPacket 1514 = = > MTU = 1674B

```none
# Check whether or not your Ethernet card and driver support *EncapOverhead
PS C:\ > Test-EncapOverheadSettings

Verifying Physical Nic : <NIC> Ethernet Adapter #2
Physical Nic  <NIC> Ethernet Adapter #2 can support SDN traffic. Encapoverhead value set on the nic is  160
Verifying Physical Nic : <NIC> Ethernet Adapter
Physical Nic  <NIC> Ethernet Adapter can support SDN traffic. Encapoverhead value set on the nic is  160
```

Per testare se la rete logica Provider HNV supporta la più grande MTU dimensioni end-to-end, usare il _Test-LogicalNetworkSupportsJumboPacket_ cmdlet:
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
* Regolare la dimensione MTU le porte del commutatore fisico almeno 1674B (inclusi 14B Ethernet intestazione e trailer)
* Se la scheda di interfaccia di rete non supporta la parola chiave EncapOverhead, modificare la parola chiave JumboPacket per essere almeno 1674B


#### <a name="check-tenant-vm-nic-connectivity"></a>Verificare la connettività di interfaccia di rete VM Tenant
Ogni interfaccia di rete della macchina virtuale assegnato a una macchina virtuale guest ha un mapping CA-PA tra il privata indirizzo cliente (CA) e lo spazio di indirizzi del Provider HNV (PA). Questi mapping vengono mantenuti nelle tabelle server OVSDB in ogni host Hyper-V e sono reperibili eseguendo il cmdlet seguente.

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
> Se si prevede che i mapping di CA-PA non vengono restituiti per un determinato tenant della macchina virtuale, controllare le risorse di VM NIC e la configurazione IP nel Controller di rete usando il _Get-NetworkControllerNetworkInterface_ cmdlet. Controllare inoltre le connessioni stabilite tra i nodi dell'agente Host dal controller di rete e Controller di rete.

Con queste informazioni, un ping della macchina virtuale tenant ora può essere avviato da host dal Controller di rete usando il _Test-VirtualNetworkConnection_ cmdlet.

## <a name="specific-troubleshooting-scenarios"></a>Scenari di risoluzione dei problemi specifici

Le sezioni seguenti forniscono indicazioni per la risoluzione dei problemi relativi a scenari specifici.

### <a name="no-network-connectivity-between-two-tenant-virtual-machines"></a>Nessuna connettività di rete tra due macchine virtuali tenant

1.  [Tenant] Verificare che Windows Firewall nelle macchine virtuali tenant non stia bloccando il traffico.  
2.  [Tenant] Verificare che gli indirizzi IP sono stati assegnati alla macchina virtuale tenant eseguendo _ipconfig_. 
3.  [Host] Eseguire **Test-VirtualNetworkConnection** dall'host Hyper-V per convalidare la connettività tra le macchine virtuali due tenant in questione. 

>[!NOTE]
>Il VSID si intende l'ID Subnet virtuale. Nel caso VXLAN, questo è l'identificatore rete VXLAN (VNI). È possibile trovare questo valore eseguendo il **Get-PACAMapping** cmdlet.

#### <a name="example"></a>Esempio

    $password = ConvertTo-SecureString -String "password" -AsPlainText -Force
    $cred = New-Object pscredential -ArgumentList (".\administrator", $password) 

Creare "sa18n30-2.sa18.nttest.microsoft.com" con indirizzo IP di 10.127.132.153 ListenerCA IP Mgmt di 192.168.1.5 che entrambe associate alla Subnet virtuale (VSID) 4114 CA-ping tra "Verde Web VM 1" con indirizzo IP SenderCA di 192.168.1.4 nell'Host.

    Test-VirtualNetworkConnection -OperationId 27 -HostName sa18n30-2.sa18.nttest.microsoft.com -MgmtIp 10.127.132.153 -Creds $cred -VMName "Green Web VM 1" -VMNetworkAdapterName "Green Web VM 1" -SenderCAIP 192.168.1.4 -SenderVSID 4114 -ListenerCAIP 192.168.1.5 -ListenerVSID 4114

    Test-VirtualNetworkConnection at command pipeline position 1

Test di ping spazio CA iniziale avviare la sessione di traccia Ping a 192.168.1.5 ha avuto esito positivo dall'indirizzo 192.168.1.4 Rtt = 0 ms


Informazioni di Routing di autorità di certificazione:

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

4.  [Tenant] Verificare che non vi sia alcun criterio di firewall distribuito specificati nelle subnet virtuali o le interfacce di rete della macchina virtuale, che bloccano il traffico.    

L'API REST del Controller di rete disponibili in un ambiente demo in sa18n30nc nel dominio sa18.nttest.microsoft.com di query.

    $uri = "https://sa18n30nc.sa18.nttest.microsoft.com"
    Get-NetworkControllerAccessControlList -ConnectionUri $uri 

# <a name="look-at-ip-configuration-and-virtual-subnets-which-are-referencing-this-acl"></a>Esaminare la configurazione IP e subnet virtuali che fanno riferimento a questo ACL

1. [Host] Eseguire ``Get-ProviderAddress`` su Hyper-V host che ospita i due tenant le macchine virtuali in questione e quindi eseguire ``Test-LogicalNetworkConnection`` o ``ping -c <compartment>`` dall'host Hyper-V per convalidare la connettività sulla rete logica Provider HNV
2.  [Host] Assicurarsi che le impostazioni di MTU siano corretti negli host Hyper-V e qualsiasi livello 2 cambio di dispositivi tra gli host Hyper-V. Eseguire ``Test-EncapOverheadValue`` in tutti gli host Hyper-V in questione. Controllare anche che tutti i commutatori di livello 2 tra MTU impostato su minimo byte 1674 per tenere conto per l'overhead massimo di byte 160.  
3.  [Host] Se non sono presenti indirizzi IP PA e/o viene interrotta la connettività di autorità di certificazione, verificare i criteri di rete sono stato ricevuto. Eseguire ``Get-PACAMapping`` per vedere se le regole di incapsulamento e mapping CA-PA necessari per la creazione di overlay della rete virtuale vengono stabilite correttamente.  
4.  [Host] Verificare che l'agente Host di Controller di rete è connesso al Controller di rete. Eseguire ``netstat -anp tcp |findstr 6640`` per verificare se la   
5.  [Host] Verificare che l'Host di ID nell'hive HKLM / corrisponde all'ID istanza delle risorse del server che ospita le macchine virtuali tenant.  
6. [Host] Verificare che l'ID del profilo di porta corrisponde all'ID di istanza di interfacce di rete della macchina virtuale di macchine virtuali tenant.  

## <a name="logging-tracing-and-advanced-diagnostics"></a>La registrazione, traccia e diagnostica avanzata

Le sezioni seguenti forniscono informazioni su diagnostica avanzata, registrazione e traccia.

### <a name="network-controller-centralized-logging"></a>Registrazione centralizzata di controller di rete 
 
Il Controller di rete può automaticamente raccogliere i log del debugger e archiviarli in una posizione centralizzata. Raccolta di log può essere abilitata quando si distribuisce il Controller di rete per la prima volta o in qualsiasi momento in un secondo momento. I log vengono raccolti dal Controller di rete e gli elementi gestiti dal Controller di rete di rete: ospitare computer, servizi di bilanciamento del carico software (SLB) e i computer gateway. 

Questi log includono i registri debug per il cluster di Controller di rete, l'applicazione Controller di rete, i log del gateway, bilanciamento del carico software, la rete virtuale e firewall distribuito. Ogni volta che viene aggiunto un nuovo host o bilanciamento del carico software/gateway per il Controller di rete, la registrazione sia avviata su tali computer. Allo stesso modo, quando un host o bilanciamento del carico software/gateway viene rimosso dal Controller di rete, la registrazione è stata arrestata su tali macchine.

#### <a name="enable-logging"></a>Enable logging (Abilita accesso)

La registrazione è abilitata automaticamente quando si installa il cluster di Controller di rete usando il **installazione NetworkControllerCluster** cmdlet. Per impostazione predefinita, i log vengono raccolti localmente sui nodi del Controller di rete in *%systemdrive%\SDNDiagnostics*. Si tratta **consigliabile** modificare questo percorso per una condivisione file remota (non locali). 

I log dei cluster di Controller di rete vengono archiviate nel *%programData%\Windows fabric\log\traces locale*. È possibile specificare una posizione centralizzata per la raccolta dei log con il **DiagnosticLogLocation** parametro con l'indicazione che si tratta anche essere una condivisione file remota. 

Se si desidera limitare l'accesso a questa posizione, è possibile fornire le credenziali di accesso con il **LogLocationCredential** parametro. Se si specificano le credenziali per accedere al percorso del log, è necessario fornire anche il **CredentialEncryptionCertificate** parametro, che viene usata per crittografare le credenziali archiviate in locale nei nodi del Controller di rete.  

Con le impostazioni predefinite, si consiglia di disporre almeno di 75 GB di spazio disponibile nella posizione centrale e 25 GB su nodi locali (se non si utilizza una posizione centrale) per un cluster di Controller di rete 3 nodi.

#### <a name="change-logging-settings"></a>Modificare le impostazioni di registrazione

È possibile modificare le impostazioni di registrazione in qualsiasi momento usando il ``Set-NetworkControllerDiagnostic`` cmdlet. È possibile modificare le impostazioni seguenti:

- **Percorso log centralizzati**.  È possibile modificare il percorso in cui archiviare tutti i log, con la ``DiagnosticLogLocation`` parametro.  
- **Le credenziali per accedere al percorso di log**.  È possibile modificare le credenziali per accedere al percorso di log, con la ``LogLocationCredential`` parametro.  
- **Spostare la registrazione locale**.  Se è stato specificato posizione centralizzata per archiviare i log, è possibile riportare la registrazione in locale nei nodi del Controller di rete con il ``UseLocalLogLocation`` parametro (scelta non consigliata a causa di requisiti di spazio su disco di grandi dimensioni).  
- **Ambito della registrazione**.  Per impostazione predefinita, tutti i log vengono raccolti. È possibile modificare l'ambito in modo da raccogliere solo i log del cluster di Controller di rete.  
- **Livello di registrazione**.  Il livello di registrazione predefinito è Informational. È possibile modificarlo per errore, avviso o dettagliato.  
- **Log del periodo di tempo di permanenza**.  I log vengono archiviati in modo circolare. Hai 3 giorni di dati di registrazione per impostazione predefinita, se si usa registrazione centralizzata o locale. È possibile modificare questo limite di tempo con **LogTimeLimitInDays** parametro.  
- **Log di dimensioni di Aging**.  Per impostazione predefinita, si avrà un massimo 75 GB di dati di registrazione se si usa la registrazione centralizzata e 25 GB, se si usa registrazione locale. È possibile modificare questo limite con il **LogSizeLimitInMBs** parametro.

#### <a name="collecting-logs-and-traces"></a>Le tracce e la raccolta dei log

Per impostazione predefinita, le distribuzioni di VMM utilizzano registrazione centralizzata per il Controller di rete. Quando si distribuisce il modello di servizio di Controller di rete, viene specificato il percorso della condivisione file per tali log.

Se un percorso di file non è stato specificato, locale la registrazione verrà utilizzata in ogni nodo di Controller di rete con i log salvati sotto C:\Windows\tracing\SDNDiagnostics. Questi log vengono salvati utilizzando la gerarchia seguente:

- CrashDumps
- NCApplicationCrashDumps
- NCApplicationLogs
- PerfCounters
- SDNDiagnostics
- Tracce

Il Controller di rete Usa Service Fabric (Azure). Log di Service Fabric potrebbe essere necessario risolvere alcuni problemi. Questi log sono disponibili in ogni nodo di Controller di rete in C:\ProgramData\Microsoft\Service Fabric.

Se un utente è stato eseguito il _NetworkController Debug_ cmdlet, i log saranno disponibili in ogni host Hyper-V che è stata specificata con una risorsa del server del controller di rete. Questi registri (e le tracce se abilitati) vengono mantenute in C:\NCDiagnostics

### <a name="slb-diagnostics"></a>Diagnostica di bilanciamento del carico software

#### <a name="slbm-fabric-errors-hosting-service-provider-actions"></a>Errori infrastruttura SLBM (azioni di provider di Hosting del servizio)

1.  Verificare che funzioni Software Load Balancer Manager (SLBM) e che i livelli di orchestrazione possono comunicare tra loro: SLBM -> Mux SLB e SLBM -> agenti di Host di bilanciamento del carico software. Eseguire [DumpSlbRestState](https://github.com/Microsoft/SDN/blob/master/Diagnostics/DumpSlbRestState.ps1) da qualsiasi nodo con accesso all'Endpoint REST del Controller di rete.  
2.  Convalidare il *SDNSLBMPerfCounters* Perfmon su una delle macchine virtuali (preferibilmente il Controller di rete nodo primario - Get-NetworkControllerReplica) del nodo di Controller di rete:
    1.  Motore di Load Balancer (LB) è connesso a SLBM? (*SLBM LBEngine configurazioni totale* > 0)  
    2.  SLBM almeno conoscere il proprio endpoint? (*Totale degli endpoint VIP* > = 2)  
    3.  Gli host Hyper-V (DIP) sono connesse a SLBM? (*HP client connessi* = = server num)   
    4.  SLBM è connesso ai MUX? (*Connessi i MUX* == *integra i MUX su SLBM* == *i MUX reporting integro* = # le macchine virtuali i MUX di bilanciamento del carico software).  
3.  Verificare che il router BGP configurato viene completato il peering con il MUX SLB  
    1.  Se si usa RRAS con accesso remoto (ad esempio una macchina virtuale BGP):  
        1.  Get-BgpPeer deve mostrare connesso  
        2.  Get-BgpRouteInformation deve visualizzare almeno una route per SLBM VIP self  
    2.  Se uso fisico Top-of-Rack (ToR) passa come Peer BGP, consultare la documentazione  
        1.  Ad esempio: & Mostra istanza bgp  
4.  Convalidare il *SlbMuxPerfCounters* e *SLBMUX* contatori Perfmon nella macchina virtuale Mux SLB
5.  Controllare lo stato della configurazione e gli intervalli di indirizzi VIP in Manager di servizio di bilanciamento del carico Software Resource  
    1.  Get-NetworkControllerLoadBalancerConfiguration - ConnectionUri < https://<FQDN or IP>| convertto-json-depth 8 (verificare gli intervalli di indirizzi VIP nel pool di IP e verificare self SLBM indirizzi VIP (*LoadBalanacerManagerIPAddress*) e l'eventuale gli indirizzi VIP per il tenant si trovano all'interno di tali intervalli)  
        1. Get-NetworkControllerIpPool - NetworkId "< pubblica/privata indirizzo VIP logico ID risorsa della rete >" - SubnetId "< pubblica/privata indirizzo VIP logico ID risorsa della Subnet >" - ResourceId "<IP Pool Resource Id>" - ConnectionUri $uri | convertto-json-depth 8 
    2.  Debug-NetworkControllerConfigurationState-  

Se uno dei controlli precedenti ha esito negativo, il tenant lo stato di bilanciamento del carico software sarà anche in una modalità di errore.  

**Monitoraggio e aggiornamento**   
Le seguenti informazioni diagnostiche presentate in base, correggere il seguente:  
* Accertarsi che siano connessi multiplexer SLB  
  * Risolvere i problemi di certificato  
  * Correggere i problemi di connettività di rete  
* Verificare che sia correttamente configurati informazioni relative al peering BGP  
* Assicurarsi che ID Host nel Registro di sistema corrispondente di ID istanza del Server nella risorsa del Server (fare riferimento a appendice relativa alla *HostNotConnected* codice di errore)  
* Raccogli log  

#### <a name="slbm-tenant-errors-hosting-service-provider--and-tenant-actions"></a>Errori di Tenant SLBM (Hosting provider e tenant le azioni di servizio)

1.  [Host] Controllare *Debug NetworkControllerConfigurationState* per vedere se tutte le risorse di bilanciamento del carico sono in stato di errore. Provare a ridurre seguendo le azioni di tabella nell'appendice.   
    1.  Verificare che un endpoint di indirizzo VIP sia presente e annuncio route  
    2.  Controllare gli endpoint DIP quante sono stati individuati per l'endpoint di indirizzo VIP  
2.  [Tenant] Convalidare le risorse di bilanciamento del carico siano specificate correttamente  
    1.  Convalidare i DIP endpoint che sono registrati in SLBM ospita le macchine virtuali tenant che corrispondono alle configurazioni di IP del pool di indirizzi Back-end LoadBalancer  
3.  [Host] Se gli endpoint DIP non sono individuati o connesso:   
    1.  Controllare *NetworkControllerConfigurationState di Debug*  
        1.  Convalidare tale controller di rete e dell'agente Host di bilanciamento del carico software è stata eseguita la connessione tramite il coordinatore di eventi di Controller di rete ``netstat -anp tcp |findstr 6640)``  
    2.  Controllare *HostId* nelle *nchostagent* regkey service (riferimento *HostNotConnected* codice di errore nell'appendice) corrispondente istanza della risorsa server corrispondente (Id ``Get-NCServer |convertto-json -depth 8``)  
    3.  Verificare id profilo di porta per la macchina virtuale porta corrisponda all'Id istanza corrispondente macchina virtuale NIC della risorsa   
4.  [Hosting di provider] Raccogliere i log   

#### <a name="slb-mux-tracing"></a>Traccia di Mux SLB

Le informazioni dal MUX servizio di bilanciamento del carico Software possono essere determinate anche tramite il Visualizzatore eventi. 
1. Fare clic su "Mostra analitico e Debug Logs" sotto il menu di visualizzazione del Visualizzatore eventi
2. Passare a "Registri applicazioni e servizi" > Microsoft > Windows > SlbMuxDriver > nel Visualizzatore eventi di traccia 
3. Fare clic con il pulsante destro su di esso e selezionare "Attiva registro"

>[!NOTE]
>È consigliabile avere solo questa registrazione abilitata per un breve periodo di tempo mentre si tenta di riprodurre un problema

### <a name="vfp-and-vswitch-tracing"></a>VFP e traccia vSwitch

Da qualsiasi Hyper-V host che ospita un guest macchina virtuale collegata a una rete virtuale del tenant, si è raccogliere una traccia VFP per determinare dove potrebbero cadere problemi.

```
netsh trace start provider=Microsoft-Windows-Hyper-V-VfpExt overwrite=yes tracefile=vfp.etl report=disable provider=Microsoft-Windows-Hyper-V-VmSwitch 
netsh trace stop
netsh trace convert .\vfp.etl ov=yes 
```
