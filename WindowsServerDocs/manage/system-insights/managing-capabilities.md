---
title: Gestione delle funzionalità
description: Sistema Insights espone una serie di impostazioni che possono essere configurati per ogni funzionalità e queste impostazioni possono essere ottimizzate per ogni esigenza specifica della distribuzione. Questo argomento descrive come gestire le varie impostazioni per ogni funzionalità attraverso Windows Admin Center o PowerShell, offrendo esempi di base in PowerShell e Windows Admin Center schermate per illustrare come modificare queste impostazioni.
ms.custom: na
ms.prod: windows-server
ms.technology: system-insights
ms.topic: article
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 6/05/2018
ms.openlocfilehash: 9081a0b576ab9871b47df38255047b6cbe889419
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868022"
---
# <a name="managing-capabilities"></a>Gestione delle funzionalità

>Si applica a: Windows Server 2019

In Windows Server 2019, sistema Insights espone una serie di impostazioni che possono essere configurati per ogni funzionalità e queste impostazioni possono essere ottimizzate per ogni esigenza specifica della distribuzione. Questo argomento descrive come gestire le varie impostazioni per ogni funzionalità attraverso Windows Admin Center o PowerShell, offrendo esempi di base in PowerShell e Windows Admin Center schermate per illustrare come modificare queste impostazioni. 

>[!TIP]
>È anche possibile usare questi brevi video per iniziare a usare e gestire in tutta sicurezza sistema Insights: [Introduzione a System Insights in 10 minuti](https://blogs.technet.microsoft.com/filecab/2018/07/24/getting-started-with-system-insights-in-10-minutes/)

Anche se in questa sezione vengono forniti esempi di PowerShell, è possibile usare la [documentazione di PowerShell di Insights sistema](https://aka.ms/systeminsightspowershell) per visualizzare tutti i set di cmdlet, parametri e parametri all'interno di Insights di sistema. 

## <a name="viewing-capabilities"></a>Funzioni di visualizzazione

Per iniziare, è possibile elencare tutte le funzionalità disponibili tramite il **Get-InsightsCapability** cmdlet: 

```PowerShell
Get-InsightsCapability
``` 
Queste funzionalità sono inoltre visibili nell'estensione Application Insights di sistema:

![Pagina di panoramica di informazioni di sistema elenca le funzionalità disponibili](media/overview-page-contoso.png)

## <a name="enabling-and-disabling-a-capability"></a>Abilitazione e disabilitazione di una funzionalità
Ogni funzionalità può essere attivata o disattivata. La disabilitazione di una funzionalità impedisce tale funzionalità da cui viene richiamato e per le funzionalità non predefinito, la disabilitazione di una funzionalità arresta raccolta di tutti i dati per tale funzionalità. Per impostazione predefinita, tutte le funzionalità sono abilitate, ed è possibile verificare lo stato di una funzionalità usando il **Get-InsightsCapability** cmdlet. 

Per abilitare o disabilitare una funzionalità, usare il **Enable-InsightsCapability** e il **Disable-InsightsCapability** cmdlet:

```PowerShell
Enable-InsightsCapability -Name "CPU capacity forecasting"
Disable-InsightsCapability -Name "Networking capacity forecasting"
``` 
Queste impostazioni possono inoltre essere attivata/disattivate. per selezionata una funzionalità in Windows Admin Center facendo clic sui **abilitare** oppure **disabilitare** pulsanti.

### <a name="invoking-a-capability"></a>Richiamo di una funzionalità
Richiama immediatamente una funzionalità viene eseguita la possibilità di recuperare una stima e gli amministratori possono richiamare una funzionalità di qualsiasi momento facendo il **Invoke** button in Windows Admin Center o utilizzando il  **Invoke-InsightsCapability** cmdlet:

```PowerShell
Invoke-InsightsCapability -Name "CPU capacity forecasting"
```

>[!TIP]
>Per assicurarsi che la chiamata di una funzionalità non entrino in conflitto con le operazioni critiche nel computer, è consigliabile pianificare le stime durante l'orario off.

## <a name="retrieving-capability-results"></a>Recupero dei risultati di funzionalità
Dopo aver chiamata una funzionalità, i risultati più recenti sono visibili tramite **Get-InsightsCapability** oppure **Get-InsightsCapabilityResult**. Questi cmdlet di output più recente **lo stato** e **descrizione dello stato** di ogni funzionalità, che descrivono il risultato di ogni stima. Il **lo stato** e **descrizione dello stato** campi sono descritte dettagliatamente nel [documento funzionalità comprensione](understanding-capabilities.md). 

Inoltre, è possibile usare la **Get-InsightsCapabilityResult** cmdlet per visualizzare i risultati di 30 stima ultimo e recuperare i dati associati alla stima: 

```PowerShell
# Specify the History parameter to see the last 30 prediction results.
Get-InsightsCapabilityResult -Name "CPU capacity forecasting" -History

# Use the Output field to locate and then show the results of "CPU capacity forecasting."
# Specify the encoding as UTF8, so that Get-Content correctly parses non-English characters.
$Output = Get-Content (Get-InsightsCapabilityResult -Name "CPU capacity forecasting").Output -Encoding UTF8 | ConvertFrom-Json
$Output.ForecastingResults
```
L'estensione Insights sistema automaticamente consente di visualizzare la cronologia di stima e analizza i risultati del risultato JSON, consentendo un grafico intuitivo e ad alta fedeltà di ogni previsione:

![Pagina di funzionalità Single che mostra un grafico di previsione e la cronologia stima](media/cpu-forecast-2.png)

### <a name="using-the-event-log-to-retrieve-capability-results"></a>Usando il registro eventi per recuperare i risultati di funzionalità
Insights sistema registra un evento ogni volta che una funzionalità viene completata una stima. Questi eventi sono visibili nel **Microsoft-Windows-System-Insights/Admin** channel e sistema Insights pubblica un ID evento diverso per ogni stato:   

| Stato di stima | ID evento |
| --------------- | --------------- |
| OK | 151 |
| Avviso | 148 |
| Critico | 150 |
| Errore | 149 |
| Nessuno | 132 |

>[!TIP]
>Uso [monitoraggio di Azure](https://azure.microsoft.com/services/monitor/) oppure [System Center Operations Manager](https://docs.microsoft.com/system-center/scom/welcome?view=sc-om-1807) per aggregare questi eventi e visualizzare i risultati della stima tra un gruppo di computer.


## <a name="setting-a-capability-schedule"></a>Impostazione di una pianificazione di capacità
Oltre alle stime on demand, è possibile configurare periodiche stime per ogni funzionalità in modo che la funzionalità specificata viene richiamata automaticamente secondo una pianificazione predefinita. Usare la **Get-InsightsCapabilitySchedule** cmdlet per visualizzare le pianificazioni di funzionalità: 

>[!TIP]
>Usare l'operatore pipeline di PowerShell per visualizzare informazioni per tutte le funzionalità restituito dal **Get-InsightsCapability** cmdlet.

```PowerShell
Get-InsightsCapability | Get-InsightsCapabilitySchedule
```

Previsioni periodiche sono abilitate per impostazione predefinita anche se può essere disabilitate in qualsiasi momento tramite il **Enable-InsightsCapabilitySchedule** e **Disable-InsightsCapabilitySchedule** cmdlet:

```PowerShell
Enable-InsightsCapabilitySchedule -Name "Total storage consumption forecasting"
Disable-InsightsCapabilitySchedule -Name "Volume consumption forecasting"
```

Ogni funzionalità predefinita è pianificata per eseguire ogni giorno alle 3:00. Tuttavia, è possibile creare pianificazioni personalizzate per ogni funzionalità, e sistema Insights supporta un'ampia gamma di tipi di pianificazione, che possono essere configurati usando le **Set-InsightsCapabilitySchedule** cmdlet: 

```PowerShell
Set-InsightsCapabilitySchedule -Name "CPU capacity forecasting" -Daily -DaysInterval 2 -At 4:00PM
Set-InsightsCapabilitySchedule -Name "Networking capacity forecasting" -Daily -DaysOfWeek Saturday, Sunday -At 2:30AM
Set-InsightsCapabilitySchedule -Name "Total storage consumption forecasting" -Hourly -HoursInterval 2 -DaysOfWeek Monday, Wednesday, Friday
Set-InsightsCapabilitySchedule -Name "Volume consumption forecasting" -Minute -MinutesInterval 30 
```
>[!NOTE]
>Poiché le funzionalità predefinite analizzano i dati giornalieri, è consigliabile usare pianificazioni giornaliere di queste funzionalità. Altre informazioni sulle funzionalità predefinite [qui](understanding-capabilities.md).

È anche possibile usare Windows Admin Center per visualizzare e impostare le pianificazioni per ogni funzionalità facendo **impostazioni**. La pianificazione corrente viene visualizzata sulla **pianificazione** tab ed è possibile usare gli strumenti di interfaccia utente grafica per creare una nuova pianificazione:

![Le impostazioni di pagina che Visualizza pianificazione corrente](media/schedule-page-contoso.png)

## <a name="creating-remediation-actions"></a>Creazione di azioni di correzione
Sistema Insights consente di avviare script di correzione personalizzati in base al risultato di una funzionalità. Per ogni funzionalità, è possibile configurare uno script di PowerShell personalizzato per ogni stato di stima, consentendo agli amministratori di adottare misure correttive automaticamente, invece di richiedere l'intervento manuale. 

Le azioni di correzione di esempio includono Pulitura disco in esecuzione, estendere un volume, che esegue la deduplicazione, in tempo reale la migrazione di macchine virtuali e la configurazione di sincronizzazione File di Azure.

È possibile visualizzare le azioni per ogni funzionalità usando il **Get-InsightsCapabilityAction** cmdlet:

```PowerShell
Get-InsightsCapability | Get-InsightsCapabilityAction
```

È possibile creare nuove azioni o eliminare le azioni esistenti usando il **Set-InsightsCapabilityAction** e **Remove-InsightsCapabilityAction** cmdlet. Ogni azione viene eseguita utilizzando le credenziali specificate nella **ActionCredential** parametro.

>[!NOTE]
>Nella versione del sistema Insights iniziale, è necessario specificare gli script di monitoraggio e aggiornamento di fuori di directory dell'utente. Questo problema verrà risolto in una versione futura.

```PowerShell
$Cred = Get-Credential
Set-InsightsCapabilityAction -Name "CPU capacity forecasting" -Type Warning -Action "C:\Users\Public\WarningScript.ps1" -ActionCredential $Cred
Set-InsightsCapabilityAction -Name "CPU capacity forecasting" -Type Critical -Action "C:\Users\Public\CriticalScript.ps1" -ActionCredential $Cred

Remove-InsightsCapabilityAction -Name "CPU capacity forecasting" -Type Warning
```

È anche possibile usare Windows Admin Center per impostare le azioni correttive tramite il **azioni** scheda all'interno di **impostazioni** pagina:

![Pagina delle impostazioni in cui utente può specificare le azioni correttive](media/actions-page-contoso.png)


## <a name="see-also"></a>Vedere anche
Per altre informazioni su System Insights, usare le risorse seguenti:

- [Panoramica sistema Insights](overview.md)
- [Funzionalità di comprensione](understanding-capabilities.md)
- [Aggiunta e lo sviluppo di funzionalità](adding-and-developing-capabilities.md)
- [Sistema Insights domande frequenti](faq.md)