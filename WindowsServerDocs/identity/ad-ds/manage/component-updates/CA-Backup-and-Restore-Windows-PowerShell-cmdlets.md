---
ms.assetid: 7e195f5b-b194-40f3-a26d-5cf4ade5fc4d
title: Cmdlet di Backup della CA e ripristino configurazione di Windows PowerShell
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: a4bbeeedfb40e789a799103f9a29a848a2b32324
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877352"
---
# <a name="ca-backup-and-restore-windows-powershell-cmdlets"></a>Cmdlet di Backup della CA e ripristino configurazione di Windows PowerShell

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
>
**Autore**: Justin. Turner, Senior Support Escalation Engineer presso il gruppo di Windows  
  
> [!NOTE]  
> Questo contenuto è stato redatto da un ingegnere del Supporto tecnico Microsoft ed è destinato ad amministratori esperti e architetti di sistemi che desiderano una spiegazione tecnica delle funzionalità e delle soluzioni relative a Windows Server 2012 R2 più approfondita rispetto agli argomenti solitamente disponibili su TechNet. Non è stato tuttavia sottoposto agli stessi passaggi redazionali e, di conseguenza, per alcune lingue potrebbe essere meno accurato della documentazione che si trova in genere su TechNet.  
  
## <a name="overview"></a>Panoramica  
Il modulo ADCSAdministration Windows PowerShell è stato introdotto in Windows Server 2012.  Sono stati aggiunti due nuovi cmdlet per questo modulo in Window Server 2012 R2 per supportare la copia di Backup e ripristino di un'autorità di certificazione.  
  
-   Backup-CARoleService  
  
-   Restore-CARoleService  
  
## <a name="backup-caroleservice"></a>Backup-CARoleService  
**Tabella SEQ tabella \\ \* 17 ARABO: Backup e ripristino configurazione di Windows PowerShell cmdlet**  
  
**ADCSAdministration Cmdlet: Backup-CARoleService**  
  
|Argomenti - **grassetto** sono necessari argomenti|Descrizione|  
|------------------------------------------------|---------------|  
|**-Path**|-Stringa - percorso in cui salvare il backup<br />-Si tratta dell'unico parametro senza nome<br />-parametro posizionale<br /><br />**Esempio:**<br /><br />Backup-CARoleService.-Path c:\adcsbackup1<br /><br />Backup-CARoleService c:\adcsbackup2|  
|-KeyOnly|-Eseguire il backup del certificato di autorità di certificazione senza database<br /><br />**Esempio:**<br /><br />Backup-CARoleService c:\adcsbackup3 - KeyOnly|  
|-Password|-Specifica la password per proteggere i certificati della CA e le chiavi private<br />-Deve essere una stringa sicura<br />-Non valido con il parametro - DatabaseOnly<br /><br />Esempio:<br /><br />Backup-CARoleService c:\adcsbackup4-Password (Read-Host - prompt "Password:" - AsSecureString)<br /><br />Backup-CARoleService c:\adcsbackup5-Password (ConvertTo-SecureString "Pa55w0rd!" -AsPlainText -Force)|  
|-DatabaseOnly|-Il backup del database senza il certificato della CA<br /><br />Backup-CARoleService c:\adcsbackup6 - DatabaseOnly|  
|-Force|1.  Consente di sovrascrivere il set di backup che preesiste nel percorso specificato nel parametro - Path<br /><br />Backup-CARoleService c:\adcsbackup1-Force|  
|-Incrementale|-È possibile eseguire un backup incrementale<br /><br />Backup-CARoleService c:\adcsbackup7-incrementale|  
|-KeepLog|1.  Indica al comando per mantenere i file di log. Se l'opzione non viene specificato, file di registro troncati per impostazione predefinita, ad eccezione nello scenario incrementale<br /><br />Backup-CARoleService c:\adcsbackup7 -KeepLog|  
  
### <a name="-password-secure-string"></a>-Password <Secure String>  
Se-Password parametro viene utilizzato, la password fornita deve essere una stringa sicura.  Usare la **Read-Host** cmdlet per avviare un prompt interattivo per l'immissione di password di protezione oppure utilizzare il **ConvertTo-SecureString** cmdlet per specificare la password nella riga.  
  
Esaminare gli esempi seguenti  
  
**Specifica una stringa sicura per il parametro Password usando Read-Host**  
  
```powershell  
Backup-CARoleService c:\adcsbackup4 -Password (Read-Host -prompt "Password:" -AsSecureString)  
```  
  
**Specifica una stringa sicura per il parametro Password usando ConvertTo-SecureString**  
  
```powershell  
Backup-CARoleService c:\adcsbackup5 -Password (ConvertTo-SecureString "Pa55w0rd!" -AsPlainText -Force)  
```  
  
## <a name="restore-caroleservice"></a>Restore-CARoleService  
**ADCSAdministration Cmdlet: Restore-CARoleService**  
  
|Argomenti - **grassetto** sono necessari argomenti|Descrizione|  
|------------------------------------------------|---------------|  
|**-Path**|-Stringa - percorso di backup da ripristinare<br />-Si tratta dell'unico parametro senza nome<br />-parametro posizionale<br /><br />**Esempio:**<br /><br />Restore-CARoleService.-Path c:\adcsbackup1 -Force<br /><br />Restore-CARoleService c:\adcsbackup2-Force|  
|-KeyOnly|-Ripristinare il certificato della CA senza database<br />-Specificare se è stato eseguito il backup con l'opzione - KeyOnly<br /><br />**Esempio:**<br /><br />Restore-CARoleService c:\adcsbackup3 - KeyOnly-Force|  
|-Password|-Specifica la password dei certificati della CA e delle chiavi private<br />-Deve essere una stringa sicura<br /><br />**Esempio:**<br /><br />Restore-CARoleService c:\adcsbackup4-Password (read-host - prompt "Password:" - AsSecureString)-Force<br /><br />Restore-CARoleService c:\adcsbackup5-Password (ConvertTo-SecureString "Pa55w0rd!" -AsPlainText -Force) -Force|  
|-DatabaseOnly|-Ripristinare il database senza il certificato della CA<br /><br />Restore-CARoleService c:\adcsbackup6 - DatabaseOnly|  
|-Force|-Consente di sovrascrivere le chiavi già esistente<br />-È un parametro facoltativo, ma durante il ripristino sul posto, è probabilmente necessario<br /><br />Restore-CARoleService c:\adcsbackup1-Force|  
  
### <a name="issues"></a>Problemi  
Un backup protetto diverso dalle password viene eseguito se la funzione di ConvertTo-SecureString non riesce quando si usa il Backup-CARoleService con parametro - Password.  
  
![Backup della CA e ripristino](media/CA-Backup-and-Restore-Windows-PowerShell-cmdlets/GTR_ADDS_BackupCARole.gif)  
  
**Tabella SEQ tabella \\ \* 18 ARABO: Errori comuni**  
  
|Azione|Errore|Commento|  
|----------|---------|-----------|  
|**Restore-CARoleService C:\ADCSBackup**|Restore-CARoleService: Il processo non è possibile accedere al file perché è in uso da un altro processo. (Eccezione da HRESULT:<br /><br />0x80070020)|Arrestare il servizio Servizi certificati Active Directory prima dell'esecuzione del cmdlet Restore-CARoleService|  
|**Restore-CARoleService C:\ADCSBackup**|Restore-CARoleService: La directory non è vuota. (Eccezione da HRESULT: 0x80070091)|Usare il parametro - Force per sovrascrivere le chiavi già esistente|  
|**Backup-CARoleService C:\ADCSBackup-Password (Read-Host - Prompt "Password:" - AsSecureString) - DatabaseOnly**|Backup-CARoleService: Set di parametri non può essere risolto utilizzando l'oggetto specificato parametri denominati.|-Password parametro viene utilizzato solo per password proteggere le chiavi private e pertanto non è valido quando si sta eseguendo non backup|  
|**Restore-CARoleService C:\ADCSBack15 -Password (Read-Host -Prompt "Password:" -AsSecureString) -DatabaseOnly**|Restore-CARoleService: Set di parametri non può essere risolto utilizzando l'oggetto specificato parametri denominati.|-Password parametro viene utilizzato solo per password proteggere le chiavi private e pertanto non è valido quando si sta ripristinando non li|  
|**Restore-CARoleService C:\ADCSBack14 -Password (Read-Host -Prompt "Password:" -AsSecureString)**|Restore-CARoleService: Impossibile trovare il file specificato. (Eccezione da HRESULT: 0x80070002)|Il percorso specificato non contiene una copia di backup di database valido.  Ad esempio il percorso non è valido o è stato eseguito il backup con l'opzione - KeysOnly?|  
  
## <a name="additional-resources"></a>Risorse aggiuntive  
[Guida alla migrazione di servizi certificati Active Directory](https://technet.microsoft.com/library/ee126170(v=ws.10).aspx)  
  
[Backup di un database e di una chiave privata della CA](https://technet.microsoft.com/library/ee126140(v=ws.10).aspx#BKMK_BackUpDB)  
  
[Ripristino del database e della configurazione CA nel server di destinazione](https://technet.microsoft.com/library/ee126140(v=ws.10).aspx#BKMK_RestoreCA)  
  
## <a name="try-this-backup-the-ca-in-your-lab-using-windows-powershell"></a>Provare quanto segue: L'autorità di certificazione nell'ambiente lab tramite Windows PowerShell di backup  
  
1.  Usare i comandi in questa lezione per eseguire il backup il database CA e la chiave privata protetta con una password.  
  
2.  Rimandare il ripristino dell'autorità di certificazione in questo momento.  
  


