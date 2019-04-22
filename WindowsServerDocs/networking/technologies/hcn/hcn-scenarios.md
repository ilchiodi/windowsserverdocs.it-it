---
title: Scenari di rete di calcolo (HCN) host
description: ''
ms.author: jmesser
author: jmesser81
ms.date: 11/05/2018
ms.openlocfilehash: 2a81b09c41c3b665d6a026da4b63a6119fbe202d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816302"
---
# <a name="common-scenarios"></a>Scenari comuni

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

## <a name="scenario-hcn"></a>Scenario: HCN 


### <a name="create-an-hcn"></a>Creare un HCN

In questo esempio viene illustrato come usare l'API del servizio rete di calcolo Host per creare una rete di calcolo di Host sull'host che può essere usato per connettersi NIC virtuali alle macchine virtuali o i contenitori.

```C++
using unique_hcn_network = wil::unique_any< 
    HCN_NETWORK, 
    decltype(&HcnCloseNetwork), 
    HcnCloseNetwork>; 


/// Creates a simple HCN Network, waiting synchronously to finish the task
void CreateHcnNetwork() 
{

    unique_hcn_network hcnnetwork;
    wil::unique_cotaskmem_string result;
    std::wstring settings = LR"( 
    { 
        "SchemaVersion": { 
            "Major": 2, 
            "Minor": 0 
        }, 
        "Owner" : "WDAGNetwork", 
        "Flags" : 0,
        "Type"  : 0,
        "Ipams" : [
            {
                "Type" : 0,
                "Subnets" : [
                    {
                        "IpAddressPrefix" : "192.168.1.0/24",
                        "Policies" : [
                            {
                                "Type" : "VLAN",
                                "IsolationId" : 100,
                            }
                        ],
                        "Routes" : [
                            {
                                "NextHop" : "192.168.1.1",
                                "DestinationPrefix" : "0.0.0.0/0",
                            }
                        ]

                    }
                ],
            },
        ],
        "MacPool":  {
            "Ranges" : [
                {
                    "EndMacAddress":  "00-15-5D-52-CF-FF",
                    "StartMacAddress":  "00-15-5D-52-C0-00"
                }
            ],
        },
        "Dns" : {
            "Suffix" : "net.home",
            "ServerList" : ["10.0.0.10"],
        }
    }
    })";    
    
    GUID networkGuid;  
    HRESULT result = CoCreateGuid(&networkGuid);

    result = HcnCreateNetwork(
        networkGuid,              // Unique ID 
        settings.c_str(),      // Compute system settings document 
        &hcnnetwork,
        &result 
        );
    if (FAILED(result)) 
    { 
                    // UnMarshal  the result Json
     // ErrorSchema
        //   {
        //  "ErrorCode" : <uint32>,
        //  "Error" : <string>,
        //  "Success" : <bool>,
       //   }

        // Failed to create network
        THROW_HR(result); 
    }  

    // Close the Handle
    result = HcnCloseNetwork(hcnnetwork.get());

    if (FAILED(result)) 
    {
        // UnMarshal  the result Json
        THROW_HR(result);
    }
        
}
```

### <a name="delete-an-hcn"></a>Eliminare un HCN

Questo esempio viene illustrato come usare l'API del servizio di rete di calcolo Host per aprire ed elimina una rete Host di calcolo 

```C++
    wil::unique_cotaskmem_string errorRecord;
    GUID networkGuid; // Initialize it to appropriate network guid value 
    HRESULT hr = HcnDeleteNetwork(networkGuid, &errorRecord);

    if (FAILED(hr))
    {
        // UnMarshal the result Json
        THROW_HR(hr);
    }
```


### <a name="enumerate-all-networks"></a>Enumerare tutte le reti

In questo esempio viene illustrato come usare API del servizio rete Host di calcolo per enumerare tutte le reti di calcolo di host.

```C++
     wil::unique_cotaskmem_string resultNetworks;
     wil::unique_cotaskmem_string errorRecord;

     // Filter to select Networks based on properties
     std::wstring filter [] = LR"(
     { 
         "Name"  : "WDAG",
     })";
     HRESULT result = HcnEnumerateNetworks(filter.c_str(), &resultNetworks, &errorRecord);
     if (FAILED(result))
     {
         // UnMarshal  the result Json

         THROW_HR(result);
     }
```


### <a name="query-network-properties"></a>Proprietà di rete di query

In questo esempio viene illustrato come usare l'API del servizio rete di calcolo Host cercare le proprietà di rete.

```C++
    unique_hcn_network hcnnetwork;
    wil::unique_cotaskmem_string errorRecord;
    wil::unique_cotaskmem_string properties;
    std:wstring query = LR"(
    { 
        // Future
    })";
    GUID networkGuid; // Initialize it to appropriate network guid value
    HRESULT hr = HcnOpenNetwork(networkGuid, &hcnnetwork, &errorRecord);

    if (FAILED(hr))
    {
        // UnMarshal  the result Json
        THROW_HR(hr);
    }


    hr = HcnQueryNetworkProperties(hcnnetwork.get(), query.c_str(), &properties, &errorRecord);

    if (FAILED(hr))
    {
        // UnMarshal  the result Json
        THROW_HR(hr);
    }
```


## <a name="scenario-hcn-endpoint"></a>Scenario: Endpoint HCN

### <a name="create-an-hcn-endpoint"></a>Creare un endpoint HCN

In questo esempio viene illustrato come usare l'API del servizio rete di calcolo Host per creare un Endpoint di rete Host di calcolo e quindi aggiunta a caldo alla macchina virtuale o un contenitore.

```C++
using unique_hcn_endpoint = wil::unique_any< 
    HCN_ENDPOINT, 
    decltype(&HcnCloseEndpoint), 
    HcnCloseEndpoint>; 

void CreateAndHotAddEndpoint() 
{
    unique_hcn_endpoint hcnendpoint;
    unique_hcn_network hcnnetwork;

    wil::unique_cotaskmem_string errorRecord;


    std::wstring settings[] = LR"( 
    { 
        "SchemaVersion": { 
            "Major": 2, 
            "Minor": 0 
        }, 
        "Owner" : "Sample", 
                   "Flags" : 0,
        "HostComputeNetwork" : "87fdcf16-d210-426e-959d-2a1d4f41d6d3",
        "DNS" : {
            "Suffix" : "net.home",
            "ServerList" : "10.0.0.10",
        }
    })”;
    GUID endpointGuid;  
    HRESULT result = CoCreateGuid(&endpointGuid);

    result = HcnOpenNetwork(
        networkGuid,              // Unique ID 
        &hcnnetwork,
        &errorRecord
        );
    if (FAILED(result)) 
    { 
        // Failed to find network
        THROW_HR(result); 
    }                

    result = HcnCreateEndpoint(
        hcnnetwork.get(),
        endpointGuid,              // Unique ID 
        settings.c_str(),      // Compute system settings document 
        &hcnendpoint,
        &errorRecord
        );

    if (FAILED(result)) 
    { 
        // Failed to create endpoint
        THROW_HR(result); 
    }

    // Can use the sample from HCS API Spec on how to attach this endpoint 
    // to the VM using AddNetworkAdapterToVm   

    result = HcnCloseEndpoint(hcnendpoint.get());

    if (FAILED(result))
    {
        // UnMarshal  the result Json
        THROW_HR(result);
    }
             
}
```


### <a name="delete-an-endpoint"></a>Eliminare un endpoint

In questo esempio viene illustrato come usare l'API del servizio rete di calcolo Host per eliminare un Endpoint di rete Host di calcolo.

```C++
    wil::unique_cotaskmem_string errorRecord;
    GUID endpointGuid; // Initialize it to appropriate endpoint guid value
    HRESULT hr = HcnDeleteEndpoint(endpointGuid, &errorRecord);

    if (FAILED(hr))
    {
        // UnMarshal  the result Json
        THROW_HR(hr);
    }
```


### <a name="modify-and-endpoint"></a>Modifica e l'endpoint

In questo esempio viene illustrato come usare API del servizio rete Host di calcolo per modificare un Endpoint di rete Host di calcolo.

```C++
    unique_hcn_endpoint hcnendpoint;
    GUID endpointGuid; // Initialize it to appropriate endpoint guid value
    
    HRESULT hr = HcnOpenEndpoint(endpointGuid, &hcnendpoint, &errorRecord);

    if (FAILED(hr))
    {
        // UnMarshal  the result Json
        THROW_HR(hr);
    }


    std::wstring  ModifySettingAddPortJson = LR"(
    {
        "ResourceType" : 0,
        "RequestType" : 0,
        "Settings" : {
            "PortName" : "acbd341a-ec08-44c0-9d5e-61af0ee86902"
            "VirtualNicName" : "641313e1-7ae8-4ddb-94e5-3215f3a0b218--87fdcf16-d210-426e-959d-2a1d4f41d6d1"
            "VirtualMachineId" : "641313e1-7ae8-4ddb-94e5-3215f3a0b218"
        }
    }
    )";


    hr = HcnModifyEndpoint(hcnendpoint.get(), ModifySettingAddPortJson.c_str(), &errorRecord);

    if (FAILED(hr))
    {
        // UnMarshal  the result Json
        THROW_HR(hr);
    }
```


### <a name="enumerate-all-enpoints"></a>Enumerare enpoints tutti

In questo esempio viene illustrato come usare API del servizio rete Host di calcolo per enumerare tutti i Host calcolare gli endpoint di rete.

```C++
    wil::unique_cotaskmem_string errorRecord;

    wil::unique_cotaskmem_string resultEndpoints;
    wil::unique_cotaskmem_string errorRecord;

    // Filter to select Endpoint based on properties
    std::wstring filter [] = LR"(
    { 
        "Name"  : "sampleNetwork",
    })";
    result = HcnEnumerateEndpoints(filter.c_str(), &resultEndpoints, &errorRecord);
    if (FAILED(result))
    {
        THROW_HR(result);
    }
```


### <a name="query-endpoint-properties"></a>Proprietà di endpoint di query

In questo esempio viene illustrato come usare API del servizio rete Host di calcolo per eseguire query di tutte le proprietà di un Endpoint di rete Host di calcolo.

```C++
    unique_hcn_endpoint hcnendpoint;
    wil::unique_cotaskmem_string errorRecord;
    GUID endpointGuid; // Initialize it to appropriate endpoint guid value

    HRESULT hr = HcnOpenEndpoint(endpointGuid, &hcnendpoint, &errorRecord);

    if (FAILED(hr))
    {
        // UnMarshal  the result Json
        THROW_HR(hr);
    }



    wil::unique_cotaskmem_string properties;
    std:wstring query = LR"(
    { 
        // Future
    })";

    hr = HcnQueryEndpointProperties(hcnendpoint.get(), query.c_str(), &properties, &errorRecord);

    if (FAILED(hr))
    {
        // UnMarshal  the errorRecord Json
        THROW_HR(hr);
    }
```


## <a name="scenario-hcn-namespace"></a>Scenario: Spazio dei nomi HCN

### <a name="create-an-hcn-namespace"></a>Creare uno spazio dei nomi HCN

Questo esempio viene illustrato come usare l'API del servizio di rete di calcolo Host per creare un Host di calcolo rete Namespace nell'host che può essere utilizzato per la connessione dell'Endpoint e i contenitori.

```C++
using unique_hcn_namespace = wil::unique_any< 
    HCN_NAMESPACE, 
    decltype(&HcnCloseNamespace), 
    HcnCloseNamespace>; 

/// Creates a simple HCN Network, waiting synchronously to finish the task
void CreateHcnNamespace() 
{

    unique_hcn_namespace handle;
    wil::unique_cotaskmem_string errorRecord;
    std::wstring settings = LR"( 
    { 
        "SchemaVersion": { 
            "Major": 2, 
            "Minor": 0 
        }, 
        "Owner" : "Sample", 
        "Flags" : 0,
        "Type" : 0,
    })";    
   
    GUID namespaceGuid;  
    HRESULT result = CoCreateGuid(&namespaceGuid);

    result = HcnCreateNamespace(
        namespaceGuid,              // Unique ID 
        settings.c_str(),      // Compute system settings document 
        &handle,
        &errorRecord
        );
    if (FAILED(result)) 
    { 
                    // UnMarshal  the result Json
     // ErrorSchema
        //   {
        //  "ErrorCode" : <uint32>,
        //  "Error" : <string>,
        //  "Success" : <bool>,
       //   }

        // Failed to create network
        THROW_HR(result); 
    } 

    result = HcnCloseNamespace(handle.get());

    if (FAILED(result))
    {
        // UnMarshal  the result Json
        THROW_HR(result);
    }
         
}
```


### <a name="delete-an-hcn-namespace"></a>Eliminare uno spazio dei nomi HCN

In questo esempio viene illustrato come usare l'API del servizio rete di calcolo Host per eliminare un Host di calcolo rete Namespace.

```C++
    wil::unique_cotaskmem_string errorRecord;
    GUID namespaceGuid; // Initialize it to appropriate namespace guid value
    HRESULT hr = HcnDeleteNamespace(namespaceGuid, &errorRecord);

    if (FAILED(hr))
    {
        // UnMarshal the result Json
        THROW_HR(hr);
    }

```


### <a name="modify-an-hcn-namespace"></a>Modificare uno spazio dei nomi HCN

In questo esempio viene illustrato come usare API del servizio rete Host di calcolo per modificare un Host di calcolo rete Namespace.

```C++
    unique_hcn_namespace handle;
    GUID namespaceGuid; // Initialize it to appropriate namespace guid value
    HRESULT hr = HcnOpenNamespace(namespaceGuid, &handle, &errorRecord);

    if (FAILED(hr))
    {
        // UnMarshal  the result Json
        THROW_HR(hr);
    }

    wil::unique_cotaskmem_string errorRecord;
    static std::wstring  ModifySettingAddEndpointJson = LR"(
    {
        "ResourceType" : 1,
        "ResourceType" : 0,
        "Settings" : {
            "EndpointId" : "87fdcf16-d210-426e-959d-2a1d4f41d6d1"
        }
    }
    )";

    
    hr = HcnModifyNamespace(handle.get(), ModifySettingAddEndpointJson.c_str(), &errorRecord);

    if (FAILED(hr))
    {
        // UnMarshal the result Json
        THROW_HR(hr);
    }
    hr = HcnCloseNamespace(handle.get());

    if (FAILED(hr))
    {
        // UnMarshal  the result Json
        THROW_HR(hr);
    }
    
```


### <a name="enumerate-all-namespaces"></a>Enumerare tutti gli spazi dei nomi

Questo esempio viene illustrato come usare l'API del servizio di rete di calcolo Host per enumerare tutti i Host di calcolo rete spazi dei nomi.

```C++
    wil::unique_cotaskmem_string resultNamespaces;
    wil::unique_cotaskmem_string errorRecord;

    std::wstring filter [] = LR"(
    { 
            // Future       
    })";
    HRESULT hr = HcnEnumerateNamespace(filter.c_str(), &resultNamespaces, &errorRecord);
    if (FAILED(hr))
    {
        // UnMarshal  the result Json
        THROW_HR(hr);
    }

```


### <a name="query-namespace-properties"></a>Proprietà spazio dei nomi di query

In questo esempio viene illustrato come usare l'API del servizio rete di calcolo Host cercare le proprietà di Host di calcolo rete Namespace

```C++
    unique_hcn_namespace handle;
    GUID namespaceGuid; // Initialize it to appropriate namespace guid value
    HRESULT hr = HcnOpenNamespace(namespaceGuid, &handle, &errorRecord);

    if (FAILED(hr))
    {
        // UnMarshal  the result Json
        THROW_HR(hr);
    }


    wil::unique_cotaskmem_string errorRecord;
    wil::unique_cotaskmem_string properties;
    std:wstring query = LR"(
    { 
        // Future
    })";

    HRESULT hr = HcnQueryNamespaceProperties(handle.get(), query.c_str(), &properties, &errorRecord);

    if (FAILED(hr))
    {
        // UnMarshal  the result Json
        THROW_HR(hr);
    }

```


## <a name="scenario-hcn-load-balancer"></a>Scenario: Servizio di bilanciamento del carico HCN

### <a name="create-an-hcn-load-balancer"></a>Creare un servizio di bilanciamento del carico HCN

In questo esempio viene illustrato come usare l'API del servizio rete di calcolo Host per creare un Host di calcolo rete bilanciamento del carico nell'host che può essere utilizzato per bilanciare il carico Endpoint tra calcolo.

```C++
using unique_hcn_loadbalancer = wil::unique_any< 
    HCN_LOADBALANCER, 
    decltype(&HcnCloseLoadBalancer), 
    HcnCloseLoadBalancer>; 

/// Creates a simple HCN LoadBalancer, waiting synchronously to finish the task
void CreateHcnLoadBalancer() 
{

    unique_hcn_loadbalancer handle;
    wil::unique_cotaskmem_string errorRecord;
    std::wstring settings = LR"( 
     { 
        "SchemaVersion": { 
            "Major": 2, 
            "Minor": 0 
        }, 
        "Owner" : "Sample", 
        "HostComputeEndpoints" : [
            "87fdcf16-d210-426e-959d-2a1d4f41d6d1"
        ],
        "VirtualIPs" : [ "10.0.0.10" ],
        "PortMappings" : [
            {
                "Protocol" : 0,
                "InternalPort" : 8080,
                "ExternalPort" : 80,
            }
        ],
        "EnableDirectServerReturn" : true,
        "InternalLoadBalancer" : false,
    }
     )";    
   
    GUID lbGuid;  
    HRESULT result = CoCreateGuid(&lbGuid);


    HRESULT hr = HcnCreateLoadBalancer(
        lbGuid,              // Unique ID 
        settings.c_str(),      // LoadBalancer settings document 
        &handle,
        &errorRecord
        );
    if (FAILED(hr)) 
    { 
                    // UnMarshal  the result Json
     // ErrorSchema
        //   {
        //  "ErrorCode" : <uint32>,
        //  "Error" : <string>,
        //  "Success" : <bool>,
       //   }

        // Failed to create network
        THROW_HR(hr); 
    }

    hr = HcnCloseLoadBalancer(handle.get());

    if (FAILED(hr))
    {
        // UnMarshal  the result Json
        THROW_HR(hr);
    }
          
}
```


### <a name="delete-an-hcn-load-balancer"></a>Eliminare un servizio di bilanciamento del carico HCN

In questo esempio viene illustrato come usare l'API del servizio rete di calcolo Host per eliminare un Host di calcolo rete bilanciamento del carico.

```C++
    wil::unique_cotaskmem_string errorRecord;
    GUID lbGuid; // Initialize it to appropriate loadbalancer guid value
    HRESULT hr = HcnDeleteLoadBalancer(lbGuid , &errorRecord);

    if (FAILED(hr))
    {
        // UnMarshal the result Json
        THROW_HR(hr);
    }
```


### <a name="modify-an-hcn-load-balancer"></a>Modificare un servizio di bilanciamento del carico HCN

In questo esempio viene illustrato come usare API del servizio rete Host di calcolo per modificare un Host di calcolo rete Namespace.

```C++
    unique_hcn_loadbalancer handle;
    GUID lbGuid; // Initialize it to appropriate loadbalancer guid value

    HRESULT hr = HcnOpenLoadBalancer(lbGuid, &handle, &errorRecord);

    if (FAILED(hr))
    {
        // UnMarshal  the result Json
        THROW_HR(hr);
    }

    wil::unique_cotaskmem_string errorRecord;
    static std::wstring  ModifySettingAddEndpointJson = LR"(
    {
        "ResourceType" : 1,
        "ResourceType" : 0,
        "Settings" : {
            "EndpointId" : "87fdcf16-d210-426e-959d-2a1d4f41d6d1"
        }
    }
    )";

    
    hr = HcnModifyLoadBalancer(handle.get(), ModifySettingAddEndpointJson.c_str(), &errorRecord);

    if (FAILED(hr))
    {
        // UnMarshal the result Json
        THROW_HR(hr);
    }
    hr = HcnCloseLoadBalancer(handle.get());

    if (FAILED(hr))
    {
        // UnMarshal  the result Json
        THROW_HR(hr);
    }
```


### <a name="enumerate-all-load-balancers"></a>Enumerare tutti i servizi di bilanciamento del carico

In questo esempio viene illustrato come usare API del servizio rete Host di calcolo per enumerare tutti i Host di calcolo rete bilanciamento del carico.

```C++
    wil::unique_cotaskmem_string resultLoadBalancers;
    wil::unique_cotaskmem_string errorRecord;

    std::wstring filter [] = LR"(
    { 
         // Future

    })";
    HRESULT result = HcnEnumerateLoadBalancers(filter.c_str(), & resultLoadbalancers, &errorRecord);
    if (FAILED(result))
    {
            // UnMarshal  the result Json

            THROW_HR(result);
    }
```


### <a name="query-load-balancer-properties"></a>Proprietà del servizio di bilanciamento carico di query

In questo esempio viene illustrato come usare API del servizio rete Host di calcolo per le proprietà di Host di calcolo rete LoadBalancer della query.

```C++
    unique_hcn_loadbalancer handle;
    GUID lbGuid; // Initialize it to appropriate loadbalancer guid value

    HRESULT hr = HcnOpenLoadBalancer(lbGuid, &handle, &errorRecord);

    if (FAILED(hr))
    {
        // UnMarshal  the result Json
        THROW_HR(hr);
    }


    wil::unique_cotaskmem_string errorRecord;
    wil::unique_cotaskmem_string properties;
    std:wstring query = LR"(
    { 
        "ID"  : "",
        "Type" : 0,
    })";

    hr = HcnQueryNProperties(handle.get(), query.c_str(), &properties, &errorRecord);

    if (FAILED(hr))
    {
        // UnMarshal  the result Json
        THROW_HR(hr);
    }
```


## <a name="scenario-hcn-notifications"></a>Scenario: Notifiche HCN

### <a name="register-and-unregister-service-wide-notifications"></a>Registrare e annullare la registrazione delle notifiche a livello di servizio

Questo esempio viene illustrato come usare l'API di rete del servizio Host di calcolo per registrare e annullare la registrazione per le notifiche a livello di servizio. Ciò consente al chiamante di ricevere una notifica (tramite la funzione di callback sono specificati durante la registrazione) ogni volta che si è verificata un'operazione a livello di servizio, ad esempio un nuovo evento di creazione della rete.

```C++
using unique_hcn_callback = wil::unique_any< 
    HCN_CALLBACK, 
    decltype(&HcnUnregisterServiceCallback), 
    HcnUnregisterServiceCallback>; 

// Callback handle returned by registration api. Kept at 
// global or module scope as it will automatically be
// unregistered when it goes out of scope.
unique_hcn_callback g_Callback;

// Event notification callback function.
void
CALLBACK
ServiceCallback(
    DWORD   NotificationType,
    void*   Context,
    HRESULT NotificationStatus,
    PCWSTR  NotificationData)
{
    // Optional client context
    UNREFERENCED_PARAMETER(context);
    // Reserved for future use
    UNREFERENCED_PARAMETER(NotificationStatus);

    switch (NotificationType)
    {
        case HcnNotificationNetworkCreate:
            // TODO: UnMarshal the NotificationData
            //
            // // Notification
            // {
            //     "ID" : Guid,
            //     "Flags" : <uint32>,
            // };
            break;

        case HcnNotificationNetworkDelete:
            // TODO: UnMarshal the NotificationData
            break;

        Default:
            // TODO: handle other events.
            break;
    }
}

/// Register for service-wide notifications
void RegisterForServiceNotifications() 
{
    THROW_IF_FAILED(HcnRegisterServiceCallback(
        ServiceCallback,
        nullptr,
        &g_Callback));        
}

/// Unregister from service-wide notifications
void UnregisterForServiceNotifications()
{
    // As this is a unique_hcn_callback, this will cause HcnUnregisterServiceCallback to be invoked
    g_Callback.reset();

}
```

## <a name="next-steps"></a>Passaggi successivi

- Altre informazioni sul [gestisce il contesto RPC per HCN](hcn-declaration-handles.md).

- Altre informazioni sul [schemi di documento JSON HCN](hcn-json-document-schemas.md).