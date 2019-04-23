---
title: Risoluzione di problemi generali
description: Questo argomento fa parte della Guida alla distribuzione di più server di accesso remoto in una distribuzione multisito di Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 354ae5e3-bae1-44f9-afd7-7eaba70f2346
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fe1fdc4fb5aff2e34555b08d3b2c4347e643085e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831082"
---
# <a name="troubleshooting-general-issues"></a>Risoluzione di problemi generali

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

In questo argomento contiene informazioni sulla risoluzione dei problemi generali correlati all'accesso remoto.  
  
## <a name="gpo-retrieval-error"></a>Errore di recupero di oggetti Criteri di gruppo  
**Errore ricevuto**. Impostazioni oggetto Criteri di gruppo del server DirectAccess non possono essere recuperate. Assicurarsi di che disporre delle autorizzazioni di modifica per l'oggetto Criteri di gruppo.  
  
La console di gestione accesso remoto non risponde dopo avere ricevuto questo errore.  
  
**Causa**  
  
DirectAccess non può accedere il GPO di uno dei punti di ingresso nella distribuzione e di conseguenza la configurazione non riesce a caricare.  
  
**Soluzione**  
  
Assicurarsi che ogni punto di ingresso nella distribuzione disponga di un oggetto Criteri di gruppo corrispondente per il controller di dominio e verificare che l'utente ha effettuato l'accesso dispone di autorizzazioni lettura e scrittura per tutti i GPO configurati nella distribuzione di accesso remoto.  
  
In alternativa, usare i cmdlet di configurazione anziché tramite la console di gestione accesso remoto; ad esempio, usando `Get-RemoteAccess` e `Get-DAEntryPoint`.  
  
> [!NOTE]  
> Questo scenario non si verifica quando il server oggetto Criteri di gruppo del punto di ingresso corrente non è disponibile.  
  
È possibile usare la `Get-DAEntryPointDC` cmdlet per elencare tutti i controller di dominio che archiviano oggetti Criteri di gruppo di server e `Get-DAMultiSite` unitamente `Get-RemoteAccess` per recuperare un elenco completo degli oggetti Criteri di gruppo server nella distribuzione. Ad esempio:   
  
```  
$ServerGpos = Get-DAEntryPointDC | ForEach-Object {   
   @{   
       GpoName = (Get-RemoteAccess -EntryPoint $_.EntryPointName).ServerGpoName;   
        DC = $_.DomainControllerName }   
}  
$ServerGpos | ForEach-Object { $GpoName = $_['GpoName'] ; $DC = $_['DC'] ; Write-Host "Server GPO '$GpoName' on DC '$DC'" }  
```  
  
## <a name="windows-7-to-windows-8-or-10-client-upgrade"></a>Windows 7 a Windows 8 o 10 l'aggiornamento del client  
**Sintomo**. Dopo che un client Windows 7 viene aggiornato a Windows 10 o Windows 8 in una distribuzione multisito, la connessione DirectAccess non è visibile nell'elenco delle reti.  
  
**Causa**  
  
I GPO di Windows 7 in una distribuzione multisito non contengono la configurazione di Windows 8 Network Connectivity Assistant.  
  
 I client Windows 7 devono utilizzare DirectAccess Connectivity Assistant per monitorarne lo stato della connettività DirectAccess che richiede una configurazione manuale separata in Windows 7 client oggetti Criteri di gruppo. Quando i client Windows 7 vengono aggiornati a Windows 10 o Windows 8, l'Assistente connettività di rete non funzionerà se l'oggetto Criteri di gruppo client Windows 7 viene ancora applicata.  
  
**Soluzione**  
  
Se le impostazioni di DirectAccess Connectivity Assistant sono configurate in oggetti Criteri di gruppo di Windows 7, è possibile risolvere questo problema prima di aggiornare i computer client modificando i GPO di Windows 7 con i cmdlet di PowerShell seguenti:  
  
```  
Set-GPRegistryValue -Name <Windows7GpoName> -Domain <DomainName> -Key "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\NetworkConnectivityAssistant" -ValueName "TemporaryValue" -Type Dword -Value 1  
Remove-GPRegistryValue -Name <Windows7GpoName> -Domain <DomainName> -Key "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\NetworkConnectivityAssistant"  
```  
  
Se un client è già stato aggiornato o DCA non è configurato, spostare il computer client al gruppo di sicurezza di Windows 10 o Windows 8.  
  
## <a name="general-cmdlet-errors"></a>Errori generali cmdlet  
  
-   **Problema 1**  
  
    **Errore ricevuto**. Impossibile raggiungere il controller di dominio < controller_dominio > per < nome_server o nome_punto_di_ingresso >.  
  
    **Causa**  
  
    Per mantenere la coerenza di configurazione in una distribuzione multisito, è importante assicurarsi che ogni oggetto Criteri di gruppo sia gestito da un singolo controller di dominio. Quando il controller di dominio che gestisce server di un punto di ingresso oggetto Criteri di gruppo non è disponibile, non è possibile leggere o modificare le impostazioni di configurazione di accesso remoto.  
  
    **Soluzione**  
  
    Seguire la procedura "per modificare il controller di dominio che gestisce oggetti Criteri di gruppo di server" descritta in [2.4. Configurare oggetti Criteri di gruppo](assetId:///b1960686-a81e-4f48-83f1-cc4ea484df43#ConfigGPOs).  
  
-   **Problema 2**  
  
    **Errore ricevuto**. Impossibile raggiungere il controller di dominio primario nel dominio < nome_dominio >.  
  
    **Causa**  
  
    Per mantenere la coerenza di configurazione in una distribuzione multisito, è importante assicurarsi che ogni oggetto Criteri di gruppo sia gestito da un singolo controller di dominio. Gli oggetti Criteri di gruppo client sono gestiti nel controller di dominio primario. Se il controller di dominio primario non è disponibile, le impostazioni di configurazione di Accesso remoto non possono essere lette o modificate.  
  
    **Soluzione**  
  
    Seguire la procedura "per trasferire il ruolo emulatore PDC" descritta in [2.4. Configurare oggetti Criteri di gruppo](assetId:///b1960686-a81e-4f48-83f1-cc4ea484df43#ConfigGPOs).  
  


