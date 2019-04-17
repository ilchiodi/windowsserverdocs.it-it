---
ms.assetid: 7e195f5b-b194-40f3-a26d-5cf4ade5fc4d
title: Cmdlet di Backup della CA e ripristino configurazione di Windows PowerShell
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: aba86ed080cc0b4043805531f0a2138b1b1d3cf8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="ca-backup-and-restore-windows-powershell-cmdlets"></a>Cmdlet di Backup della CA e ripristino configurazione di Windows PowerShell

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
>
**Autore**: Justin Turner, Senior Support Escalation Engineer con il gruppo di Windows  
  
> [!NOTE]  
> Questo contenuto è stato redatto da un ingegnere del supporto tecnico Microsoft ed è destinato ad amministratori esperti e architetti di sistemi che desiderano per una spiegazione tecnica più approfondita delle funzionalità e le soluzioni in Windows Server 2012 R2 che solitamente disponibili su TechNet. Tuttavia, non è stata sottoposta stessi passaggi redazionali, in modo che il linguaggio potrebbe sembrare che meno accurato della documentazione che cosa si trova in genere su TechNet.  
  
## <a name="overview"></a>Panoramica  
Nella finestra Server 2012 è stato introdotto il modulo ADCSAdministration Windows PowerShell.  Sono stati aggiunti due nuovi cmdlet per questo modulo nella finestra Server 2012 R2 per supportare i Backup e ripristino di un'autorità di certificazione.  
  
-   Backup-CARoleService  
  
-   Restore-CARoleService  
  
## <a name="backup-caroleservice"></a>Backup-CARoleService  
**Tabella SEQ tabella \ \ \ * ARABIC 17: Backup e ripristino di cmdlet di Windows PowerShell**  
  
**ADCSAdministration Cmdlet: Backup-CARoleService**  
  
|Gli argomenti - **grassetto** sono necessari argomenti|Descrizione|  
|------------------------------------------------|---------------|  
|**-Path**|-Stringa - percorso in cui salvare il backup<br />-Questo è l'unico parametro senza nome<br />-parametro posizionale<br /><br />**Esempio:**<br /><br />Backup-CARoleService.-c:\adcsbackup1 percorso<br /><br />Backup-CARoleService c:\adcsbackup2|  
|-KeyOnly|-Il certificato della CA senza il database di backup<br /><br />**Esempio:**<br /><br />Backup-CARoleService c:\adcsbackup3 - KeyOnly|  
|-Password|-Specifica la password per proteggere i certificati della CA e le chiavi private<br />-Deve essere una stringa sicura<br />-Non valida con il parametro - DatabaseOnly<br /><br />Esempio:<br /><br />Backup-CARoleService c:\adcsbackup4-Password (Read-Host - prompt "Password:" - AsSecureString)<br /><br />Backup-CARoleService c:\adcsbackup5-Password (ConvertTo-SecureString "Pa55w0rd!" -AsPlainText - Force)|  
|-DatabaseOnly|-Il backup del database senza il certificato della CA<br /><br />Backup-CARoleService c:\adcsbackup6 - DatabaseOnly|  
|-Force|1. consente di sovrascrivere il backup preesiste nel percorso specificato nel parametro - Path<br /><br />Backup-CARoleService c:\adcsbackup1-Force|  
|-Incrementale|-È possibile eseguire un backup incrementale<br /><br />Backup-CARoleService c:\adcsbackup7-incrementale|  
|-KeepLog|1. indica il comando per mantenere i file di registro. Se l'opzione non è specificata, file di registro troncati per impostazione predefinita, ad eccezione nello scenario incrementale<br /><br />Backup-CARoleService c:\adcsbackup7 - KeepLog|  
  
### <a name="-password-secure-string"></a>-Password<Secure String>  
Se la - Password parametro viene utilizzato, la password deve essere una stringa sicura.  Utilizzare il **Read-Host** cmdlet per avviare un prompt interattivo per l'immissione di password di protezione oppure utilizzare il **ConvertTo-SecureString** cmdlet per specificare la password in linea.  
  
Esaminare gli esempi seguenti  
  
**Se si specifica una stringa sicura per il parametro Password utilizzando Read-Host**  
  
```powershell  
Backup-CARoleService c:\adcsbackup4 -Password (Read-Host -prompt "Password:" -AsSecureString)  
```  
  
**Se si specifica una stringa sicura per il parametro Password utilizzando ConvertTo-SecureString.**  
  
```powershell  
Backup-CARoleService c:\adcsbackup5 -Password (ConvertTo-SecureString "Pa55w0rd!" -AsPlainText -Force)  
```  
  
## <a name="restore-caroleservice"></a>Restore-CARoleService  
**ADCSAdministration Cmdlet: Restore-CARoleService**  
  
|Gli argomenti - **grassetto** sono necessari argomenti|Descrizione|  
|------------------------------------------------|---------------|  
|**-Path**|-Stringa - percorso di backup da ripristinare<br />-Questo è l'unico parametro senza nome<br />-parametro posizionale<br /><br />**Esempio:**<br /><br />Ripristino-CARoleService.-percorso c:\adcsbackup1-Force<br /><br />Restore-CARoleService c:\adcsbackup2-Force|  
|-KeyOnly|-Il certificato della CA senza il database di ripristino<br />-Deve essere specificato se è stato eseguito il backup con l'opzione - KeyOnly<br /><br />**Esempio:**<br /><br />Restore-CARoleService c:\adcsbackup3 - KeyOnly-Force|  
|-Password|-Specifica la password dei certificati di autorità di certificazione e chiavi private<br />-Deve essere una stringa sicura<br /><br />**Esempio:**<br /><br />Restore-CARoleService c:\adcsbackup4-Password (read-host - prompt "Password:" - AsSecureString)-Force<br /><br />Restore-CARoleService c:\adcsbackup5-Password (ConvertTo-SecureString "Pa55w0rd!" -AsPlainText - Force)-Force|  
|-DatabaseOnly|-Ripristinare il database senza il certificato della CA<br /><br />Restore-CARoleService c:\adcsbackup6 - DatabaseOnly|  
|-Force|-Consente di sovrascrivere le chiavi preesistenti<br />-È un parametro facoltativo, ma durante il ripristino sul posto, è probabile che richiesto<br /><br />Restore-CARoleService c:\adcsbackup1-Force|  
  
### <a name="issues"></a>Problemi di  
Viene eseguito un backup protetto non password se la funzione di ConvertTo-SecureString non riesce durante l'uso di Backup-CARoleService con il parametro-Password.  
  
![Autorità di certificazione backup e ripristino](media/CA-Backup-and-Restore-Windows-PowerShell-cmdlets/GTR_ADDS_BackupCARole.gif)  
  
**Tabella SEQ tabella \ \ \ * ARABIC 18: gli errori comuni**  
  
|Azione|Errore|Commento|  
|----------|---------|-----------|  
|**Restore-CARoleService C:\ADCSBackup**|Restore-CARoleService: Il processo non può accedere al file perché è in uso da un altro processo. (Eccezione da HRESULT:<br /><br />0x80070020)|Arrestare il servizio Servizi certificati Active Directory prima di eseguire il cmdlet Restore-CARoleService|  
|**Restore-CARoleService C:\ADCSBackup**|Restore-CARoleService: La directory non è vuota. (Eccezione da HRESULT: 0x80070091)|Usa il parametro - Force per sovrascrivere le chiavi preesistenti|  
|**Backup-CARoleService C:\ADCSBackup-Password (Read-Host - Prompt "Password:" - AsSecureString) - DatabaseOnly**|Backup-CARoleService: Set di parametri non possono essere risolti utilizzando l'oggetto specificato denominato parametri.|-Password parametro è solo per password proteggere le chiavi private, pertanto non valido quando si sono non backup|  
|**Restore-CARoleService C:\ADCSBack15-Password (Read-Host - Prompt "Password:" - AsSecureString) - DatabaseOnly**|Restore-CARoleService: Set di parametri non possono essere risolti utilizzando l'oggetto specificato denominato parametri.|-Password parametro è solo per password proteggere le chiavi private, pertanto non valido quando si sta ripristinando non li|  
|**Restore-CARoleService C:\ADCSBack14-Password (Read-Host - Prompt "Password:" - AsSecureString)**|Restore-CARoleService: Impossibile trovare il file specificato. (Eccezione da HRESULT: 0x80070002)|Il percorso specificato non contiene una copia di backup di database valido.  Ad esempio il percorso è valido o è stato eseguito il backup con l'opzione - KeysOnly?|  
  
## <a name="additional-resources"></a>Risorse aggiuntive  
[Guida alla migrazione di servizi certificati Active Directory](https://technet.microsoft.com/library/ee126170(v=ws.10).aspx)  
  
[Backup di un database CA e la chiave privata](https://technet.microsoft.com/library/ee126140(v=ws.10).aspx#BKMK_BackUpDB)  
  
[Ripristino del database CA e la configurazione nel server di destinazione](https://technet.microsoft.com/library/ee126140(v=ws.10).aspx#BKMK_RestoreCA)  
  
## <a name="try-this-backup-the-ca-in-your-lab-using-windows-powershell"></a>Prova così: Backup CA nell'ambiente di utilizzo di Windows PowerShell  
  
1.  Utilizzare i comandi in questo lezione per eseguire il backup il database CA e la chiave privata protetta con una password.  
  
2.  Aspetta il ripristino dell'autorità di certificazione in questo momento.  
  


