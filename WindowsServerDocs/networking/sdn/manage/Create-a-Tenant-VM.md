---
title: Creare una macchina virtuale e connettersi a un Tenant virtuale rete o VLAN
description: Questo argomento fa parte della Guida alla rete definita dal Software su come gestire carichi di lavoro Tenant e reti virtuali in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3c62f533-1815-4f08-96b1-dc271f5a2b36
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 001eb3efa073e4ffbdfad41f88949303159a7274
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="create-a-vm-and-connect-to-a-tenant-virtual-network-or-vlan"></a>Creare una macchina virtuale e connettersi a un Tenant virtuale rete o VLAN

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per creare un tenant virtuale \(VM\) del computer e connettere la macchina virtuale a una rete virtuale che hai creato con virtualizzazione rete Hyper-V o a un \(VLAN\) rete locale virtuale. 

In questo argomento contiene le sezioni seguenti.

- [Creare una macchina virtuale e connettersi a una rete virtuale utilizzando i cmdlet di Controller di rete di Windows PowerShell](#bkmk_vn)
- [Creare una macchina virtuale e connettersi a una VLAN utilizzando NetworkControllerRESTWrappers](#bkmk_vlan)

## <a name="requirements"></a>Requisiti
Prima di eseguire le procedure descritte nelle sezioni seguenti, tenere presente i seguenti requisiti.

1. È necessario creare schede di rete VM con supporto statico controllo di accesso che \(mac\) indirizzi in modo che l'indirizzo MAC della macchina virtuale non modificare la durata della macchina virtuale. 
>[!NOTE]
>Se l'indirizzo MAC di macchina virtuale cambia nel corso della durata della macchina virtuale, i Controller di rete non può configurare i criteri necessari per la scheda di rete. Se i criteri per la scheda di rete non sono configurato, la scheda di rete viene impedita l'elaborazione del traffico di rete e tutte le comunicazioni con la rete ha esito negativo. 

2. Se la macchina virtuale richiede l'accesso alla rete all'avvio, è importante che non si avvia la macchina virtuale dopo l'ultimo passaggio di configurazione - impostazione dell'ID di interfaccia nella macchina virtuale porta scheda di rete. Se si avvia la macchina virtuale prima di completare questo passaggio, la macchina virtuale non può comunicare in rete fino a quando non viene creata l'interfaccia di rete nel Controller di rete e il controller è applicato a tutti i criteri - criteri di rete virtuale, gli elenchi di controllo di accesso \(ACLs\) e la qualità del servizio \(QoS\).

È inoltre possibile utilizzare i processi che sono descritti in questo argomento per la distribuzione di Appliance virtuali. Con alcuni passaggi aggiuntivi, è possibile configurare dispositivi per elaborare o analizzare i pacchetti di dati che passano a o da altre macchine virtuali nella rete virtuale.

>[!IMPORTANT]
>Le sezioni seguenti includono esempi di comandi Windows PowerShell che contengono i valori di esempio per numero di parametri. Assicurarsi di sostituire i valori di esempio in questi comandi con i valori appropriati per la distribuzione prima di eseguire questi comandi.  

## <a name="bkmk_vn"></a>Creare una macchina virtuale e connettersi a una rete virtuale utilizzando i cmdlet di Controller di rete di Windows PowerShell

In questa sezione include gli argomenti seguenti.

1.  [Creare una macchina virtuale con una scheda di rete di macchina virtuale che dispone di un indirizzo MAC statico](#bkmk_create)
2.  [Ottenere la rete virtuale che contiene la subnet a cui si desidera connettere la scheda di rete](#bkmk_getvn)
3.  [Creare un oggetto di interfaccia di rete nel Controller di rete](#bkmk_object)
4.  [Ottenere l'ID istanza per l'interfaccia di rete dal Controller di rete](#bkmk_getinstance)
5.  [Impostare l'ID di interfaccia nella macchina virtuale Hyper-V porta scheda di rete](#bkmk_setinstance)
6.  [Avviare la macchina virtuale](#bkmk_start)

### <a name="bkmk_create"></a>Creare una macchina virtuale con una scheda di rete di macchina virtuale che dispone di un indirizzo MAC statico

Per creare una macchina virtuale con una scheda di rete con un indirizzo MAC statico, utilizzare il comando seguente.

    
    New-VM -Generation 2 -Name "MyVM" -Path "C:\VMs\MyVM" -MemoryStartupBytes 4GB -VHDPath "c:\VMs\MyVM\Virtual Hard Disks\WindowsServer2016.vhdx" -SwitchName "SDNvSwitch" 
    
    Set-VM -Name "MyVM" -ProcessorCount 4
    
    Set-VMNetworkAdapter -VMName "MyVM" -StaticMacAddress "00-11-22-33-44-55" 
    

### <a name="bkmk_getvn"></a>Ottenere la rete virtuale che contiene la subnet a cui si desidera connettere la scheda di rete
Assicurarsi che una rete virtuale è già stato creato prima di utilizzare questo comando di esempio. Per ulteriori informazioni, vedere [Create, Delete o Update reti virtuali dei Tenant](https://technet.microsoft.com/windows-server-docs/networking/sdn/manage/create%2c-delete%2c-or-update-tenant-virtual-networks).

Per ottenere la rete virtuale, utilizzare il comando seguente.

    
    $vnet = get-networkcontrollervirtualnetwork -connectionuri $uri -ResourceId “Contoso_WebTier”
    

>[!NOTE]
>Se sono necessari ACL personalizzato per l'interfaccia di rete, quindi creare l'ACL ora utilizzando le istruzioni nell'argomento [elenchi di controllo di accesso utilizzare (ACL) per gestire Data Center rete traffico](../../sdn/manage/Use-Access-Control-Lists--ACLs--to-Manage-Datacenter-Network-Traffic-Flow.md)

### <a name="bkmk_object"></a>Creare un oggetto di interfaccia di rete nel Controller di rete

Per creare un oggetto di interfaccia di rete nel Controller di rete, utilizzare il comando seguente.

>[!NOTE]
>Se hai creato un ACL personalizzato dopo il passaggio precedente, è possibile utilizzare ora.

    
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
    

### <a name="bkmk_getinstance"></a>Ottenere l'ID istanza per l'interfaccia di rete dal Controller di rete
Per ottenere l'ID istanza per l'interfaccia di rete dal Controller di rete, utilizzare il comando seguente.

    
    $nic = Get-NetworkControllerNetworkInterface -ConnectionUri $uri -ResourceId "MyVM-Ethernet1"
    

### <a name="bkmk_setinstance"></a>Impostare l'ID di interfaccia nella macchina virtuale Hyper-V porta scheda di rete
Per impostare l'ID di interfaccia nella macchina virtuale Hyper-V porta scheda di rete, utilizzare il comando seguente.

>[!NOTE]
>È necessario eseguire questi comandi nell'host Hyper-V in cui è installata la macchina virtuale.

    
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
    

### <a name="bkmk_start"></a>Avviare la macchina virtuale
Per avviare la macchina virtuale, utilizzare il comando seguente.

    
    Get-VM -Name “MyVM” | Start-VM 
    
Si dispone ora correttamente creata una macchina virtuale, connessa la macchina virtuale a un rete virtuale del tenant e avviare la macchina virtuale in modo che possa elaborare carichi di lavoro tenant.

## <a name="bkmk_vlan"></a>Creare una macchina virtuale e connettersi a una VLAN utilizzando NetworkControllerRESTWrappers

In questa sezione include gli argomenti seguenti.

1. [Creare la macchina virtuale e assegnare un indirizzo MAC statico](#bkmk_mac)
2. [Impostare l'ID VLAN nella scheda di rete VM](#bkmk_vid)
3. [Ottenere la subnet di rete logica e creare l'interfaccia di rete](#bkmk_subnet)
4. [Impostare l'ID istanza sulla porta Hyper-V](#bkmk_instance)
5. [Avviare la macchina virtuale](#bkmk_startvlan)


###<a name="bkmk_mac"></a>Creare la macchina virtuale e assegnare un indirizzo MAC statico
Per creare una macchina virtuale e assegnare un supporto statico \(MAC\) indirizzo MAC per la macchina virtuale, è possibile utilizzare i seguenti comandi di esempio.

    New-VM -Generation 2 -Name "MyVM" -Path "C:\VMs\MyVM" -MemoryStartupBytes 4GB -VHDPath "c:\VMs\MyVM\Virtual Hard Disks\WindowsServer2016.vhdx" -SwitchName "SDNvSwitch" 

    Set-VM -Name "MyVM" -ProcessorCount 4

    Set-VMNetworkAdapter -VMName "MyVM" -StaticMacAddress "00-11-22-33-44-55" 

###<a name="bkmk_vid"></a>Impostare l'ID VLAN nella scheda di rete VM
Per impostare l'ID VLAN nella scheda di rete, è possibile utilizzare il comando seguente.


    Set-VMNetworkAdapterIsolation –VMName “MyVM” -AllowUntaggedTraffic $true -IsolationMode VLAN -DefaultIsolationId 123


###<a name="bkmk_subnet"></a>Ottenere la subnet di rete logica e creare l'interfaccia di rete

Per ottenere la subnet di rete logica e creare l'interfaccia di rete con subnet della rete logica, è possibile utilizzare i seguenti comandi di esempio.


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
    

###<a name="bkmk_instance"></a>Impostare l'ID istanza sulla porta Hyper-V
Per impostare l'ID istanza sulla porta Hyper-V, è possibile utilizzare i seguenti comandi di esempio nell'host Hyper-V in cui si trova la macchina virtuale.

  
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


###<a name="bkmk_startvlan"></a>Avviare la macchina virtuale
Per avviare la macchina virtuale, è possibile utilizzare il comando seguente.


    Get-VM -Name “MyVM” | Start-VM 

Si dispone ora correttamente creata una macchina virtuale, connessa la macchina virtuale a una VLAN e avviare la macchina virtuale in modo che possa elaborare carichi di lavoro tenant.

  

