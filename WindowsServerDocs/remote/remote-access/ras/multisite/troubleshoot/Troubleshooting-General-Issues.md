---
title: Risoluzione di problemi generali
description: Questo argomento fa parte della Guida distribuire più server di accesso remoto in una distribuzione multisito di Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 354ae5e3-bae1-44f9-afd7-7eaba70f2346
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a2b8d7decad482ca8756aa4d82baa35abf16f5fe
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404442"
---
# <a name="troubleshooting-general-issues"></a>Risoluzione di problemi generali

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Questo argomento contiene informazioni sulla risoluzione dei problemi generali relativi all'accesso remoto.  
  
## <a name="gpo-retrieval-error"></a>Errore di recupero degli oggetti Criteri di gruppo  
**Errore ricevuto**. Impossibile recuperare le impostazioni dell'oggetto Criteri di gruppo del server DirectAccess. Assicurarsi di disporre delle autorizzazioni di modifica per l'oggetto Criteri di gruppo.  
  
La console di gestione accesso remoto non risponde dopo la ricezione di questo errore.  
  
**Causa**  
  
DirectAccess non è in grado di accedere all'oggetto Criteri di gruppo di uno dei punti di ingresso nella distribuzione e, di conseguenza, non è possibile caricare la configurazione.  
  
**Soluzione**  
  
Verificare che ogni punto di ingresso nella distribuzione disponga di un oggetto Criteri di gruppo corrispondente sul controller di dominio e verificare che l'utente connesso disponga delle autorizzazioni di lettura e scrittura per tutti gli oggetti Criteri di gruppo configurati nella distribuzione di accesso remoto.  
  
Come soluzione alternativa, usare i cmdlet di configurazione invece di usare la console di gestione accesso remoto; ad esempio, usando `Get-RemoteAccess` e `Get-DAEntryPoint`.  
  
> [!NOTE]  
> Questo scenario non si verifica quando l'oggetto Criteri di gruppo del server del punto di ingresso corrente non è disponibile.  
  
È possibile utilizzare il cmdlet `Get-DAEntryPointDC` per elencare tutti i controller di dominio in cui sono archiviati gli oggetti Criteri di gruppo del server e `Get-DAMultiSite` insieme a `Get-RemoteAccess` per recuperare un elenco completo degli oggetti Criteri di gruppo del server nella distribuzione. Esempio:  
  
```  
$ServerGpos = Get-DAEntryPointDC | ForEach-Object {   
   @{   
       GpoName = (Get-RemoteAccess -EntryPoint $_.EntryPointName).ServerGpoName;   
        DC = $_.DomainControllerName }   
}  
$ServerGpos | ForEach-Object { $GpoName = $_['GpoName'] ; $DC = $_['DC'] ; Write-Host "Server GPO '$GpoName' on DC '$DC'" }  
```  
  
## <a name="windows-7-to-windows-8-or-10-client-upgrade"></a>Aggiornamento da Windows 7 a Windows 8 o 10 client  
**Sintomo**. Dopo l'aggiornamento di un client Windows 7 a Windows 10 o Windows 8 in una distribuzione multisito, la connessione DirectAccess non è visibile nell'elenco reti.  
  
**Causa**  
  
Gli oggetti Criteri di gruppo di Windows 7 in una distribuzione multisito non contengono la configurazione di assistente connettività di rete di Windows 8.  
  
 I client Windows 7 devono utilizzare DirectAccess Connectivity Assistant per monitorare lo stato della connettività DirectAccess che richiede una configurazione manuale separata negli oggetti Criteri di gruppo del client di Windows 7. Quando i client Windows 7 vengono aggiornati a Windows 10 o Windows 8, l'assistente connettività di rete non funzionerà se l'oggetto Criteri di gruppo del client Windows 7 è ancora applicato.  
  
**Soluzione**  
  
Se le impostazioni dell'assistente connettività DirectAccess sono configurate negli oggetti Criteri di gruppo di Windows 7, è possibile risolvere il problema prima di aggiornare i computer client modificando gli oggetti Criteri di gruppo di Windows 7 con i cmdlet di PowerShell seguenti:  
  
```  
Set-GPRegistryValue -Name <Windows7GpoName> -Domain <DomainName> -Key "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\NetworkConnectivityAssistant" -ValueName "TemporaryValue" -Type Dword -Value 1  
Remove-GPRegistryValue -Name <Windows7GpoName> -Domain <DomainName> -Key "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\NetworkConnectivityAssistant"  
```  
  
Se un client è già stato aggiornato o il DCA non è configurato, spostare il computer client nel gruppo di sicurezza Windows 10 o Windows 8.  
  
## <a name="general-cmdlet-errors"></a>Errori generali del cmdlet  
  
-   **Problema 1**  
  
    **Errore ricevuto**. Impossibile raggiungere il controller di dominio < > Domain_Controller per < nome_server o entry_point_name >.  
  
    **Causa**  
  
    Per mantenere la coerenza di configurazione in una distribuzione multisito, è importante assicurarsi che ogni oggetto Criteri di gruppo sia gestito da un singolo controller di dominio. Quando il controller di dominio che gestisce un oggetto Criteri di gruppo del server di un punto di ingresso non è disponibile, non è possibile leggere o modificare le impostazioni di configurazione di accesso remoto.  
  
    **Soluzione**  
  
    Seguire la procedura "per modificare il controller di dominio che gestisce gli oggetti Criteri di gruppo del server" descritto in [2,4. Configurare gli oggetti Criteri di gruppo @ no__t-0.  
  
-   **Problema 2**  
  
    **Errore ricevuto**. Impossibile raggiungere il controller di dominio primario nel dominio < domain_name >.  
  
    **Causa**  
  
    Per mantenere la coerenza di configurazione in una distribuzione multisito, è importante assicurarsi che ogni oggetto Criteri di gruppo sia gestito da un singolo controller di dominio. Gli oggetti Criteri di gruppo client sono gestiti nel controller di dominio primario. Se il controller di dominio primario non è disponibile, le impostazioni di configurazione di Accesso remoto non possono essere lette o modificate.  
  
    **Soluzione**  
  
    Attenersi alla procedura "per trasferire il ruolo emulatore PDC" descritta in [2,4. Configurare gli oggetti Criteri di gruppo @ no__t-0.  
  


