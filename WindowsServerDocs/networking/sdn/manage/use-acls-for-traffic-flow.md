---
title: Utilizzare elenchi di controllo di accesso (ACL) per gestire il flusso di traffico di rete di Data Center
description: Questo argomento fa parte della Guida alla rete definita dal Software su come gestire carichi di lavoro Tenant e reti virtuali in Windows Server 2016.
manager: brianlic
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
ms.openlocfilehash: 64b7e1abf1ddb8132a8c6692fe82521c589f32df
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="use-access-control-lists-acls-to-manage-datacenter-network-traffic-flow"></a>Utilizzare elenchi di controllo di accesso (ACL) per gestire il flusso di traffico di rete di Data Center

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per informazioni su come configurare gli elenchi di controllo di accesso per gestire il flusso del traffico dati con Firewall del centro dati e gli ACL nella subnet virtuali.  
  
È possibile abilitare e configurare i Firewall del centro dati creando ACL da applicare a una subnet virtuale o un'interfaccia di rete.  
  
Gli esempi seguenti illustrano come utilizzare Windows PowerShell per creare questi ACL.  
  
## <a name="configure-datacenter-firewall-to-allow-all-traffic"></a>Configurare il Firewall di Data Center per consentire tutto il traffico  
  
Dopo la distribuzione di SDN, è consigliabile testare la connettività di rete di base nel proprio ambiente di nuovo.  
  
A tale scopo, è possibile creare una regola per Firewall del centro dati che consente di tutto il traffico di rete, senza restrizioni.   
  
È possibile utilizzare le voci nella tabella seguente per creare un set di regole che consentono di tutto il traffico di rete in entrata e in uscita.  
  
  
  
IP di origine|Destinazione IP|Protocollo|Porta di origine|Porta di destinazione|Direzione|Azione|Priorità   
---------|---------|---------|---------|---------|---------|---------|---------  
*    |   *      |   Tutti      |    *     |      *   |     Connessioni in entrata    |   Consenti      |   100        
*    |     *    |     Tutti    |     *    |     *    |   In uscita      |  Consenti       |  110         
  
  
Questo script di esempio crea un ACL che contiene due regole:    
- La prima regola "AllowAll_Inbound" consente tutto il traffico di rete passare l'interfaccia di rete in cui è configurata l'ACL.    
- La seconda regola, "AllowAllOutbound" consente tutto il traffico passare all'esterno dell'interfaccia di rete.  
L'ACL, identificato dall'id della risorsa "ConsentiTutto-1" è ora pronto per essere usato nelle interfacce di rete e subnet virtuali.  
  
Lo script di esempio seguente usa i comandi di Windows PowerShell esportati dal **NetworkController** modulo per creare l'ACL.  
  
  
```  
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
>Il riferimento ai comandi di Windows PowerShell per Controller di rete si trova nell'argomento [cmdlet di Controller di rete](https://technet.microsoft.com/library/mt576401.aspx).  
  
## <a name="use-acls-to-limit-traffic-on-a-subnet"></a>Utilizzare gli ACL per limitare il traffico in una Subnet  
  
È possibile utilizzare questo esempio mostra come creare un ACL che impedisce di macchine virtuali (VM) all'interno della subnet 192.168.0.0/24 di comunicare tra loro.   
  
Questo tipo di ACL è utile per limitare la possibilità di un utente malintenzionato di distribuire lateralmente all'interno della subnet, consentendo al tempo stesso le macchine virtuali per ricevere le richieste di fuori della subnet, nonché per comunicare con altri servizi di altre subnet.  
  
  
IP di origine|Destinazione IP|Protocollo|Porta di origine|Porta di destinazione|Direzione|Azione|Priorità   
---------|---------|---------|---------|---------|---------|---------|---------  
192.168.0.1    | * | Tutti | * | * | Connessioni in entrata|   Consenti      |   100        
* |192.168.0.1 | Tutti | * | * | In uscita|  Consenti       |  101         
192.168.0.0/24 | * | Tutti | * | * | Connessioni in entrata| Blocco | 102  
* | 192.168.0.0/24 |Tutti| * | * | In uscita | Blocco |103  
* | *  |Tutti| * | * | Connessioni in entrata | Consenti |104  
* | *  |Tutti| * | * | In uscita | Consenti |105  
  
L'ACL creati dallo script di esempio seguente, identificato dall'id della risorsa **Subnet-192-168-0-0**, ora possono essere applicati a una subnet di rete virtuale che utilizza l'indirizzo di subnet "192.168.0.0/24".  Qualsiasi interfaccia di rete collegata alla subnet di rete virtuale automaticamente Ottiene il precedente regole ACL applicate.  
  
Di seguito è uno script di esempio con comandi di Windows Powershell per creare l'ACL usando l'API REST di Controller di rete:  
  
```  
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
  
