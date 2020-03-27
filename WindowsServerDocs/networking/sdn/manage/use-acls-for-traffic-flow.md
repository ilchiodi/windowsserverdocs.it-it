---
title: Usare gli elenchi di controllo di accesso (ACL) per gestire il flusso del traffico di rete del Data Center
description: In questo argomento si apprenderà come configurare gli elenchi di controllo di accesso (ACL) per gestire il flusso di traffico dei dati usando il firewall del centro dati e gli ACL nelle subnet virtuali. Per abilitare e configurare il firewall di Data Center, è possibile creare ACL che vengono applicati a una subnet virtuale o a un'interfaccia di rete.
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6a7ac5af-85e9-4440-a631-6a3a38e9015d
ms.author: lizross
author: eross-msft
ms.date: 08/27/2018
ms.openlocfilehash: 1f18ad9ddb0ea1a7575f6fcb26189f36f818ada2
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80317480"
---
# <a name="use-access-control-lists-acls-to-manage-datacenter-network-traffic-flow"></a>Usare gli elenchi di controllo di accesso (ACL) per gestire il flusso del traffico di rete del Data Center

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

In questo argomento si apprenderà come configurare gli elenchi di controllo di accesso (ACL) per gestire il flusso di traffico dei dati usando il firewall del centro dati e gli ACL nelle subnet virtuali. Per abilitare e configurare il firewall di Data Center, è possibile creare ACL che vengono applicati a una subnet virtuale o a un'interfaccia di rete.   

Gli esempi seguenti in questo argomento illustrano come usare Windows PowerShell per creare questi ACL.  

## <a name="configure-datacenter-firewall-to-allow-all-traffic"></a>Configurare il firewall del Data Center per consentire tutto il traffico  

Dopo aver distribuito SDN, è necessario verificare la connettività di rete di base nel nuovo ambiente.  A tale scopo, creare una regola per il firewall del data center che consenta tutto il traffico di rete, senza alcuna restrizione.   

Usare le voci nella tabella seguente per creare un set di regole che consentano tutto il traffico di rete in ingresso e in uscita.


| IP origine | IP destinazione | Protocollo | Porta di origine | Porta di destinazione | Direzione | Azione | Priorità |
|:---------:|:--------------:|:--------:|:-----------:|:----------------:|:---------:|:------:|:--------:|
|    \*     |       \*       |   Tutte    |     \*      |        \*        |  Inserimento in  | Consenti  |   100    |
|    \*     |       \*       |   Tutte    |     \*      |        \*        | Inserimento in  | Consenti  |   110    |

---       

### <a name="example-create-an-acl"></a>Esempio: creare un ACL 
In questo esempio viene creato un ACL con due regole:

1. **AllowAll_Inbound** : consente a tutto il traffico di rete di passare all'interfaccia di rete in cui è configurato questo ACL.    
2. **AllowAllOutbound** -consente a tutto il traffico di passare dall'interfaccia di rete. Questo ACL, identificato dall'ID risorsa "AllowAll-1", è ora pronto per essere usato nelle subnet virtuali e nelle interfacce di rete.  

Lo script di esempio seguente usa i comandi di Windows PowerShell esportati dal modulo **NetworkController** per creare questo ACL.  


```PowerShell
$ruleproperties = new-object Microsoft.Windows.NetworkController.AclRuleProperties  
$ruleproperties.Protocol = "All"  
$ruleproperties.SourcePortRange = "0-65535"  
$ruleproperties.DestinationPortRange = "0-65535"  
$ruleproperties.Action = "Allow"  
$ruleproperties.SourceAddressPrefix = "*"  
$ruleproperties.DestinationAddressPrefix = "*"  
$ruleproperties.Priority = "100"  
$ruleproperties.Type = "Inbound"  
$ruleproperties.Logging = "Enabled"  
$aclrule1 = new-object Microsoft.Windows.NetworkController.AclRule  
$aclrule1.Properties = $ruleproperties  
$aclrule1.ResourceId = "AllowAll_Inbound"  
$ruleproperties = new-object Microsoft.Windows.NetworkController.AclRuleProperties  
$ruleproperties.Protocol = "All"  
$ruleproperties.SourcePortRange = "0-65535"  
$ruleproperties.DestinationPortRange = "0-65535"  
$ruleproperties.Action = "Allow"  
$ruleproperties.SourceAddressPrefix = "*"  
$ruleproperties.DestinationAddressPrefix = "*"  
$ruleproperties.Priority = "110"  
$ruleproperties.Type = "Outbound"  
$ruleproperties.Logging = "Enabled"  
$aclrule2 = new-object Microsoft.Windows.NetworkController.AclRule  
$aclrule2.Properties = $ruleproperties  
$aclrule2.ResourceId = "AllowAll_Outbound"  
$acllistproperties = new-object Microsoft.Windows.NetworkController.AccessControlListProperties  
$acllistproperties.AclRules = @($aclrule1, $aclrule2)  
New-NetworkControllerAccessControlList -ResourceId "AllowAll" -Properties $acllistproperties -ConnectionUri <NC REST FQDN>  
```  

>[!NOTE]  
>La Guida di riferimento ai comandi di Windows PowerShell per il controller di rete si trova nell'argomento [cmdlet del controller di rete](https://technet.microsoft.com/library/mt576401.aspx).  

## <a name="use-acls-to-limit-traffic-on-a-subnet"></a>Usare gli ACL per limitare il traffico in una subnet  
In questo esempio viene creato un ACL che impedisce alle macchine virtuali all'interno della subnet 192.168.0.0/24 di comunicare tra loro. Questo tipo di ACL è utile per limitare la capacità di un utente malintenzionato di diffondersi in un secondo momento all'interno della subnet, consentendo allo stesso tempo alle macchine virtuali di ricevere richieste dall'esterno della subnet, nonché di comunicare con altri servizi su altre subnet.   


|   IP origine    | IP destinazione | Protocollo | Porta di origine | Porta di destinazione | Direzione | Azione | Priorità |
|:--------------:|:--------------:|:--------:|:-----------:|:----------------:|:---------:|:------:|:--------:|
|  192.168.0.1   |       \*       |   Tutte    |     \*      |        \*        |  Inserimento in  | Consenti  |   100    |
|       \*       |  192.168.0.1   |   Tutte    |     \*      |        \*        | Inserimento in  | Consenti  |   101    |
| 192.168.0.0/24 |       \*       |   Tutte    |     \*      |        \*        |  Inserimento in  | Blocco  |   102    |
|       \*       | 192.168.0.0/24 |   Tutte    |     \*      |        \*        | Inserimento in  | Blocco  |   103    |
|       \*       |       \*       |   Tutte    |     \*      |        \*        |  Inserimento in  | Consenti  |   104    |
|       \*       |       \*       |   Tutte    |     \*      |        \*        | Inserimento in  | Consenti  |   105    |

--- 

È ora possibile applicare l'ACL creato dallo script di esempio seguente, identificato dall'ID risorsa **subnet-192-168-0-0**, a una subnet di rete virtuale che usa l'indirizzo subnet "192.168.0.0/24".  Qualsiasi interfaccia di rete collegata alla subnet della rete virtuale ottiene automaticamente le regole ACL sopra applicate.  

Di seguito è riportato uno script di esempio che usa i comandi di Windows PowerShell per creare questo ACL usando l'API REST del controller di rete:  

```PowerShell  
import-module networkcontroller  
$ncURI = "https://mync.contoso.local"  
$aclrules = @()  

$ruleproperties = new-object Microsoft.Windows.NetworkController.AclRuleProperties  
$ruleproperties.Protocol = "All"  
$ruleproperties.SourcePortRange = "0-65535"  
$ruleproperties.DestinationPortRange = "0-65535"  
$ruleproperties.Action = "Allow"  
$ruleproperties.SourceAddressPrefix = "192.168.0.1"  
$ruleproperties.DestinationAddressPrefix = "*"  
$ruleproperties.Priority = "100"  
$ruleproperties.Type = "Inbound"  
$ruleproperties.Logging = "Enabled"  

$aclrule = new-object Microsoft.Windows.NetworkController.AclRule  
$aclrule.Properties = $ruleproperties  
$aclrule.ResourceId = "AllowRouter_Inbound"  
$aclrules += $aclrule  

$ruleproperties = new-object Microsoft.Windows.NetworkController.AclRuleProperties  
$ruleproperties.Protocol = "All"  
$ruleproperties.SourcePortRange = "0-65535"  
$ruleproperties.DestinationPortRange = "0-65535"  
$ruleproperties.Action = "Allow"  
$ruleproperties.SourceAddressPrefix = "*"  
$ruleproperties.DestinationAddressPrefix = "192.168.0.1"  
$ruleproperties.Priority = "101"  
$ruleproperties.Type = "Outbound"  
$ruleproperties.Logging = "Enabled"  

$aclrule = new-object Microsoft.Windows.NetworkController.AclRule  
$aclrule.Properties = $ruleproperties  
$aclrule.ResourceId = "AllowRouter_Outbound"  
$aclrules += $aclrule  

$ruleproperties = new-object Microsoft.Windows.NetworkController.AclRuleProperties  
$ruleproperties.Protocol = "All"  
$ruleproperties.SourcePortRange = "0-65535"  
$ruleproperties.DestinationPortRange = "0-65535"  
$ruleproperties.Action = "Deny"  
$ruleproperties.SourceAddressPrefix = "192.168.0.0/24"  
$ruleproperties.DestinationAddressPrefix = "*"  
$ruleproperties.Priority = "102"  
$ruleproperties.Type = "Inbound"  
$ruleproperties.Logging = "Enabled"  

$aclrule = new-object Microsoft.Windows.NetworkController.AclRule  
$aclrule.Properties = $ruleproperties  
$aclrule.ResourceId = "DenySubnet_Inbound"  
$aclrules += $aclrule  

$ruleproperties = new-object Microsoft.Windows.NetworkController.AclRuleProperties  
$ruleproperties.Protocol = "All"  
$ruleproperties.SourcePortRange = "0-65535"  
$ruleproperties.DestinationPortRange = "0-65535"  
$ruleproperties.Action = "Deny"  
$ruleproperties.SourceAddressPrefix = "*"  
$ruleproperties.DestinationAddressPrefix = "192.168.0.0/24"  
$ruleproperties.Priority = "103"  
$ruleproperties.Type = "Outbound"  
$ruleproperties.Logging = "Enabled"  

$aclrule = new-object Microsoft.Windows.NetworkController.AclRule  
$aclrule.Properties = $ruleproperties  
$aclrule.ResourceId = "DenySubnet_Outbound"  

$ruleproperties = new-object Microsoft.Windows.NetworkController.AclRuleProperties  
$ruleproperties.Protocol = "All"  
$ruleproperties.SourcePortRange = "0-65535"  
$ruleproperties.DestinationPortRange = "0-65535"  
$ruleproperties.Action = "Allow"  
$ruleproperties.SourceAddressPrefix = "*"  
$ruleproperties.DestinationAddressPrefix = "*"  
$ruleproperties.Priority = "104"  
$ruleproperties.Type = "Inbound"  
$ruleproperties.Logging = "Enabled"  

$aclrule = new-object Microsoft.Windows.NetworkController.AclRule  
$aclrule.Properties = $ruleproperties  
$aclrule.ResourceId = "AllowAll_Inbound"  
$aclrules += $aclrule  

$ruleproperties = new-object Microsoft.Windows.NetworkController.AclRuleProperties  
$ruleproperties.Protocol = "All"  
$ruleproperties.SourcePortRange = "0-65535"  
$ruleproperties.DestinationPortRange = "0-65535"  
$ruleproperties.Action = "Allow"  
$ruleproperties.SourceAddressPrefix = "*"  
$ruleproperties.DestinationAddressPrefix = "*"  
$ruleproperties.Priority = "105"  
$ruleproperties.Type = "Outbound"  
$ruleproperties.Logging = "Enabled"  

$aclrule = new-object Microsoft.Windows.NetworkController.AclRule  
$aclrule.Properties = $ruleproperties  
$aclrule.ResourceId = "AllowAll_Outbound"  
$aclrules += $aclrule  

$acllistproperties = new-object Microsoft.Windows.NetworkController.AccessControlListProperties  
$acllistproperties.AclRules = $aclrules  

New-NetworkControllerAccessControlList -ResourceId "Subnet-192-168-0-0" -Properties $acllistproperties -ConnectionUri $ncURI   
```  

