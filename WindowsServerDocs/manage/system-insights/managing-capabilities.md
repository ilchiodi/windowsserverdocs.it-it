---
title: Gestione delle funzionalità
description: System Insights espone una serie di impostazioni che possono essere configurate per ogni funzionalità e queste impostazioni possono essere ottimizzate per soddisfare le esigenze specifiche della distribuzione. Questo argomento descrive come gestire le varie impostazioni per ogni funzionalità tramite l'interfaccia di amministrazione di Windows o PowerShell, fornendo esempi di PowerShell di base e screenshot dell'interfaccia di amministrazione di Windows per dimostrare come modificare queste impostazioni.
ms.prod: windows-server
ms.technology: system-insights
ms.topic: article
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 6/05/2018
ms.openlocfilehash: b93365474e591ce6fde59867c42b851ec45de50c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80819734"
---
# <a name="managing-capabilities"></a>Gestione delle funzionalità

>Si applica a: Windows Server 2019

In Windows Server 2019, System Insights espone una serie di impostazioni che possono essere configurate per ogni funzionalità e queste impostazioni possono essere ottimizzate per soddisfare le esigenze specifiche della distribuzione. Questo argomento descrive come gestire le varie impostazioni per ogni funzionalità tramite l'interfaccia di amministrazione di Windows o PowerShell, fornendo esempi di PowerShell di base e screenshot dell'interfaccia di amministrazione di Windows per dimostrare come modificare queste impostazioni. 

>[!TIP]
>È anche possibile usare questi brevi video per iniziare e gestire in modo sicuro System Insights: [Introduzione a System Insights in 10 minuti](https://blogs.technet.microsoft.com/filecab/2018/07/24/getting-started-with-system-insights-in-10-minutes/)

Sebbene questa sezione fornisca esempi di PowerShell, è possibile usare la [documentazione di PowerShell per System Insights](https://aka.ms/systeminsightspowershell) per visualizzare tutti i cmdlet, i parametri e i set di parametri all'interno di System Insights. 

## <a name="viewing-capabilities"></a>Visualizzazione delle funzionalità

Per iniziare, è possibile elencare tutte le funzionalità disponibili usando il cmdlet **Get-InsightsCapability** : 

```PowerShell
Get-InsightsCapability
``` 
Queste funzionalità sono visibili anche nell'estensione System Insights:

![Pagina Panoramica di System Insights che elenca le funzionalità disponibili](media/overview-page-contoso.png)

## <a name="enabling-and-disabling-a-capability"></a>Abilitazione e disabilitazione di una funzionalità
Ogni funzionalità può essere abilitata o disabilitata. La disabilitazione di una funzionalità impedisce che venga richiamata la funzionalità e, per le funzionalità non predefinite, la disabilitazione di una funzionalità interrompe la raccolta di tutti i dati per tale funzionalità. Per impostazione predefinita, tutte le funzionalità sono abilitate ed è possibile controllare lo stato di una funzionalità usando il cmdlet **Get-InsightsCapability** . 

Per abilitare o disabilitare una funzionalità, usare i cmdlet **Enable-InsightsCapability** e **Disable-InsightsCapability** :

```PowerShell
Enable-InsightsCapability -Name "CPU capacity forecasting"
Disable-InsightsCapability -Name "Networking capacity forecasting"
``` 
È possibile attivare o disattivare queste impostazioni anche selezionando una funzionalità nell'interfaccia di amministrazione di Windows facendo clic sui pulsanti **Abilita** o **Disabilita** .

### <a name="invoking-a-capability"></a>Richiamo di una funzionalità
Richiamando una funzionalità viene immediatamente eseguita la funzionalità per recuperare una stima e gli amministratori possono richiamare una funzionalità in qualsiasi momento facendo clic sul pulsante **richiama** nell'interfaccia di amministrazione di Windows o usando il cmdlet **Invoke-InsightsCapability** :

```PowerShell
Invoke-InsightsCapability -Name "CPU capacity forecasting"
```

>[!TIP]
>Per assicurarsi che il richiamo di una funzionalità non sia in conflitto con le operazioni critiche nel computer, è consigliabile pianificare le stime durante l'orario di ufficio.

## <a name="retrieving-capability-results"></a>Recupero dei risultati della funzionalità
Dopo che una funzionalità è stata richiamata, i risultati più recenti sono visibili usando **Get-InsightsCapability** o **Get-InsightsCapabilityResult**. Questi cmdlet restituiscono lo **stato** più recente e la **Descrizione** dello stato di ogni funzionalità, che descrivono il risultato di ogni stima. I campi **Descrizione** **stato** e stato sono descritti ulteriormente nel [documento informazioni sulle funzionalità](understanding-capabilities.md). 

Inoltre, è possibile utilizzare il cmdlet **Get-InsightsCapabilityResult** per visualizzare gli ultimi 30 risultati stima e recuperare i dati associati alla stima: 

```PowerShell
# Specify the History parameter to see the last 30 prediction results.
Get-InsightsCapabilityResult -Name "CPU capacity forecasting" -History

# Use the Output field to locate and then show the results of "CPU capacity forecasting."
# Specify the encoding as UTF8, so that Get-Content correctly parses non-English characters.
$Output = Get-Content (Get-InsightsCapabilityResult -Name "CPU capacity forecasting").Output -Encoding UTF8 | ConvertFrom-Json
$Output.ForecastingResults
```
L'estensione System Insights Mostra automaticamente la cronologia delle stime e analizza i risultati del risultato JSON, offrendo un grafo intuitivo e ad alta fedeltà di ogni previsione:

![Pagina di funzionalità singola che mostra un grafico di previsione e la cronologia di stima](media/cpu-forecast-2.png)

### <a name="using-the-event-log-to-retrieve-capability-results"></a>Utilizzo del registro eventi per recuperare i risultati della funzionalità
System Insights registra un evento ogni volta che una funzionalità termina una stima. Questi eventi sono visibili nel canale **Microsoft-Windows-System-Insights/admin** e System Insights pubblica un ID evento diverso per ogni stato:   

| Stato stima | ID evento |
| --------------- | --------------- |
| OK | 151 |
| Avviso | 148 |
| Critico | 150 |
| Errore | 149 |
| None | 132 |

>[!TIP]
>Usare [monitoraggio di Azure](https://azure.microsoft.com/services/monitor/) o [System Center Operations Manager](https://docs.microsoft.com/system-center/scom/welcome?view=sc-om-1807) per aggregare questi eventi e visualizzare i risultati della stima in un gruppo di computer.


## <a name="setting-a-capability-schedule"></a>Impostazione di una pianificazione delle funzionalità
Oltre alle stime su richiesta, è possibile configurare stime periodiche per ogni funzionalità in modo che la funzionalità specificata venga richiamata automaticamente in base a una pianificazione predefinita. Usare il cmdlet **Get-InsightsCapabilitySchedule** per visualizzare le pianificazioni delle funzionalità: 

>[!TIP]
>Usare l'operatore pipeline in PowerShell per visualizzare le informazioni relative a tutte le funzionalità restituite dal cmdlet **Get-InsightsCapability** .

```PowerShell
Get-InsightsCapability | Get-InsightsCapabilitySchedule
```

Le stime periodiche sono abilitate per impostazione predefinita, anche se possono essere disabilitate in qualsiasi momento usando i cmdlet **Enable-InsightsCapabilitySchedule** e **Disable-InsightsCapabilitySchedule** :

```PowerShell
Enable-InsightsCapabilitySchedule -Name "Total storage consumption forecasting"
Disable-InsightsCapabilitySchedule -Name "Volume consumption forecasting"
```

Ogni funzionalità predefinita è pianificata per essere eseguita ogni giorno alle 3. È tuttavia possibile creare pianificazioni personalizzate per ogni funzionalità e System Insights supporta un'ampia gamma di tipi di pianificazione, che possono essere configurati con il cmdlet **set-InsightsCapabilitySchedule** : 

```PowerShell
Set-InsightsCapabilitySchedule -Name "CPU capacity forecasting" -Daily -DaysInterval 2 -At 4:00PM
Set-InsightsCapabilitySchedule -Name "Networking capacity forecasting" -Daily -DaysOfWeek Saturday, Sunday -At 2:30AM
Set-InsightsCapabilitySchedule -Name "Total storage consumption forecasting" -Hourly -HoursInterval 2 -DaysOfWeek Monday, Wednesday, Friday
Set-InsightsCapabilitySchedule -Name "Volume consumption forecasting" -Minute -MinutesInterval 30 
```
>[!NOTE]
>Poiché le funzionalità predefinite analizzano i dati giornalieri, è consigliabile usare pianificazioni giornaliere per queste funzionalità. Altre informazioni sulle funzionalità predefinite sono disponibili [qui](understanding-capabilities.md).

È anche possibile usare l'interfaccia di amministrazione di Windows per visualizzare e impostare le pianificazioni per ogni funzionalità facendo clic su **Impostazioni**. La pianificazione corrente viene visualizzata nella scheda **pianificazione** ed è possibile usare gli strumenti dell'interfaccia utente grafica per creare una nuova pianificazione:

![Pagina impostazioni che mostra la pianificazione corrente](media/schedule-page-contoso.png)

## <a name="creating-remediation-actions"></a>Creazione di azioni correttive
System Insights consente di avviare gli script di monitoraggio e aggiornamento personalizzati in base al risultato di una funzionalità. Per ogni funzionalità, è possibile configurare uno script di PowerShell personalizzato per ogni stato di stima, consentendo agli amministratori di adottare automaticamente un'azione correttiva, anziché richiedere l'intervento manuale. 

Le azioni correttive di esempio includono l'esecuzione della pulizia del disco, l'estensione di un volume, l'esecuzione della deduplicazione, la migrazione in tempo reale delle VM e la configurazione di Sincronizzazione file di Azure.

È possibile visualizzare le azioni per ogni funzionalità usando il cmdlet **Get-InsightsCapabilityAction** :

```PowerShell
Get-InsightsCapability | Get-InsightsCapabilityAction
```

È possibile creare nuove azioni o eliminare le azioni esistenti usando i cmdlet **set-InsightsCapabilityAction** e **Remove-InsightsCapabilityAction** . Ogni azione viene eseguita utilizzando le credenziali specificate nel parametro **ActionCredential** .

>[!NOTE]
>Nella versione iniziale di System Insights è necessario specificare gli script di monitoraggio e aggiornamento all'esterno delle directory utente. Questo problema verrà risolto in una versione futura.

```PowerShell
$Cred = Get-Credential
Set-InsightsCapabilityAction -Name "CPU capacity forecasting" -Type Warning -Action "C:\Users\Public\WarningScript.ps1" -ActionCredential $Cred
Set-InsightsCapabilityAction -Name "CPU capacity forecasting" -Type Critical -Action "C:\Users\Public\CriticalScript.ps1" -ActionCredential $Cred

Remove-InsightsCapabilityAction -Name "CPU capacity forecasting" -Type Warning
```

È anche possibile usare l'interfaccia di amministrazione di Windows per impostare azioni correttive usando la scheda **azioni** nella pagina **Impostazioni** :

![Pagina impostazioni in cui l'utente può specificare azioni correttive](media/actions-page-contoso.png)


## <a name="see-also"></a>Vedere anche
Per altre informazioni su System Insights, usare le risorse seguenti:

- [Panoramica di System Insights](overview.md)
- [Descrizione delle funzionalità](understanding-capabilities.md)
- [Aggiunta e sviluppo di funzionalità](adding-and-developing-capabilities.md)
- [Domande frequenti su System Insights](faq.md)