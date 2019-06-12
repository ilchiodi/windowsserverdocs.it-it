---
title: Controllo di accesso di Usa elenchi (ACL) per gestire il flusso del Data Center rete traffico
description: In questo argomento descrive come configurare elenchi di controllo di accesso (ACL) per gestire il flusso del traffico dei dati tramite Firewall del centro dati e gli ACL in subnet virtuali. Abilitare e configurare Firewall del centro dati creando gli ACL che vengano applicati a una subnet virtuale o un'interfaccia di rete.
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6a7ac5af-85e9-4440-a631-6a3a38e9015d
ms.author: pashort
author: shortpatti
ms.date: 08/27/2018
ms.openlocfilehash: 7bfb74e0964735d357226ab1e5af826796c48d81
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446298"
---
# <a name="use-access-control-lists-acls-to-manage-datacenter-network-traffic-flow"></a>Controllo di accesso di Usa elenchi (ACL) per gestire il flusso del Data Center rete traffico

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

In questo argomento descrive come configurare elenchi di controllo di accesso (ACL) per gestire il flusso del traffico dei dati tramite Firewall del centro dati e gli ACL in subnet virtuali. Abilitare e configurare Firewall del centro dati creando gli ACL che vengano applicati a una subnet virtuale o un'interfaccia di rete.   

Gli esempi seguenti in questo argomento illustrano come usare Windows PowerShell per creare questi ACL.  

## <a name="configure-datacenter-firewall-to-allow-all-traffic"></a>Configurare il Firewall di Data Center per consentire tutto il traffico  

Quando si distribuisce SDN, è consigliabile testare la connettività di rete di base nel tuo nuovo ambiente.  A tale scopo, creare una regola di Firewall di Data Center che consente tutto il traffico di rete, senza alcuna restrizione.   

Usare le voci nella tabella seguente per creare un set di regole che consentono di tutto il traffico di rete in ingresso e in uscita.


| IP origine | IP destinazione | Protocol | Porta di origine | Porta di destinazione | Direction | Azione | Priority |
|:---------:|:--------------:|:--------:|:-----------:|:----------------:|:---------:|:------:|:--------:|
|    \*     |       \*       |   Tutte    |     \*      |        \*        |  In entrata  | Consenti  |   100    |
|    \*     |       \*       |   Tutte    |     \*      |        \*        | In uscita  | Consenti  |   110    |

---       

### <a name="example-create-an-acl"></a>Esempio: Creare un ACL 
In questo esempio, si crea un ACL con due regole:

1. **AllowAll_Inbound** -consente tutto il traffico di rete da passare all'interfaccia di rete in cui viene configurato l'ACL.    
2. **AllowAllOutbound** -consente tutto il traffico da passare all'esterno dell'interfaccia di rete. L'ACL, identificato dall'id di risorsa "AllowAll-1" è ora pronto per essere usato nelle interfacce di rete e subnet virtuali.  

Lo script di esempio seguente usa i comandi di Windows PowerShell esportati dal **NetworkController** modulo per creare l'ACL.  


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
>I riferimenti ai comandi di Windows PowerShell per il Controller di rete si trova nell'argomento [cmdlet Controller di rete](https://technet.microsoft.com/library/mt576401.aspx).  

## <a name="use-acls-to-limit-traffic-on-a-subnet"></a>Usare gli elenchi di limitare il traffico in una subnet  
In questo esempio, si crea un ACL che impedisce che le macchine virtuali all'interno della subnet 192.168.0.0/24 che comunicano tra loro. Questo tipo di ACL è utile per limitare la capacità di un utente malintenzionato per diffondere lateralmente all'interno della subnet, consentendo allo stesso tempo le macchine virtuali per la ricezione di richieste provenienti dall'esterno della subnet, nonché per comunicare con altri servizi su altre subnet.   


|   IP origine    | IP destinazione | Protocol | Porta di origine | Porta di destinazione | Direction | Azione | Priority |
|:--------------:|:--------------:|:--------:|:-----------:|:----------------:|:---------:|:------:|:--------:|
|  192.168.0.1   |       \*       |   Tutte    |     \*      |        \*        |  In entrata  | Consenti  |   100    |
|       \*       |  192.168.0.1   |   Tutte    |     \*      |        \*        | In uscita  | Consenti  |   101    |
| 192.168.0.0/24 |       \*       |   Tutte    |     \*      |        \*        |  In entrata  | Blocca  |   102    |
|       \*       | 192.168.0.0/24 |   Tutte    |     \*      |        \*        | In uscita  | Blocca  |   103    |
|       \*       |       \*       |   Tutte    |     \*      |        \*        |  In entrata  | Consenti  |   104    |
|       \*       |       \*       |   Tutte    |     \*      |        \*        | In uscita  | Consenti  |   105    |

--- 

L'ACL creato dallo script di esempio riportato di seguito, identificata dall'id di risorsa **Subnet-192-168-0-0**, è ora possibile applicare a una subnet di rete virtuale che usa l'indirizzo di subnet "192.168.0.0/24".  Qualsiasi interfaccia di rete viene connesso automaticamente a tale subnet di rete virtuale Ottiene le regole ACL precedenti applicate.  

Di seguito è riportato un esempio di script usando i comandi di Windows Powershell per creare questo ACL usando l'API REST del Controller di rete:  

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

