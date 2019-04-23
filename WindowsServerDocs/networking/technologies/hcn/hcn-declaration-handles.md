---
title: Handle di contesto RPC per HCN
description: ''
ms.author: jmesser
author: jmesser81
ms.date: 11/05/2018
ms.openlocfilehash: 48e4626f52ef7af19da6d3e0bb28799665498fe5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884852"
---
# <a name="rpc-context-handles-for-hcn"></a>Handle di contesto RPC per HCN

>Si applica a: Windows Server (canale semestrale), Windows Server 2016


## <a name="hcnnetwork"></a>HCN_Network

Una rete HCN è un'entità che viene usata per rappresentare un host di calcolo di rete alle risorse di sistema associate e i relativi criteri. Ad esempio, una rete HCN sarà in genere costituito da un set di metadati (ad esempio id, nome, tipo), un commutatore virtuale, una scheda di rete virtuale host (che funge da gateway predefinito per la rete), un'istanza NAT (se richiesto dal tipo di rete), un set di subnet e pool MAC , e tutti i criteri a livello di rete per essere applicati (ad esempio ACL).

Le entità rete HCN vengono rappresentate usando handle contesto HCN_NETWORK RPC.

```

/// Handle to an operation
DECLARE_HANDLE(HCN_NETWORK);

/// Return a list of existing Networks
///
/// \param  Query          Optionally specifies a JSON document for a query
///                        containing properties of the specific Networks to
///                        return. By default, all Networks are returned.
/// \retval Networks       Receives a JSON document with the list of Networks.
/// \retval ErrorRecord    Optional, receives a JSON document on failure with extended result
///                        information. The caller must release the buffer using
///                        CoTaskMemFree.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnEnumerateNetworks(
    _In_ PCWSTR Query,
    _Outptr_ PWSTR* Networks,
    _Outptr_opt_ PWSTR* ErrorRecord
    );
/// Create a Network
///
/// \param  Id             Specifies the unique ID for the new Network.
/// \param  Settings       JSON document specifying the settings of the new Network.
/// \retval Network        Receives a handle to the new Network.
/// \retval ErrorRecord    Optional, receives a JSON document on failure with extended result
///                        information. The caller must release the buffer using
///                        CoTaskMemFree.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnCreateNetwork(
    _In_ REFGUID Id,
    _In_ PCWSTR Settings,
    _Out_ PHCN_NETWORK Network,
    _Outptr_opt_ PWSTR* ErrorRecord
    );
/// Opens a handle to an existing Network.
///
/// \param  Id             Unique ID of the existing Network.
/// \retval Network        Receives a handle to the Network.
/// \retval ErrorRecord    Optional, receives a JSON document on failure with extended result
///                        information. The caller must release the buffer using
///                        CoTaskMemFree.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnOpenNetwork(
    _In_ REFGUID Id,
    _Out_ PHCN_NETWORK Network,
    _Outptr_opt_ PWSTR* ErrorRecord
    );
/// Modify the settings of a Network
///
/// \param  Network        Handle to a Network.
/// \param  Settings       JSON document specifying the new settings of the Network.
/// \retval ErrorRecord    Optional, receives a JSON document on failure with extended result
///                        information. The caller must release the buffer using
///                        CoTaskMemFree.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnModifyNetwork(
    _In_ HCN_NETWORK Network,
    _In_ PCWSTR Settings,
    _Outptr_opt_ PWSTR* ErrorRecord
    );

/// Query Network properties
///
/// \param  Network        Handle to a Network.
/// \param  Query          Optionally specifies a JSON document for a query
///                        containing specific properties of the Network
///                        return. By default all properties are returned.
/// \retval Properties     Receives a JSON document with Network properties.
/// \retval ErrorRecord    Optional, receives a JSON document on failure with extended result
///                        information. The caller must release the buffer using
///                        CoTaskMemFree.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnQueryNetworkProperties(
    _In_ HCN_NETWORK Network,
    _In_ PCWSTR Query,
    _Outptr_ PWSTR* Properties,
    _Outptr_opt_ PWSTR* ErrorRecord
    );
/// Delete a Network
///
/// \param  Id             Unique ID of the existing Network.
/// \retval ErrorRecord    Optional, receives a JSON document on failure with extended result
///                        information. The caller must release the buffer using
///                        CoTaskMemFree.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnDeleteNetwork(
    _In_ REFGUID Id,
    _Outptr_opt_ PWSTR* ErrorRecord
    );
/// Close a handle to a Network
///
/// \param  Network        Handle to a Network.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnCloseNetwork(
    _In_ HCN_NETWORK Network
    ); 
```

## <a name="hcnendpoint"></a>HCN_Endpoint

Un Endpoint HCN è un'entità che viene usata per rappresentare un endpoint IP in una rete HCN risorse di sistema associate e i relativi criteri. Ad esempio, un endpoint HCN sarà in genere costituito da un set di metadati (ad esempio id, nome, id di rete principale), la propria identità di rete (ad esempio, indirizzo IP, indirizzo MAC) e qualsiasi endpoint specifici criteri a essere applicato (ad esempio ACL, route).
Le entità HCN Endpoint vengono rappresentate usando handle contesto HCN_ENDPOINT RPC.

```

/// Handle to an operation
DECLARE_HANDLE(HCN_ENDPOINT);

/// Return a list of existing Endpoints
///
/// \param  Query          Optionally specifies a JSON document for a query
///                        containing properties of the specific Endpoints to
///                        return. By default all Endpoints are returned.
/// \retval Endpoints      Receives a JSON document with the list of Endpoints.
/// \retval ErrorRecord    Optional, receives a JSON document on failure with extended result
///                        information. The caller must release the buffer using
///                        CoTaskMemFree.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnEnumerateEndpoints(
    _In_ PCWSTR Query,
    _Outptr_ PWSTR* Endpoints,
    _Outptr_opt_ PWSTR* ErrorRecord
    );
/// Create an Endpoint
///
/// \param  Id             Specifies the unique ID for the new Endpoint.
/// \param  Network        Handle to the network on which endpoint is to be created.
/// \param  Settings       JSON document specifying the settings of the new Endpoint.
/// \retval Endpoint       Receives a handle to the new Endpoint.
/// \retval ErrorRecord    Optional, receives a JSON document on failure with extended result
///                        information. The caller must release the buffer using
///                        CoTaskMemFree.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnCreateEndpoint(
    _In_ HCN_NETWORK Network,
    _In_ REFGUID Id,
    _In_ PCWSTR Settings,
    _Out_ PHCN_ENDPOINT Endpoint,
    _Outptr_opt_ PWSTR* ErrorRecord
    );
/// Opens a handle to an existing Endpoint.
///
/// \param  Id             Unique ID of the existing Endpoint.
/// \retval Endpoint       Receives a handle to the Endpoint.
/// \retval ErrorRecord    Optional, receives a JSON document on failure with extended result
///                        information. The caller must release the buffer using
///                        CoTaskMemFree.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnOpenEndpoint(
    _In_ REFGUID Id,
    _Out_ PHCN_ENDPOINT Endpoint,
    _Outptr_opt_ PWSTR* ErrorRecord
    );
/// Modify the settings of an Endpoint
///
/// \param  Endpoint       Handle to an Endpoint.
/// \param  Settings       JSON document specifying the new settings of the Endpoint.
/// \retval ErrorRecord    Optional, receives a JSON document on failure with extended result
///                        information. The caller must release the buffer using
///                        CoTaskMemFree.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnModifyEndpoint(
    _In_ HCN_ENDPOINT Endpoint,
    _In_ PCWSTR Settings,
    _Outptr_opt_ PWSTR* ErrorRecord
    );
/// Query Endpoint properties
///
/// \param  Endpoint       Handle to an Endpoint.
/// \param  Query          Optionally specifies a JSON document for a query
///                        containing specific properties of the Endpoint
///                        return. By default all properties are returned.
/// \retval Properties     Receives a JSON document with Endpoint properties.
/// \retval ErrorRecord    Optional, receives a JSON document on failure with extended result
///                        information. The caller must release the buffer using
///                        CoTaskMemFree.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnQueryEndpointProperties(
    _In_ HCN_ENDPOINT Endpoint,
    _In_ PCWSTR Query,
    _Outptr_ PWSTR* Properties,
    _Outptr_opt_ PWSTR* ErrorRecord
    );
/// Delete an Endpoint
///
/// \param  Id             Unique ID of the existing Endpoint.
/// \retval ErrorRecord    Optional, receives a JSON document on failure with extended result
///                        information. The caller must release the buffer using
///                        CoTaskMemFree.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnDeleteEndpoint(
    _In_ REFGUID Id,
    _Outptr_opt_ PWSTR* ErrorRecord
    );
/// Close a handle to an Endpoint
///
/// \param  Endpoint       Handle to an Endpoint.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnCloseEndpoint(
    _In_ HCN_ENDPOINT Endpoint
    );
 
```

## <a name="hcnnamespace"></a>HCN_Namespace

Un Namespace HCN è un'entità che viene usata per rappresentare uno spazio dei nomi di host calcolo rete. Gli spazi dei nomi consentono di disporre di ambienti di rete in un singolo host, in cui ogni spazio dei nomi ha un proprio interfacce di rete e un tabella di routing, isolati separati da altri spazi dei nomi.

Le entità HCN Namespace vengono rappresentate usando handle contesto HCN_NAMESPACE RPC.

```
/// Handle to an operation
DECLARE_HANDLE(HCN_NAMESPACE);

/// Return a list of existing Namespaces
///
/// \param  Query          Optionally specifies a JSON document for a query
///                        containing properties of the specific Namespaces to
///                        return. By default all Namespaces are returned.
/// \retval Namespaces     Receives a JSON document with the list of Namespaces.
/// \retval ErrorRecord    Optional, receives a JSON document on failure with extended result
///                        information. The caller must release the buffer using
///                        CoTaskMemFree.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnEnumerateNamespaces(
    _In_ PCWSTR Query,
    _Outptr_ PWSTR* Namespaces,
    _Outptr_opt_ PWSTR* ErrorRecord
    );

/// Create a Namespace
///
/// \param  Id             Specifies the unique ID for the new Namespace.
/// \param  Settings       JSON document specifying the settings of the new Namespace.
/// \retval Namespace      Receives a handle to the new Namespace.
/// \retval ErrorRecord    Optional, receives a JSON document on failure with extended result
///                        information. The caller must release the buffer using
///                        CoTaskMemFree.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnCreateNamespace(
    _In_ REFGUID Id,
    _In_ PCWSTR Settings,
    _Out_ PHCN_NAMESPACE Namespace,
    _Outptr_opt_ PWSTR* ErrorRecord
    );

/// Opens a handle to an existing Namespace.
///
/// \param  Id             Unique ID of the existing Namespace.
/// \retval Namespace      Receives a handle to the Namespace.
/// \retval ErrorRecord    Optional, receives a JSON document on failure with extended result
///                        information. The caller must release the buffer using
///                        CoTaskMemFree.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnOpenNamespace(
    _In_ REFGUID Id,
    _Out_ PHCN_NAMESPACE Namespace,
    _Outptr_opt_ PWSTR* ErrorRecord
    );
/// Modify the settings of a Namespace
///
/// \param  Namespace      Handle to a Namespace.
/// \param  Settings       JSON document specifying the new settings of the Namespace.
/// \retval ErrorRecord    Optional, receives a JSON document on failure with extended result
///                        information. The caller must release the buffer using
///                        CoTaskMemFree.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnModifyNamespace(
    _In_ HCN_NAMESPACE Namespace,
    _In_ PCWSTR Settings,
    _Outptr_opt_ PWSTR* ErrorRecord
    );

/// Query Namespace properties
///
/// \param  Namespace      Handle to a Namespace.
/// \param  Query          Optionally specifies a JSON document for a query
///                        containing specific properties of the Namespace
///                        return. By default all properties are returned.
/// \retval Properties     Receives a JSON document with Namespace properties.
/// \retval ErrorRecord    Optional, receives a JSON document on failure with extended result
///                        information. The caller must release the buffer using
///                        CoTaskMemFree.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnQueryNamespaceProperties(
    _In_ HCN_NAMESPACE Namespace,
    _In_ PCWSTR Query,
    _Outptr_ PWSTR* Properties,
    _Outptr_opt_ PWSTR* ErrorRecord
    );
/// Delete a Namespace
///
/// \param  Id             Unique ID of the existing Namespace.
/// \retval ErrorRecord    Optional, receives a JSON document on failure with extended result
///                        information. The caller must release the buffer using
///                        CoTaskMemFree.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnDeleteNamespace(
    _In_ REFGUID Id,
    _Outptr_opt_ PWSTR* ErrorRecord
    );

/// Close a handle to a Namespace
///
/// \param  Namespace      Handle to a Namespace.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnCloseNamespace(
    _In_ HCN_NAMESPACE Namespace
    );

```

## <a name="hcnloadbalancer"></a>HCN_LoadBalancer

Un LoadBalancer HCN è un'entità che viene usata per rappresentare un loadbalancer di rete host calcolo. LoadBalancers consentono di calcolare gli endpoint di rete host con bilanciamento di carico.
Le entità servizio di bilanciamento del carico HCN vengono rappresentate usando handle contesto HCN_LOADBALANCER RPC.

```
/// Handle to an operation
DECLARE_HANDLE(HCN_LOADBALANCER);

//////
/// LoadBalancer Methods

/// Return a list of existing LoadBalancers
///
/// \param  Query          Optionally specifies a JSON document for a query
///                        containing properties of the specific LoadBalancers to
///                        return. By default all LoadBalancers are returned.
/// \retval LoadBalancers    Receives a JSON document with the list of LoadBalancers.
/// \retval ErrorRecord    Optional, receives a JSON document with extended errorCode
///                        information. The caller must release the buffer using
///                        CoTaskMemFree.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnEnumerateLoadBalancers(
    _In_ PCWSTR Query,
    _Outptr_ PWSTR* LoadBalancer,
    _Outptr_opt_ PWSTR* ErrorRecord
    );

/// Create a LoadBalancer
///
/// \param  Id             Specifies the unique ID for the new LoadBalancer.
/// \param  Settings       JSON document specifying the settings of the new LoadBalancer.
/// \retval LoadBalancer     Receives a handle to the new LoadBalancer.
/// \retval ErrorRecord    Optional, receives a JSON document with extended errorCode
///                        information. The caller must release the buffer using
///                        CoTaskMemFree.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnCreateLoadBalancer(
    _In_ REFGUID Id,
    _In_ PCWSTR Settings,
    _Out_ PHCN_LOADBALANCER LoadBalancer,
    _Outptr_opt_ PWSTR* ErrorRecord
    );

/// Opens a handle to an existing LoadBalancer.
///
/// \param  Id             Unique ID of the existing LoadBalancer.
/// \retval LoadBalancer     Receives a handle to the LoadBalancer.
/// \retval ErrorRecord    Optional, receives a JSON document with extended errorCode
///                        information. The caller must release the buffer using
///                        CoTaskMemFree.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnOpenLoadBalancer(
    _In_ REFGUID Id,
    _Out_ PHCN_LOADBALANCER LoadBalancer,
    _Outptr_opt_ PWSTR* ErrorRecord
    );

/// Modify the settings of a PolcyList
///
/// \param  PolcyList      Handle to a PolcyList.
/// \param  Settings       JSON document specifying the new settings of the PolcyList.
/// \retval ErrorRecord    Optional, receives a JSON document with extended errorCode
///                        information. The caller must release the buffer using
///                        CoTaskMemFree.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnModifyLoadBalancer(
    _In_ HCN_LOADBALANCER LoadBalancer,
    _In_ PCWSTR Settings,
    _Outptr_opt_ PWSTR* ErrorRecord
    );

/// Query LoadBalancer properties
///
/// \param  LoadBalancer     Handle to a LoadBalancer.
/// \param  Query          Optionally specifies a JSON document for a query
///                        containing specific properties of the LoadBalancer
///                        return. By default all properties are returned.
/// \retval Properties     Receives a JSON document with LoadBalancer properties.
/// \retval ErrorRecord    Optional, receives a JSON document with extended errorCode
///                        information. The caller must release the buffer using
///                        CoTaskMemFree.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnQueryLoadBalancerProperties(
    _In_ HCN_LOADBALANCER LoadBalancer,
    _In_ PCWSTR Query,
    _Outptr_ PWSTR* Properties,
    _Outptr_opt_ PWSTR* ErrorRecord
    );

/// Delete a LoadBalancer
///
/// \param  Id             Unique ID of the existing LoadBalancer.
/// \retval ErrorRecord    Optional, receives a JSON document with extended errorCode
///                        information. The caller must release the buffer using
///                        CoTaskMemFree.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnDeleteLoadBalancer(
    _In_ REFGUID Id,
    _Outptr_opt_ PWSTR* ErrorRecord
    );

/// Close a handle to a LoadBalancer
///
/// \param  LoadBalancer     Handle to a LoadBalancer.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnCloseLoadBalancer(
    _In_ HCN_LOADBALANCER LoadBalancer

```

## <a name="hcnnotificationcallback"></a>HCN_Notification_Callback

Sono funzioni forniscono l'accesso alle operazioni a livello di servizio, ad esempio le notifiche (ad esempio notifiche ricevente di una nuova creazione di una rete).

```
/// Registers a callback function to receive notifications of service-wide events such as network
/// creations/deletions.
///
/// \param  Callback           Function pointer to notification callback.
/// \param  Context            Context pointer.
/// \retval CallbackHandle     Receives a handle to a callback registered on a Service.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT WINAPI
HcnRegisterServiceCallback(
    _In_ HCN_NOTIFICATION_CALLBACK Callback,
    _In_ void* Context,
    _Out_ HCN_CALLBACK* CallbackHandle
    );

/// Unregisters from service-wide notifications
///
/// \retval CallbackHandle     Handle to a callback registered on a Service.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT WINAPI
HcnUnregisterServiceCallback(
    _In_ HCN_CALLBACK CallbackHandle
    );
```


