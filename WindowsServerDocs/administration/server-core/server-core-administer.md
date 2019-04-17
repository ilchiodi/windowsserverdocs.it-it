---
title: Amministrare Server Core
description: Scopri come gestire un'installazione Server Core di Windows Server
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.author: elizapo
ms.localizationpriority: medium
ms.date: 12/18/2018
ms.openlocfilehash: 39fbb92645d39a46613f2142d0258c78a6ba425b
ms.sourcegitcommit: 4df1bc940338219316627ad03f0525462a05a606
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/19/2018
ms.locfileid: "8977998"
---
# Amministrare un server di Server Core

>Si applica a: Windows Server 2016 (Canale semestrale) e Windows Server 2016

Poiché un'interfaccia utente non dispone di Server Core, è necessario utilizzare i cmdlet Windows PowerShell, gli strumenti da riga di comando o gli strumenti remoti per eseguire attività di amministrazione di base. Le sezioni seguenti delineano i cmdlet di PowerShell e i comandi usati per attività di base. È anche possibile utilizzare [Windows Admin Center](../../manage/windows-admin-center/overview.md), un portale di gestione unificata attualmente in anteprima pubblica, per amministrare l'installazione. 

## Attività amministrative utilizzando i cmdlet di PowerShell
Usa le informazioni seguenti per eseguire attività amministrative di base con i cmdlet di Windows PowerShell.

### Impostare un indirizzo IP statico
Quando si installa un server di Server Core, per impostazione predefinita include DHCP un indirizzo. Se hai bisogno di un indirizzo IP statico, è possibile impostare con i passaggi seguenti.

Per visualizzare la configurazione della rete corrente, utilizzare **Get-NetIPConfiguration**.

Per visualizzare gli indirizzi IP che già usi, utilizzare **Get-NetIPAddress**.

Per impostare un indirizzo IP statico, Esegui le operazioni seguenti: 

1. Esegui **Get-NetIPInterface**. 
2. Prendere nota del numero nella colonna **IfIndex** per l'interfaccia IP o la stringa **InterfaceDescription** . Se hai più di una scheda di rete, tieni presente il numero o la stringa corrispondente all'interfaccia che si desidera impostare l'indirizzo IP statico.
3. Eseguire il cmdlet seguente per impostare l'indirizzo IP statico:

   ```powershell
   New-NetIPaddress -InterfaceIndex 12 -IPAddress 192.0.2.2 -PrefixLength 24 -DefaultGateway 192.0.2.1
   ```

   dove:
   - **InterfaceIndex** è il valore di **IfIndex** dal passaggio 2. (Nel nostro esempio, 12)
   - **Indirizzo IP** è l'indirizzo IP statico che vuoi impostare. (Nel nostro esempio, 191.0.2.2)
   - **PrefixLength** è la lunghezza del prefisso (un altro formato della subnet mask) per l'indirizzo IP che imposti. (Ad esempio, 24)
   - **DefaultGateway** è l'indirizzo IP del gateway predefinito. (Ad esempio, 192.0.2.1)
4. Eseguire il cmdlet seguente per impostare il client DNS indirizzo del server: 

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

   in questo esempio, in cui, **192.0.2.4** e **192.0.2.5** sono entrambi gli indirizzi IP del server DNS.

Se devi passare a usando DHCP, Esegui **DnsClientServerAddress Set-InterfaceIndex 12-ResetServerAddresses**.

### Aggiunta a un dominio
Utilizzare i cmdlet seguenti per aggiungere un computer a un dominio.

1. Eseguire **Aggiungi Computer**. Ti verrà richiesto per entrambe le credenziali per l'aggiunta al dominio e il nome di dominio.
2. Se devi aggiungere un account utente di dominio al gruppo Administrators locale, Esegui il comando seguente al prompt dei comandi (non nella finestra di PowerShell):

   ```
   net localgroup administrators /add <DomainName>\<UserName>
   ```
3. Riavvia il computer. Puoi farlo eseguendo **Restart-Computer**.

### Rinomina il server
Utilizzare la procedura seguente per rinominare il server.

1. Determinare il nome del server con il **nome host** o **ipconfig** comando corrente.
2. Eseguire **Rinomina Computer \<new_name\ - ComputerName >**.
3. Riavvia il computer.

### Attivare il server

Eseguire **slmgr. vbs – ipk\<productkey\ >**. Quindi Esegui **slmgr. vbs – /ato**. Se l'attivazione ha esito positivo, non ricevi un messaggio.

> [!NOTE]
> È inoltre possibile attivare il server tramite telefono, usando un [server del servizio di gestione delle chiavi (KMS)](../../get-started/server-2016-activation.md), o in remoto. Per attivare in remoto, Esegui il cmdlet seguente da un computer remoto: 

>```powershell
>**cscript windows\system32\slmgr.vbs <ServerName> <UserName> <password>:-ato**
>```
 
### Configurare Windows Firewall

È possibile configurare Windows Firewall in locale nel computer di Server Core con script e i cmdlet di Windows PowerShell. Vedi [NetSecurity](/powershell/module/netsecurity/?view=win10-ps) per i cmdlet che è possibile utilizzare per configurare Windows Firewall.

### Abilitare la comunicazione remota di Windows PowerShell

È possibile abilitare Windows PowerShell Remoting, in cui i comandi digitati in Windows PowerShell in un computer eseguire in un altro computer. Abilitare la comunicazione remota di Windows PowerShell con **Enable-PSRemoting**.

Per altre informazioni, vedi [Sulle domande frequenti su remoto](/powershell/module/microsoft.powershell.core/about/about_remote_faq?view=powershell-5.1)
</br>

## Attività amministrative dalla riga di comando
Usa le informazioni di riferimento seguenti per eseguire attività amministrative dalla riga di comando.

### Installazione e configurazione
|Attività | Comando |
|-----|-------|
|Impostare la password amministrativa locale| **amministratore degli utenti NET** \* |
|Aggiungere un computer a un dominio| **netdom join % computername %** **/domain:\<domain\ > /userd:\<domain\\username\ > /passwordd:**\* <br> Riavvia il computer.|
|Verifica che il dominio è cambiato| **set** |
|Rimuovere un computer da un dominio|**Rimuovi Netdom \ < computername\ >**| 
|Aggiungere un utente al gruppo Administrators locale|**gli amministratori del gruppo locale del comando NET / aggiungere \ < domain\\username\ >** |
|Rimuovere un utente dal gruppo Administrators locale|**NET gruppo locale Administrators /delete \ < domain\\username\ >** |
|Aggiungere un utente al computer locale|**utente netto \ < domain\username\ > * / Aggiungi** |
|Aggiungere un gruppo di computer locale|**NET gruppo locale \ < gruppo name\ > / Aggiungi**|
|Modificare il nome di un computer appartenenti al dominio|**Netdom renamecomputer % computername % /NewName:\<new computer name\ > /userd:\<domain\\username\ > /passwordd:** * |
|Verifica che il nuovo nome del computer|**set**| 
|Modificare il nome di un computer in un gruppo di lavoro|**Netdom renamecomputer \ < currentcomputername\ > /NewName: \ < newcomputername\ >** <br>Riavvia il computer.|
|Disabilitare la gestione dei file di paging|**WMIC computer in cui name = "< computername\ >" imposta AutomaticManagedPagefile = False**| 
|Configurare il file di paging|**WMIC pagefileset dove name = "< percorso/filename\ >" impostare InitialSize = \ < initialsize\ > MaximumSize = \ < maxsize\ >** <br>Dove *percorso/nome del file* è il percorso e nome del file di paging, *initialsize* è la dimensione iniziale del file di paging, in byte, e *maxsize* è la dimensione massima del file di paging, in byte.|
|Modifica in un indirizzo IP statico|**ipconfig/tutti** <br>Registrare le informazioni pertinenti o reindirizzate a un file di testo (**ipconfig/tutti > ipconfig txt**).<br>**interfacce di mostrare netsh interface ipv4**<br>Verificare che ci sia un elenco delle interfacce.<br>**netsh interface ipv4 impostare il nome di indirizzo \ origine < ID dall'interfaccia list\ > = indirizzo statico = \ < preferito IP: [indirizzo e-mail > gateway = \ <: [indirizzo e-mail gateway >**<br>Eseguire **ipconfig/tutti** a vierfy che DHCP abilitato è impostato su **No**.|
|Impostare un indirizzo statico DNS.|**netsh interface ipv4 aggiungere ServerDNS name = \<name o ID di card\ di interfaccia di rete > indirizzo = \<IP indirizzo del server DNS primari > indice = 1 <br> **netsh interface ipv4 Aggiungi nome ServerDNS = \<name di server DNS secondario > indirizzo = \<IP indirizzo del server DNS secondario > indice = 2 * * <br> Ripeti come appropriato per aggiungere server aggiuntivi.<br>Eseguire **ipconfig/tutti** per verificare che gli indirizzi siano corretti.|
|Passare a un indirizzo IP DHCP fornita da un indirizzo IP statico|**netsh interface ipv4 impostare il nome di indirizzo = \ origine < indirizzo IP locale è > = DHCP** <br>Eseguire **Ipconfig/tutti** per verificare che DHCP abilitato è impostata su **Yes**.|
|Immettere un codice product key|**slmgr. vbs – ipk \ < codice prodotto >**| 
|Attivare il server in locale|**slmgr. vbs - /ato**| 
|Attivare il server in remoto|**cscript slmgr. vbs – ipk \ < codice prodotto > \ < name\ server > \ < username\ > \ < password\ >** <br>**cscript slmgr. vbs - /ato \ < servername\ > \ < username\ > \ < password\ >** <br>Ottenere il GUID del computer eseguendo **slmgr. vbs cscript-fatto** <br> Eseguire **cscript slmgr. vbs - dli \<GUID\ >** <br>Verifica che lo stato di licenza è impostato su **concesso in licenza (attivata)**.


### Rete e firewall

|Attività|Comando| 
|----|-------|
|Configurare il server per usare un server proxy|**Netsh Winhttp set proxy \ < servername\ >: \ < porta \<sequence >** <br>**Nota:** Installazioni Server Core non possono accedere a Internet tramite un proxy che richiede una password per consentire le connessioni.|
|Configurare il server per ignorare il proxy per indirizzi Internet|**Netsh winttp impostare proxy \ < servername\ >: \ < porta \<sequence >-l'elenco di esclusione = "< local\ >"**| 
|Visualizzare o modificare la configurazione IPSEC|**netsh ipsec**| 
|Visualizzare o modificare la configurazione di protezione accesso alla rete|**Protezione accesso alla rete Netsh**| 
|Visualizzare o modificare IP per la conversione degli indirizzi fisici|**arp**| 
|Visualizzare o configurare la tabella di routing locale|**route**| 
|Visualizzare o configurare le impostazioni del server DNS|**nslookup**| 
|Visualizzare le statistiche di protocollo e le connessioni di rete TCP/IP correnti|**netstat**| 
|Visualizzare le statistiche di protocollo e le connessioni TCP/IP correnti utilizzando NetBIOS su TCP/IP (NBT)|**nbtstat**| 
|Visualizzazione dei passaggi per le connessioni di rete|**pathping**| 
|Tenere traccia dei passaggi per le connessioni di rete|**tracert**| 
|Visualizzare la configurazione del router multicast|**mrinfo**| 
|Abilita l'amministrazione remota del firewall|**Netsh advfirewall firewall set gruppo regola = "Gestione remota di Windows Firewall" nuova enable = yes**| 
 

### Aggiornamenti, segnalazione errori e feedback

|Attività|Comando| 
|----|-------|
|Installare un aggiornamento|**WUSA \ < update\ > msu /quiet**| 
|Aggiornamenti installato elenco|**systeminfo**| 
|Rimuovere un aggiornamento|**Espandi /f:\* \ < update\ > msu c:\test** <br>Passa a c:\test\ e Apri \ < update\ > XML in un editor di testo.<br>Sostituisci **installare** con **rimuovere** e quindi Salva il file.<br>**pkgmgr /n:\ < update\ > XML**|
|Configurare gli aggiornamenti automatici|Per verificare l'impostazione corrente: * * cscript %systemroot%\system32\scregedit.wsf /AU /v * *<br>Per abilitare gli aggiornamenti automatici: **scregedit.wsf cscript /AU 4** <br>Per disattivare gli aggiornamenti automatici: **%systemroot%\system32\scregedit.wsf cscript /AU 1**| 
|Abilitare la segnalazione errori|Per verificare l'impostazione corrente: **/query serverWerOptin** <br>Per inviare automaticamente report dettagliati: **serverWerOptin / dettagliate** <br>Per inviare automaticamente i rapporti di riepilogo: **serverWerOptin /summary** <br>Per disattivare la segnalazione: **/disable serverWerOptin**|
|Partecipare al programma Analisi utilizzo software (analisi utilizzo software)|Per verificare l'impostazione corrente: **/query serverCEIPOptin** <br>Per abilitare l'analisi utilizzo software: **/enable serverCEIPOptin** <br>Per disabilitare l'analisi utilizzo software: **/disable serverCEIPOptin**|

### Servizi, processi e le prestazioni

|Attività|Comando| 
|----|-------|
|Elencare i servizi in esecuzione|**sc query** o **net start**| 
|Avviare un servizio|**sc start \<service name\ >** o **net start \<service name\ >**| 
|Arrestare un servizio|**sc stop \<service name\ >** o **net stop \<service name\ >**| 
|Recuperare un elenco di applicazioni in esecuzione e processi associati|**tasklist**||Arrestare forzare un processo|Eseguire **tasklist** recuperare l'ID processo (PID), quindi eseguire **taskkill /PID \<process ID\ >**|
|Avviare Gestione attività|**Taskmgr**| 
|Creare e gestire i registri eventi di traccia sessione e prestazioni|Per creare un contatore, analisi, la raccolta dei dati di configurazione o API: **logman ceate** <br>Per le proprietà di agente di raccolta dei dati di query: **logman query** <br>Per avviare o interrompere la raccolta dei dati: **start\ logman | stop** <br>Per eliminare un agente di raccolta: **Elimina logman** <br> Per aggiornare le proprietà di un agente di raccolta: **logman aggiornare** <br>Per importare un set di agente di raccolta dei dati da un file XML o esportarlo in un file XML: **import\ logman | export**|

### Registri eventi

|Attività|Comando| 
|----|-------|
|Registri eventi di elenco|**el wevtutil**| 
|Eventi di query in un registro specificato|**wevtutil qe /f:text \ < registrare name\ >**| 
|Esportare un registro eventi|**wevtutil. exe epl \ < registrare name\ >**| 
|Cancellare un registro eventi|**cl wevtutil \ < registrare name\ >**| 


### Disco e nel file system

|Attività|Comando|
|----|-------|
|Gestire le partizioni del disco|Per un elenco completo dei comandi, Esegui **diskpart /?**|  
|Gestire RAID software|Per un elenco completo dei comandi, Esegui **diskraid /?**|  
|Gestire i punti di montaggio|Per un elenco completo dei comandi, Esegui **mountvol /?**| 
|Deframmenta un volume|Per un elenco completo dei comandi, Esegui **deframmentazione /?**|  
|Convertire un volume al file system NTFS|**convertire \ < volume letter\ > /FS**| 
|Compatta un file|Per un elenco completo dei comandi, Esegui **compact /?**|  
|Amministrare i file aperti|Per un elenco completo dei comandi, Esegui **openfiles /?**|  
|Amministrazione delle cartelle VSS|Per un elenco completo dei comandi, Esegui **vssadmin /?**| 
|Amministrare il file system|Per un elenco completo dei comandi, Esegui **fsutil /?**||Verificare una firma di file|**Sigverif /?**| 
|Acquisizione proprietà di un file o una cartella|Per un elenco completo dei comandi, Esegui **icacls /?**| 
 
### Hardware

|Attività|Comando| 
|----|-------|
|Aggiungere un driver per un nuovo dispositivo hardware|Copiare il driver in una cartella alla %homedrive%\\\ < driver folder\ >. Eseguire **pnputil -i - un folder\ %homedrive%\\\<driver > \\\<driver\ > inf**|
|Rimuovere un driver per un dispositivo hardware|Per un elenco di driver caricati, Esegui **tipo di query sc = driver**. Quindi Esegui **sc eliminare \<service_name\ >**|
