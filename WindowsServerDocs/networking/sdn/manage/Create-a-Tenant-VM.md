---
title: Creare una macchina virtuale e connettersi a una rete virtuale tenant o VLAN
description: In questo argomento viene illustrato come creare una VM tenant e connetterla a una rete virtuale creata con la virtualizzazione rete Hyper-V o a una rete locale virtuale (VLAN).
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3c62f533-1815-4f08-96b1-dc271f5a2b36
ms.author: pashort
author: shortpatti
ms.date: 08/24/2018
ms.openlocfilehash: 3e0678fb204e0895bf4429e8bb877a3f1c0e7a97
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355863"
---
# <a name="create-a-vm-and-connect-to-a-tenant-virtual-network-or-vlan"></a>Creare una macchina virtuale e connettersi a una rete virtuale tenant o VLAN

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Questo argomento illustra come creare una VM tenant e connetterla a una rete virtuale creata con la virtualizzazione rete Hyper-V o a una rete locale virtuale (VLAN). È possibile usare i cmdlet del controller di rete di Windows PowerShell per connettersi a una rete virtuale o NetworkControllerRESTWrappers per connettersi a una VLAN.

Usare i processi descritti in questo argomento per distribuire appliance virtuali. Con alcuni passaggi aggiuntivi, è possibile configurare dispositivi per elaborare o analizzare i pacchetti di dati che passano a o da altre macchine virtuali nella rete virtuale.

Nelle sezioni di questo argomento sono inclusi i comandi di Windows PowerShell di esempio che contengono valori di esempio per molti parametri. Assicurarsi di sostituire i valori di esempio in questi comandi con i valori appropriati per la distribuzione prima di eseguire questi comandi. 


## <a name="prerequisites"></a>Prerequisiti

1. Schede di rete VM create con indirizzi MAC statici per la durata della macchina virtuale.<p>Se l'indirizzo MAC cambia durante la durata della macchina virtuale, il controller di rete non è in grado di configurare i criteri necessari per la scheda di rete. La mancata configurazione dei criteri per la rete impedisce alla scheda di rete di elaborare il traffico di rete e tutte le comunicazioni con la rete hanno esito negativo.  

2. Se la macchina virtuale richiede l'accesso alla rete all'avvio, non avviare la macchina virtuale fino a quando non si imposta l'ID di interfaccia nella porta della scheda di rete della macchina virtuale. Se si avvia la macchina virtuale prima di impostare l'ID di interfaccia e l'interfaccia di rete non esiste, la macchina virtuale non può comunicare sulla rete nel controller di rete e tutti i criteri applicati.

3. Se sono necessari ACL personalizzato per l'interfaccia di rete, quindi creare l'ACL ora utilizzando le istruzioni nell'argomento [elenchi di controllo di accesso utilizzare (ACL) per gestire Data Center rete traffico](../../sdn/manage/Use-Access-Control-Lists--ACLs--to-Manage-Datacenter-Network-Traffic-Flow.md)

Assicurarsi che una rete virtuale è già stato creato prima di utilizzare questo comando di esempio. Per ulteriori informazioni, vedere [Create, Delete o Update reti virtuali dei Tenant](https://technet.microsoft.com/windows-server-docs/networking/sdn/manage/create%2c-delete%2c-or-update-tenant-virtual-networks).

## <a name="create-a-vm-and-connect-to-a-virtual-network-by-using-the-windows-powershell-network-controller-cmdlets"></a>Creare una macchina Virtuale e connettersi a una rete virtuale utilizzando i cmdlet di Controller di rete di Windows PowerShell


1. Creare una VM con una scheda di rete VM con un indirizzo MAC statico. 

   ```PowerShell    
   New-VM -Generation 2 -Name "MyVM" -Path "C:\VMs\MyVM" -MemoryStartupBytes 4GB -VHDPath "c:\VMs\MyVM\Virtual Hard Disks\WindowsServer2016.vhdx" -SwitchName "SDNvSwitch" 
    
   Set-VM -Name "MyVM" -ProcessorCount 4
    
   Set-VMNetworkAdapter -VMName "MyVM" -StaticMacAddress "00-11-22-33-44-55" 
   ```

2. Ottenere la rete virtuale che contiene la subnet a cui si desidera connettere la scheda di rete.

   ```Powershell 
   $vnet = get-networkcontrollervirtualnetwork -connectionuri $uri -ResourceId “Contoso_WebTier”
   ```

3. Creare un oggetto dell'interfaccia di rete nel controller di rete.

   >[!TIP]
   >In questo passaggio si userà l'ACL personalizzato.

   ```PowerShell
   $vmnicproperties = new-object Microsoft.Windows.NetworkController.NetworkInterfaceProperties
   $vmnicproperties.PrivateMacAddress = "001122334455" 
   $vmnicproperties.PrivateMacAllocationMethod = "Static" 
   $vmnicproperties.IsPrimary = $true 

   $vmnicproperties.DnsSettings = new-object Microsoft.Windows.NetworkController.NetworkInterfaceDnsSettings
   $vmnicproperties.DnsSettings.DnsServers = @("24.30.1.11", "24.30.1.12")
    
   $ipconfiguration = new-object Microsoft.Windows.NetworkController.NetworkInterfaceIpConfiguration
   $ipconfiguration.resourceid = "MyVM_IP1"
   $ipconfiguration.properties = new-object Microsoft.Windows.NetworkController.NetworkInterfaceIpConfigurationProperties
   $ipconfiguration.properties.PrivateIPAddress = “24.30.1.101”
   $ipconfiguration.properties.PrivateIPAllocationMethod = "Static"
    
   $ipconfiguration.properties.Subnet = new-object Microsoft.Windows.NetworkController.Subnet
   $ipconfiguration.properties.subnet.ResourceRef = $vnet.Properties.Subnets[0].ResourceRef
    
   $vmnicproperties.IpConfigurations = @($ipconfiguration)
   New-NetworkControllerNetworkInterface –ResourceID “MyVM_Ethernet1” –Properties $vmnicproperties –ConnectionUri $uri
   ```

4. Ottenere InstanceId per l'interfaccia di rete dal controller di rete.

   ```PowerShell 
    $nic = Get-NetworkControllerNetworkInterface -ConnectionUri $uri -ResourceId "MyVM-Ethernet1"
   ```

5. Impostare l'ID di interfaccia nella porta della scheda di rete della macchina virtuale Hyper-V.

   >[!NOTE]
   >È necessario eseguire questi comandi nell'host Hyper-V in cui è installata la macchina Virtuale.

   ```PowerShell 
   #Do not change the hardcoded IDs in this section, because they are fixed values and must not change.
    
   $FeatureId = "9940cd46-8b06-43bb-b9d5-93d50381fd56"
    
   $vmNics = Get-VMNetworkAdapter -VMName “MyVM”
    
   $CurrentFeature = Get-VMSwitchExtensionPortFeature -FeatureId $FeatureId -VMNetworkAdapter $vmNics
    
   if ($CurrentFeature -eq $null)
   {
   $Feature = Get-VMSystemSwitchExtensionPortFeature -FeatureId $FeatureId
    
   $Feature.SettingData.ProfileId = "{$($nic.InstanceId)}"
   $Feature.SettingData.NetCfgInstanceId = "{56785678-a0e5-4a26-bc9b-c0cba27311a3}"
   $Feature.SettingData.CdnLabelString = "TestCdn"
   $Feature.SettingData.CdnLabelId = 1111
   $Feature.SettingData.ProfileName = "Testprofile"
   $Feature.SettingData.VendorId = "{1FA41B39-B444-4E43-B35A-E1F7985FD548}"
   $Feature.SettingData.VendorName = "NetworkController"
   $Feature.SettingData.ProfileData = 1
    
   Add-VMSwitchExtensionPortFeature -VMSwitchExtensionFeature  $Feature -VMNetworkAdapter $vmNics
   }
   else
   {
   $CurrentFeature.SettingData.ProfileId = "{$($nic.InstanceId)}"
   $CurrentFeature.SettingData.ProfileData = 1
    
   Set-VMSwitchExtensionPortFeature -VMSwitchExtensionFeature $CurrentFeature  -VMNetworkAdapter $vmNic
   }
   ```

6. Avviare la macchina virtuale.

   ```PowerShell
    Get-VM -Name “MyVM” | Start-VM 
   ```

È stata creata correttamente una VM, connessa la macchina virtuale a una rete virtuale tenant e la VM è stata avviata in modo da poter elaborare i carichi di lavoro del tenant.

## <a name="create-a-vm-and-connect-to-a-vlan-by-using-networkcontrollerrestwrappers"></a>Creare una macchina Virtuale e connettersi a una VLAN utilizzando NetworkControllerRESTWrappers


1. Creare la macchina virtuale e assegnare un indirizzo MAC statico alla macchina virtuale.

   ```PowerShell
   New-VM -Generation 2 -Name "MyVM" -Path "C:\VMs\MyVM" -MemoryStartupBytes 4GB -VHDPath "c:\VMs\MyVM\Virtual Hard Disks\WindowsServer2016.vhdx" -SwitchName "SDNvSwitch" 

   Set-VM -Name "MyVM" -ProcessorCount 4

   Set-VMNetworkAdapter -VMName "MyVM" -StaticMacAddress "00-11-22-33-44-55" 
   ```

2. Impostare l'ID VLAN nella scheda di rete della macchina virtuale.

   ```PowerShell
   Set-VMNetworkAdapterIsolation –VMName “MyVM” -AllowUntaggedTraffic $true -IsolationMode VLAN -DefaultIsolationId 123
   ```

3. Ottenere la subnet di rete logica e creare l'interfaccia di rete. 

   ```PowerShell
    $logicalnet = get-networkcontrollerLogicalNetwork -connectionuri $uri -ResourceId "00000000-2222-1111-9999-000000000002"

    $vmnicproperties = new-object Microsoft.Windows.NetworkController.NetworkInterfaceProperties
    $vmnicproperties.PrivateMacAddress = "00-1D-C8-B7-01-02"
    $vmnicproperties.PrivateMacAllocationMethod = "Static"
    $vmnicproperties.IsPrimary = $true 
    
    $vmnicproperties.DnsSettings = new-object Microsoft.Windows.NetworkController.NetworkInterfaceDnsSettings
    $vmnicproperties.DnsSettings.DnsServers = $logicalnet.Properties.Subnets[0].DNSServers

    $ipconfiguration = new-object Microsoft.Windows.NetworkController.NetworkInterfaceIpConfiguration
    $ipconfiguration.resourceid = "MyVM_Ip1"
    $ipconfiguration.properties = new-object Microsoft.Windows.NetworkController.NetworkInterfaceIpConfigurationProperties
    $ipconfiguration.properties.PrivateIPAddress = “10.127.132.177”
    $ipconfiguration.properties.PrivateIPAllocationMethod = "Static"

    $ipconfiguration.properties.Subnet = new-object Microsoft.Windows.NetworkController.Subnet
    $ipconfiguration.properties.subnet.ResourceRef = $logicalnet.Properties.Subnets[0].ResourceRef

    $vmnicproperties.IpConfigurations = @($ipconfiguration)
    $vnic = New-NetworkControllerNetworkInterface –ResourceID “MyVM_Ethernet1” –Properties $vmnicproperties –ConnectionUri $uri

    $vnic.InstanceId
   ```

4. Impostare InstanceId sulla porta Hyper-V.

   ```PowerShell  
   #The hardcoded Ids in this section are fixed values and must not change.
   $FeatureId = "9940cd46-8b06-43bb-b9d5-93d50381fd56"

   $vmNics = Get-VMNetworkAdapter -VMName “MyVM”

   $CurrentFeature = Get-VMSwitchExtensionPortFeature -FeatureId $FeatureId -VMNetworkAdapter $vmNic
        
   if ($CurrentFeature -eq $null)
   {
       $Feature = Get-VMSystemSwitchExtensionFeature -FeatureId $FeatureId
        
       $Feature.SettingData.ProfileId = "{$InstanceId}"
       $Feature.SettingData.NetCfgInstanceId = "{56785678-a0e5-4a26-bc9b-c0cba27311a3}"
       $Feature.SettingData.CdnLabelString = "TestCdn"
       $Feature.SettingData.CdnLabelId = 1111
       $Feature.SettingData.ProfileName = "Testprofile"
       $Feature.SettingData.VendorId = "{1FA41B39-B444-4E43-B35A-E1F7985FD548}"
       $Feature.SettingData.VendorName = "NetworkController"
       $Feature.SettingData.ProfileData = 1
                
       Add-VMSwitchExtensionFeature -VMSwitchExtensionFeature  $Feature -VMNetworkAdapter $vmNic
   }        
   else
   {
       $CurrentFeature.SettingData.ProfileId = "{$InstanceId}"
       $CurrentFeature.SettingData.ProfileData = 1

       Set-VMSwitchExtensionPortFeature -VMSwitchExtensionFeature $CurrentFeature  -VMNetworkAdapter $vmNic
   }
   ```

5. Avviare la macchina virtuale.

   ```PowerShell
   Get-VM -Name “MyVM” | Start-VM 
   ```

È stata creata una macchina virtuale, è stata connessa la macchina virtuale a una VLAN e la macchina virtuale è stata avviata in modo da poter elaborare i carichi di lavoro del tenant.

  

