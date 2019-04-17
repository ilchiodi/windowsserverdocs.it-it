---
title: Distribuire DHCP con Windows PowerShell
description: È possibile utilizzare questo argomento per distribuire un server DHCP di Windows Server 2016 IP (Internet Protocol) versione 4 che fornisce ai client DHCP IPv4 connesso a una o più subnet nella rete automatica di indirizzi IP e le opzioni DHCP.
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: article
ms.assetid: 7110ad21-a33e-48d5-bb3c-129982913bc8
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2be3c02f32229c9b9ee411f5e97305776f9825d8
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="deploy-dhcp-using-windows-powershell"></a>Distribuire DHCP con Windows PowerShell

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Questa guida fornisce istruzioni su come usare Windows PowerShell per distribuire un server \(DHCP\) Dynamic Host Configuration Protocol versione 4 di IP (Internet Protocol) che assegna automaticamente indirizzi IP e le opzioni DHCP a client DHCP IPv4 che sono connessi a una o più subnet nella rete.

>[!NOTE]
>Per scaricare il documento in formato Word dalla TechNet Gallery, vedere [distribuire DHCP con Windows PowerShell in Windows Server 2016](https://gallery.technet.microsoft.com/Deploy-DHCP-Using-Windows-246dd293).

Utilizzo dei server DHCP per assegnare IP indirizzi Salva il carico amministrativo in quanto non è necessario configurare manualmente le impostazioni di TCP/IP v4 per ogni scheda di rete in ogni computer della rete. Con DHCP, TCP/IP v4 configurazione viene eseguita automaticamente quando un computer o un altro client DHCP è connesso alla rete.

È possibile distribuire server DHCP in un gruppo di lavoro come un server autonomo o come parte di un dominio Active Directory.

Questa guida contiene le sezioni seguenti.

- [Cenni preliminari sulla distribuzione di DHCP](#bkmk_overview)
- [Panoramiche delle tecnologie](#bkmk_technologies)
- [Pianificare la distribuzione di DHCP](#bkmk_plan)
- [Utilizzando questa Guida in un ambiente di testing](#bkmk_lab)
- [Distribuzione di DHCP](#bkmk_deploy)
- [Verificare le funzionalità di Server](#bkmk_verify)
- [Comandi di Windows PowerShell per DHCP](#bkmk_dhcpwps)
- [Elenco di comandi di Windows PowerShell in questa Guida](#bkmk_list)

## <a name="bkmk_overview"></a>Cenni preliminari sulla distribuzione di DHCP

La figura seguente illustra lo scenario che è possibile distribuire utilizzando questa Guida. Lo scenario include un server DHCP in un dominio di Active Directory. Il server è configurato per fornire indirizzi IP ai client DHCP in due subnet diverse. Le subnet sono separate da un router che dispone di inoltro DHCP abilitato.

![Panoramica della topologia di rete DHCP](../../media/Core-Network-Guide/cng16_overview.jpg)

## <a name="bkmk_technologies"></a>Panoramiche delle tecnologie

Le sezioni seguenti forniscono una breve panoramica del TCP/IP e DHCP.

### <a name="dhcp-overview"></a>Panoramica di DHCP

DHCP è uno standard IP per semplificare la gestione della configurazione IP dell'host. Lo standard DHCP prevede l'utilizzo di server DHCP come un modo per gestire l'allocazione dinamica degli indirizzi IP e altri dettagli relativi alla configurazione per client DHCP sulla rete.

DHCP consente di utilizzare un server DHCP per assegnare dinamicamente un indirizzo IP a un computer o un altro dispositivo, ad esempio una stampante, nella rete locale, anziché configurare manualmente tutti i dispositivi con un indirizzo IP statico.

In ogni computer in una rete TCP/IP deve essere un indirizzo IP univoco, poiché l'indirizzo IP e il subnet mask correlata identificano sia il computer host e la subnet a cui è collegato al computer. Mediante il protocollo DHCP, è possibile verificare che tutti i computer che sono configurati come client DHCP ricevano un indirizzo IP appropriato per il percorso di rete e subnet e utilizzando le opzioni DHCP, ad esempio gateway predefinito e i server DNS, è possibile fornire automaticamente i client DHCP con le informazioni necessarie per funzionare correttamente nella rete.

Per le reti basate su TCP/IP, il protocollo DHCP riduce la complessità e la quantità di lavoro amministrativo coinvolto nella configurazione dei computer.

### <a name="tcpip-overview"></a>Panoramica di TCP/IP

Per impostazione predefinita, tutte le versioni dei sistemi operativi Windows Server e Client Windows hanno impostazioni TCP/IP IP versione 4 connessioni di rete configurate per ottenere automaticamente un indirizzo IP e altre informazioni, denominati opzioni DHCP, da un server DHCP. Per questo motivo, non è necessario configurare manualmente le impostazioni TCP/IP, a meno che il computer è un computer server o un altro dispositivo che richiede un indirizzo IP statico e configurato manualmente. 

Ad esempio, è consigliabile configurare manualmente l'indirizzo IP del server DHCP e gli indirizzi IP dei server DNS e controller di dominio che eseguono servizi di dominio Active Directory \(AD DS\).

TCP/IP in Windows Server 2016 è la seguente:

-   Software di rete basato su protocolli di rete standard del settore.

-   Protocollo di rete instradabile enterprise che supporta la connessione del computer basato su Windows dalla rete locale (LAN) sia gli ambienti di wide area network (WAN).

-   Tecnologie di base e le utilità per la connessione di computer basati su Windows con sistemi di tipo diverso al fine di condividere informazioni.

-   Una base per l'accesso a servizi Internet globali, ad esempio server Web e FTP File Transfer Protocol ().

-   Un framework client/server multipiattaforma, scalabile e affidabile.

TCP/IP include utilità TCP/IP di base che consentono ai computer basati su Windows di connettersi e condividere informazioni con altri sistemi Microsoft e non Microsoft, tra cui:

- Windows Server 2016

- Windows 10

- Windows Server 2012 R2

- Windows 8.1

- Windows Server 2012

- Windows 8

- Windows Server 2008 R2

- Windows 7

- Windows Server 2008

- Windows Vista

- Host Internet

- Sistemi Apple Macintosh

- Mainframe IBM

- Sistemi UNIX e Linux

- Sistemi Open VMS

- Stampanti di rete

- Tablet e telefoni cellulari con Ethernet cablate o wireless 802.11 abilitato

## <a name="bkmk_plan"></a>Pianificare la distribuzione di DHCP

Di seguito sono i passaggi di pianificazione chiave prima di installare il ruolo server DHCP.

### <a name="planning-dhcp-servers-and-dhcp-forwarding"></a>Pianificazione dei server DHCP e l'inoltro DHCP

Poiché i messaggi DHCP sono messaggi trasmessi, non vengono inoltrati fra le subnet dai router. Se si dispone di più subnet e si desidera fornire il servizio DHCP per ogni subnet, è necessario eseguire una delle operazioni seguenti:

-   Installare un server DHCP in ogni subnet

-   Configurare i router per l'inoltro dei messaggi trasmessi DHCP tra le subnet e configurare più ambiti nel server DHCP, un ambito per ogni subnet.

Nella maggior parte dei casi, configurare i router per l'inoltro dei messaggi trasmessi DHCP è più conveniente della distribuzione di un server DHCP in ogni segmento fisico della rete.

### <a name="planning-ip-address-ranges"></a>Pianificazione di intervalli di indirizzi IP

Ogni subnet deve disporre di un proprio intervallo di indirizzi IP univoco. Tali intervalli sono rappresentati in un server DHCP con ambiti.

Un ambito è un raggruppamento amministrativo di indirizzi IP per i computer in una subnet che utilizzano il servizio DHCP. L'amministratore crea innanzitutto un ambito per ogni subnet fisica e quindi Usa l'ambito per definire i parametri utilizzati dai client.

Un ambito ha le proprietà seguenti:

-   Un intervallo di indirizzi IP da cui si desidera includere o escludere gli indirizzi utilizzati per offerte di lease del servizio DHCP.

-   Una subnet mask, che determina il prefisso di subnet per un determinato indirizzo IP.

-   Un nome di ambito assegnato quando viene creato.

-   Valori di durata del lease, che vengono assegnati ai client DHCP che ricevono gli indirizzi IP allocati dinamicamente.

-   Le opzioni di ambito DHCP configurate per l'assegnazione ai client DHCP, ad esempio l'indirizzo IP del server DNS e indirizzo IP del gateway predefinito al router.

-   Le prenotazioni possono essere utilizzate per garantire che un client DHCP riceva sempre lo stesso indirizzo IP.

Prima di distribuire i server, specificare le subnet e l'intervallo di indirizzi IP che si desidera utilizzare per ogni subnet.

### <a name="planning-subnet-masks"></a>Pianificazione delle subnet mask

ID di rete e ID all'interno di un indirizzo IP dell'host vengono distinti applicando una subnet mask. Ogni subnet mask è un numero a 32 bit che utilizza gruppi di bit consecutivi di tutti gli uno (1) per identificare la rete ID e da soli zero (0) per identificare le parti di ID host di un indirizzo IP.

Ad esempio, la subnet mask normalmente utilizzata con l'indirizzo IP 131.107.16.200 è il seguente numero binario a 32 bit:

```
11111111 11111111 00000000 00000000
```

Questo numero di subnet mask è 16 uno (1) seguiti da 16 bit zero, che indica che le sezioni di ID host e ID rete dell'indirizzo IP sono entrambi 16 bit. In genere, questa subnet mask viene visualizzata nella notazione decimale come 255.255.0.0.

Nella tabella seguente mostra le subnet mask per le classi di indirizzi Internet.

|Classe di indirizzo|BITS per la subnet mask|La subnet mask|
|-----------------|------------------------|---------------|
|Classe A|11111111 00000000 00000000 00000000|255.0.0.0|
|Classe B|11111111 11111111 00000000 00000000|255.255.0.0|
|Classe C|11111111 11111111 11111111 00000000|255.255.255.0|

Quando si crea un ambito in DHCP e si immette l'intervallo di indirizzi IP per l'ambito, DHCP fornisce queste subnet mask predefinite. In genere, subnet mask predefinite sono accettabili per la maggior parte delle reti senza requisiti particolari e in ogni segmento di rete IP corrisponde a una singola rete fisica.

In alcuni casi, è possibile utilizzare subnet mask personalizzate per implementare l'utilizzo di subnet IP. Con la subnet IP, è possibile suddividere la parte di ID host predefinita di un indirizzo IP per specificare le subnet, che sono suddivisioni dell'ID di rete basata su classe originale.

Personalizzando la lunghezza della subnet mask, è possibile ridurre il numero di bit che vengono utilizzati per l'ID host effettivo.

Per evitare problemi di indirizzamento e routing, è necessario assicurarsi che tutti i computer TCP/IP in un segmento di rete utilizzino la stessa subnet mask e che ogni computer o dispositivo ha un indirizzo IP univoco.

### <a name="planning-exclusion-ranges"></a>Pianificazione degli intervalli di esclusione

Quando si crea un ambito in un server DHCP, specificare un intervallo di indirizzi IP che include tutti gli indirizzi IP che il server DHCP è autorizzato a concedere in lease ai client DHCP, ad esempio computer e altri dispositivi. Se si passa quindi e configurare manualmente alcuni server e altri dispositivi con static indirizzi IP dallo stesso intervallo di indirizzi IP che utilizza il server DHCP, è possibile creare accidentalmente un conflitto di indirizzi IP, entrambi in cui il server DHCP avere assegnato lo stesso indirizzo IP a dispositivi diversi.

Per risolvere questo problema, è possibile creare un intervallo di esclusione per l'ambito DHCP. Intervallo di esclusione è un contigui intervallo di indirizzi IP all'interno di intervallo di indirizzi IP dell'ambito che il server DHCP non è consentito utilizzare. Se si crea un intervallo di esclusione, il server DHCP non assegnare gli indirizzi in tale intervallo e consente di assegnare manualmente tali indirizzi senza creare un conflitto di indirizzi IP.

È possibile escludere gli indirizzi IP dalla distribuzione dal server DHCP mediante la creazione di un intervallo di esclusione per ogni ambito. È necessario utilizzare esclusioni per tutti i dispositivi che sono configurati con un indirizzo IP statico. Gli indirizzi esclusi devono includere tutti gli indirizzi IP assegnati manualmente ad altri server, i client non DHCP, workstation senza dischi o i client di Routing e accesso remoto e PPP.

È consigliabile configurare l'intervallo di esclusione con alcuni indirizzi supplementari per supportare l'espansione futura della rete. Nella tabella seguente fornisce un intervallo di esclusione di esempio per un ambito con un intervallo di indirizzi IP di 10.0.0.1 - 10.0.0.254 e una subnet mask 255.255.255.0.

|Elementi di configurazione|Valori di esempio|
|-----------------------|------------------|
|Indirizzo IP iniziale dell'intervallo di esclusione|10.0.0.1|
|Indirizzo IP finale dell'intervallo di esclusione|10.0.0.25|

### <a name="planning-tcpip-static-configuration"></a>Pianificazione della configurazione di TCP/IP statici

Alcuni dispositivi, ad esempio router, server DHCP e server DNS, devono essere configurati con un indirizzo IP statico. Inoltre, potrebbe essere altri dispositivi, ad esempio stampanti, che si desidera abbiano sempre lo stesso indirizzo IP. Elencare i dispositivi che si desidera configurare in modo statico per ogni subnet e quindi pianificare l'intervallo di esclusione che si desidera utilizzare nel server DHCP per verificare che il server DHCP non assegni in lease l'indirizzo IP di un dispositivo configurato in modo statico. Intervallo di esclusione è una sequenza limitata di indirizzi IP all'interno di un ambito esclusi dalle offerte del servizio DHCP. Gli intervalli di esclusione assicurano che tutti gli indirizzi in questi intervalli non sono disponibili dal server per i client DHCP sulla rete.

Ad esempio, se l'intervallo di indirizzi IP per una subnet è 192.168.0.1 tramite 192.168.0.254 e sono presenti dieci dispositivi che si desidera configurare con un indirizzo IP statico, è possibile creare un intervallo di esclusione per il 192.168.0. *x* ambito contenente dieci o più indirizzi IP: 192.168.0.1 tramite 192.168.0.15.

In questo esempio dieci degli indirizzi IP esclusi usi per configurare i server e altri dispositivi con indirizzi IP statici e altri cinque indirizzi IP restano a disposizione per configurazione statica di nuovi dispositivi che si desidera aggiungere in futuro. Con questo intervallo di esclusione, il server DHCP viene lasciato con un pool di indirizzi di 192.168.0.16 tramite 192.168.0.254.

Nella tabella seguente vengono forniti gli elementi di configurazione di esempio aggiuntive per Active Directory e DNS.

|Elementi di configurazione|Valori di esempio|
|-----------------------|------------------|
|Binding connessioni di rete|Ethernet|
|Impostazioni del Server DNS|DC1.corp.contoso.com|
|Indirizzo IP del server DNS preferito|10.0.0.2|
|Valori di ambito<br /><br />1. Nome ambito<br />2. Indirizzo IP iniziale<br />3. Indirizzo IP finale<br />4. Subnet Mask<br />5. Gateway predefinito (facoltativo)<br />6. Durata del lease|1. Subnet principale<br />2.  10.0.0.1<br />3.  10.0.0.254<br />4.  255.255.255.0<br />5.  10.0.0.1<br />6. 8 giorni|
|Modalità operativa di Server DHCP IPv6|Non è abilitato|

## <a name="bkmk_lab"></a>Utilizzando questa Guida in un ambiente di testing

È possibile utilizzare questa Guida alla distribuzione di DHCP in un ambiente di testing prima di distribuire in un ambiente di produzione. 

>[!NOTE]
>Se non si desidera distribuire DHCP in un ambiente di testing, è possibile ignorare la sezione [distribuire DHCP](#bkmk_deploy).

I requisiti per l'ambiente diversi a seconda se si usano server fisici o macchine virtuali \(VMs\) e, se si utilizza un dominio Active Directory o la distribuzione di un server DHCP autonomo. 

È possibile utilizzare le seguenti informazioni per determinare le risorse minime che necessarie per testare la distribuzione di DHCP utilizzando questa Guida.

### <a name="test-lab-requirements-with-vms"></a>Requisiti del laboratorio di test con macchine virtuali

Per distribuire DHCP in un ambiente di testing con macchine virtuali, è necessario le risorse seguenti.

Per la distribuzione di dominio o distribuzione autonoma, è necessario un server configurato come un host Hyper\-V.

**Distribuzione di dominio**

Questa distribuzione richiede un unico server fisico, un commutatore virtuale, due server virtuali e un client virtuale:

Nel server fisici, creare i seguenti elementi nella console di gestione Hyper-V.

1. Un **interna** commutatore virtuale. Non creare un **esterno** virtuale perché, se l'host Hyper\-V è una subnet che include un server DHCP, il test di macchine virtuali riceverà un indirizzo IP dal server DHCP. Inoltre, il server DHCP di test che si distribuisce potrebbe assegnare indirizzi IP in altri computer nella subnet in cui è installato nell'host Hyper\-V.
1. Una macchina virtuale che esegue Windows Server 2106 configurato come controller di dominio con servizi di dominio Active Directory che è connessa al commutatore virtuale interno che è stato creato. Per trovare questa Guida, questo server deve disporre di un indirizzo IP configurato in modo statico di 10.0.0.2. Per informazioni sulla distribuzione di Active Directory, vedere la sezione **la distribuzione di DC1** in Windows Server 2016 [Guida alla rete Core](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide#BKMK_deployADDNS01).
1. Una macchina virtuale che esegue Windows Server 2106 che sarà configurato come server DHCP utilizzando questa Guida e che è connessa alla interna virtuale commutatore creato. 
1. Una macchina virtuale in esecuzione un sistema operativo client di Windows che è connesso a interna virtuale commutatore creato e che si utilizzerà per verificare che il server DHCP è in modo dinamico l'allocazione degli indirizzi IP e le opzioni DHCP ai client DHCP.

**Distribuzione del server DHCP standalone**

Questa distribuzione richiede un unico server fisico, un commutatore virtuale, un server virtuale e un client virtuale:

Nel server fisici, creare i seguenti elementi nella console di gestione Hyper-V.

1. Un **interna** commutatore virtuale. Non creare un **esterno** virtuale perché, se l'host Hyper\-V è una subnet che include un server DHCP, il test di macchine virtuali riceverà un indirizzo IP dal server DHCP. Inoltre, il server DHCP di test che si distribuisce potrebbe assegnare indirizzi IP in altri computer nella subnet in cui è installato nell'host Hyper\-V.
1. Una macchina virtuale che esegue Windows Server 2106 che sarà configurato come server DHCP utilizzando questa Guida e che è connessa alla interna virtuale commutatore creato.
1. Una macchina virtuale in esecuzione un sistema operativo client di Windows che è connesso a interna virtuale commutatore creato e che si utilizzerà per verificare che il server DHCP è in modo dinamico l'allocazione degli indirizzi IP e le opzioni DHCP ai client DHCP.

### <a name="test-lab-requirements-with-physical-servers"></a>Requisiti del laboratorio di test con i server fisici

Per distribuire DHCP in un ambiente di testing con server fisici, è necessario le risorse seguenti.

**Distribuzione di dominio**

Questa distribuzione richiede un hub o switch, due server fisici e un client fisico:

1. Un hub o commutatore Ethernet a cui è possibile connettere i computer fisici con cavi Ethernet
1. Un computer fisico che esegue Windows Server 2106 configurato come controller di dominio con servizi di dominio Active Directory. Per trovare questa Guida, questo server deve disporre di un indirizzo IP configurato in modo statico di 10.0.0.2. Per informazioni sulla distribuzione di Active Directory, vedere la sezione **la distribuzione di DC1** in Windows Server 2016 [Guida alla rete Core](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide#BKMK_deployADDNS01).
1. Un computer fisico che esegue Windows Server 2106 che sarà configurato come server DHCP utilizzando questa Guida. 
1. Un computer fisico che esegue un sistema operativo client di Windows che si utilizzerà per verificare che il server DHCP è in modo dinamico l'allocazione degli indirizzi IP e le opzioni DHCP ai client DHCP.

>[!NOTE]
>Se non è sufficiente computer di test per la distribuzione, è possibile utilizzare un computer di test per servizi di dominio Active Directory e DHCP - tuttavia questa configurazione non è consigliata per un ambiente di produzione.

**Distribuzione del server DHCP standalone**

Questa distribuzione richiede un hub o switch, un unico server fisico e un client fisico:

1. Un hub o commutatore Ethernet a cui è possibile connettere i computer fisici con cavi Ethernet
2. Un computer fisico che esegue Windows Server 2106 che sarà configurato come server DHCP utilizzando questa Guida. 
3. Un computer fisico che esegue un sistema operativo client di Windows che si utilizzerà per verificare che il server DHCP è in modo dinamico l'allocazione degli indirizzi IP e le opzioni DHCP ai client DHCP.


## <a name="bkmk_deploy"></a>Distribuzione di DHCP

Questa sezione fornisce esempi di comandi Windows PowerShell che è possibile utilizzare per la distribuzione di DHCP in un server. Prima di eseguire questi comandi di esempio nel server, è necessario modificare i comandi per trovare la corrispondenza della rete e l'ambiente. 

Ad esempio, prima di eseguire i comandi, devi sostituire i valori di esempio nei comandi per gli elementi seguenti:

- Nomi di computer
- Intervallo di indirizzi IP per ogni ambito a cui che si desidera configurare (1 ambito per ogni subnet)
- Subnet mask per ogni intervallo di indirizzi IP che si desidera configurare
- Nome dell'ambito per ogni ambito
- Intervallo di esclusione per ogni ambito
- Valori di opzione DHCP, ad esempio gateway predefinito, nome di dominio e server DNS o WINS
- Nomi di interfaccia

>[!IMPORTANT]
>Esaminare e modificare tutti i comandi per l'ambiente prima di eseguire il comando.

### <a name="where-to-install-dhcp---on-a-physical-computer-or-a-vm"></a>Posizione in cui installare DHCP - in un computer fisico o una macchina virtuale?

È possibile installare il ruolo server DHCP in un computer fisico o in una macchina virtuale \(VM\) che viene installato in un host Hyper\-V. Se si sta installando DHCP in una macchina virtuale e si desidera che il server DHCP per fornire le assegnazioni di indirizzi IP ai computer nella rete fisica a cui è connesso l'host Hyper-V, è necessario connettersi la scheda di rete virtuale della macchina virtuale a un commutatore virtuale Hyper-V che è **esterno**.

Per ulteriori informazioni, vedere la sezione **creare un commutatore virtuale con Hyper-V Manager** nell'argomento [creare una rete virtuale](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/connect-to-network).

### <a name="run-windows-powershell-as-an-administrator"></a>Eseguire Windows PowerShell come amministratore

È possibile utilizzare la procedura seguente per eseguire Windows PowerShell con privilegi di amministratore.

1. In un computer che esegue Windows Server 2016, fare clic su **Start**, quindi fare doppio clic sull'icona di Windows PowerShell. Viene visualizzato un menu. 

2. Nel menu, fare clic su **più**, quindi fare clic su **Esegui come amministratore**. Se richiesto, digitare le credenziali per un account che dispone di privilegi di amministratore sul computer. Se l'account utente con cui si è connessi al computer è un account di livello di amministratore, non si riceverà una richiesta di credenziali.

3. Verrà visualizzata la finestra di Windows PowerShell con privilegi di amministratore.

### <a name="rename-the-dhcp-server-and-configure-a-static-ip-address"></a>Rinominare il server DHCP e configurare un indirizzo IP statico

Se non è già stato fatto, è possibile utilizzare i comandi di Windows PowerShell seguenti per rinominare il server DHCP e configurare un indirizzo IP statico per il server.

**Configurare un indirizzo IP statico**

È possibile utilizzare i comandi seguenti per assegnare un indirizzo IP statico per il server DHCP e per configurare le proprietà TCP/IP del server DHCP con l'indirizzo IP di server DNS corretto. È inoltre necessario sostituire i nomi di interfaccia e gli indirizzi IP in questo esempio con i valori che si desidera utilizzare per configurare il computer.

 `New-NetIPAddress -IPAddress 10.0.0.3 -InterfaceAlias "Ethernet" -DefaultGateway 10.0.0.1 -AddressFamily IPv4 -PrefixLength 24`

 `Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 10.0.0.2`

Per ulteriori informazioni su questi comandi, vedere gli argomenti seguenti.

- [New-NetIPAddress](https://technet.microsoft.com/itpro/powershell/windows/tcpip/new-netipaddress)
- [Set-DnsClientServerAddress](https://technet.microsoft.com/itpro/powershell/windows/dns-client/set-dnsclientserveraddress)

**Rinominare il computer**

È possibile utilizzare i comandi seguenti per rinominare e riavviare il computer.

`Rename-Computer -Name DHCP1`

 `Restart-Computer`

Per ulteriori informazioni su questi comandi, vedere gli argomenti seguenti.

- [Rename-Computer](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.management/rename-computer)
- [Restart-Computer](https://msdn.microsoft.com/powershell/reference/4.0/microsoft.powershell.management/restart-computer)

### <a name="join-the-computer-to-the-domain-optional"></a>Aggiungere il computer al \(Optional\) dominio

Se si installa il server DHCP in un ambiente di dominio Active Directory, è necessario aggiungere il computer al dominio. Aprire Windows PowerShell con privilegi di amministratore e quindi eseguire il comando seguente dopo la sostituzione il nome NetBios del dominio **CORP** con un valore appropriato per l'ambiente.

    Add-Computer CORP

Quando richiesto, digitare le credenziali per un account utente di dominio che dispone dell'autorizzazione per aggiungere un computer al dominio. 

    Restart-Computer

Per ulteriori informazioni sul comando Add-Computer, vedere l'argomento seguente.

- [Add-Computer](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.management/add-computer)

### <a name="install-dhcp"></a>Installare DHCP

Dopo il riavvio del computer, aprire Windows PowerShell con privilegi di amministratore e quindi installare DHCP eseguendo il comando seguente.

    Install-WindowsFeature DHCP -IncludeManagementTools

Per ulteriori informazioni su questo comando, vedere l'argomento seguente.

- [Install-WindowsFeature](https://technet.microsoft.com/itpro/powershell/windows/server-manager/install-windowsfeature)

### <a name="create-dhcp-security-groups"></a>Creare gruppi di sicurezza DHCP

Per creare gruppi di sicurezza, eseguire un comando \(netsh\) Shell di rete in Windows PowerShell e quindi riavviare il servizio DHCP in modo che i nuovi gruppi diventano attivi.

Quando si esegue il comando netsh seguente nel server DHCP, il **DHCP Administrators** e **DHCP Users** gruppi di sicurezza vengono creati in **utenti e gruppi locali** nel server DHCP.

    netsh dhcp add securitygroups

Il comando seguente riavvia il servizio DHCP nel computer locale.

    Restart-service dhcpserver

Per ulteriori informazioni su questi comandi, vedere gli argomenti seguenti.

- [Network Shell (Netsh)](../netsh/netsh.md)
- [Restart-Service](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.management/restart-service)

### <a name="authorize-the-dhcp-server-in-active-directory-optional"></a>Autorizzare il server DHCP in Active Directory \(Optional\)

Se si installa DHCP in un ambiente di dominio, è necessario eseguire i passaggi seguenti per autorizzare il server DHCP per il funzionamento del dominio.

>[!NOTE]
>Server DHCP non autorizzati che vengono installati nei domini di Active Directory non può funzionare correttamente e non lease di indirizzi IP ai client DHCP. La disattivazione automatica di server DHCP non autorizzati è una funzionalità di sicurezza che impedisce l'assegnazione di indirizzi IP non corretti ai client nella rete di server DHCP non autorizzati.

È possibile utilizzare il comando seguente per aggiungere il server DHCP all'elenco dei server DHCP autorizzati in Active Directory. 

>[!NOTE]
>Se non hai un ambiente di dominio, non eseguire questo comando.

 `Add-DhcpServerInDC -DnsName DHCP1.corp.contoso.com -IPAddress 10.0.0.3`

Per verificare che il server DHCP è autorizzato in Active Directory, è possibile utilizzare il comando seguente.

    Get-DhcpServerInDC

Di seguito sono i risultati di esempio che vengono visualizzati in Windows PowerShell.

    
        IPAddress   DnsName
        ---------   -------
        10.0.0.3    DHCP1.corp.contoso.com
    

Per ulteriori informazioni su questi comandi, vedere gli argomenti seguenti.

- [Add-DhcpServerInDC](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/add-dhcpserverindc)
- [Get-DhcpServerInDC](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/get-dhcpserverindc)

### <a name="notify-server-manager-that-post-install-dhcp-configuration-is-complete-optional"></a>Notificare tale DHCP post\-installazione di Server Manager configurazione è \(Optional\) completo

Dopo aver completato le attività post\-installazione, ad esempio la creazione di gruppi di sicurezza e autorizzare il server DHCP in Active Directory, Server Manager potrebbe comunque in grado di visualizzare un avviso nell'interfaccia utente che informa che è necessario completare i passaggi dell'installazione di post\ utilizzando la configurazione di DHCP Post installazione guidata.

Questo messaggio now\-non necessari e accurato per impedire che venga visualizzato in Server Manager configurando la seguente chiave del Registro di sistema con questo comando di Windows PowerShell.

    Set-ItemProperty –Path registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ServerManager\Roles\12 –Name ConfigurationState –Value 2

Per ulteriori informazioni su questo comando, vedere l'argomento seguente.

- [Set-ItemProperty](https://msdn.microsoft.com/powershell/reference/4.0/microsoft.powershell.management/set-itemproperty?f=255&MSPPError=-2147217396)

### <a name="set-server-level-dns-dynamic-update-configuration-settings-optional"></a>Impostare server livello DNS aggiornamento dinamico configurazione impostazioni \(Optional\)

Se si desidera che il server DHCP per eseguire gli aggiornamenti dinamici DNS per i computer client DHCP, è possibile eseguire il comando seguente per configurare questa impostazione. Si tratta di un livello di server, l'impostazione, non un ambito livello impostazione, in modo che avrà effetto su tutti gli ambiti configurati nel server. Questo comando di esempio consente di configurare anche il server DHCP per eliminare i record di risorse DNS per i client quando il client almeno scade.

    Set-DhcpServerv4DnsSetting -ComputerName "DHCP1.corp.contoso.com" -DynamicUpdates "Always" -DeleteDnsRRonLeaseExpiry $True

È possibile utilizzare il comando seguente per configurare le credenziali che il server DHCP Usa per registrare o annullare la registrazione di record client in un server DNS. Questo esempio salva una credenziale in un server DHCP. Il primo comando Usa **Get-Credential** per creare un **PSCredential** oggetto e quindi archivia l'oggetto nel **$Credential** variabile. Il comando viene richiesto per nome utente e password, pertanto, assicurarsi di fornire le credenziali per un account che dispone dell'autorizzazione per aggiornare il record di risorse sul server DNS.

    
    $Credential = Get-Credential
    Set-DhcpServerDnsCredential -Credential $Credential -ComputerName "DHCP1.corp.contoso.com"
    

Per ulteriori informazioni su questi comandi, vedere gli argomenti seguenti.

- [Set-DhcpServerv4DnsSetting](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/set-dhcpserverv4dnssetting)
- [Set-DhcpServerDnsCredential](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/set-dhcpserverdnscredential)

### <a name="configure-the-corpnet-scope"></a>Configurare l'ambito della rete aziendale

Al termine dell'installazione di DHCP, è possibile utilizzare i comandi seguenti per configurare e attivare l'ambito della rete aziendale, creare un intervallo di esclusione per l'ambito e configurare il gateway predefinito di opzioni DHCP, indirizzo IP del server DNS e nome di dominio DNS.

    
    Add-DhcpServerv4Scope -name "Corpnet" -StartRange 10.0.0.1 -EndRange 10.0.0.254 -SubnetMask 255.255.255.0 -State Active`
    
    Add-DhcpServerv4ExclusionRange -ScopeID 10.0.0.0 -StartRange 10.0.0.1 -EndRange 10.0.0.15`
    
    Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.0.1 -ScopeID 10.0.0.0 -ComputerName DHCP1.corp.contoso.com`
    
    Set-DhcpServerv4OptionValue -DnsDomain corp.contoso.com -DnsServer 10.0.0.2
    

Per ulteriori informazioni su questi comandi, vedere gli argomenti seguenti.

- [DhcpServerv4Scope aggiungere](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/add-dhcpserverv4scope)
- [DhcpServerv4ExclusionRange aggiungere](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/add-dhcpserverv4exclusionrange)
- [Set-DhcpServerv4OptionValue](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/set-dhcpserverv4optionvalue)

### <a name="configure-the-corpnet2-scope-optional"></a>Configurare il Corpnet2 ambito \(Optional\)

Se si dispone di una subnet seconda che è connesso alla subnet del primo con un router in cui è abilitata l'inoltro DHCP, è possibile utilizzare i comandi seguenti per aggiungere un secondo ambito, denominato Corpnet2 per questo esempio. In questo esempio anche configurato un intervallo di esclusione e l'indirizzo IP del gateway predefinito \ (il router indirizzo IP di subnet\) della subnet Corpnet2.

 `Add-DhcpServerv4Scope -name "Corpnet2" -StartRange 10.0.1.1 -EndRange 10.0.1.254 -SubnetMask 255.255.255.0 -State Active`

 `Add-DhcpServerv4ExclusionRange -ScopeID 10.0.1.0 -StartRange 10.0.1.1 -EndRange 10.0.1.15`

 `Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.1.1 -ScopeID 10.0.1.0 -ComputerName DHCP1.corp.contoso.com`

Se hai altre subnet servite dal server DHCP, è possibile ripetere questi comandi, con valori diversi per tutti i parametri di comando, aggiungere gli ambiti per ogni subnet.

>[!IMPORTANT]
>Assicurarsi che tutti i router tra i client DHCP e server DHCP sono configurati per l'inoltro dei messaggi DHCP. Vedere la documentazione del router per informazioni su come configurare l'inoltro DHCP.

## <a name="bkmk_verify"></a>Verificare le funzionalità di Server

Per verificare che il server DHCP fornisce allocazione dinamica degli indirizzi IP ai client DHCP, un altro computer per connettersi a una subnet elaborata. Dopo aver connesso il cavo Ethernet per la scheda di rete e la potenza del computer, richiederà un indirizzo IP dal server DHCP. È possibile verificare la corretta configurazione utilizzando il **ipconfig /all** comando ed esaminato i risultati oppure eseguendo i test di connettività, ad esempio il tentativo di accedere alle risorse Web con i browser o condivisione di file con Esplora risorse o altre applicazioni.

Se il client non riceve un indirizzo IP dal server DHCP, eseguire i passaggi di risoluzione dei problemi seguenti.

1. Assicurarsi che il cavo Ethernet collegato sia il computer e il Ethernet commutatori, hub o router.
1. Se il computer client è collegato a un segmento di rete è separato dal server DHCP da un router, assicurarsi che il router è configurato per l'inoltro di messaggi DHCP.
1. Assicurarsi che il server DHCP è autorizzato in Active Directory eseguendo il comando seguente per recuperare l'elenco dei server DHCP autorizzati da Active Directory. [Get-DhcpServerInDC](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/get-dhcpserverindc).
1. Assicurarsi che gli ambiti sono attivati, aprire la console DHCP \ (Server Manager, **strumenti**, **DHCP**\), espandere l'albero del server per esaminare gli ambiti e quindi facendo clic con ogni ambito. Se il menu risulta comprende la selezione **attiva**, fare clic su **attiva**. \ (Se l'ambito è già attivato, la selezione di menu legge **disattiva**. \) 

## <a name="bkmk_dhcpwps"></a>Comandi di Windows PowerShell per DHCP

Il riferimento seguente fornisce le descrizioni comandi e sintassi per tutti i comandi di Windows PowerShell di Server DHCP per Windows Server 2016. L'argomento sono elencati i comandi in ordine alfabetico in base al verbo all'inizio dei comandi, ad esempio **ottenere** o **impostare**.

>[!NOTE]
>In Windows Server 2012 R2, è possibile utilizzare i comandi di Windows Server 2016.

- [Modulo DhcpServer](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/index)

Il riferimento seguente fornisce le descrizioni comandi e sintassi per tutti i comandi di Windows PowerShell di Server DHCP per Windows Server 2012 R2. L'argomento sono elencati i comandi in ordine alfabetico in base al verbo all'inizio dei comandi, ad esempio **ottenere** o **impostare**.

>[!NOTE]
>In Windows Server 2016, è possibile utilizzare i comandi di Windows Server 2012 R2.

- [Cmdlet per Server DHCP in Windows PowerShell](https://technet.microsoft.com/library/jj590751.aspx)

## <a name="bkmk_list"></a>Elenco di comandi di Windows PowerShell in questa Guida

Ecco un semplice elenco di comandi e i valori di esempio utilizzati in questa Guida.

    
    New-NetIPAddress -IPAddress 10.0.0.3 -InterfaceAlias "Ethernet" -DefaultGateway 10.0.0.1 -AddressFamily IPv4 -PrefixLength 24
    Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 10.0.0.2
    Rename-Computer -Name DHCP1
    Restart-Computer
    
    Add-Computer CORP
    Restart-Computer
    
    Install-WindowsFeature DHCP -IncludeManagementTools
    netsh dhcp add securitygroups
    Restart-service dhcpserver
    
    Add-DhcpServerInDC -DnsName DHCP1.corp.contoso.com -IPAddress 10.0.0.3
    Get-DhcpServerInDC
    
    Set-ItemProperty –Path registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ServerManager\Roles\12 –Name ConfigurationState –Value 2
    
    Set-DhcpServerv4DnsSetting -ComputerName "DHCP1.corp.contoso.com" -DynamicUpdates "Always" -DeleteDnsRRonLeaseExpiry $True
    
    $Credential = Get-Credential
    Set-DhcpServerDnsCredential -Credential $Credential -ComputerName "DHCP1.corp.contoso.com"
    
    rem At prompt, supply credential in form DOMAIN\user, password
    
    
    rem Configure scope Corpnet
    
    Add-DhcpServerv4Scope -name "Corpnet" -StartRange 10.0.0.1 -EndRange 10.0.0.254 -SubnetMask 255.255.255.0 -State Active
    
    Add-DhcpServerv4ExclusionRange -ScopeID 10.0.0.0 -StartRange 10.0.0.1 -EndRange 10.0.0.15
    
    Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.0.1 -ScopeID 10.0.0.0 -ComputerName DHCP1.corp.contoso.com
    
    Set-DhcpServerv4OptionValue -DnsDomain corp.contoso.com -DnsServer 10.0.0.2
    
    rem Configure scope Corpnet2
    
    Add-DhcpServerv4Scope -name "Corpnet2" -StartRange 10.0.1.1 -EndRange 10.0.1.254 -SubnetMask 255.255.255.0 -State Active
    
    Add-DhcpServerv4ExclusionRange -ScopeID 10.0.1.0 -StartRange 10.0.1.1 -EndRange 10.0.1.15
    
    Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.1.1 -ScopeID 10.0.1.0 -ComputerName DHCP1.corp.contoso.com
    


