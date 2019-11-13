---
title: Installare Active Directory Domain Services in una macchina virtuale di Azure
description: Come creare una nuova foresta di Active Directory in una macchina virtuale (VM) in una macchina virtuale di Azure.
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 04/11/2019
ms.technology: identity-adds
ms.topic: article
ms.prod: windows-server
ms.openlocfilehash: e0a10e27e044fd1df7cbdb943964440983ce494c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390654"
---
# <a name="install-a-new-active-directory-forest-using-azure-cli"></a>Installare una nuova foresta Active Directory usando l'interfaccia della riga di comando di Azure

Servizi di dominio Active Directory può essere eseguito in una macchina virtuale (VM) di Azure nello stesso modo in cui viene eseguito in molte istanze locali. Questo articolo illustra la distribuzione di una nuova foresta di servizi di dominio Active Directory, in due nuovi controller di dominio, in un set di disponibilità di Azure con il portale di Azure e l'interfaccia della riga di comando Molti clienti trovano questo materiale sussidiario utile durante la creazione di un Lab o la preparazione alla distribuzione di controller di dominio in Azure.

## <a name="components"></a>Componenti

* Un gruppo di risorse in cui inserire tutti gli elementi.
* Una [rete virtuale di Azure](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview.md), una subnet, un gruppo di sicurezza di rete e una regola per consentire l'accesso RDP alle macchine virtuali.
* Un [set di disponibilità](https://docs.microsoft.com/azure/virtual-machines/windows/regions-and-availability#availability-sets) di macchine virtuali di Azure in cui inserire due controller di dominio di Active Directory Domain Services (ad DS).
* Due macchine virtuali di Azure per l'esecuzione di servizi di dominio Active Directory e DNS.

### <a name="items-that-are-not-covered"></a>Elementi non analizzati

* [Creazione di una connessione VPN da sito a sito](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) da una posizione locale
* [Sicurezza del traffico di rete in Azure](https://docs.microsoft.com/azure/security/azure-security-network-security-best-practices.md)
* [Progettazione della topologia del sito](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/designing-the-site-topology)
* [Pianificazione del posizionamento del ruolo master operazioni](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/planning-operations-master-role-placement)
* [Distribuzione di Azure AD Connect per sincronizzare le identità con Azure AD](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-install-express)

## <a name="build-the-test-environment"></a>Compilare l'ambiente di test

Per la creazione dell'ambiente vengono usati il [portale di Azure](https://portal.azure.com) e l'interfaccia della riga di comando di [Azure](https://docs.microsoft.com/cli/azure/overview?view=azure-cli-latest) .

L'interfaccia della riga di comando di Azure viene usata per creare e gestire le risorse di Azure dalla riga di comando o negli script. Questa esercitazione illustra come usare l'interfaccia della riga di comando di Azure per distribuire le macchine virtuali che eseguono Windows Server 2019. Al termine della distribuzione, ci si connette ai server e si installa Servizi di dominio Active Directory.

Se non si ha una sottoscrizione di Azure, [creare un account gratuito prima di](https://azure.microsoft.com/free) iniziare.

### <a name="using-azure-cli"></a>Uso dell'interfaccia della riga di comando

Lo script seguente automatizza il processo di creazione di due macchine virtuali Windows Server 2019, allo scopo di creare controller di dominio per una nuova foresta di Active Directory in Azure. Un amministratore può modificare le variabili seguenti per soddisfare le proprie esigenze, quindi completarle come un'unica operazione. Lo script crea il gruppo di risorse necessario, il gruppo di sicurezza di rete con una regola del traffico per Desktop remoto, rete virtuale e subnet e gruppo di disponibilità. Ogni macchina virtuale viene quindi compilata con un disco dati da 20 GB con memorizzazione nella cache disabilitata per l'installazione di servizi di dominio Active Directory.

Lo script seguente può essere eseguito direttamente dalla portale di Azure. Se si sceglie di installare e usare l'interfaccia della riga di comando in locale, per questa Guida introduttiva è necessario eseguire l'interfaccia della riga di comando di Azure versione 2.0.4 o successiva. Eseguire `az --version` per trovare la versione. Se è necessario eseguire l'installazione o l'aggiornamento, vedere Installare l'interfaccia della riga di comando di [Azure 2,0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

| Nome variabile | Scopo |
| :---: | :--- |
| Con valori AdminUsername | Nome utente da configurare in ogni macchina virtuale come amministratore locale. |
| AdminPassword | Password non crittografata da configurare in ogni macchina virtuale come password dell'amministratore locale. |
| ResourceGroupName | Nome da usare per il gruppo di risorse. Non deve duplicare un nome esistente. |
| Location | Nome della località di Azure in cui si vuole eseguire la distribuzione. Elenca le aree supportate per la sottoscrizione corrente usando `az account list-locations`. |
| VNetName | Il nome da assegnare alla rete virtuale di Azure non deve duplicare un nome esistente. |
| VNetAddress | Ambito IP da usare per la rete di Azure. Non deve duplicare un intervallo esistente. |
| SubnetName | Nome da assegnare alla subnet IP. Non deve duplicare un nome esistente. |
| SubnetAddress | Indirizzo subnet per i controller di dominio. Deve essere una subnet all'interno di VNet. |
| Abilitate per | Nome del set di disponibilità a cui si aggiungeranno le macchine virtuali del controller di dominio. |
| VMSize | Dimensioni standard della macchina virtuale di Azure disponibili nella posizione per la distribuzione. |
| DataDiskSize | Dimensioni in GB per il disco dati in cui viene installato Servizi di dominio Active Directory. |
| DomainController1 | Nome del primo controller di dominio. |
| DC1IP | Indirizzo IP per il primo controller di dominio. |
| DomainController2 | Nome del secondo controller di dominio. |
| DC2IP | Indirizzo IP per il secondo controller di dominio. |

```azurecli
#Update based on your organizational requirements
Location=westus2
ResourceGroupName=ADonAzureVMs
NetworkSecurityGroup=NSG-DomainControllers
VNetName=VNet-AzureVMsWestUS2
VNetAddress=10.10.0.0/16
SubnetName=Subnet-AzureDCsWestUS2
SubnetAddress=10.10.10.0/24
AvailabilitySet=DomainControllers
VMSize=Standard_DS1_v2
DataDiskSize=20
AdminUsername=azureuser
AdminPassword=ChangeMe123456
DomainController1=AZDC01
DC1IP=10.10.10.11
DomainController2=AZDC02
DC2IP=10.10.10.12

# Create a resource group.
az group create --name $ResourceGroupName \
                --location $Location

# Create a network security group
az network nsg create --name $NetworkSecurityGroup \
                      --resource-group $ResourceGroupName \
                      --location $Location

# Create a network security group rule for port 3389.
az network nsg rule create --name PermitRDP \
                           --nsg-name $NetworkSecurityGroup \
                           --priority 1000 \
                           --resource-group $ResourceGroupName \
                           --access Allow \
                           --source-address-prefixes "*" \
                           --source-port-ranges "*" \
                           --direction Inbound \
                           --destination-port-ranges 3389

# Create a virtual network.
az network vnet create --name $VNetName \
                       --resource-group $ResourceGroupName \
                       --address-prefixes $VNetAddress \
                       --location $Location \

# Create a subnet
az network vnet subnet create --address-prefix $SubnetAddress \
                              --name $SubnetName \
                              --resource-group $ResourceGroupName \
                              --vnet-name $VNetName \
                              --network-security-group $NetworkSecurityGroup

# Create an availability set.
az vm availability-set create --name $AvailabilitySet \
                              --resource-group $ResourceGroupName \
                              --location $Location

# Create two virtual machines.
az vm create \
    --resource-group $ResourceGroupName \
    --availability-set $AvailabilitySet \
    --name $DomainController1 \
    --size $VMSize \
    --image Win2019Datacenter \
    --admin-username $AdminUsername \
    --admin-password $AdminPassword \
    --data-disk-sizes-gb $DataDiskSize \
    --data-disk-caching None \
    --nsg $NetworkSecurityGroup \
    --private-ip-address $DC1IP \
    --no-wait

az vm create \
    --resource-group $ResourceGroupName \
    --availability-set $AvailabilitySet \
    --name $DomainController2 \
    --size $VMSize \
    --image Win2019Datacenter \
    --admin-username $AdminUsername \
    --admin-password $AdminPassword \
    --data-disk-sizes-gb $DataDiskSize \
    --data-disk-caching None \
    --nsg $NetworkSecurityGroup \
    --private-ip-address $DC2IP

```

## <a name="dns-and-active-directory"></a>DNS e Active Directory

Se le macchine virtuali di Azure create come parte di questo processo saranno un'estensione di un'infrastruttura di Active Directory locale esistente, è necessario modificare le impostazioni DNS nella rete virtuale per includere i server DNS locali prima della distribuzione. Questo passaggio è importante per consentire ai controller di dominio appena creati in Azure di risolvere le risorse locali e consentire la replica. Altre informazioni su DNS, Azure e su come configurare le impostazioni sono reperibili nella sezione [risoluzione dei nomi che usa il proprio server DNS](https://docs.microsoft.com/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances#name-resolution-that-uses-your-own-dns-server).

Dopo aver innalzato di nuovo i controller di dominio in Azure, sarà necessario impostarli sui server DNS primari e secondari per la rete virtuale e tutti i server DNS locali verrebbero abbassati a terziario e oltre. Altre informazioni sulla modifica dei server DNS sono disponibili nell'articolo [creare, modificare o eliminare una rete virtuale](https://docs.microsoft.com/azure/virtual-network/manage-virtual-network#change-dns-servers).

Per informazioni sull'estensione di una rete locale in Azure, vedere l'articolo creazione di [una connessione VPN da sito a sito](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal
).

## <a name="configure-the-vms-and-install-active-directory-domain-services"></a>Configurare le macchine virtuali e installare Active Directory Domain Services

Al termine dello script, passare alla [portale di Azure](https://portal.azure.com), quindi alle **macchine virtuali**.

### <a name="configure-the-first-domain-controller"></a>Configurare il primo controller di dominio

Connettersi a AZDC01 usando le credenziali specificate nello script.

* Inizializzare e formattare il disco dati come F:
   * Aprire il menu Start e passare a **Gestione computer**
   * Passare ad **archiviazione** > **Gestione disco**
   * Inizializzare il disco come MBR
   * Creare un nuovo volume semplice e assegnare la lettera di unità F: è possibile specificare un'etichetta di volume se si desidera
* Installare Active Directory Domain Services utilizzando Server Manager
* Innalzare di livello il controller di dominio come primo in una nuova foresta
   * Lasciare il server Domain Name System (DNS) e il catalogo globale (GC) controllati nella pagina Opzioni controller di dominio
   * Specificare una password per la modalità ripristino servizi directory in base ai requisiti dell'organizzazione
   * Modificare i percorsi da C: per puntare all'unità F: creata quando viene richiesto il percorso
   * Controllare le selezioni effettuate nella procedura guidata e scegliere **Avanti**

> [!NOTE]
> Il controllo dei prerequisiti avvertirà che alla scheda di rete fisica non sono assegnati indirizzi IP statici, è possibile ignorarli in modo sicuro poiché nella rete virtuale di Azure sono assegnati indirizzi IP statici.

* Scegliere **Installa**

Al termine del processo di installazione, la macchina virtuale verrà riavviata.

Quando la macchina virtuale viene riavviata, l'accesso viene eseguito con le credenziali usate, ma questa volta come membro del dominio creato.

   > [!NOTE]
   > Il primo accesso dopo la promozione a un controller di dominio potrebbe richiedere più tempo del normale e questo comportamento è accettabile. Prendi una tazza di tè, caffè, acqua o altra bevanda preferita.

Poiché le [reti virtuali di Azure non supportano IPv6](https://docs.microsoft.com/azure/virtual-network/virtual-networks-faq#do-vnets-support-ipv6) , è consigliabile impostare le VM in modo da preferire IPv4 su IPv6. Per informazioni su come completare questa attività, vedere l'articolo della Knowledge [Guida per la configurazione di IPv6 in Windows per gli utenti avanzati](https://support.microsoft.com/help/929852/guidance-for-configuring-ipv6-in-windows-for-advanced-users).

### <a name="configure-the-second-domain-controller"></a>Configurare il secondo controller di dominio

Connettersi a AZDC02 usando le credenziali specificate nello script.

* Inizializzare e formattare il disco dati come F:
   * Aprire il menu Start e passare a **Gestione computer**
   * Passare ad **archiviazione** > **Gestione disco**
   * Inizializzare il disco come MBR
   * Creare un nuovo volume semplice e assegnare la lettera di unità F: è possibile specificare un'etichetta di volume se si desidera
* Installare Active Directory Domain Services utilizzando Server Manager
* Innalzamento di livello del controller di dominio
   * Aggiungere un controller di dominio a un dominio esistente-CONTOSO.com
   * Fornire le credenziali per eseguire l'operazione
   * Modificare i percorsi da C: per puntare all'unità F: creata quando viene richiesto il percorso
   * Verificare che Domain Name System server DNS e il catalogo globale (GC) siano selezionati nella pagina Opzioni controller di dominio
   * Specificare una password per la modalità ripristino servizi directory in base ai requisiti dell'organizzazione
   * Controllare le selezioni effettuate nella procedura guidata e scegliere **Avanti**

> [!NOTE]
> Il controllo dei prerequisiti avvertirà che alla scheda di rete fisica non sono assegnati indirizzi IP statici, è possibile ignorarli in modo sicuro poiché nella rete virtuale di Azure sono assegnati indirizzi IP statici.

* Scegliere **Installa**

Al termine del processo di installazione, la macchina virtuale verrà riavviata.

Quando la macchina virtuale viene riavviata, l'accesso viene eseguito con le credenziali usate, ma questa volta come membro del dominio CONTOSO.com

Poiché le [reti virtuali di Azure non supportano IPv6](https://docs.microsoft.com/azure/virtual-network/virtual-networks-faq#do-vnets-support-ipv6) , è consigliabile impostare le VM in modo da preferire IPv4 su IPv6. Per informazioni su come completare questa attività, vedere l'articolo della Knowledge [Guida per la configurazione di IPv6 in Windows per gli utenti avanzati](https://support.microsoft.com/help/929852/guidance-for-configuring-ipv6-in-windows-for-advanced-users).

### <a name="configure-dns"></a>Configurare DNS

Dopo aver innalzato di nuovo i controller di dominio in Azure, sarà necessario impostarli sui server DNS primari e secondari per la rete virtuale e i server DNS locali verrebbero abbassati al terziario e oltre. Altre informazioni sulla modifica dei server DNS sono disponibili nell'articolo [creare, modificare o eliminare una rete virtuale](https://docs.microsoft.com/azure/virtual-network/manage-virtual-network#change-dns-servers).

### <a name="wrap-up"></a>Eseguire il wrapping

A questo punto, l'ambiente ha una coppia di controller di dominio e la rete virtuale di Azure è stata configurata in modo da poter aggiungere altri server all'ambiente. A questo punto, è necessario completare le attività successive all'installazione per Active Directory Domain Services come la configurazione di siti e servizi, il controllo, il backup e la protezione dell'account Administrator predefinito.

## <a name="removing-the-environment"></a>Rimozione dell'ambiente

Per rimuovere l'ambiente dopo aver completato il test, è possibile eliminare il gruppo di risorse creato in precedenza. questo passaggio rimuove tutti i componenti che fanno parte di tale gruppo di risorse.

### <a name="remove-using-the-azure-portal"></a>Rimuovere usando il portale di Azure

Dalla portale di Azure passare a gruppi di **risorse** e scegliere il gruppo di risorse creato in questo esempio ADonAzureVMs, quindi selezionare **Elimina gruppo di risorse**. Il processo richiede la conferma prima di eliminare tutte le risorse contenute nel gruppo di risorse.

### <a name="remove-using-the-azure-cli"></a>Rimuovere usando l'interfaccia della riga di comando di Azure

Dall'interfaccia della riga di comando di Azure eseguire il comando seguente:

```azurecli
az group delete --name ADonAzureVMs
```

## <a name="next-steps"></a>Passaggi successivi

* [Virtualizzazione sicura di Active Directory Domain Services (AD DS)](../../Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md)
* [Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-express)
* [Backup e ripristino](https://docs.microsoft.com/azure/virtual-machines/windows/backup-recovery)
* [Connettività VPN da sito a sito](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)
* [Monitoraggio](https://docs.microsoft.com/azure/virtual-machines/windows/monitor)
* [Sicurezza e criteri](https://docs.microsoft.com/azure/virtual-machines/windows/security-policy)
* [Manutenzione e aggiornamenti](https://docs.microsoft.com/azure/virtual-machines/windows/maintenance-and-updates)
