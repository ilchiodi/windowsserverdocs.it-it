---
title: Distribuire DHCP mediante Windows PowerShell
description: È possibile utilizzare questo argomento per distribuire un server DHCP di Windows Server 2016 IP (Internet Protocol) versione 4 che fornisce gli indirizzi IP automatici e le opzioni DHCP ai client DHCP IPv4 connesso a una o più subnet nella rete.
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: article
ms.assetid: 7110ad21-a33e-48d5-bb3c-129982913bc8
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8a53f6293067fa0f7014e7794696cf75f7179545
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849092"
---
# <a name="deploy-dhcp-using-windows-powershell"></a>Distribuire DHCP mediante Windows PowerShell

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Questa guida vengono fornite istruzioni su come usare Windows PowerShell per distribuire un IP (Internet Protocol) versione 4 Dynamic Host Configuration Protocol \(DHCP\) opzioni server che consente di assegnare automaticamente indirizzi IP e DHCP a IPv4 DHCP client connessi a una o più subnet nella rete.

>[!NOTE]
>Per scaricare il documento in formato Word dalla TechNet Gallery, vedere [distribuire DHCP con Windows PowerShell in Windows Server 2016](https://gallery.technet.microsoft.com/Deploy-DHCP-Using-Windows-246dd293).

Usando i server DHCP per assegnare IP indirizzi Salva il carico amministrativo poiché non è necessario configurare manualmente le impostazioni di TCP/IP v4 per ogni scheda di rete in ogni computer nella rete. Con DHCP, TCP/IP v4 configurazione viene eseguita automaticamente quando un computer o altri client DHCP è connesso alla rete.

È possibile distribuire il server DHCP in un gruppo di lavoro come server autonomo o come parte di un dominio di Active Directory.

Questa guida contiene le sezioni seguenti.

- [Cenni preliminari sulla distribuzione di DHCP](#bkmk_overview)
- [Panoramiche sulla tecnologia](#bkmk_technologies)
- [Pianificare la distribuzione di DHCP](#bkmk_plan)
- [Utilizzando questa Guida in un Lab di Test](#bkmk_lab)
- [Distribuire il DHCP](#bkmk_deploy)
- [Verificare le funzionalità di Server](#bkmk_verify)
- [Comandi di Windows PowerShell per DHCP](#bkmk_dhcpwps)
- [Elenco di comandi di Windows PowerShell in questa Guida](#bkmk_list)

## <a name="bkmk_overview"></a>Cenni preliminari sulla distribuzione di DHCP

La figura seguente illustra lo scenario che è possibile distribuire in questa Guida. Lo scenario include un server DHCP in un dominio di Active Directory. Il server è configurato per fornire indirizzi IP ai client DHCP in due diverse subnet. Le subnet sono separate da un router che ha attivato l'inoltro DHCP.

![Panoramica della topologia di rete DHCP](../../media/Core-Network-Guide/cng16_overview.jpg)

## <a name="bkmk_technologies"></a>Panoramiche sulla tecnologia

Le sezioni seguenti forniscono una breve panoramica di TCP/IP e DHCP.

### <a name="dhcp-overview"></a>Panoramica di DHCP

DHCP è uno standard IP che semplifica la gestione della configurazione IP degli host. Lo standard DHCP prevede l'utilizzo di server DHCP nella gestione dell'allocazione dinamica degli indirizzi IP e in altri dettagli correlati alla configurazione dei client della rete abilitati per DHCP.

DHCP consente di usare un server DHCP per assegnare dinamicamente un indirizzo IP a un computer o un altro dispositivo, ad esempio una stampante, sulla rete locale, anziché configurare manualmente ogni dispositivo con un indirizzo IP statico.

Ogni computer di una rete TCP/IP deve avere un indirizzo IP univoco, poiché questo indirizzo IP e la subnet mask correlata identificano sia il computer host, sia la subnet alla quale il computer è collegato. Utilizzando DHCP si assicura che tutti i computer configurati come client DHCP ricevano un indirizzo IP idoneo per il percorso di rete e la subnet. Inoltre utilizzando le opzioni DHCP, ad esempio gateway e server DNS predefiniti, è possibile fornire automaticamente ai client DHCP le informazioni necessarie per il loro corretto funzionamento nella rete.

Per le reti basate su TCP/IP, il protocollo DHCP riduce la complessità e la quantità delle attività amministrative necessarie nella configurazione dei computer.

### <a name="tcpip-overview"></a>Panoramica di TCP/IP

Per impostazione predefinita, tutte le versioni dei sistemi operativi Windows Server e Client Windows hanno impostazioni TCP/IP IP versione 4 le connessioni di rete configurate per ottenere automaticamente un indirizzo IP e altre informazioni, denominate opzioni DHCP, da un server DHCP. Per questo motivo, non è necessario configurare manualmente le impostazioni TCP/IP, a meno che il computer è un computer server o un altro dispositivo che richiede un indirizzo IP statico, configurato manualmente. 

Ad esempio, si consiglia di configurare manualmente l'indirizzo IP del server DHCP e gli indirizzi IP dei server DNS e controller di dominio che eseguono servizi di dominio Active Directory \(Active Directory Domain Services\).

TCP/IP in Windows Server 2016 è la seguente:

-   Software di rete basato su protocolli di rete standard.

-   Un protocollo instradabile per reti aziendali che supporta la connessione di un computer basato su Windows sia alla rete locale (LAN), sia ad ambienti di rete WAN (Wide Area Network).

-   Utilità e tecnologie di base per la connessione di computer basati su Windows a sistemi di tipo diverso al fine di condividere informazioni.

-   Una base per l'accesso a servizi Internet globali, ad esempio server Web e FTP File Transfer Protocol ().

-   Un framework client/server multipiattaforma affidabile e scalabile.

Il servizio TCP/IP include utilità TCP/IP di base che consentono ai computer basati su Windows di connettersi e condividere informazioni con altri sistemi Microsoft e non Microsoft, quali:

- Windows Server 2016

- Windows 10

- Windows Server 2012 R2

- Windows 8.1

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

- Tablet e telefoni cellulari con Ethernet cablate e senza fili 802.11 abilitato

## <a name="bkmk_plan"></a>Pianificare la distribuzione di DHCP

Ecco i passaggi di pianificazione chiave prima di installare il ruolo server DHCP.

### <a name="planning-dhcp-servers-and-dhcp-forwarding"></a>Pianificazione dei server DHCP e dell'inoltro DHCP

Poiché i messaggi DHCP sono messaggi trasmessi, non vengono inoltrati fra le subnet dai router. Se sono disponibili più subnet e si desidera fornire il servizio DHCP per ogni subnet, è necessario eseguire una delle operazioni seguenti:

-   Installare un server DHCP in ogni subnet

-   Configurare i router per l'inoltro dei messaggi trasmessi DHCP tra le subnet e configurare un ambito per ogni subnet sul server DHCP.

Nella maggior parte dei casi è più conveniente configurare i router per l'inoltro dei messaggi trasmessi DHCP che non distribuire un server DHCP in ogni segmento fisico della rete.

### <a name="planning-ip-address-ranges"></a>Pianificazione degli intervalli di indirizzi IP

Ogni subnet deve disporre di un proprio intervallo di indirizzi IP univoco. In un server DHCP tali intervalli sono rappresentati dagli ambiti.

Un ambito è un raggruppamento amministrativo di indirizzi IP per i computer di una subnet che utilizzano il servizio DHCP. L'amministratore crea innanzitutto un ambito per ogni subnet fisica, dopodiché lo utilizza per definire i parametri utilizzati dai client.

Ogni ambito è caratterizzato dalle proprietà seguenti:

-   Un intervallo di indirizzi IP in cui includere o escludere gli indirizzi utilizzati per le offerte di lease del servizio DHCP.

-   Una subnet mask, che determina il prefisso subnet per un determinato indirizzo IP.

-   Un nome di ambito assegnato al momento della creazione.

-   I valori di durata dei lease, che vengono assegnati ai client DHCP che ricevono gli indirizzi IP allocati dinamicamente.

-   Tutte le opzioni relative all'ambito DHCP configurate per l'assegnazione ai client DHCP, ad esempio l'indirizzo IP del server DNS e l'indirizzo IP del router/gateway predefinito.

-   Facoltativamente è possibile utilizzare le prenotazioni per garantire che un client DHCP riceva sempre lo stesso indirizzo IP.

Prima di distribuire i server, specificare le subnet e l'intervallo di indirizzi IP che si desidera utilizzare per ogni subnet.

### <a name="planning-subnet-masks"></a>Pianificazione delle subnet mask

All'interno di un indirizzo IP, l'ID di rete e l'ID dell'host vengono distinti applicando una subnet mask. Una subnet mask è un numero di 32 bit che utilizza gruppi di bit consecutivi composti da soli uno (1) per identificare la parte dell'indirizzo IP corrispondente all'ID di rete e da soli zero (0) per identificare la parte corrispondente all'ID dell'host.

La subnet mask normalmente utilizzata con l'indirizzo IP 131.107.16.200, ad esempio, è il seguente numero binario di 32 bit:

```
11111111 11111111 00000000 00000000
```

Questo numero di subnet mask è 16 uno (1) seguiti da 16 bit zero, che indica che le sezioni di ID host e ID di rete dell'indirizzo IP sono entrambi 16 bit. Questa subnet mask viene in genere visualizzata in notazione decimale puntata, ovvero 255.255.0.0.

Nella tabella seguente sono riportate le subnet mask per le classi di indirizzi Internet.

|Classe di indirizzi|Subnet mask in formato binario|Subnet mask|
|-----------------|------------------------|---------------|
|Classe A|11111111 00000000 00000000 00000000|255.0.0.0|
|Classe B|11111111 11111111 00000000 00000000|255.255.0.0|
|Classe C|11111111 11111111 11111111 00000000|255.255.255.0|

Quando si crea un ambito in DHCP e si immette l'intervallo di indirizzi IP per tale ambito, DHCP fornisce queste subnet mask predefinite. In genere i valori delle subnet mask predefinite sono accettabili per la maggioranza delle reti senza requisiti particolari, in cui il segmento della rete IP corrisponde a una sola rete fisica.

In alcuni casi è possibile utilizzare subnet mask personalizzate per implementare la suddivisione in subnet degli indirizzi IP. Con la suddivisione in subnet degli indirizzi IP è possibile suddividere la porzione predefinita relativa all'ID dell'host di un indirizzo IP in modo da specificare le subnet, che sono a loro volta suddivisioni dell'ID di rete basato su classe originale.

Personalizzando la lunghezza della subnet mask è possibile ridurre il numero di bit utilizzato per l'ID effettivo dell'host.

Per evitare problemi di indirizzamento e routing, è consigliabile verificare che tutti i computer TCP/IP connessi a un determinato segmento di rete utilizzino la stessa subnet e che ogni computer o dispositivo utilizzi un indirizzo IP univoco.

### <a name="planning-exclusion-ranges"></a>Pianificazione degli intervalli di esclusione

Quando si crea un ambito in un server DHCP, si specifica un intervallo di indirizzi IP comprendente tutti gli indirizzi IP che il server DHCP è autorizzato a concedere in lease ai client DHCP, ad esempio computer e altri dispositivi. Se in seguito si configurano manualmente alcuni server e altri dispositivi con indirizzi IP statici compresi nello stesso intervallo di indirizzi IP utilizzato dal server DHCP, potrebbe crearsi un confitto involontario di indirizzi IP per cui l'utente e il server DHCP assegnano lo stesso indirizzo IP a dispositivi diversi.

Per risolvere questo problema, è possibile creare un intervallo di esclusione per l'ambito DHCP. Intervallo di esclusione è un contigui intervallo di indirizzi IP nell'intervallo di indirizzi IP dell'ambito che il server DHCP non è consentito utilizzare. Se si crea un intervallo di esclusione, il server DHCP non assegnerà gli indirizzi compresi in tale intervallo e consentirà così l'assegnazione manuale di tali indirizzi senza creare nessun conflitto di indirizzi IP.

È possibile impedire al server DHCP di distribuire specifici indirizzi IP creando un intervallo di esclusione per ogni ambito. È consigliabile utilizzare le esclusioni per tutti i dispositivi configurati con un indirizzo IP statico. Dovrebbero far parte degli indirizzi esclusi tutti gli indirizzi IP che sono stati assegnati manualmente ad altri server, a client non DHCP, a workstation senza disco o a client di Routing e Accesso remoto e PPP.

È consigliabile configurare l'intervallo di esclusione con alcuni indirizzi supplementari per tenere conto dell'eventuale espansione futura della rete. Nella tabella seguente fornisce un intervallo di esclusione di esempio per un ambito con un indirizzo IP 10.0.0.1 - 10.0.0.254 e una subnet mask 255.255.255.0.

|Elementi di configurazione|Valori di esempio|
|-----------------------|------------------|
|Indirizzo IP iniziale dell'intervallo di esclusione|10.0.0.1|
|Indirizzo IP finale dell'intervallo di esclusione|10.0.0.25|

### <a name="planning-tcpip-static-configuration"></a>Pianificazione della configurazione degli indirizzi TCP/IP statici

Alcuni dispositivi, ad esempio i router, i server DHCP e i server DNS, devono essere configurati con un indirizzo IP statico. Nella rete potrebbero essere presenti anche altri dispositivi, ad esempio stampanti, che si desidera abbiano sempre lo stesso indirizzo IP. Creare un elenco dei dispositivi da configurare in modo statico per ogni subnet, quindi pianificare l'intervallo di esclusione da utilizzare sul server DHCP per garantire che quest'ultimo non assegni in lease l'indirizzo IP di un dispositivo configurato in modo statico. L'intervallo di esclusione è una sequenza limitata di indirizzi IP all'interno di un ambito, che viene esclusa dalle offerte del servizio DHCP. Gli indirizzi inclusi in un intervallo di esclusione non possono essere offerti dal server ai client DHCP della rete.

Se a una determinata subnet è associato l'intervallo di indirizzi IP da 192.168.0.1 a 192.168.0.254 e sono presenti dieci dispositivi che si desidera configurare con un indirizzo IP statico, per l'ambito 192.168.0.*x* sarà possibile creare un intervallo di esclusione contenente dieci o più indirizzi IP, ad esempio da 192.168.0.1 a 192.168.0.15.

In questo esempio dieci degli indirizzi IP esclusi vengono utilizzati per configurare i server e gli altri dispositivi con gli indirizzi IP statici, mentre altri cinque indirizzi IP restano a disposizione per l'eventuale configurazione statica di nuovi dispositivi aggiunti in seguito. Con questo intervallo di esclusione, il server DHCP ha ancora a disposizione gli indirizzi compresi tra 192.168.0.16 e 192.168.0.254.

Nella tabella seguente vengono forniti gli elementi di configurazione di esempio aggiuntive per Active Directory e DNS.

|Elementi di configurazione|Valori di esempio|
|-----------------------|------------------|
|Binding connessioni di rete|Ethernet|
|Impostazioni server DNS|DC1.corp.contoso.com|
|Indirizzo IP del server DNS preferito|10.0.0.2|
|Valori di ambito<br /><br />1.  Nome dell'ambito<br />2.  Indirizzo IP iniziale<br />3.  Indirizzo IP finale<br />4.  Subnet mask<br />5.  Gateway predefinito (facoltativo)<br />6.  Durata del lease|1.  Subnet principale<br />2.  10.0.0.1<br />3.  10.0.0.254<br />4.  255.255.255.0<br />5.  10.0.0.1<br />6. 8 giorni|
|Modalità operativa server DHCP IPv6|Non abilitato|

## <a name="bkmk_lab"></a>Utilizzando questa Guida in un Lab di Test

È possibile usare questa guida per la distribuzione di DHCP in un lab di test prima di distribuire in un ambiente di produzione. 

>[!NOTE]
>Se non vuoi distribuire DHCP in un lab di test, è possibile passare alla sezione [distribuire DHCP](#bkmk_deploy).

I requisiti per l'ambiente lab variano a seconda se si usa macchine virtuali o server fisici \(macchine virtuali\), e se si usa un dominio Active Directory o la distribuzione di un server DHCP autonomo. 

È possibile usare le informazioni seguenti per determinare le risorse minime che necessarie per eseguire test usando questa Guida alla distribuzione di DHCP.

### <a name="test-lab-requirements-with-vms"></a>Requisiti di Lab di test con le macchine virtuali

Per distribuire il DHCP in un lab di test con le macchine virtuali, è necessario le risorse seguenti.

Per la distribuzione del dominio o distribuzione autonoma, è necessario un server configurato come una Hyper\-host V.

**Distribuzione del dominio**

Questa distribuzione richiede un unico server fisico, un commutatore virtuale, due server virtuali e un client virtuale:

Nel server fisico, creare gli elementi seguenti nella console di gestione Hyper-V.

1. Uno **Internal** commutatore virtuale. Non creare un **esterne** commutatore virtuale, perché se Hyper\-V host si trova in una subnet che include un server DHCP, le macchine virtuali di test verranno visualizzato un indirizzo IP dal server DHCP. Inoltre, il server DHCP di test che si distribuisce potrebbe assegnare indirizzi IP in altri computer sulla subnet in cui il Hyper\-host V è installato.
1. Una macchina virtuale che esegue Windows Server 2016 configurate come controller di dominio con servizi di dominio Active Directory che è connessa al commutatore virtuale interno che è stato creato. Per trovare questa Guida, questo server deve avere un indirizzo IP configurato staticamente 10.0.0.2. Per informazioni sulla distribuzione di Active Directory Domain Services, vedere la sezione **distribuzione di DC1** in Windows Server 2016 [Guida alla rete Core](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide#BKMK_deployADDNS01).
1. Una macchina virtuale che esegue Windows Server 2016 che sarà configurato come server DHCP utilizzando questa Guida e che sia connessa alla interna virtuale passare creato. 
1. Una macchina virtuale in esecuzione un sistema operativo client Windows che è connesso a interna virtuale passare creato e che userà per verificare che il server DHCP sta allocando in modo dinamico gli indirizzi IP e le opzioni DHCP ai client DHCP.

**Distribuzione di server DHCP standalone**

Questa distribuzione richiede un unico server fisico, un commutatore virtuale, un server virtuale e un client virtuale:

Nel server fisico, creare gli elementi seguenti nella console di gestione Hyper-V.

1. Uno **Internal** commutatore virtuale. Non creare un **esterne** commutatore virtuale, perché se Hyper\-V host si trova in una subnet che include un server DHCP, le macchine virtuali di test verranno visualizzato un indirizzo IP dal server DHCP. Inoltre, il server DHCP di test che si distribuisce potrebbe assegnare indirizzi IP in altri computer sulla subnet in cui il Hyper\-host V è installato.
1. Una macchina virtuale che esegue Windows Server 2016 che sarà configurato come server DHCP utilizzando questa Guida e che sia connessa alla interna virtuale passare creato.
1. Una macchina virtuale in esecuzione un sistema operativo client Windows che è connesso a interna virtuale passare creato e che userà per verificare che il server DHCP sta allocando in modo dinamico gli indirizzi IP e le opzioni DHCP ai client DHCP.

### <a name="test-lab-requirements-with-physical-servers"></a>Requisiti di Lab di test con i server fisici

Per distribuire il DHCP in un lab di test con i server fisici, è necessario le risorse seguenti.

**Distribuzione del dominio**

Questa distribuzione richiede un unico hub o switch, due server fisici e un client fisico:

1. Un hub Ethernet o switch a cui è possibile connettere i computer fisici con cavi Ethernet
1. Un computer fisico che esegue Windows Server 2016 configurate come controller di dominio con Active Directory Domain Services. Per trovare questa Guida, questo server deve avere un indirizzo IP configurato staticamente 10.0.0.2. Per informazioni sulla distribuzione di Active Directory Domain Services, vedere la sezione **distribuzione di DC1** in Windows Server 2016 [Guida alla rete Core](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide#BKMK_deployADDNS01).
1. Un computer fisico che esegue Windows Server 2016 che sarà configurato come server DHCP utilizzando questa Guida. 
1. Un computer fisico esegue un sistema operativo client Windows che si userà per verificare che il server DHCP è in modo dinamico l'allocazione degli indirizzi IP e le opzioni DHCP ai client DHCP.

>[!NOTE]
>Se non è sufficiente per questa distribuzione le macchine di test, è possibile utilizzare un computer di test per Active Directory Domain Services e DHCP, tuttavia, questa configurazione non è consigliata per un ambiente di produzione.

**Distribuzione di server DHCP standalone**

Questa distribuzione richiede un unico hub o switch, un unico server fisico e un client fisico:

1. Un hub Ethernet o switch a cui è possibile connettere i computer fisici con cavi Ethernet
2. Un computer fisico che esegue Windows Server 2016 che sarà configurato come server DHCP utilizzando questa Guida. 
3. Un computer fisico esegue un sistema operativo client Windows che si userà per verificare che il server DHCP è in modo dinamico l'allocazione degli indirizzi IP e le opzioni DHCP ai client DHCP.


## <a name="bkmk_deploy"></a>Distribuire il DHCP

In questa sezione fornisce esempi di comandi di Windows PowerShell che è possibile usare per distribuire il DHCP in un unico server. Prima di eseguire questi comandi di esempio nel server, è necessario modificare i comandi per la corrispondenza di rete e ambiente. 

Ad esempio, prima di eseguire i comandi, sostituire i valori di esempio nei comandi per gli elementi seguenti:

- Nomi dei computer
- Intervallo di indirizzi IP per ogni ambito da configurare (1 ambito per ogni subnet)
- Subnet mask per ogni intervallo di indirizzi IP da configurare
- Nome dell'ambito per ogni ambito
- Intervallo di esclusione per ogni ambito
- Valori delle opzioni DHCP, come gateway predefinito, il nome di dominio e server DNS o WINS
- Nomi di interfaccia

>[!IMPORTANT]
>Esaminare e modificare tutti i comandi per l'ambiente prima di eseguire il comando.

### <a name="where-to-install-dhcp---on-a-physical-computer-or-a-vm"></a>Posizione in cui installare DHCP - in un computer fisico o una macchina virtuale?

È possibile installare il ruolo server DHCP in un computer fisico o in una macchina virtuale \(VM\) installata in una Hyper\-host V. Se si sta installando DHCP in una macchina virtuale e si desidera che il server DHCP specificare assegnazioni di indirizzi IP ai computer nella rete fisica a cui è connesso l'host Hyper-V, è necessario connettersi la scheda di rete virtuale della macchina virtuale a un commutatore virtuale Hyper-V che viene **Esterni**.

Per altre informazioni, vedere la sezione **creare un commutatore virtuale con gestione Hyper-V** nell'argomento [creare una rete virtuale](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/connect-to-network).

### <a name="run-windows-powershell-as-an-administrator"></a>Eseguire Windows PowerShell come amministratore

È possibile utilizzare la procedura seguente per eseguire Windows PowerShell con privilegi di amministratore.

1. In un computer che esegue Windows Server 2016, fare clic su **avviare**, quindi fare doppio clic sull'icona di Windows PowerShell. Viene visualizzato un menu. 

2. Nel menu, fare clic su **altre**, quindi fare clic su **Esegui come amministratore**. Se richiesto, digitare le credenziali per un account con privilegi di amministratore nel computer. Se l'account utente con cui si è connessi al computer è un account a livello di amministratore, non si riceverà una richiesta di credenziali.

3. Verrà visualizzata la finestra di Windows PowerShell con privilegi di amministratore.

### <a name="rename-the-dhcp-server-and-configure-a-static-ip-address"></a>Rinominare il server DHCP e configurare un indirizzo IP statico

Se non è già stato fatto, è possibile usare i comandi di Windows PowerShell seguenti per rinominare il server DHCP e configurare un indirizzo IP statico per il server.

**Configurare un indirizzo IP statico**

È possibile usare i comandi seguenti per assegnare un indirizzo IP statico per il server DHCP e configurare le proprietà TCP/IP del server DHCP con l'indirizzo IP di server DNS corretto. È inoltre necessario sostituire i nomi di interfaccia e gli indirizzi IP riportati in questo esempio con i valori che si desidera utilizzare per configurare il proprio computer.

 `New-NetIPAddress -IPAddress 10.0.0.3 -InterfaceAlias "Ethernet" -DefaultGateway 10.0.0.1 -AddressFamily IPv4 -PrefixLength 24`

 `Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 10.0.0.2`

Per altre informazioni su questi comandi, vedere gli argomenti seguenti.

- [New-NetIPAddress](https://technet.microsoft.com/itpro/powershell/windows/tcpip/new-netipaddress)
- [Set-DnsClientServerAddress](https://technet.microsoft.com/itpro/powershell/windows/dns-client/set-dnsclientserveraddress)

**Rinominare il computer**

È possibile usare i comandi seguenti per rinominare e quindi riavviare il computer.

`Rename-Computer -Name DHCP1`

 `Restart-Computer`

Per altre informazioni su questi comandi, vedere gli argomenti seguenti.

- [Rename-Computer](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.management/rename-computer)
- [Restart-Computer](https://msdn.microsoft.com/powershell/reference/4.0/microsoft.powershell.management/restart-computer)

### <a name="join-the-computer-to-the-domain-optional"></a>Aggiungere il computer al dominio \(facoltativo\)

Se si installa il server DHCP in un ambiente di dominio Active Directory, è necessario aggiungere il computer al dominio. Aprire Windows PowerShell con privilegi di amministratore e quindi eseguire il comando seguente dopo aver sostituito il nome NetBios del dominio **CORP** con un valore appropriato per l'ambiente.

    Add-Computer CORP

Quando richiesto, digitare le credenziali per un account utente di dominio che dispone dell'autorizzazione per aggiungere un computer al dominio. 

    Restart-Computer

Per altre informazioni sul comando Add-Computer, vedere l'argomento seguente.

- [Add-Computer](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.management/add-computer)

### <a name="install-dhcp"></a>Installare DHCP

Dopo il riavvio del computer, aprire Windows PowerShell con privilegi di amministratore e quindi installare DHCP eseguendo il comando seguente.

    Install-WindowsFeature DHCP -IncludeManagementTools

Per altre informazioni su questo comando, vedere l'argomento seguente.

- [Install-WindowsFeature](https://technet.microsoft.com/itpro/powershell/windows/server-manager/install-windowsfeature)

### <a name="create-dhcp-security-groups"></a>Creare gruppi di sicurezza DHCP

Per creare i gruppi di sicurezza, è necessario eseguire una Shell di rete \(netsh\) comando in Windows PowerShell e quindi riavviare il servizio DHCP in modo che i nuovi gruppi diventano attivi.

Quando si esegue il comando netsh seguenti nel server DHCP, il **DHCP Administrators** e **DHCP Users** vengono creati gruppi di sicurezza nelle **utenti e gruppi locali** in DHCP Server.

    netsh dhcp add securitygroups

Il comando seguente riavvia il servizio DHCP nel computer locale.

    Restart-service dhcpserver

Per altre informazioni su questi comandi, vedere gli argomenti seguenti.

- [Network Shell (Netsh)](../netsh/netsh.md)
- [Restart-Service](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.management/restart-service)

### <a name="authorize-the-dhcp-server-in-active-directory-optional"></a>Autorizzare il server DHCP in Active Directory \(facoltativo\)

Se si sta installando DHCP in un ambiente di dominio, è necessario eseguire i passaggi seguenti per autorizzare il server DHCP per il funzionamento del dominio.

>[!NOTE]
>Server DHCP non autorizzati che sono installati nei domini Active Directory non può funzionare correttamente e non assegnare in lease gli indirizzi IP ai client DHCP. La disattivazione automatica del server DHCP non autorizzati è una funzionalità di sicurezza che impedisce l'assegnazione di indirizzi IP non corretti ai client nella rete di server DHCP non autorizzati.

È possibile usare il comando seguente per aggiungere il server DHCP per l'elenco dei server DHCP autorizzati in Active Directory. 

>[!NOTE]
>Se non si dispone di un ambiente di dominio, non eseguire questo comando.

 `Add-DhcpServerInDC -DnsName DHCP1.corp.contoso.com -IPAddress 10.0.0.3`

Per verificare che il server DHCP sia autorizzato in Active Directory, è possibile usare il comando seguente.

    Get-DhcpServerInDC

Di seguito sono i risultati di esempio che vengono visualizzati in Windows PowerShell.

    
        IPAddress   DnsName
        ---------   -------
        10.0.0.3    DHCP1.corp.contoso.com
    

Per altre informazioni su questi comandi, vedere gli argomenti seguenti.

- [Add-DhcpServerInDC](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/add-dhcpserverindc)
- [Get-DhcpServerInDC](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/get-dhcpserverindc)

### <a name="notify-server-manager-that-post-install-dhcp-configuration-is-complete-optional"></a>Inviare una notifica di Server Manager che pubblichino\-configurazione DHCP dell'installazione è stata completata \(facoltativo\)

Dopo aver completato post\-attività di installazione, ad esempio la creazione di gruppi di sicurezza e autorizzazione del server DHCP in Active Directory, Server Manager continuerà a essere potrebbe essere visualizzato un avviso nell'interfaccia utente che indica che tale post\- con la procedura guidata di configurazione post-installazione DHCP, è necessario completare i passaggi di installazione.

È possibile evitare questo problema ora\-non necessario e non accurato messaggio venga visualizzato in Server Manager per configurare la seguente chiave del Registro di sistema tramite questo comando di Windows PowerShell.

    Set-ItemProperty –Path registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ServerManager\Roles\12 –Name ConfigurationState –Value 2

Per altre informazioni su questo comando, vedere l'argomento seguente.

- [Set-ItemProperty](https://msdn.microsoft.com/powershell/reference/4.0/microsoft.powershell.management/set-itemproperty?f=255&MSPPError=-2147217396)

### <a name="set-server-level-dns-dynamic-update-configuration-settings-optional"></a>Impostare le impostazioni di configurazione di aggiornamento dinamico DNS a livello server \(facoltativo\)

Se si desidera che il server DHCP per eseguire aggiornamenti dinamici DNS per i computer client DHCP, è possibile eseguire il comando seguente per configurare questa impostazione. Si tratta di un livello di server l'impostazione, non un ambito livello si imposta, in modo che influirà su tutti gli ambiti configurate nel server. Questo comando di esempio configura anche il server DHCP per eliminare i record risorsa DNS per i client quando il client almeno scade.

    Set-DhcpServerv4DnsSetting -ComputerName "DHCP1.corp.contoso.com" -DynamicUpdates "Always" -DeleteDnsRRonLeaseExpiry $True

È possibile usare il comando seguente per configurare le credenziali che il server DHCP Usa per registrare o annullare la registrazione di record client in un server DNS. In questo esempio consente di salvare le credenziali in un server DHCP. Il primo comando Usa **Get-Credential** per creare un **PSCredential** dell'oggetto e quindi archivia l'oggetto nel **$Credential** variabile. Il comando richiede nome utente e password, pertanto assicurarsi di fornire le credenziali per un account che dispone dell'autorizzazione per aggiornare i record di risorse sul server DNS.

    
    $Credential = Get-Credential
    Set-DhcpServerDnsCredential -Credential $Credential -ComputerName "DHCP1.corp.contoso.com"
    

Per altre informazioni su questi comandi, vedere gli argomenti seguenti.

- [Set-DhcpServerv4DnsSetting](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/set-dhcpserverv4dnssetting)
- [Set-DhcpServerDnsCredential](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/set-dhcpserverdnscredential)

### <a name="configure-the-corpnet-scope"></a>Configurare l'ambito della rete aziendale

Al termine dell'installazione di DHCP, è possibile usare i comandi seguenti per configurare e attiva l'ambito della rete aziendale, creare un intervallo di esclusione per l'ambito e configurare il gateway predefinito di opzioni DHCP, l'indirizzo IP del server DNS e nome di dominio DNS.

    
    Add-DhcpServerv4Scope -name "Corpnet" -StartRange 10.0.0.1 -EndRange 10.0.0.254 -SubnetMask 255.255.255.0 -State Active`
    
    Add-DhcpServerv4ExclusionRange -ScopeID 10.0.0.0 -StartRange 10.0.0.1 -EndRange 10.0.0.15`
    
    Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.0.1 -ScopeID 10.0.0.0 -ComputerName DHCP1.corp.contoso.com`
    
    Set-DhcpServerv4OptionValue -DnsDomain corp.contoso.com -DnsServer 10.0.0.2
    

Per altre informazioni su questi comandi, vedere gli argomenti seguenti.

- [Add-DhcpServerv4Scope](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/add-dhcpserverv4scope)
- [Add-DhcpServerv4ExclusionRange](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/add-dhcpserverv4exclusionrange)
- [Set-DhcpServerv4OptionValue](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/set-dhcpserverv4optionvalue)

### <a name="configure-the-corpnet2-scope-optional"></a>Configurare l'ambito Corpnet2 \(facoltativo\)

Se si dispone di una seconda subnet connessa alla prima subnet con un router in cui è abilitato l'inoltro DHCP, è possibile usare i comandi seguenti per aggiungere un secondo ambito, denominato Corpnet2 per questo esempio. In questo esempio configura anche un intervallo di esclusione e l'indirizzo IP del gateway predefinito \(l'indirizzo IP del router della subnet\) della subnet Corpnet2.

 `Add-DhcpServerv4Scope -name "Corpnet2" -StartRange 10.0.1.1 -EndRange 10.0.1.254 -SubnetMask 255.255.255.0 -State Active`

 `Add-DhcpServerv4ExclusionRange -ScopeID 10.0.1.0 -StartRange 10.0.1.1 -EndRange 10.0.1.15`

 `Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.1.1 -ScopeID 10.0.1.0 -ComputerName DHCP1.corp.contoso.com`

Se si dispone di subnet aggiuntive che vengono gestite dal server DHCP, è possibile ripetere questi comandi, usando i valori diversi per tutti i parametri di comando, per aggiungere gli ambiti per ogni subnet.

>[!IMPORTANT]
>Assicurarsi che tutti i router tra il client DHCP e il server DHCP siano configurati per l'inoltro dei messaggi DHCP. Vedere la documentazione del router per informazioni su come configurare l'inoltro DHCP.

## <a name="bkmk_verify"></a>Verificare le funzionalità di Server

Per verificare quale il server DHCP fornisce l'allocazione dinamica degli indirizzi IP ai client DHCP, è possibile connettersi a un altro computer a una subnet servita. Dopo aver connesso il cavo Ethernet per la scheda di rete e la potenza del computer, richiede un indirizzo IP dal server DHCP. È possibile verificare la corretta configurazione usando il **ipconfig /all** comando e revisione dei risultati oppure eseguendo i test di connettività, ad esempio il tentativo di accedere alle risorse Web con il browser o una condivisione file con Windows Explorer o da altre applicazioni.

Se il client non riceve un indirizzo IP dal server DHCP, seguire i passaggi di risoluzione dei problemi seguenti.

1. Assicurarsi che il cavo Ethernet è collegato sia il computer e il Ethernet commutatore, hub o router.
1. Se il computer client è collegato a un segmento di rete che è separato dal server DHCP da un router, assicurarsi che il router sia configurato per inoltrare i messaggi DHCP.
1. Assicurarsi che il server DHCP sia autorizzato eseguendo il comando seguente per recuperare l'elenco dei server DHCP autorizzati da Active Directory in Active Directory. [Get-DhcpServerInDC](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/get-dhcpserverindc).
1. Assicurarsi che gli ambiti sono attivati per l'apertura della console DHCP \(Server Manager **Tools**, **DHCP**\), quindi espandere l'albero del server per esaminare gli ambiti, a destra\-facendo clic su ogni ambito. Se il menu risulta include la selezione **Activate**, fare clic su **Activate**. \(Se l'ambito è già attivato, la selezione dei menu legge **disattiva**.\) 

## <a name="bkmk_dhcpwps"></a>Comandi di Windows PowerShell per DHCP

Il riferimento seguente fornisce le descrizioni dei comandi e la sintassi per tutti i comandi di PowerShell per Windows Server DHCP per Windows Server 2016. Questo argomento elenca i comandi in ordine alfabetico in base al verbo all'inizio dei comandi, ad esempio **ottenere** oppure **impostare**.

>[!NOTE]
>Non è possibile utilizzare i comandi di Windows Server 2016 in Windows Server 2012 R2.

- [Modulo DhcpServer](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/index)

Il riferimento seguente fornisce le descrizioni dei comandi e la sintassi per tutti i comandi di PowerShell per Windows Server DHCP per Windows Server 2012 R2. Questo argomento elenca i comandi in ordine alfabetico in base al verbo all'inizio dei comandi, ad esempio **ottenere** oppure **impostare**.

>[!NOTE]
>È possibile usare i comandi di Windows Server 2012 R2 in Windows Server 2016.

- [Cmdlet per Server DHCP in Windows PowerShell](https://technet.microsoft.com/library/jj590751.aspx)

## <a name="bkmk_list"></a>Elenco di comandi di Windows PowerShell in questa Guida

Seguito è riportato un semplice elenco di comandi e valori di esempio usati in questa Guida.

    
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
    


