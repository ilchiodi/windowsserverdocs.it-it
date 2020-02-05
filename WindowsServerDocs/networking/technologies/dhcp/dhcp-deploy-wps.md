---
title: Distribuire DHCP mediante Windows PowerShell
description: È possibile utilizzare questo argomento per distribuire un server DHCP Windows Server 2016 Internet Protocol (IP) versione 4 che fornisce gli indirizzi IP automatici e le opzioni DHCP ai client DHCP IPv4 connessi a una o più subnet nella rete.
ms.prod: windows-server
ms.technology: networking-dhcp
ms.topic: article
ms.assetid: 7110ad21-a33e-48d5-bb3c-129982913bc8
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1e750a72ac8d47f99ea3382d076b8854acc24576
ms.sourcegitcommit: 3f9bcd188dda12dc5803defb47b2c3a907504255
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/04/2020
ms.locfileid: "77001896"
---
# <a name="deploy-dhcp-using-windows-powershell"></a>Distribuire DHCP mediante Windows PowerShell

> Si applica a: Windows Server (Canale semestrale), Windows Server 2016

In questa guida vengono fornite istruzioni su come utilizzare Windows PowerShell per distribuire un protocollo IP (Internet Protocol) versione 4 Dynamic Host Configuration Protocol \(server DHCP\) che assegna automaticamente gli indirizzi IP e le opzioni DHCP ai client DHCP IPv4 connessi a una o più subnet della rete.

> [!NOTE]
> Per scaricare questo documento in formato Word dalla raccolta TechNet, vedere [distribuire DHCP con Windows PowerShell in Windows Server 2016](https://gallery.technet.microsoft.com/Deploy-DHCP-Using-Windows-246dd293).

L'utilizzo dei server DHCP per l'assegnazione di indirizzi IP consente di risparmiare sul sovraccarico amministrativo, perché non è necessario configurare manualmente le impostazioni TCP/IP v4 per ogni scheda di rete in ogni computer della rete. Con DHCP, la configurazione TCP/IP v4 viene eseguita automaticamente quando un computer o un altro client DHCP è connesso alla rete.

È possibile distribuire il server DHCP in un gruppo di lavoro come server autonomo o come parte di un dominio Active Directory.

Questa guida contiene le sezioni seguenti.

- [Panoramica della distribuzione DHCP](#bkmk_overview)
- [Panoramica sulla tecnologia](#bkmk_technologies)
- [Pianificare la distribuzione DHCP](#bkmk_plan)
- [Uso di questa guida in un Lab di test](#bkmk_lab)
- [Distribuire DHCP](#bkmk_deploy)
- [Verificare la funzionalità del server](#bkmk_verify)
- [Comandi di Windows PowerShell per DHCP](#bkmk_dhcpwps)
- [Elenco dei comandi di Windows PowerShell in questa guida](#bkmk_list)

## <a name="bkmk_overview"></a>Panoramica della distribuzione DHCP

Nella figura seguente viene illustrato lo scenario che è possibile distribuire con questa guida. Lo scenario include un server DHCP in un dominio Active Directory. Il server è configurato per fornire gli indirizzi IP ai client DHCP su due subnet diverse. Le subnet sono separate da un router in cui è abilitato l'invio DHCP.

![Panoramica della topologia di rete DHCP](../../media/Core-Network-Guide/cng16_overview.jpg)

## <a name="bkmk_technologies"></a>Panoramica sulla tecnologia

Le sezioni seguenti forniscono una breve panoramica di DHCP e TCP/IP.

### <a name="dhcp-overview"></a>Panoramica di DHCP

DHCP è uno standard IP che semplifica la gestione della configurazione IP degli host. Lo standard DHCP prevede l'utilizzo di server DHCP nella gestione dell'allocazione dinamica degli indirizzi IP e in altri dettagli correlati alla configurazione dei client della rete abilitati per DHCP.

DHCP consente di usare un server DHCP per assegnare dinamicamente un indirizzo IP a un computer o a un altro dispositivo, ad esempio una stampante, nella rete locale, anziché configurare manualmente ogni dispositivo con un indirizzo IP statico.

Ogni computer di una rete TCP/IP deve avere un indirizzo IP univoco, poiché questo indirizzo IP e la subnet mask correlata identificano sia il computer host, sia la subnet alla quale il computer è collegato. Utilizzando DHCP si assicura che tutti i computer configurati come client DHCP ricevano un indirizzo IP idoneo per il percorso di rete e la subnet. Inoltre utilizzando le opzioni DHCP, ad esempio gateway e server DNS predefiniti, è possibile fornire automaticamente ai client DHCP le informazioni necessarie per il loro corretto funzionamento nella rete.

Per le reti basate su TCP/IP, DHCP riduce la complessità e la quantità di lavoro amministrativo necessario per la configurazione dei computer.

### <a name="tcpip-overview"></a>Panoramica di TCP/IP

Per impostazione predefinita, tutte le versioni dei sistemi operativi Windows Server e client Windows hanno impostazioni TCP/IP per le connessioni di rete IP versione 4 configurate per ottenere automaticamente un indirizzo IP e altre informazioni, denominate opzioni DHCP, da un server DHCP. Per questo motivo, non è necessario configurare manualmente le impostazioni TCP/IP, a meno che il computer non sia un computer server o un altro dispositivo che richiede un indirizzo IP statico configurato manualmente. 

Si consiglia, ad esempio, di configurare manualmente l'indirizzo IP del server DHCP e gli indirizzi IP dei server DNS e dei controller di dominio che eseguono Active Directory Domain Services \(\)di servizi di dominio Active Directory.

TCP/IP in Windows Server 2016 è il seguente:

- Software di rete basato su protocolli di rete standard.

- Un protocollo instradabile per reti aziendali che supporta la connessione di un computer basato su Windows sia alla rete locale (LAN), sia ad ambienti di rete WAN (Wide Area Network).

- Utilità e tecnologie di base per la connessione di computer basati su Windows a sistemi di tipo diverso al fine di condividere informazioni.

- Base per accedere ai servizi Internet globali, ad esempio i server Web e File Transfer Protocol (FTP).

- Un framework client/server multipiattaforma affidabile e scalabile.

Il servizio TCP/IP include utilità TCP/IP di base che consentono ai computer basati su Windows di connettersi e condividere informazioni con altri sistemi Microsoft e non Microsoft, quali:

- Windows Server 2016

- Windows 10

- Windows Server 2012 R2

- Windows 8.1

- Windows Server 2012

- Windows 8

- Windows Server 2008 R2

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

## <a name="bkmk_plan"></a>Pianificare la distribuzione DHCP

Di seguito sono riportati i passaggi di pianificazione chiave prima di installare il ruolo server DHCP.

### <a name="planning-dhcp-servers-and-dhcp-forwarding"></a>Pianificazione dei server DHCP e dell'inoltro DHCP

Poiché i messaggi DHCP sono messaggi trasmessi, non vengono inoltrati fra le subnet dai router. Se sono disponibili più subnet e si desidera fornire il servizio DHCP per ogni subnet, è necessario eseguire una delle operazioni seguenti:

- Installare un server DHCP in ogni subnet

- Configurare i router per l'inoltro dei messaggi trasmessi DHCP tra le subnet e configurare un ambito per ogni subnet sul server DHCP.

Nella maggior parte dei casi è più conveniente configurare i router per l'inoltro dei messaggi trasmessi DHCP che non distribuire un server DHCP in ogni segmento fisico della rete.

### <a name="planning-ip-address-ranges"></a>Pianificazione degli intervalli di indirizzi IP

Ogni subnet deve disporre di un proprio intervallo di indirizzi IP univoco. In un server DHCP tali intervalli sono rappresentati dagli ambiti.

Un ambito è un raggruppamento amministrativo di indirizzi IP per i computer di una subnet che utilizzano il servizio DHCP. L'amministratore crea innanzitutto un ambito per ogni subnet fisica, dopodiché lo utilizza per definire i parametri utilizzati dai client.

Ogni ambito è caratterizzato dalle proprietà seguenti:

- Un intervallo di indirizzi IP in cui includere o escludere gli indirizzi utilizzati per le offerte di lease del servizio DHCP.

- Una subnet mask, che determina il prefisso subnet per un determinato indirizzo IP.

- Un nome di ambito assegnato al momento della creazione.

- I valori di durata dei lease, che vengono assegnati ai client DHCP che ricevono gli indirizzi IP allocati dinamicamente.

- Tutte le opzioni relative all'ambito DHCP configurate per l'assegnazione ai client DHCP, ad esempio l'indirizzo IP del server DNS e l'indirizzo IP del router/gateway predefinito.

- Facoltativamente è possibile utilizzare le prenotazioni per garantire che un client DHCP riceva sempre lo stesso indirizzo IP.

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

Ad esempio, se l'intervallo di indirizzi IP per una subnet è da 192.168.0.1 a 192.168.0.254 e sono presenti dieci dispositivi che si desidera configurare con un indirizzo IP statico, è possibile creare un intervallo di esclusione per il 192.168.0.*x* ambito che include dieci o più indirizzi IP: da 192.168.0.1 a 192.168.0.15.

In questo esempio dieci degli indirizzi IP esclusi vengono utilizzati per configurare i server e gli altri dispositivi con gli indirizzi IP statici, mentre altri cinque indirizzi IP restano a disposizione per l'eventuale configurazione statica di nuovi dispositivi aggiunti in seguito. Con questo intervallo di esclusione, il server DHCP ha ancora a disposizione gli indirizzi compresi tra 192.168.0.16 e 192.168.0.254.

Nella tabella seguente vengono forniti gli elementi di configurazione di esempio aggiuntive per Active Directory e DNS.

|Elementi di configurazione|Valori di esempio|
|-----------------------|------------------|
|Binding connessioni di rete|Ethernet|
|Impostazioni server DNS|DC1.corp.contoso.com|
|Indirizzo IP del server DNS preferito|10.0.0.2|
|Valori di ambito<br /><br />1. nome ambito<br />2. indirizzo IP iniziale<br />3. indirizzo IP finale<br />4. subnet mask<br />5. gateway predefinito (facoltativo)<br />6. durata lease|1. subnet primaria<br />2.10.0.0.1<br />3.10.0.0.254<br />4.255.255.255.0<br />5.10.0.0.1<br />6. 8 giorni|
|Modalità operativa server DHCP IPv6|Non abilitata|

## <a name="bkmk_lab"></a>Uso di questa guida in un Lab di test

È possibile utilizzare questa guida per distribuire DHCP in un Lab di test prima di distribuire in un ambiente di produzione. 

> [!NOTE]
> Se non si desidera distribuire DHCP in un Lab di test, è possibile passare alla sezione [distribuire DHCP](#bkmk_deploy).

I requisiti per il Lab variano a seconda che si utilizzino server fisici o macchine virtuali \(VM\)e se si utilizza un dominio di Active Directory o si distribuisce un server DHCP autonomo.

È possibile usare le informazioni seguenti per determinare le risorse minime necessarie per testare la distribuzione DHCP con questa guida.

### <a name="test-lab-requirements-with-vms"></a>Requisiti del Lab di test con macchine virtuali

Per distribuire DHCP in un Lab di test con macchine virtuali, sono necessarie le risorse seguenti.

Per la distribuzione del dominio o la distribuzione autonoma, è necessario un server configurato come host Hyper\-V.

**Distribuzione del dominio**

Questa distribuzione richiede un solo server fisico, un commutire virtuale, due server virtuali e un client virtuale:

Nel server fisico, nella console di gestione di Hyper-V, creare gli elementi seguenti.

1. Uno switch virtuale **interno** . Non creare un commutire virtuale **esterno** , perché se l'host Hyper\-V si trova in una subnet che include un server DHCP, le VM di test riceveranno un indirizzo IP dal server DHCP. Inoltre, il server DHCP di test distribuito potrebbe assegnare indirizzi IP ad altri computer nella subnet in cui è installato l'host Hyper\-V.
1. Una macchina virtuale che esegue Windows Server 2016 è configurata come controller di dominio con Active Directory Domain Services connessa al commutivo virtuale interno creato. Per corrispondere a questa guida, il server deve disporre di un indirizzo IP configurato in modo statico 10.0.0.2. Per informazioni sulla distribuzione di servizi di dominio Active Directory, vedere la sezione **Deploying DC1** nella [Guida alla rete core](https://docs.microsoft.com/windows-server/networking/core-network-guide/core-network-guide#BKMK_deployADDNS01)di Windows Server 2016.
1. Una macchina virtuale che esegue Windows Server 2016 che verrà configurata come server DHCP usando questa guida e connessa al commutivo virtuale interno creato. 
1. Una macchina virtuale che esegue un sistema operativo client Windows connessa al commessore virtuale interno creato e che verrà usato per verificare che il server DHCP stia allocando in modo dinamico gli indirizzi IP e le opzioni DHCP ai client DHCP.

**Distribuzione server DHCP autonomo**

Questa distribuzione richiede un solo server fisico, un commutire virtuale, un server virtuale e un client virtuale:

Nel server fisico, nella console di gestione di Hyper-V, creare gli elementi seguenti.

1. Uno switch virtuale **interno** . Non creare un commutire virtuale **esterno** , perché se l'host Hyper\-V si trova in una subnet che include un server DHCP, le VM di test riceveranno un indirizzo IP dal server DHCP. Inoltre, il server DHCP di test distribuito potrebbe assegnare indirizzi IP ad altri computer nella subnet in cui è installato l'host Hyper\-V.
2. Una macchina virtuale che esegue Windows Server 2016 che verrà configurata come server DHCP usando questa guida e connessa al commutivo virtuale interno creato.
3. Una macchina virtuale che esegue un sistema operativo client Windows connessa al commessore virtuale interno creato e che verrà usato per verificare che il server DHCP stia allocando in modo dinamico gli indirizzi IP e le opzioni DHCP ai client DHCP.

### <a name="test-lab-requirements-with-physical-servers"></a>Requisiti del Lab di test con server fisici

Per distribuire DHCP in un Lab di test con server fisici, sono necessarie le risorse seguenti.

**Distribuzione del dominio**

Questa distribuzione richiede un hub o uno switch, due server fisici e un client fisico:

1. Un hub Ethernet o un switch a cui è possibile connettere i computer fisici con cavi Ethernet
2. Un computer fisico che esegue Windows Server 2016 configurato come controller di dominio con Active Directory Domain Services. Per corrispondere a questa guida, il server deve disporre di un indirizzo IP configurato in modo statico 10.0.0.2. Per informazioni sulla distribuzione di servizi di dominio Active Directory, vedere la sezione **Deploying DC1** nella [Guida alla rete core](https://docs.microsoft.com/windows-server/networking/core-network-guide/core-network-guide#BKMK_deployADDNS01)di Windows Server 2016.
3. Un computer fisico che esegue Windows Server 2016 che verrà configurato come server DHCP utilizzando questa guida.
4. Un computer fisico che esegue un sistema operativo client Windows che verrà usato per verificare che il server DHCP stia allocando in modo dinamico gli indirizzi IP e le opzioni DHCP ai client DHCP.

> [!NOTE]
> Se non si dispone di un numero sufficiente di computer di test per questa distribuzione, è possibile utilizzare un solo computer di test per servizi di dominio Active Directory e DHCP. questa configurazione non è tuttavia consigliata per un ambiente di produzione.

**Distribuzione server DHCP autonomo**

Questa distribuzione richiede un hub o uno switch, un server fisico e un client fisico:

1. Un hub Ethernet o un switch a cui è possibile connettere i computer fisici con cavi Ethernet
2. Un computer fisico che esegue Windows Server 2016 che verrà configurato come server DHCP utilizzando questa guida.
3. Un computer fisico che esegue un sistema operativo client Windows che verrà usato per verificare che il server DHCP stia allocando in modo dinamico gli indirizzi IP e le opzioni DHCP ai client DHCP.


## <a name="bkmk_deploy"></a>Distribuire DHCP

In questa sezione vengono forniti i comandi di Windows PowerShell di esempio che è possibile utilizzare per distribuire DHCP in un server. Prima di eseguire questi comandi di esempio nel server, è necessario modificare i comandi in modo che corrispondano alla rete e all'ambiente. 

Prima di eseguire i comandi, ad esempio, è necessario sostituire i valori di esempio nei comandi per gli elementi seguenti:

- Nomi computer
- Intervallo di indirizzi IP per ogni ambito che si vuole configurare (1 ambito per subnet)
- Subnet mask per ogni intervallo di indirizzi IP che si vuole configurare
- Nome ambito per ogni ambito
- Intervallo di esclusione per ogni ambito
- Valori delle opzioni DHCP, ad esempio gateway predefinito, nome di dominio e server DNS o WINS
- Nomi di interfaccia

> [!IMPORTANT]
> Esaminare e modificare ogni comando per l'ambiente prima di eseguire il comando.

### <a name="where-to-install-dhcp---on-a-physical-computer-or-a-vm"></a>Dove installare DHCP in un computer fisico o in una macchina virtuale?

È possibile installare il ruolo server DHCP in un computer fisico o in una macchina virtuale \(\) VM installata in un host Hyper\-V. Se si installa DHCP in una macchina virtuale e si desidera che il server DHCP fornisca le assegnazioni degli indirizzi IP ai computer nella rete fisica a cui è connesso l'host Hyper-V, è necessario connettere la scheda di rete virtuale della macchina virtuale a un commutire virtuale Hyper-V **esterno**.

Per ulteriori informazioni, vedere la sezione **creare un Commutire virtuale con la console di gestione di Hyper-V** nell'argomento [creare una rete virtuale](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/connect-to-network).

### <a name="run-windows-powershell-as-an-administrator"></a>Eseguire Windows PowerShell come amministratore

È possibile utilizzare la procedura seguente per eseguire Windows PowerShell con privilegi di amministratore.

1. In un computer che esegue Windows Server 2016, fare clic sul pulsante **Start**, quindi fare clic con il pulsante destro del mouse sull'icona di Windows PowerShell. Viene visualizzato un menu.

2. Nel menu fare clic su **altro**e quindi su **Esegui come amministratore**. Se richiesto, digitare le credenziali di un account con privilegi di amministratore per il computer. Se l'account utente con cui si è connessi al computer è un account a livello di amministratore, non si riceverà una richiesta di credenziali.

3. Windows PowerShell viene aperto con privilegi di amministratore.

### <a name="rename-the-dhcp-server-and-configure-a-static-ip-address"></a>Rinominare il server DHCP e configurare un indirizzo IP statico

Se non è già stato fatto, è possibile usare i comandi di Windows PowerShell seguenti per rinominare il server DHCP e configurare un indirizzo IP statico per il server.

**Configurare un indirizzo IP statico**

È possibile utilizzare i comandi seguenti per assegnare un indirizzo IP statico al server DHCP e per configurare le proprietà TCP/IP del server DHCP con l'indirizzo IP del server DNS corretto. È inoltre necessario sostituire i nomi di interfaccia e gli indirizzi IP riportati in questo esempio con i valori che si desidera utilizzare per configurare il proprio computer.

```
New-NetIPAddress -IPAddress 10.0.0.3 -InterfaceAlias "Ethernet" -DefaultGateway 10.0.0.1 -AddressFamily IPv4 -PrefixLength 24
Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 10.0.0.2
```

Per ulteriori informazioni su questi comandi, vedere gli argomenti seguenti.

- [New-NetIPAddress](https://docs.microsoft.com/powershell/module/nettcpip/New-NetIPAddress)
- [Set-DnsClientServerAddress](https://docs.microsoft.com/powershell/module/dnsclient/Set-DnsClientServerAddress)

**Rinominare il computer**

È possibile utilizzare i comandi seguenti per rinominare e riavviare il computer.

```
Rename-Computer -Name DHCP1
Restart-Computer
```

Per ulteriori informazioni su questi comandi, vedere gli argomenti seguenti.

- [Rinomina-computer](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/rename-computer)
- [Restart-Computer](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/restart-computer)

### <a name="join-the-computer-to-the-domain-optional"></a>Aggiungere il computer al dominio \(facoltativo\)

Se si installa il server DHCP in un ambiente di dominio Active Directory, è necessario aggiungere il computer al dominio. Aprire Windows PowerShell con privilegi di amministratore, quindi eseguire il comando seguente dopo aver sostituito il nome NetBios del dominio **Corp** con un valore appropriato per l'ambiente in uso.

```
Add-Computer CORP
```

Quando richiesto, digitare le credenziali per un account utente di dominio con l'autorizzazione per aggiungere un computer al dominio. 

```
Restart-Computer
```

Per ulteriori informazioni sul comando Add-computer, vedere l'argomento seguente.

- [Aggiungi computer](https://docs.microsoft.com/powershell/module/microsoft.powershell.management/add-computer?view=powershell-5.1)

### <a name="install-dhcp"></a>Installare DHCP

Dopo il riavvio del computer, aprire Windows PowerShell con privilegi di amministratore e quindi installare DHCP eseguendo il comando seguente.

```
Install-WindowsFeature DHCP -IncludeManagementTools
```

Per ulteriori informazioni su questo comando, vedere l'argomento seguente.

- [Install-WindowsFeature](https://docs.microsoft.com/powershell/module/servermanager/install-windowsfeature)

### <a name="create-dhcp-security-groups"></a>Creazione di gruppi di sicurezza DHCP

Per creare gruppi di sicurezza, è necessario eseguire una shell di rete \(comando netsh\) in Windows PowerShell, quindi riavviare il servizio DHCP in modo che i nuovi gruppi diventino attivi.

Quando si esegue il comando netsh seguente sul server DHCP, i gruppi di sicurezza DHCP **Administrators** e **DHCP Users** vengono creati in **utenti e gruppi locali** nel server DHCP.

```
netsh dhcp add securitygroups
```

Il comando seguente riavvia il servizio DHCP nel computer locale.

```
Restart-Service dhcpserver
```

Per ulteriori informazioni su questi comandi, vedere gli argomenti seguenti.

- [Shell di rete (Netsh)](../netsh/netsh.md)
- [Restart-Service](https://docs.microsoft.com/powershell/module/microsoft.powershell.management/restart-service)

### <a name="authorize-the-dhcp-server-in-active-directory-optional"></a>Autorizzare il server DHCP in Active Directory \(facoltativo\)

Se si installa DHCP in un ambiente di dominio, è necessario eseguire la procedura seguente per autorizzare il server DHCP a operare nel dominio.

> [!NOTE]
> I server DHCP non autorizzati installati in Active Directory domini non possono funzionare correttamente e non devono eseguire il lease degli indirizzi IP ai client DHCP. La disabilitazione automatica dei server DHCP non autorizzati è una funzionalità di sicurezza che impedisce ai server DHCP non autorizzati di assegnare indirizzi IP non corretti ai client della rete.

È possibile utilizzare il comando seguente per aggiungere il server DHCP all'elenco di server DHCP autorizzati in Active Directory. 

> [!NOTE]
> Se non si dispone di un ambiente di dominio, non eseguire questo comando.

```
Add-DhcpServerInDC -DnsName DHCP1.corp.contoso.com -IPAddress 10.0.0.3
```

Per verificare che il server DHCP sia autorizzato in Active Directory, è possibile utilizzare il comando seguente.

```
Get-DhcpServerInDC
```

Di seguito sono riportati i risultati di esempio visualizzati in Windows PowerShell.

```
IPAddress   DnsName
---------   -------
10.0.0.3    DHCP1.corp.contoso.com
```

Per ulteriori informazioni su questi comandi, vedere gli argomenti seguenti.

- [Add-DhcpServerInDC](https://docs.microsoft.com/powershell/module/dhcpserver/add-dhcpserverindc)
- [Get-DhcpServerInDC](https://docs.microsoft.com/powershell/module/dhcpserver/get-dhcpserverindc)

### <a name="notify-server-manager-that-post-install-dhcp-configuration-is-complete-optional"></a>Notifica Server Manager che la configurazione di post\-installazione DHCP è stata completata \(facoltativo\)

Dopo aver completato le attività post\-installazione, ad esempio la creazione di gruppi di sicurezza e l'autorizzazione del server DHCP in Active Directory, Server Manager possibile che venga visualizzato un avviso nell'interfaccia utente che informa che è necessario completare la procedura di post\-installazione tramite la configurazione guidata post-installazione DHCP.

È possibile evitare questo problema\-un messaggio non corretto e non accurato viene visualizzato in Server Manager configurando la chiave del registro di sistema seguente utilizzando questo comando di Windows PowerShell.

```
Set-ItemProperty –Path registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ServerManager\Roles\12 –Name ConfigurationState –Value 2
```

Per ulteriori informazioni su questo comando, vedere l'argomento seguente.

- [Set-ItemProperty](https://docs.microsoft.com/powershell/module/microsoft.powershell.management/set-itemproperty)

### <a name="set-server-level-dns-dynamic-update-configuration-settings-optional"></a>Impostare le impostazioni di configurazione degli aggiornamenti dinamici DNS a livello di server \(facoltativo\)

Se si desidera che il server DHCP esegua aggiornamenti dinamici DNS per i computer client DHCP, è possibile eseguire il comando seguente per configurare questa impostazione. Si tratta di un'impostazione a livello di server, non di un livello di ambito, che influirà su tutti gli ambiti configurati nel server. Questo comando di esempio configura anche il server DHCP per eliminare i record di risorse DNS per i client quando il client scade almeno.

```
Set-DhcpServerv4DnsSetting -ComputerName "DHCP1.corp.contoso.com" -DynamicUpdates "Always" -DeleteDnsRRonLeaseExpiry $True
```

È possibile utilizzare il comando seguente per configurare le credenziali utilizzate dal server DHCP per registrare o annullare la registrazione dei record client in un server DNS. In questo esempio viene salvata una credenziale in un server DHCP. Il primo comando USA **Get-Credential** per creare un oggetto **PSCredential** e quindi archivia l'oggetto nella variabile **$Credential** . Il comando richiede il nome utente e la password, quindi assicurarsi di fornire le credenziali per un account che disponga dell'autorizzazione per aggiornare i record di risorse nel server DNS.
 
```
$Credential = Get-Credential
Set-DhcpServerDnsCredential -Credential $Credential -ComputerName "DHCP1.corp.contoso.com"
``` 

Per ulteriori informazioni su questi comandi, vedere gli argomenti seguenti.

- [Set-DhcpServerv4DnsSetting](https://docs.microsoft.com/en-us/powershell/module/dhcpserver/set-dhcpserverv4dnssetting)
- [Set-DhcpServerDnsCredential](https://docs.microsoft.com/en-us/powershell/module/dhcpserver/set-dhcpserverdnscredential)

### <a name="configure-the-corpnet-scope"></a>Configurare l'ambito corpnet

Al termine dell'installazione DHCP, è possibile usare i comandi seguenti per configurare e attivare l'ambito corpnet, creare un intervallo di esclusione per l'ambito e configurare il gateway predefinito delle opzioni DHCP, l'indirizzo IP del server DNS e il nome di dominio DNS.

```
Add-DhcpServerv4Scope -name "Corpnet" -StartRange 10.0.0.1 -EndRange 10.0.0.254 -SubnetMask 255.255.255.0 -State Active    
Add-DhcpServerv4ExclusionRange -ScopeID 10.0.0.0 -StartRange 10.0.0.1 -EndRange 10.0.0.15
Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.0.1 -ScopeID 10.0.0.0 -ComputerName DHCP1.corp.contoso.com
Set-DhcpServerv4OptionValue -DnsDomain corp.contoso.com -DnsServer 10.0.0.2
```

Per ulteriori informazioni su questi comandi, vedere gli argomenti seguenti.

- [Add-DhcpServerv4Scope](https://docs.microsoft.com/powershell/module/dhcpserver/Add-DhcpServerv4Scope)
- [Add-DhcpServerv4ExclusionRange](https://docs.microsoft.com/powershell/module/dhcpserver/Add-DhcpServerv4ExclusionRange)
- [Set-DhcpServerv4OptionValue](https://docs.microsoft.com/powershell/module/dhcpserver/Set-DhcpServerv4OptionValue)

### <a name="configure-the-corpnet2-scope-optional"></a>Configurare l'ambito Corpnet2 \(facoltativo\)

Se è presente una seconda subnet connessa alla prima subnet con un router in cui è abilitato l'invio DHCP, è possibile usare i comandi seguenti per aggiungere un secondo ambito, denominato Corpnet2, per questo esempio. Questo esempio configura anche un intervallo di esclusione e l'indirizzo IP per il gateway predefinito \(indirizzo IP del router nella subnet\) della subnet Corpnet2.

```
Add-DhcpServerv4Scope -name "Corpnet2" -StartRange 10.0.1.1 -EndRange 10.0.1.254 -SubnetMask 255.255.255.0 -State Active
Add-DhcpServerv4ExclusionRange -ScopeID 10.0.1.0 -StartRange 10.0.1.1 -EndRange 10.0.1.15
Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.1.1 -ScopeID 10.0.1.0 -ComputerName DHCP1.corp.contoso.com
```

Se si dispone di subnet aggiuntive gestite da questo server DHCP, è possibile ripetere questi comandi, usando valori diversi per tutti i parametri del comando, per aggiungere ambiti per ogni subnet.

> [!IMPORTANT]
> Verificare che tutti i router tra i client DHCP e il server DHCP siano configurati per l'inoltro dei messaggi DHCP. Per informazioni su come configurare l'invio DHCP, vedere la documentazione del router.

## <a name="bkmk_verify"></a>Verificare la funzionalità del server

Per verificare che il server DHCP fornisca l'allocazione dinamica degli indirizzi IP ai client DHCP, è possibile connettere un altro computer a una subnet servita. Dopo aver connesso il cavo Ethernet alla scheda di rete e avere alimentato il computer, verrà richiesto un indirizzo IP dal server DHCP. Per verificare la corretta configurazione, è possibile usare il comando **ipconfig** /e per esaminare i risultati oppure eseguire test di connettività, ad esempio il tentativo di accedere alle risorse Web con il browser o le condivisioni file con Esplora risorse o altre applicazioni.

Se il client non riceve un indirizzo IP dal server DHCP, seguire questa procedura di risoluzione dei problemi.

1. Verificare che il cavo Ethernet sia collegato al computer e al Commuter, all'hub o al router Ethernet.
2. Se il computer client è stato collegato a un segmento di rete separato dal server DHCP da un router, verificare che il router sia configurato per l'invio dei messaggi DHCP.
3. Verificare che il server DHCP sia autorizzato in Active Directory eseguendo il comando seguente per recuperare l'elenco dei server DHCP autorizzati da Active Directory. [Get-DhcpServerInDC](https://docs.microsoft.com/powershell/module/dhcpserver/Get-DhcpServerInDC).
4. Verificare che gli ambiti siano attivati aprendo la console DHCP \(Server Manager, **strumenti**, **DHCP**\), espandendo l'albero del server per esaminare gli ambiti, quindi\-fare clic con il pulsante destro del mouse su ogni ambito. Se il menu risultante include la selezione **attiva**, fare clic su **attiva**. \(se l'ambito è già attivato, la selezione del menu viene **disattivata**.\)

## <a name="bkmk_dhcpwps"></a>Comandi di Windows PowerShell per DHCP

Il riferimento seguente fornisce le descrizioni dei comandi e la sintassi per tutti i comandi di Windows PowerShell del server DHCP per Windows Server 2016. Nell'argomento vengono elencati i comandi in ordine alfabetico in base al verbo all'inizio dei comandi, ad esempio **Get** o **set**.

> [!NOTE]
> Non è possibile usare i comandi di Windows Server 2016 in Windows Server 2012 R2.

- [Modulo DhcpServer](https://docs.microsoft.com/en-us/powershell/module/dhcpserver/)

Il riferimento seguente fornisce le descrizioni dei comandi e la sintassi per tutti i comandi di Windows PowerShell del server DHCP per Windows Server 2012 R2. Nell'argomento vengono elencati i comandi in ordine alfabetico in base al verbo all'inizio dei comandi, ad esempio **Get** o **set**.

> [!NOTE]
> È possibile usare i comandi di Windows Server 2012 R2 in Windows Server 2016.

- [Cmdlet per server DHCP in Windows PowerShell](https://docs.microsoft.com/windows-server/networking/technologies/dhcp/dhcp-deploy-wps)

## <a name="bkmk_list"></a>Elenco dei comandi di Windows PowerShell in questa guida

Di seguito è riportato un semplice elenco di comandi e valori di esempio usati in questa guida.

```
New-NetIPAddress -IPAddress 10.0.0.3 -InterfaceAlias "Ethernet" -DefaultGateway 10.0.0.1 -AddressFamily IPv4 -PrefixLength 24
Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 10.0.0.2
Rename-Computer -Name DHCP1
Restart-Computer

Add-Computer CORP
Restart-Computer

Install-WindowsFeature DHCP -IncludeManagementTools
netsh dhcp add securitygroups
Restart-Service dhcpserver

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
```
