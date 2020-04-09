---
title: API del servizio HCN (host Compute Network) per VM e contenitori
description: L'API del servizio HCN (host Compute Network) è un'API Win32 pubblica che fornisce l'accesso a livello di piattaforma per gestire le reti virtuali, gli endpoint della rete virtuale e i criteri associati. Questa funzionalità fornisce connettività e sicurezza per le macchine virtuali e i contenitori in esecuzione in un host Windows.
ms.author: jmesser
author: jmesser81
ms.prod: windows-server
ms.date: 11/05/2018
ms.openlocfilehash: 4afde574802bd63db8ea8ca8db9f5daf1a53dc93
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859844"
---
# <a name="host-compute-network-hcn-service-api-for-vms-and-containers"></a>API del servizio HCN (host Compute Network) per VM e contenitori

>Si applica a: Windows Server (canale semestrale), Windows Server 2019

L'API del servizio HCN (host Compute Network) è un'API Win32 pubblica che fornisce l'accesso a livello di piattaforma per gestire le reti virtuali, gli endpoint della rete virtuale e i criteri associati. Questa funzionalità fornisce connettività e sicurezza per le macchine virtuali e i contenitori in esecuzione in un host Windows. 

Gli sviluppatori usano l'API del servizio HCN per gestire la rete per le macchine virtuali e i contenitori nei flussi di lavoro dell'applicazione. L'API HCN è stata progettata per offrire la migliore esperienza per gli sviluppatori. Gli utenti finali non interagiscono direttamente con queste API.  

## <a name="features-of-the-hcn-service-api"></a>Funzionalità dell'API del servizio HCN
-    Implementata come API C ospitata dal servizio di rete host (HNS) su Oncore/VM.

-    Offre la possibilità di creare, modificare, eliminare ed enumerare oggetti HCN, ad esempio reti, endpoint, spazi dei nomi e criteri. Le operazioni vengono eseguite sugli handle per gli oggetti (ad esempio, un handle di rete) e internamente questi handle vengono implementati usando gli handle del contesto RPC.

-    Basato su Schema. La maggior parte delle funzioni dell'API definisce parametri di input e output come stringhe contenenti gli argomenti della chiamata di funzione come documenti JSON. I documenti JSON sono basati su schemi fortemente tipizzati e con controllo delle versioni, questi schemi fanno parte della documentazione pubblica. 

-    Viene fornita un'API di sottoscrizione/callback per consentire ai client di eseguire la registrazione per le notifiche degli eventi a livello di servizio, ad esempio la creazione e l'eliminazione di una rete.

-    L'API HCN funziona in desktop Bridge (noto anche come Centennial) app in esecuzione nei servizi di sistema. L'API controlla l'ACL recuperando il token utente dal chiamante.

>[!TIP]
>L'API del servizio HCN è supportata nelle attività in background e nelle finestre non in primo piano. 

## <a name="terminology-host-vs-compute"></a>Terminologia: host e calcolo
Il servizio di calcolo host consente ai chiamanti di creare e gestire le macchine virtuali e i contenitori in un singolo computer fisico. Viene denominato per seguire la terminologia del settore. 

- L' **host** è ampiamente usato nel settore della virtualizzazione per fare riferimento al sistema operativo che fornisce le risorse virtualizzate.

- Il **calcolo** viene usato per fare riferimento ai metodi di virtualizzazione più ampi delle sole macchine virtuali. Il servizio di rete di calcolo host consente ai chiamanti di creare e gestire la rete per le macchine virtuali e il contenitore in un singolo computer fisico.

## <a name="schema-based-configuration-documents"></a>Documenti di configurazione basati su schema
I documenti di configurazione basati su schemi ben definiti sono uno standard del settore consolidato nello spazio di virtualizzazione. La maggior parte delle soluzioni di virtualizzazione, ad esempio Docker e Kubernetes, fornisce API basate sui documenti di configurazione. Diverse iniziative di settore, con la partecipazione di Microsoft, guidano un ecosistema per la definizione e la convalida di questi schemi, ad esempio [openapi](https://www.openapis.org/).  Queste iniziative guidano inoltre la standardizzazione di definizioni dello schema specifiche per gli schemi utilizzati per i contenitori, ad esempio [Open Container Initiative (OCI)](https://www.opencontainers.org/).

Il linguaggio usato per la creazione di documenti di configurazione è [JSON](https://tools.ietf.org/html/rfc8259), che è possibile usare in combinazione con:
-    Definizioni dello schema che definiscono un modello a oggetti per il documento
-    Convalida dell'eventuale conformità di un documento JSON a uno schema
-    Conversione automatica dei documenti JSON da e verso rappresentazioni native di questi schemi nei linguaggi di programmazione usati dai chiamanti delle API 

Le definizioni dello schema usate di frequente sono [openapi](https://www.openapis.org/) e lo [schema JSON](http://json-schema.org/), che consente di specificare le definizioni dettagliate delle proprietà in un documento, ad esempio:
-    Set valido di valori per una proprietà, ad esempio 0-100, per una proprietà che rappresenta una percentuale.
-    Definizione di enumerazioni, rappresentate come un set di stringhe valide per una proprietà.
-    Espressione regolare per il formato previsto di una stringa. 

Nell'ambito della documentazione delle API di HCN, si prevede di pubblicare lo schema dei documenti JSON come specifica OpenAPI. In base a questa specifica, le rappresentazioni specifiche della lingua dello schema possono consentire l'uso indipendente dai tipi degli oggetti dello schema nel linguaggio di programmazione utilizzato dal client. 

### <a name="example"></a>Esempio 

Di seguito è riportato un esempio di questo flusso di lavoro per l'oggetto che rappresenta un controller SCSI nel documento di configurazione di una macchina virtuale. 

Nel codice sorgente di Windows vengono definiti gli schemi usando i file Mars: onecore/VM/DV/NET/HNS/schema/Mars/schema/HCN. Schema. Network. Mars

```
enum IpamType
{
    [NewIn("2.0")] Static,
    [NewIn("2.0")] Dhcp,
};

class Ipam
{
    // Type : dhcp
    [NewIn("2.0"),OmitEmpty] IpamType   Type;
    [NewIn("2.0"),OmitEmpty] Subnet     Subnets[];
};

class Subnet : HCN.Schema.Common.Base
{
    [NewIn("2.0"),OmitEmpty] string         IpAddressPrefix;
    [NewIn("2.0"),OmitEmpty] SubnetPolicy   Policies[];
    [NewIn("2.0"),OmitEmpty] Route          Routes[];
};


enum SubnetPolicyType
{
    [NewIn("2.0")] VLAN
};

class SubnetPolicy
{
    [NewIn("2.0"),OmitEmpty] SubnetPolicyType                 Type;
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Common.PolicySettings Data;
};

class PolicySettings
{
    [NewIn("2.0"),OmitEmpty]  string      Name;
};

class VlanPolicy : HCN.Schema.Common.PolicySettings 
{
    [NewIn("2.0")] uint32 IsolationId;
};

class Route 
{
    [NewIn("2.0"),OmitEmpty] string NextHop;
    [NewIn("2.0"),OmitEmpty] string DestinationPrefix;
    [NewIn("2.0"),OmitEmpty] uint16 Metric;
};

```

>[!TIP]
>Le annotazioni [NewIn ("2.0") fanno parte del supporto del controllo delle versioni per le definizioni dello schema.

Da questa definizione interna vengono generate le specifiche OpenAPI per lo schema:

```
{ 
    "swagger" : "2.0", 
    "info" : { 
       "version" : "2.1", 
       "title" : "HCN API" 
    },
    "definitions": {        
        "Ipam": {
            "type": "object",
            "properties": {
                "Type": {
                    "type": "string",
                    "enum": [
                        "Static",
                        "Dhcp"
                    ],
                    "description": " Type : dhcp"
                },
                "Subnets": {
                    "type": "array",
                    "items": {
                        "$ref": "#/definitions/Subnet"
                    }
                }
            }
        },
        "Subnet": {
            "type": "object",
            "properties": {
                "ID": {
                    "type": "string",
                    "pattern": "^[0-9A-Fa-f]{8}-([0-9A-Fa-f]{4}-){3}[0-9A-Fa-f]{12}$"
                },                
                "IpAddressPrefix": {
                    "type": "string"
                },
                "Policies": {
                    "type": "array",
                    "items": {
                        "$ref": "#/definitions/SubnetPolicy"
                    }
                },
                "Routes": {
                    "type": "array",
                    "items": {
                        "$ref": "#/definitions/Route"
                    }
                }
            }
        },
        "SubnetPolicy": {
            "type": "object",
            "properties": {
                "Type": {
                    "type": "string",
                    "enum": [
                        "VLAN",
                        "VSID"
                    ]
                },
                "Data": {
                    "$ref": "#/definitions/PolicySettings"
                }
            }
        },
        "PolicySettings": {
            "type": "object",
            "properties": {
                "Name": {
                    "type": "string"
                }
            }
        },                      
        "VlanPolicy": {
            "type": "object",
            "properties": {
                "Name": {
                    "type": "string"
                },
                "IsolationId": {
                    "type": "integer",
                    "format": "uint32"
                }
            }
        },
        "Route": {
            "type": "object",
            "properties": {
                "NextHop": {
                    "type": "string"
                },
                "DestinationPrefix": {
                    "type": "string"
                },
                "Metric": {
                    "type": "integer",
                    "format": "uint16"
                }
            }
        }        
    }
} 
```

È possibile utilizzare strumenti, ad esempio [spavalderia](https://swagger.io/), per generare rappresentazioni specifiche della lingua del linguaggio di programmazione dello schema utilizzato da un client. Spavalderia supporta un'ampia gamma di linguaggi C#, ad esempio, go, JavaScript e Python.

- [Esempio di codice C# generato](example-c-sharp.md) per l'oggetto & subnet di gestione indirizzi IP di primo livello.

- [Esempio di codice go generato](example-go.md) per l'oggetto & subnet di gestione indirizzi IP di primo livello. Go viene usato da Docker e Kubernetes che sono due dei consumer delle API del servizio di rete di calcolo host. Go dispone del supporto incorporato per il marshalling dei tipi go da e verso i documenti JSON.

Oltre alla generazione e alla convalida del codice, è possibile usare gli strumenti per semplificare il lavoro con i documenti JSON, ovvero [Visual Studio Code](https://code.visualstudio.com/Docs/languages/json).

### <a name="top-level-objects-defined-in-the-hcnschemasmars-file"></a>Oggetti di primo livello definiti in HCN. File schemas. Mars
Come indicato in precedenza, è possibile trovare lo schema del documento per i documenti usati dalle API HCN in un set di file con estensione Mars in: onecore/VM/DV/NET/HNS/schema/Mars/schema

Gli oggetti di primo livello sono:
- [HostComputeNetwork](hcn-scenarios.md#scenario-hcn)
- [HostComputeEndpoint](hcn-scenarios.md#scenario-hcn-endpoint)
- [HostComputeNamespace](hcn-scenarios.md#scenario-hcn-namespace)
- [HostComputeLoadBalancer](hcn-scenarios.md#scenario-hcn-load-balancer)

```
class HostComputeNetwork : HCN.Schema.Common.Base
{
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.NetworkMode          Type;
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.NetworkPolicy        Policies[];
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.MacPool              MacPool;
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.DNS                  Dns;
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.Ipam                 Ipams[];
};

class HostComputeEndpoint : HCN.Schema.Common.Base
{
    [NewIn("2.0"),OmitEmpty] string                                     HostComputeNetwork;
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.Endpoint.EndpointPolicy Policies[];
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.Endpoint.IpConfig       IpConfigurations[];
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.DNS                     Dns;
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.Route                   Routes[];
    [NewIn("2.0"),OmitEmpty] string                                     MacAddress;
};

class HostComputeNamespace : HCN.Schema.Common.Base
{
    [NewIn("2.0"),OmitEmpty] uint32                                    NamespaceId;
    [NewIn("2.0"),OmitEmpty] Guid                                      NamespaceGuid;
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Namespace.NamespaceType        Type;
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Namespace.NamespaceResource    Resources[];
};

class HostComputeLoadBalancer : HCN.Schema.Common.Base
{
    [NewIn("2.0"), OmitEmpty] string                                               HostComputeEndpoints[]; 
    [NewIn("2.0"), OmitEmpty] string                                               VirtualIPs[]; 
    [NewIn("2.0"), OmitEmpty] HCN.Schema.Network.Endpoint.Policy.PortMappingPolicy PortMappings[]; 
    [NewIn("2.0"), OmitEmpty] HCN.Schema.LoadBalancer.LoadBalancerPolicy           Policies[];
};
```

## <a name="next-steps"></a>Passaggi successivi

- Altre informazioni sugli [scenari comuni di HCN](hcn-scenarios.md).

- Altre informazioni sugli [handle del contesto RPC per HCN](hcn-declaration-handles.md).

- Altre informazioni sugli [schemi di documento JSON di HCN](hcn-json-document-schemas.md).