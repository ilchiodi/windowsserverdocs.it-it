---
title: Ospitare API del servizio rete di calcolo (HCN) per le macchine virtuali e contenitori
description: Host API del servizio di rete di calcolo (HCN) è un'API Win32 rivolte al pubblico che offre l'accesso a livello di piattaforma per gestire le reti virtuali, gli endpoint della rete virtuale e i criteri associati. In questo insieme fornisce connettività e sicurezza per macchine virtuali (VM) e i contenitori in esecuzione in un host Windows.
ms.author: jmesser
author: jmesser81
ms.date: 11/05/2018
ms.openlocfilehash: 50af0dab69633aa6e07ded68e9246aa0315377f0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844982"
---
# <a name="host-compute-network-hcn-service-api-for-vms-and-containers"></a>Ospitare API del servizio rete di calcolo (HCN) per le macchine virtuali e contenitori

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Host API del servizio di rete di calcolo (HCN) è un'API Win32 rivolte al pubblico che offre l'accesso a livello di piattaforma per gestire le reti virtuali, gli endpoint della rete virtuale e i criteri associati. In questo insieme fornisce connettività e sicurezza per macchine virtuali (VM) e i contenitori in esecuzione in un host Windows. 

Gli sviluppatori di usano l'API del servizio HCN per gestire la rete per le macchine virtuali e contenitori nei flussi di lavoro dell'applicazione. L'API HCN è stato progettato per offrire la migliore esperienza per gli sviluppatori. Gli utenti finali non interagiscono direttamente con queste API.  

## <a name="features-of-the-hcn-service-api"></a>Funzionalità dell'API del servizio HCN
-   Implementato come API C ospitato dall'Host Network Service (HNS) nella OnCore/macchina virtuale.

-   Offre la possibilità di creare, modificare, eliminare ed enumerare gli oggetti HCN, ad esempio reti, endpoint, gli spazi dei nomi e i criteri. Eseguono le operazioni sugli handle agli oggetti (ad esempio, un handle di rete) e i punti di controllo sono implementate internamente tramite handle di contesto RPC.

-   Basata sullo schema. La maggior parte delle funzioni dell'API di definiscono l'input e output parametri sotto forma di stringhe contenente gli argomenti della chiamata di funzione come documenti JSON. I documenti JSON sono basati sugli schemi fortemente tipizzati e con controllo delle versioni, tali schemi fanno parte della documentazione pubblica. 

-   Un sottoscrizione o del callback API viene fornito per consentire ai client di registrarsi per le notifiche degli eventi a livello di servizio, ad esempio le creazioni di rete e le eliminazioni.

-   API HCN funziona in Desktop Bridge (noto anche come App Centennial) in esecuzione nei servizi di sistema. L'API di verifica dell'ACL recuperando il token dell'utente dal chiamante.

>[!TIP]
>L'API del servizio HCN è supportato in windows non in primo piano e sfondo attività. 

## <a name="terminology-host-vs-compute"></a>Terminologia: Visual Studio host. Calcolo
Il servizio host di calcolo consente ai chiamanti di creare e gestire macchine virtuali e contenitori in un singolo computer fisico. Il file è denominato per seguire la terminologia del settore. 

- **Host** è ampiamente usato nel settore della virtualizzazione per fare riferimento al sistema operativo che fornisce risorse virtualizzate.

- **Calcolo** viene utilizzato per fare riferimento ai metodi di virtualizzazione che sono più ampi rispetto alle macchine virtuali sola. Host di calcolo del servizio di rete consente ai chiamanti di creare e gestire la rete per macchine virtuali e contenitori in un singolo computer fisico.

## <a name="schema-based-configuration-documents"></a>Documenti di configurazione basata sullo schema
I documenti di configurazione basati su schemi ben definiti è un standard del settore standard nello spazio di virtualizzazione. La maggior parte delle soluzioni di virtualizzazione, ad esempio Docker e Kubernetes, forniscono che API basano su documenti di configurazione. Unità di diverse iniziative di settore, con la partecipazione di Microsoft, un ecosistema per la definizione e la convalida di questi schemi, ad esempio [OpenAPI](https://www.openapis.org/).  Queste iniziative unità anche la standardizzazione delle definizioni di schema specifico per gli schemi utilizzati per i contenitori, ad esempio [iniziativa OCI (Open Container Initiative)](https://www.opencontainers.org/).

La lingua utilizzata per la creazione di documenti di configurazione [JSON](https://tools.ietf.org/html/rfc8259), utilizzabile in combinazione con:
-   Definizioni dello schema che definiscono un modello a oggetti per il documento
-   Convalida del fatto che un documento JSON conforme a uno schema
-   Conversione di documenti JSON in e da rappresentazioni native di questi schemi nel linguaggio di programmazione utilizzato dai chiamanti delle API automatizzata 

Definizioni dello schema usati di frequente vengono [OpenAPI](https://www.openapis.org/) e [dello Schema JSON](http://json-schema.org/), che consente di specificare le definizioni dettagliate delle proprietà in un documento, ad esempio:
-   Il set valido di valori per una proprietà, ad esempio 0-100 per una proprietà che rappresenta una percentuale.
-   La definizione delle enumerazioni, rappresentati come un set di stringhe valide per una proprietà.
-   Un'espressione regolare per il formato previsto di una stringa. 

Come parte della documentazione delle API HCN, ma è previsto pubblicare lo schema dei nostri documenti JSON come una specifica OpenAPI. Basato su questa specifica, le rappresentazioni di specifiche del linguaggio dello schema è possono consentire per l'uso di indipendente dai tipi di oggetti dello schema nel linguaggio di programmazione utilizzato dal client. 

### <a name="example"></a>Esempio 

Di seguito è riportato un esempio di questo flusso di lavoro per l'oggetto che rappresenta un controller SCSI nel documento di configurazione di una macchina virtuale. 

Nel codice sorgente di Windows, definiamo schemi utilizzando file .mars: onecore/vm/dv/net/hns/schema/mars/Schema/HCN.Schema.Network.mars

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
>Il [annotazioni NewIn("2.0") fanno parte del supporto controllo delle versioni per le definizioni dello schema.

Questa definizione interna, si generano le specifiche di OpenAPI per lo schema:

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

È possibile usare gli strumenti, ad esempio [Swagger](https://swagger.io/)per generare rappresentazioni di specifiche del linguaggio dello schema del linguaggio di programmazione utilizzato da un client. Swagger supporta un'ampia gamma di linguaggi, ad esempio C#, Go, Javascript e Python).

- [Esempio di generato C# codice](example-c-sharp.md) per la gestione indirizzi IP a livello superiore & Subnet dell'oggetto.

- [Esempio di codice di Go generato](example-go.md) di alto livello IPAM & Subnet dell'oggetto. Go viene utilizzato da Docker e Kubernetes che sono due i consumer delle API di servizio di rete Host di calcolo. Go offre supporto predefinito per il marshalling dei tipi Go da e verso i documenti JSON.

Oltre alla generazione di codice e la convalida, è possibile usare gli strumenti per semplificare il lavoro con i documenti JSON, vale a dire [Visual Studio Code](https://code.visualstudio.com/Docs/languages/json).

### <a name="top-level-objects-defined-in-the-hcnschemasmars-file"></a>Oggetti di primo livello definiti nel HCN. File schemas.MARS
Come indicato in precedenza, è possibile trovare lo schema del documento per i documenti usati dalle API HCN in un set di file .mars sotto: onecore/vm/dv/net/hns/schema/mars/Schema

Gli oggetti principali sono:
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

- Altre informazioni sul [scenari comuni HCN](hcn-scenarios.md).

- Altre informazioni sul [gestisce il contesto RPC per HCN](hcn-declaration-handles.md).

- Altre informazioni sul [schemi di documento JSON HCN](hcn-json-document-schemas.md).