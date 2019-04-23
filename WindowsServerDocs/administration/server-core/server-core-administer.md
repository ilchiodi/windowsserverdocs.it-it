---
title: Amministrare Server Core
description: Informazioni su come amministrare un'installazione Server Core di Windows Server
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.author: elizapo
ms.localizationpriority: medium
ms.date: 12/18/2018
ms.openlocfilehash: 39fbb92645d39a46613f2142d0258c78a6ba425b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842662"
---
# <a name="administer-a-server-core-server"></a>Amministrare un Server server Core

>Si applica a: Windows Server (canale semestrale) e Windows Server 2016

Perché un'interfaccia utente non dispone di Server Core, è necessario usare i cmdlet Windows PowerShell, gli strumenti da riga di comando o gli strumenti remoti per eseguire attività di amministrazione di base. Le sezioni seguenti descrivono i cmdlet di PowerShell e comandi utilizzati per attività di base. È anche possibile usare [Windows Admin Center](../../manage/windows-admin-center/overview.md), un portale di gestione unificata attualmente in anteprima pubblica, amministrare l'installazione. 

## <a name="administrative-tasks-using-powershell-cmdlets"></a>Attività amministrative utilizzando i cmdlet di PowerShell
Usare le informazioni seguenti per eseguire attività amministrative di base con i cmdlet di Windows PowerShell.

### <a name="set-a-static-ip-address"></a>Impostare un indirizzo IP statico
Quando si installa un Server server Core, per impostazione predefinita include indirizzo oggetto DHCP. Se è necessario un indirizzo IP statico, è possibile impostare usando la procedura seguente.

Per visualizzare la configurazione di rete corrente, usare **Get-NetIPConfiguration**.

Per visualizzare gli indirizzi IP è già in uso, utilizzare **Get-NetIPAddress**.

Per impostare un indirizzo IP statico, eseguire le operazioni seguenti: 

1. Eseguire **Get-NetIPInterface**. 
2. Prendere nota del numero nel **IfIndex** colonna per l'interfaccia IP o il **InterfaceDescription** stringa. Se si dispone di più di una scheda di rete, tenere presente il numero o stringa corrispondente all'interfaccia di cui per che si desidera impostare l'indirizzo IP statico.
3. Eseguire il cmdlet seguente per impostare l'indirizzo IP statico:

   ```powershell
   New-NetIPaddress -InterfaceIndex 12 -IPAddress 192.0.2.2 -PrefixLength 24 -DefaultGateway 192.0.2.1
   ```

   dove:
   - **InterfaceIndex** JE hodnotou **IfIndex** creata nel passaggio 2. (In questo esempio 12)
   - **IPAddress** è l'indirizzo IP statico si desidera impostare. (In questo esempio 191.0.2.2)
   - **PrefixLength** è la lunghezza del prefisso (un altro formato di subnet mask) per l'indirizzo IP si sta impostando. (Per questo esempio 24)
   - **DefaultGateway** è l'indirizzo IP del gateway predefinito. (Per questo esempio 192.0.2.1)
4. Eseguire il cmdlet seguente per impostare il client DNS l'indirizzo del server: 

   ```powershell
   Set-DNSClientServerAddress –InterfaceIndex 12 -ServerAddresses 192.0.2.4
   ```
   
   dove:
   - **InterfaceIndex** è il valore di IfIndex dal passaggio 2.
   - **ServerAddresses** è l'indirizzo IP del server DNS.
5. Per aggiungere più server DNS, eseguire il cmdlet seguente: 

   ```powershell
   Set-DNSClientServerAddress –InterfaceIndex 12 -ServerAddresses 192.0.2.4,192.0.2.5
   ```

   dove, in questo esempio **192.0.2.4** e **192.0.2.5** sono entrambi gli indirizzi IP dei server DNS.

Se è necessario passare all'utilizzo di DHCP, eseguire **Set-DnsClientServerAddress – InterfaceIndex 12 – ResetServerAddresses**.

### <a name="join-a-domain"></a>Aggiungere un dominio
Usare i cmdlet seguenti per aggiungere un computer a un dominio.

1. Eseguire **Add-Computer**. Verrà richiesto per entrambe le credenziali per l'aggiunta al dominio e il nome di dominio.
2. Se è necessario aggiungere un account utente di dominio al gruppo Administrators locale, eseguire il comando seguente al prompt dei comandi (non nella finestra di PowerShell):

   ```
   net localgroup administrators /add <DomainName>\<UserName>
   ```
3. Riavviare il computer. È possibile farlo eseguendo **Restart-Computer**.

### <a name="rename-the-server"></a>Rinominare il server
Utilizzare la procedura seguente per rinominare il server.

1. Determinare il nome corrente del server con il **hostname** oppure **ipconfig** comando.
2. Eseguire **Rename-Computer - ComputerName \<new_name\>**.
3. Riavviare il computer.

### <a name="activate-the-server"></a>Attivare il server

Eseguire **slmgr. vbs – ipk\<productkey\>**. Quindi eseguire **slmgr. vbs – ato**. Se l'attivazione ha esito positivo, si otterranno un messaggio.

> [!NOTE]
> È anche possibile attivare il server dal telefono, usando un [server del servizio di gestione delle chiavi (KMS)](../../get-started/server-2016-activation.md), o in remoto. Per attivare in remoto, eseguire il cmdlet seguente da un computer remoto: 

>```powershell
>**cscript windows\system32\slmgr.vbs <ServerName> <UserName> <password>:-ato**
>```
 
### <a name="configure-windows-firewall"></a>Configurare Windows Firewall

È possibile configurare Windows Firewall in locale nel computer Server Core utilizzando cmdlet di Windows PowerShell e script. Visualizzare [NetSecurity](/powershell/module/netsecurity/?view=win10-ps) per i cmdlet è possibile usare per configurare Windows Firewall.

### <a name="enable-windows-powershell-remoting"></a>Abilitare la comunicazione remota di Windows PowerShell

È possibile abilitare la comunicazione remota di Windows PowerShell, che consente di eseguire in un computer i comandi digitati in Windows PowerShell in un altro computer. Abilitare la comunicazione remota di Windows PowerShell con **Enable-PSRemoting**.

Per altre informazioni, vedere [About_remote_faq](/powershell/module/microsoft.powershell.core/about/about_remote_faq?view=powershell-5.1)
</br>

## <a name="administrative-tasks-from-the-command-line"></a>Attività amministrative dalla riga di comando
Usare le informazioni di riferimento seguente per eseguire attività amministrative dalla riga di comando.

### <a name="configuration-and-installation"></a>Configurazione e installazione
|Attività | Comando |
|-----|-------|
|Impostare la password amministrativa locale| **Amministratore utenti NET** \* |
|Aggiungere un computer a un dominio| **netdom join %computername%** **/domain:\<domain\> /userd:\<domain\\username\> /passwordd:**\* <br> Riavviare il computer.|
|Verificare che il dominio è stato modificato| **set** |
|Rimuovere un computer da un dominio|**Netdom remove \<computername\>**| 
|Aggiungere un utente al gruppo Administrators locale|**net localgroup amministratori /Add \<dominio\\username\>** |
|Rimuovere un utente dal gruppo Administrators locale|**net localgroup amministratori /delete \<dominio\\username\>** |
|Aggiungere un utente al computer locale|**utente NET \<dominio\nomeutente\> * / aggiungere** |
|Aggiungere un gruppo al computer locale|**net localgroup \<nome gruppo\> / aggiungere**|
|Modificare il nome di un computer aggiunto a un dominio|**Netdom renamecomputer % computername % /NewName:\<nuovo nome del computer\> /userd:\<domain\\username\> /passwordd:** * |
|Confermare il nuovo nome del computer|**set**| 
|Modificare il nome di un computer in un gruppo di lavoro|**Netdom renamecomputer \<NomeComputerCorrente\> /NewName:\<NuovoNomeComputer\>** <br>Riavviare il computer.|
|Disabilitare la gestione del file di paging|**wmic computersystem where name="\<computername\>" set AutomaticManagedPagefile=False**| 
|Configurare il file di paging|**WMIC pagefileset dove nome = "\<path/filename\>" impostare InitialSize =\<initialsize\>, MaximumSize =\<maxsize\>** <br>In cui *path/filename* è il percorso e nome del file di paging *initialsize* è la dimensione iniziale del file di paging, in byte, e *maxsize* è la dimensione massima del file di paging, in byte.|
|Passare a un indirizzo IP statico|**ipconfig /all** <br>Registra le informazioni rilevanti o reindirizzarlo a un file di testo (**ipconfig /all > ipconfig.txt**).<br>**interfacce di netsh interface ipv4 show**<br>Verificare che sia presente un elenco delle interfacce.<br>**netsh interface ipv4 set nome dell'indirizzo \<ID dall'elenco delle interfacce\> source = indirizzo statico =\<preferito indirizzo IP\> gateway =\<indirizzo gateway\>**<br>Eseguire **ipconfig /all** a vierfy che DHCP abilitato è impostata su **No**.|
|Impostare un indirizzo DNS statico.|**netsh interface ipv4 aggiungere dnsserver nome =\<nome o l'ID della scheda di interfaccia di rete\> indirizzo =\<indirizzo IP del server DNS primario\> indice = 1 <br>** netsh interface ipv4 aggiungere dnsserver nome =\<nome del server DNS secondario\> indirizzo =\<indirizzo IP del server DNS secondario\> indice = 2 * * <br> Ripetere base alle esigenze aggiungere altri server.<br>Eseguire **ipconfig /all** per verificare che gli indirizzi siano corretti.|
|Sostituire un indirizzo IP statico con un indirizzo IP fornito da DHCP|**netsh interface ipv4 set nome indirizzo =\<indirizzo IP del sistema locale\> origine = DHCP** <br>Eseguire **Ipconfig /all** per verificare che DHCP abilitato è impostata su **Yes**.|
|Immettere un codice Product Key|**slmgr. vbs – ipk \<"product key"\>**| 
|Attivare il server localmente|**slmgr.vbs -ato**| 
|Attivare il server in remoto|**cscript slmgr. vbs – ipk \<codice product key\>\<nome server\>\<username\>\<password\>** <br>**cscript slmgr.vbs -ato \<servername\> \<username\> \<password\>** <br>Ottiene il GUID del computer eseguendo **cscript slmgr. vbs-stato** <br> Eseguire **cscript slmgr. vbs - dli \<GUID\>** <br>Verificare che lo stato delle licenze sia impostato su **(attivato) concesso in licenza**.


### <a name="networking-and-firewall"></a>Rete e firewall

|Attività|Comando| 
|----|-------|
|Configurare il server per usare un server proxy|**Netsh Winhttp set proxy \<servername\>:\<il numero di porta\>** <br>**Nota:** Installazioni Server Core non è possibile accedere a Internet tramite un proxy che richiede una password per consentire le connessioni.|
|Configurare il server per ignorare il proxy per gli indirizzi Internet|**Netsh winttp impostazione proxy \<servername\>:\<il numero di porta\> bypass-list = "\<locale\>"**| 
|Visualizzare o modificare la configurazione di IPSEC|**netsh ipsec**| 
|Visualizzare o modificare la configurazione di protezione accesso alla rete|**netsh nap**| 
|Visualizzare o modificare l'indirizzo IP di traduzione da indirizzo fisico|**arp**| 
|Visualizzare o configurare la tabella di routing locale|**route**| 
|Visualizzare o configurare le impostazioni del server DNS|**nslookup**| 
|Visualizzare le statistiche di protocollo e le connessioni di rete TCP/IP correnti|**netstat**| 
|Visualizzare le statistiche di protocollo e le connessioni TCP/IP correnti con NetBIOS su TCP/IP (NBT)|**nbtstat**| 
|Visualizzare hop per le connessioni di rete|**pathping**| 
|Tracciare hop per le connessioni di rete|**tracert**| 
|Visualizzare la configurazione del router multicast|**mrinfo**| 
|Abilitare l'amministrazione remota del firewall|**Netsh advfirewall firewall set gruppo di regole = "Gestione remota di Windows Firewall" nuovo enable = yes**| 
 

### <a name="updates-error-reporting-and-feedback"></a>Gli aggiornamenti, segnalazione errori e commenti e suggerimenti

|Attività|Comando| 
|----|-------|
|Installare un aggiornamento|**wusa \<update\>.msu /quiet**| 
|Elencare gli aggiornamenti installati|**systeminfo**| 
|Rimuovere un aggiornamento|**expand /f:\* \<update\>.msu c:\test** <br>Passare a c:\test\ e aprire \<aggiornare\>. XML in un editor di testo.<br>Sostituire **installare** con **rimuovere** e salvare il file.<br>**pkgmgr /n:\<update\>.xml**|
|Configurare gli aggiornamenti automatici|To verify the current setting: **cscript %systemroot%\system32\scregedit.wsf /AU /v **<br>Per abilitare gli aggiornamenti automatici: **cscript scregedit.wsf /AU 4** <br>Per disabilitare gli aggiornamenti automatici: **cscript %systemroot%\system32\scregedit.wsf /AU 1**| 
|Abilitare la segnalazione di errori|Per verificare l'impostazione corrente: **serverWerOptin /query** <br>Per inviare automaticamente rapporti dettagliati: **serverWerOptin / dettagliate** <br>Per inviare automaticamente rapporti brevi: **serverWerOptin /Summary.** <br>Per disabilitare segnalazione errori: **serverWerOptin /disable**|
|Partecipare al programma Analisi utilizzo software|Per verificare l'impostazione corrente: **serverCEIPOptin /query** <br>Per abilitare analisi utilizzo software: **serverCEIPOptin /enable** <br>Per disabilitare analisi utilizzo software: **serverCEIPOptin /disable**|

### <a name="services-processes-and-performance"></a>Servizi, processi e prestazioni

|Attività|Comando| 
|----|-------|
|Elencare i servizi in esecuzione|**query sc** o **net start**| 
|Avviare un servizio|**inizio sc \<nome del servizio\>**  oppure **net start \<nome servizio\>**| 
|Arrestare un servizio|**arresto di sc \<nome del servizio\>**  oppure **net stop \<nome servizio\>**| 
|Recuperare un elenco di applicazioni in esecuzione e di processi associati|**tasklist**||Arrestare un processo in modo forzato|Eseguire **tasklist** recuperare l'ID processo (PID), quindi eseguire **taskkill /PID \<ID processo\>**|
|Avviare Gestione attività|**taskmgr**| 
|Creare e gestire registri di sessione e le prestazioni di traccia eventi|Per creare un contatore, traccia, la raccolta dei dati di configurazione o l'API: **logman creare** <br>Cercare le proprietà dell'agente di raccolta dati: **logman query** <br>Per avviare o arrestare la raccolta dei dati: **logman start\|arrestare** <br>Per eliminare un agente di raccolta: **logman delete** <br> Per aggiornare le proprietà di un agente di raccolta: **logman update** <br>Per importare un set di agenti di raccolta dati da un file XML o esportarli in un file XML: **logman import\|esportare**|

### <a name="event-logs"></a>Registri eventi

|Attività|Comando| 
|----|-------|
|Elencare i log eventi|**wevtutil el**| 
|Query sugli eventi in un registro specificato|**wevtutil qe /f:text \<nome registro\>**| 
|Esportare un log eventi|**wevtutil epl \<nome registro\>**| 
|Eliminare un log eventi|**cl wevtutil \<nome registro\>**| 


### <a name="disk-and-file-system"></a>Disco e file system

|Attività|Comando|
|----|-------|
|Gestire le partizioni del disco|Per un elenco completo dei comandi, eseguire **diskpart /?**|  
|Gestire RAID software|Per un elenco completo dei comandi, eseguire **diskraid /?**|  
|Gestire punti di montaggio dei volumi|Per un elenco completo dei comandi, eseguire **mountvol /?**| 
|Deframmentare un volume|Per un elenco completo dei comandi, eseguire **defrag /?**|  
|Convertire un volume al file system NTFS|**convertire \<lettera di volume\> /FS: NTFS**| 
|Comprimere un file|Per un elenco completo dei comandi, eseguire **compact /?**|  
|Amministrare file aperti|Per un elenco completo dei comandi, eseguire **openfiles /?**|  
|Amministrare cartelle VSS|Per un elenco completo dei comandi, eseguire **vssadmin /?**| 
|Amministrare il file system|Per un elenco completo dei comandi, eseguire **fsutil /?**||Verificare una firma di file|**sigverif /?**| 
|Diventare proprietario di un file o di una cartella|Per un elenco completo dei comandi, eseguire **icacls /?**| 
 
### <a name="hardware"></a>Hardware

|Attività|Comando| 
|----|-------|
|Aggiungere un driver per un nuovo dispositivo hardware|Copiare il driver in una cartella in % homedrive %\\\<cartella driver\>. Eseguire **pnputil -i-% homedrive %\\\<cartella driver\>\\\<driver\>. inf**|
|Rimuovere un driver per un dispositivo hardware|Per un elenco di driver caricati, eseguire **tipo di query sc = driver**. Quindi eseguire **sc delete \<service_name\>**|
