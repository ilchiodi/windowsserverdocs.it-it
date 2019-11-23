---
title: Amministrare i componenti di base del server
description: Informazioni su come amministrare un'installazione Server Core di Windows Server
ms.prod: windows-server
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.author: elizapo
ms.localizationpriority: medium
ms.date: 12/18/2018
ms.openlocfilehash: bcc4bf7b3fbdbff1aed2c8dd07b90346fe9eebab
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383428"
---
# <a name="administer-a-server-core-server"></a>Amministrare un server Server Core

>Si applica a: Windows Server 2019, Windows Server 2016 e Windows Server (canale semestrale)

Poiché Server Core non ha un'interfaccia utente, è necessario usare i cmdlet di Windows PowerShell, gli strumenti da riga di comando o Remote Tools per eseguire attività amministrative di base. Le sezioni seguenti descrivono i cmdlet e i comandi di PowerShell usati per le attività di base. Per amministrare l'installazione, è anche possibile usare l'interfaccia di [amministrazione di Windows](../../manage/windows-admin-center/overview.md), un portale di gestione unificato attualmente disponibile in anteprima pubblica. 

## <a name="administrative-tasks-using-powershell-cmdlets"></a>Attività amministrative con i cmdlet di PowerShell
Usare le informazioni seguenti per eseguire attività amministrative di base con i cmdlet di Windows PowerShell.

### <a name="set-a-static-ip-address"></a>Impostare un indirizzo IP statico
Quando si installa un server Server Core, per impostazione predefinita ha un indirizzo DHCP. Se è necessario un indirizzo IP statico, è possibile impostarlo attenendosi alla procedura seguente.

Per visualizzare la configurazione di rete corrente, usare **Get-NetIPConfiguration**.

Per visualizzare gli indirizzi IP già in uso, usare **Get-NetIPAddress**.

Per impostare un indirizzo IP statico, procedere come segue: 

1. Eseguire **Get-NetIPInterface**. 
2. Prendere nota del numero nella colonna **ifindex** per l'interfaccia IP o la stringa **InterfaceDescription** . Se si dispone di più schede di rete, annotare il numero o la stringa corrispondente all'interfaccia per cui si vuole impostare l'indirizzo IP statico.
3. Eseguire il cmdlet seguente per impostare l'indirizzo IP statico:

   ```powershell
   New-NetIPaddress -InterfaceIndex 12 -IPAddress 192.0.2.2 -PrefixLength 24 -DefaultGateway 192.0.2.1
   ```

   dove:
   - **IndiceInterfaccia** è il valore di **ifindex** del passaggio 2. (In questo esempio 12)
   - **IPAddress** è l'indirizzo IP statico che si desidera impostare. (In questo esempio, 191.0.2.2)
   - **LunghezzaPrefisso** è la lunghezza del prefisso (un altro formato di subnet mask) per l'indirizzo IP che si sta impostando. (Per questo esempio, 24)
   - **GatewayPredefinito** è l'indirizzo IP del gateway predefinito. (Per questo esempio, 192.0.2.1)
4. Eseguire il cmdlet seguente per impostare l'indirizzo del server client DNS: 

   ```powershell
   Set-DNSClientServerAddress –InterfaceIndex 12 -ServerAddresses 192.0.2.4
   ```
   
   dove:
   - **IndiceInterfaccia** è il valore di ifindex del passaggio 2.
   - **ServerAddresses** è l'indirizzo IP del server DNS.
5. Per aggiungere più server DNS, eseguire il cmdlet seguente: 

   ```powershell
   Set-DNSClientServerAddress –InterfaceIndex 12 -ServerAddresses 192.0.2.4,192.0.2.5
   ```

   dove, in questo esempio, **192.0.2.4** e **192.0.2.5** sono entrambi indirizzi IP dei server DNS.

Se è necessario passare all'utilizzo di DHCP, eseguire **set-DnsClientServerAddress – IndiceInterfaccia 12 – ResetServerAddresses**.

### <a name="join-a-domain"></a>Aggiungere un dominio
Usare i cmdlet seguenti per aggiungere un computer a un dominio.

1. Eseguire **Add-computer**. Verranno richieste le credenziali per aggiungere il dominio e il nome di dominio.
2. Se è necessario aggiungere un account utente di dominio al gruppo Administrators locale, eseguire il comando seguente al prompt dei comandi (non nella finestra di PowerShell):

   ```
   net localgroup administrators /add <DomainName>\<UserName>
   ```
3. Riavviare il computer. È possibile eseguire questa operazione eseguendo **Restart-computer**.

### <a name="rename-the-server"></a>Rinominare il server
Utilizzare la procedura seguente per rinominare il server.

1. Determinare il nome corrente del server con il nome **host** o il comando **ipconfig** .
2. Eseguire **Rename-computer-computername \<new_name\>** .
3. Riavviare il computer.

### <a name="activate-the-server"></a>Attivare il server

Eseguire **slmgr. vbs – ipk\<productkey\>** . Eseguire quindi **slmgr. vbs – ato**. Se l'attivazione ha esito positivo, non viene ricevuto alcun messaggio.

> [!NOTE]
> È inoltre possibile attivare il server tramite telefono, utilizzando un [server del servizio di gestione delle chiavi (KMS)](../../get-started/server-2016-activation.md)o in remoto. Per attivare la modalità remota, eseguire il cmdlet seguente da un computer remoto: 
> 
> ```
> cscript windows\system32\slmgr.vbs <ServerName> <UserName> <password>:-ato
> ```
 
### <a name="configure-windows-firewall"></a>Configurare Windows Firewall

È possibile configurare Windows Firewall in locale nel computer Server Core utilizzando cmdlet di Windows PowerShell e script. Vedere [NetSecurity](/powershell/module/netsecurity/?view=win10-ps) per i cmdlet che è possibile usare per configurare Windows Firewall.

### <a name="enable-windows-powershell-remoting"></a>Abilitare la comunicazione remota di Windows PowerShell

È possibile abilitare la comunicazione remota di Windows PowerShell, che consente di eseguire in un computer i comandi digitati in Windows PowerShell in un altro computer. Abilitare la comunicazione remota di Windows PowerShell con **Enable-PSRemoting**.

Per ulteriori informazioni, vedere [informazioni sulle domande frequenti Remote](/powershell/module/microsoft.powershell.core/about/about_remote_faq?view=powershell-5.1).

## <a name="administrative-tasks-from-the-command-line"></a>Attività amministrative dalla riga di comando
Utilizzare le seguenti informazioni di riferimento per eseguire attività amministrative dalla riga di comando.

### <a name="configuration-and-installation"></a>Configurazione e installazione

|                             Attività                              |                                                                                                                                                                                                                 Comando                                                                                                                                                                                                                 |
|---------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|             Impostare la password amministrativa locale             |                                                                                                                                                                                                      \* **dell'amministratore di rete**                                                                                                                                                                                                      |
|                  Aggiungere un computer a un dominio                  |                                                                                                                                                       **netdom join% computername%** **/Domain:\<dominio\>/userd:\<dominio\\nomeutente\>/passwordd:** \* <br> Riavviare il computer.                                                                                                                                                        |
|              Verificare che il dominio è stato modificato              |                                                                                                                                                                                                                 **set**                                                                                                                                                                                                                 |
|                Rimuovere un computer da un dominio                |                                                                                                                                                                                                   **netdom Remove \<nomecomputer\>**                                                                                                                                                                                                    |
|         Aggiungere un utente al gruppo Administrators locale          |                                                                                                                                                                                       **net localgroup Administrators/Add \<dominio\\nomeutente\>**                                                                                                                                                                                       |
|       Rimuovere un utente dal gruppo Administrators locale       |                                                                                                                                                                                     **net localgroup Administrators/delete \<dominio\\nomeutente\>**                                                                                                                                                                                      |
|               Aggiungere un utente al computer locale                |                                                                                                                                                                                                **utente .NET \<dominio\nomeutente\> \*/Add**                                                                                                                                                                                                 |
|               Aggiungere un gruppo al computer locale               |                                                                                                                                                                                                 **net localgroup \<nome del gruppo\>/Add**                                                                                                                                                                                                  |
|          Modificare il nome di un computer aggiunto a un dominio          |                                                                                                                                                           **netdom renamecomputer% computername%/NewName:\<nuovo nome computer\>/userd:\<dominio\\nomeutente\>/passwordd:** \*                                                                                                                                                            |
|                 Confermare il nuovo nome del computer                 |                                                                                                                                                                                                                 **set**                                                                                                                                                                                                                 |
|         Modificare il nome di un computer in un gruppo di lavoro         |                                                                                                                                                                **netdom renamecomputer \<NomeComputerCorrente\>/NewName:\<NuovoNomeComputer\>** <br>Riavviare il computer.                                                                                                                                                                 |
|                Disabilitare la gestione del file di paging                 |                                                                                                                                                                        **wmic computersystem where name = "\<nomecomputer\>" set AutomaticManagedPagefile = False**                                                                                                                                                                         |
|                   Configurare il file di paging                   |                                                            **wmic pagefileset where name = "\<Path/Filename\>" set InitialSize =\<InitialSize\>, MaximumSize =\<MaxSize\>** <br>Dove *Path/Filename* è il percorso e il nome del file di paging, *InitialSize* è la dimensione iniziale del file di paging, in byte, e *MaxSize* è la dimensione massima del file di paging, in byte.                                                             |
|                 Passare a un indirizzo IP statico                 | **ipconfig.** <br>Registrare le informazioni rilevanti o reindirizzarle a un file di testo (**ipconfig, > ipconfig. txt**).<br>**netsh interface ipv4 show interfaces**<br>Verificare che sia presente un elenco di interfacce.<br>**netsh interface ipv4 impostare il nome dell'indirizzo \<ID dall'elenco di interfacce\> source = static address =\<indirizzo IP preferito\> gateway =\<indirizzo del gateway\>**<br>Eseguire **ipconfig-all** per verificare che DHCP enabled sia impostato su **No**. |
|                   Impostare un indirizzo DNS statico.                   |   <strong>netsh interface IPv4 add dnsserver name =\<nome o ID della scheda di interfaccia di rete\> Address =\<indirizzo IP del server DNS primario\> index = 1 <br></strong>netsh interface IPv4 add dnsserver name =\<nome del server DNS secondario\> Address =\<indirizzo IP del server DNS secondario\> index = 2\*\* <br> Ripetere l'installazione in base alle esigenze per aggiungere ulteriori server.<br>Eseguire **ipconfig-all** per verificare che gli indirizzi siano corretti.   |
| Sostituire un indirizzo IP statico con un indirizzo IP fornito da DHCP |                                                                                                                                      **netsh interface ipv4 set address name =\<indirizzo IP del sistema locale\> source = DHCP** <br>Eseguire **ipconfig-all** per verificare che DCHP enabled sia impostato su **Yes**.                                                                                                                                      |
|                      Immettere un codice Product Key                      |                                                                                                                                                                                                   **slmgr. vbs – ipk \<codice Product Key\>**                                                                                                                                                                                                    |
|                  Attivare il server localmente                  |                                                                                                                                                                                                           **slmgr. vbs-ato**                                                                                                                                                                                                            |
|                 Attivare il server in remoto                  |                                            **cscript slmgr. vbs – ipk \<Product Key\>nome del server \<\>\<nome utente\>\<password\>** <br>**cscript slmgr. vbs-ato \<ServerName\> \<nomeutente\> \<password\>** <br>Ottenere il GUID del computer eseguendo **cscript slmgr. vbs-did** <br> Eseguire **cscript slmgr. vbs-dli \<GUID\>** <br>Verificare che lo stato della licenza sia impostato su **concesso in licenza (attivato)** .                                             |

### <a name="networking-and-firewall"></a>Rete e firewall

|Attività|Comando| 
|----|-------|
|Configurare il server per l'utilizzo di un server proxy|**netsh winhttp set proxy \<ServerName\>: numero di porta\<\>** <br>**Nota:** Le installazioni dei componenti di base del server non possono accedere a Internet tramite un proxy che richiede una password per consentire le connessioni.|
|Configurare il server per ignorare il proxy per gli indirizzi Internet|**netsh winhttp set proxy \<ServerName\>:\<numero di porta\> bypass-list = "\<local\>"**| 
|Visualizzare o modificare la configurazione IPSEC|**Netsh IPSec**| 
|Visualizzare o modificare la configurazione di protezione accesso alla rete|**netsh nap**| 
|Visualizzare o modificare l'indirizzo IP alla conversione degli indirizzi fisici|**arp**| 
|Visualizzare o configurare la tabella di routing locale|**Route**| 
|Visualizzare o configurare le impostazioni del server DNS|**nslookup**| 
|Visualizzare le statistiche di protocollo e le connessioni di rete TCP/IP correnti|**netstat**| 
|Visualizzare le statistiche di protocollo e le connessioni TCP/IP correnti utilizzando NetBIOS su TCP/IP (NBT)|**nbtstat**| 
|Visualizzare gli hop per le connessioni di rete|**pathping**| 
|Traccia hop per le connessioni di rete|**tracert**| 
|Visualizzare la configurazione del router multicast|**mrinfo**| 
|Abilitare l'amministrazione remota del firewall|**netsh advfirewall firewall set Rule Group = "Windows Defender Firewall Remote Management" nuovo enable = Yes**| 
 

### <a name="updates-error-reporting-and-feedback"></a>Aggiornamenti, segnalazione errori e commenti e suggerimenti

|                               Attività                                |                                                                                                                               Comando                                                                                                                                |
|-------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                         Installare un aggiornamento                         |                                                                                                                    **WUSA \<Update\>. msu/quiet**                                                                                                                    |
|                      Elencare gli aggiornamenti installati                       |                                                                                                                            **systeminfo**                                                                                                                            |
|                         Rimuovere un aggiornamento                          |                                 **Espandere/f:\* \<aggiornare\>. msu c:\test** <br>Passare a c:\test\ e aprire \<aggiornare\>. XML in un editor di testo.<br>Sostituire **Installa** con **Rimuovi** e salvare il file.<br>**pkgmgr/n:\<aggiornamento\>. XML**                                 |
|                    Configurare gli aggiornamenti automatici                    |          Per verificare l'impostazione corrente: **cscript%SystemRoot%\SYSTEM32\SCREGEDIT.wsf/AU/v \*\*<br>per abilitare gli aggiornamenti automatici: \*\*cscript scregedit. wsf/AU 4** <br>Per disabilitare gli aggiornamenti automatici: **cscript%SystemRoot%\SYSTEM32\SCREGEDIT.wsf/AU 1**          |
|                      Abilitare la segnalazione di errori                       | Per verificare l'impostazione corrente: **serverWerOptin/query** <br>Per inviare automaticamente report dettagliati: **serverWerOptin/detailed** <br>Per inviare automaticamente report di riepilogo: **serverWerOptin/Summary** <br>Per disabilitare la segnalazione errori: **serverWerOptin/Disable** |
| Partecipare al programma Analisi utilizzo software |                                                     Per verificare l'impostazione corrente: **serverCEIPOptin/query** <br>Per abilitare analisi utilizzo software: **serverCEIPOptin/Enable** <br>Per disabilitare Analisi utilizzo software: **serverCEIPOptin/Disable**                                                      |

### <a name="services-processes-and-performance"></a>Servizi, processi e prestazioni

|                               Attività                               |                                                                                                                                                                                                             Comando                                                                                                                                                                                                              |
|------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                    Elencare i servizi in esecuzione                     |                                                                                                                                                                                                  **Query SC** o **net start**                                                                                                                                                                                                   |
|                         Avviare un servizio                          |                                                                                                                                                                                 **sc start \<nome servizio\>** o **net start \<nome servizio\>**                                                                                                                                                                                  |
|                          Arrestare un servizio                          |                                                                                                                                                                                  **SC arrestare il nome del servizio \<\>** o **net stop \<nome del servizio\>**                                                                                                                                                                                   |
| Recuperare un elenco di applicazioni in esecuzione e di processi associati |                                                                                                                                                                                                           **tasklist**                                                                                                                                                                                                           |
|                        Avviare Gestione attività                        |                                                                                                                                                                                                           **taskmgr**                                                                                                                                                                                                            |
|    Creare e gestire i registri delle prestazioni e della sessione di traccia eventi    | Per creare un contatore, traccia, raccolta dati di configurazione o API: **logman creare** <br>Per eseguire query sulle proprietà dell'agente di raccolta dati: **query logman** <br>Per avviare o arrestare la raccolta dati: **logman start\|stop** <br>Per eliminare un agente di raccolta: **logman Delete** <br> Per aggiornare le proprietà di un agente di raccolta: **logman update** <br>Per importare un insieme agenti di raccolta dati da un file XML o esportarlo in un file XML: **logman import\|Export** |

### <a name="event-logs"></a>Registri eventi

|Attività|Comando| 
|----|-------|
|Elenca i registri eventi|**wevtutil El**| 
|Eseguire query sugli eventi in un log specificato|**wevtutil qe/f: nome registro \<testo\>**| 
|Esportare un registro eventi|**wevtutil epl \<nome registro\>**| 
|Cancellazione di un registro eventi|**wevtutil CL \<nome registro\>**| 


### <a name="disk-and-file-system"></a>Disco e file system

|                   Attività                   |                        Comando                        |
|------------------------------------------|-------------------------------------------------------|
|          Gestire le partizioni del disco          | Per un elenco completo dei comandi, eseguire **DiskPart/?**  |
|           Gestire RAID software           | Per un elenco completo dei comandi, eseguire **DiskRAID/?**  |
|        Gestire punti di montaggio dei volumi        | Per un elenco completo dei comandi, eseguire **mountvol/?**  |
|           Deframmentare un volume            |  Per un elenco completo dei comandi, eseguire **Defrag/?**   |
| Convertire un volume al file system NTFS |        **convertire \<lettera volume\>/FS: NTFS**         |
|              Comprimere un file              |  Per un elenco completo dei comandi, eseguire **Compact/?**  |
|          Amministrare file aperti           | Per un elenco completo dei comandi, eseguire **openfiles/?** |
|          Amministrare cartelle VSS          | Per un elenco completo dei comandi, eseguire **vssadmin/?**  |
|        Amministrare il file system        |  Per un elenco completo dei comandi, eseguire **fsutil/?**   |
|    Diventare proprietario di un file o di una cartella    |  Per un elenco completo dei comandi, eseguire **icacls/?**   |
 
### <a name="hardware"></a>Hardware

|Attività|Comando| 
|----|-------|
|Aggiungere un driver per un nuovo dispositivo hardware|Copiare il driver in una cartella in% HOMEDRIVE%\\\<cartella del driver\>. Eseguire **pnputil-i-a% HOMEDRIVE%\\\<cartella del driver\>\\\<driver\>. inf**|
|Rimuovere un driver per un dispositivo hardware|Per un elenco di driver caricati, eseguire **SC query type = driver**. Eseguire quindi **sc delete \<service_name\>**|
