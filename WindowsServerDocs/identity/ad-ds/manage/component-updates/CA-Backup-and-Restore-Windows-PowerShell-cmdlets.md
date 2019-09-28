---
ms.assetid: 7e195f5b-b194-40f3-a26d-5cf4ade5fc4d
title: Cmdlet di Windows PowerShell per il backup e il ripristino della CA
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 796c10d36428e088f3c1fffe293fc7c414993eb2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71389961"
---
# <a name="ca-backup-and-restore-windows-powershell-cmdlets"></a>Cmdlet di Windows PowerShell per il backup e il ripristino della CA

> Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
> 
> **Autore**: Justin Turner, Senior Support Escalation Engineer con il gruppo di Windows  
> 
> [!NOTE]
> Questo contenuto è stato redatto da un ingegnere del Supporto tecnico Microsoft ed è destinato ad amministratori esperti e architetti di sistemi che desiderano una spiegazione tecnica delle funzionalità e delle soluzioni relative a Windows Server 2012 R2 più approfondita rispetto agli argomenti solitamente disponibili su TechNet. Non è stato tuttavia sottoposto agli stessi passaggi redazionali e, di conseguenza, per alcune lingue potrebbe essere meno accurato della documentazione che si trova in genere su TechNet.  
  
## <a name="overview"></a>Panoramica  
Il modulo ADCSAdministration di Windows PowerShell è stato introdotto in Window Server 2012.  Sono stati aggiunti due nuovi cmdlet a questo modulo in Window Server 2012 R2 per supportare il backup e il ripristino di una CA.  
  
-   Backup-CARoleService  
  
-   Restore-CARoleService  
  
## <a name="backup-caroleservice"></a>Backup-CARoleService  
**Table SEQ Table \\ @ no__t-2 ARABIC 17: Cmdlet di Windows PowerShell per il backup e il ripristino @ no__t-0  
  
Cmdlet **ADCSAdministration: Backup-CARoleService @ no__t-0  
  
|Argomenti: gli argomenti in **grassetto** sono obbligatori|Descrizione|  
|------------------------------------------------|---------------|  
|**-Percorso**|-String-location per salvare il backup<br />-Questo è l'unico parametro senza nome<br />-parametro posizionale<br /><br />**Esempio:**<br /><br />Backup-CARoleService.-Path c:\adcsbackup1<br /><br />Backup-CARoleService c:\adcsbackup2|  
|-Solo per la|-Eseguire il backup del certificato della CA senza il database<br /><br />**Esempio:**<br /><br />Backup-CARoleService c:\adcsbackup3-solo per le tue|  
|-Password|-Specifica la password per proteggere i certificati e le chiavi private della CA<br />-Deve essere una stringa sicura<br />-Non valido con il parametro-DatabaseOnly<br /><br />Esempio:<br /><br />Backup-CARoleService c:\adcsbackup4-password (read-host-prompt "password:"-AsSecureString)<br /><br />Backup-CARoleService c:\adcsbackup5-password (ConvertTo-SecureString "Pa55w0rd!" -AsPlainText-Force)|  
|-DatabaseOnly|-Eseguire il backup del database senza il certificato della CA<br /><br />Backup-CARoleService c:\adcsbackup6-DatabaseOnly|  
|-Force|1.  Consente di sovrascrivere il backup che preesiste nel percorso specificato nel parametro-path<br /><br />Backup-CARoleService c:\adcsbackup1-Force|  
|-Incrementale|-Eseguire un backup incrementale<br /><br />Backup-CARoleService c:\adcsbackup7-incrementale|  
|-KeepLog|1.  Indica al comando di memorizzare i file di log. Se l'opzione non è specificata, i file di log vengono troncati per impostazione predefinita, eccetto nello scenario incrementale<br /><br />Backup-CARoleService c:\adcsbackup7-KeepLog|  
  
### <a name="-password-secure-string"></a>-Password <Secure String>  
Se si usa il parametro-password, la password specificata deve essere una stringa sicura.  Usare il cmdlet **Read-Host** per avviare un prompt interattivo per l'immissione di una password sicura oppure usare il cmdlet **ConvertTo-SecureString** per specificare la password inline.  
  
Esaminare gli esempi seguenti  
  
**Specifica di una stringa sicura per il parametro password con read-host**  
  
```powershell  
Backup-CARoleService c:\adcsbackup4 -Password (Read-Host -prompt "Password:" -AsSecureString)  
```  
  
**Specifica di una stringa sicura per il parametro password mediante ConvertTo-SecureString**  
  
```powershell  
Backup-CARoleService c:\adcsbackup5 -Password (ConvertTo-SecureString "Pa55w0rd!" -AsPlainText -Force)  
```  
  
## <a name="restore-caroleservice"></a>Restore-CARoleService  
Cmdlet **ADCSAdministration: Restore-CARoleService @ no__t-0  
  
|Argomenti: gli argomenti in **grassetto** sono obbligatori|Descrizione|  
|------------------------------------------------|---------------|  
|**-Percorso**|-Stringa-percorso da cui ripristinare il backup<br />-Questo è l'unico parametro senza nome<br />-parametro posizionale<br /><br />**Esempio:**<br /><br />Restore-CARoleService.-Path c:\adcsbackup1-Force<br /><br />Restore-CARoleService c:\adcsbackup2-Force|  
|-Solo per la|-Ripristinare il certificato della CA senza il database<br />-È necessario specificare se il backup è stato effettuato con l'opzione-solo<br /><br />**Esempio:**<br /><br />Restore-CARoleService c:\adcsbackup3-DataOnly-Force|  
|-Password|-Specifica la password dei certificati della CA e delle chiavi private<br />-Deve essere una stringa sicura<br /><br />**Esempio:**<br /><br />Restore-CARoleService c:\adcsbackup4-password (read-host-prompt "password:"-AsSecureString)-Force<br /><br />Restore-CARoleService c:\adcsbackup5-password (ConvertTo-SecureString "Pa55w0rd!" -AsPlainText-Force)-Force|  
|-DatabaseOnly|-Ripristinare il database senza il certificato della CA<br /><br />Restore-CARoleService c:\adcsbackup6-DatabaseOnly|  
|-Force|: Consente di sovrascrivere le chiavi preesistenti<br />-È un parametro facoltativo ma durante il ripristino sul posto, è probabile che sia necessario<br /><br />Restore-CARoleService c:\adcsbackup1-Force|  
  
### <a name="issues"></a>Problemi  
Se la funzione ConvertTo-SecureString ha esito negativo quando si usa il cmdlet Backup-CARoleService con il parametro-password, viene eseguito un backup non protetto da password.  
  
![Backup e ripristino della CA](media/CA-Backup-and-Restore-Windows-PowerShell-cmdlets/GTR_ADDS_BackupCARole.gif)  
  
**Table SEQ Table \\ @ no__t-2 ARABIC 18: Errori comuni @ no__t-0  
  
|Azione|Errore|Commento|  
|----------|---------|-----------|  
|**Restore-CARoleService C:\ADCSBackup**|Restore-CARoleService: Il processo non può accedere al file perché è in uso da un altro processo. (Eccezione da HRESULT:<br /><br />0x80070020|Arrestare il servizio Servizi certificati Active Directory prima di eseguire il cmdlet Restore-CARoleService|  
|**Restore-CARoleService C:\ADCSBackup**|Restore-CARoleService: La directory non è vuota. (Eccezione da HRESULT: 0x80070091)|Usare il parametro-Force per sovrascrivere le chiavi preesistenti|  
|**Backup-CARoleService C:\ADCSBackup-password (read-host-prompt "password:"-AsSecureString)-DatabaseOnly**|Backup-CARoleService: Impossibile risolvere il set di parametri utilizzando i parametri denominati specificati.|Il parametro-password viene usato solo per proteggere le chiavi private e pertanto non è valido quando non si esegue il backup|  
|**Restore-CARoleService C:\ADCSBack15-password (read-host-prompt "password:"-AsSecureString)-DatabaseOnly**|Restore-CARoleService: Impossibile risolvere il set di parametri utilizzando i parametri denominati specificati.|Il parametro-password viene usato solo per proteggere le chiavi private e pertanto non è valido se non viene ripristinato|  
|**Restore-CARoleService C:\ADCSBack14-password (read-host-prompt "password:"-AsSecureString)**|Restore-CARoleService: Impossibile trovare il file specificato. (Eccezione da HRESULT: 0x80070002|Il percorso specificato non contiene un backup del database valido.  Probabilmente il percorso non è valido oppure il backup è stato effettuato con l'opzione-KeysOnly?|  
  
## <a name="additional-resources"></a>Risorse aggiuntive  
[Guida alla migrazione di Servizi certificati Active Directory](https://technet.microsoft.com/library/ee126170(v=ws.10).aspx)  
  
[Backup di un database e di una chiave privata della CA](https://technet.microsoft.com/library/ee126140(v=ws.10).aspx#BKMK_BackUpDB)  
  
[Ripristino del database e della configurazione CA nel server di destinazione](https://technet.microsoft.com/library/ee126140(v=ws.10).aspx#BKMK_RestoreCA)  
  
## <a name="try-this-backup-the-ca-in-your-lab-using-windows-powershell"></a>Provare: Eseguire il backup della CA nel Lab usando Windows PowerShell  
  
1.  Usare i comandi in questa lezione per eseguire il backup del database CA e della chiave privata protetta con una password.  
  
2.  Attendere il ripristino della CA in questo momento.  
  


