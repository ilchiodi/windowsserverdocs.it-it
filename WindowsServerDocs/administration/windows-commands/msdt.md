---
title: msdt
description: Argomento di riferimento per il comando MSDT, che richiama un pacchetto di risoluzione dei problemi nella riga di comando o come parte di uno script automatizzato, e Abilita opzioni aggiuntive senza input utente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ead1b672-a120-4e16-94aa-a8e13602c1d0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 47eaf859c86a939777ce8878937d36f2c581ab31
ms.sourcegitcommit: 5e313a004663adb54c90962cfdad9ae889246151
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2020
ms.locfileid: "84354372"
---
# <a name="msdt"></a>msdt

Richiama un pacchetto di risoluzione dei problemi dalla riga di comando o come parte di uno script automatizzato e Abilita opzioni aggiuntive senza input utente.

## <a name="syntax"></a>Sintassi

```
msdt </id <name> | /path <name> | /cab < name>> <</parameter> [options] … <parameter> [options]>>
```

### <a name="parameters"></a>Parametri

| Parametro | Description |
| --------- | ----------- |
| /ID`<packagename>` | Specifica il pacchetto di diagnostica da eseguire. Per un elenco dei pacchetti disponibili, vedere la pagina relativa ai [pacchetti di risoluzione dei problemi disponibili](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/ee424379(v=ws.11)#available-troubleshooting-packs). |
| /Path`<directory|.diagpkg file|.diagcfg file>` | Specifica il percorso completo di un pacchetto di diagnostica. Se si specifica una directory, la directory deve contenere un pacchetto di diagnostica. Non è possibile usare il parametro **/path** in combinazione con i parametri * */ID * *, **/DCI**o **/CAB** . |                                                                                   |
| /dci`<passkey>` | Popola il campo passkey. Questo parametro viene utilizzato solo quando un provider di supporto ha fornito una passkey. |
| /DT`<directory>` | Visualizza la cronologia di risoluzione dei problemi nella directory specificata. I risultati diagnostici vengono archiviati nelle directory **%LocalAppData%\Diagnostics** o **%LocalAppData%\ElevatedDiagnostics** dell'utente. |
| /AF`<answerfile>` | Specifica un file di risposte in formato XML che contiene risposte a una o più interazioni di diagnostica. |
| /modal`<ownerHWND>` | Imposta la modalità modale per la risoluzione dei problemi in una finestra definita dall'handle della finestra della console padre (HWND), in formato decimale. Questo parametro viene in genere usato dalle applicazioni che avviano un pacchetto di risoluzione dei problemi. Per ulteriori informazioni sull'acquisizione degli handle della finestra della console, vedere [come ottenere un handle di finestra della console (HWND)](https://support.microsoft.com/help/124103/how-to-obtain-a-console-window-handle-hwnd). |
| /moreoptions`<true|false>` | Abilita (true) o disattiva (false) la schermata finale per la risoluzione dei problemi che chiede se l'utente desidera esplorare opzioni aggiuntive. Questo parametro viene in genere utilizzato quando il pacchetto di risoluzione dei problemi viene avviato da uno strumento di risoluzione dei problemi che non fa parte del sistema operativo. |
| /param Returns`<parameters>` | Specifica un set di risposte di interazione nella riga di comando, in modo analogo a un file di risposte. Questo parametro non viene in genere utilizzato nel contesto dei pacchetti di risoluzione dei problemi creati con la finestra di progettazione TSP. Per ulteriori informazioni sullo sviluppo di parametri personalizzati, vedere la pagina relativa alla [piattaforma di risoluzione dei problemi di Windows](https://docs.microsoft.com/previous-versions/windows/desktop/wintt/windows-troubleshooting-toolkit-portal). |
| /Advanced | Espande il collegamento avanzato nella pagina iniziale per impostazione predefinita all'avvio del pacchetto di risoluzione dei problemi. |
| /custom | Richiede all'utente di confermare ogni possibile risoluzione prima che venga applicata. |

### <a name="return-codes"></a>Codici restituiti

I pacchetti di risoluzione dei problemi includono un set di cause principali, ognuno dei quali descrive un problema tecnico specifico. Dopo aver completato le attività del pacchetto per la risoluzione dei problemi, ogni causa principale restituisce uno stato fisso, non fisso, rilevato (ma non risolvibile) o non trovato. Oltre ai risultati specifici segnalati nell'interfaccia utente di risoluzione dei problemi, il motore di risoluzione dei problemi restituisce un codice nei risultati che descrivono, in termini generali, se il problema originale è stato risolto o meno dal Troubleshooter. I codici sono i seguenti:

| Codice | Descrizione |
| ---- | ----------- |
| -1 | **Interruzione:** Lo strumento di risoluzione dei problemi è stato chiuso prima del completamento delle attività di risoluzione dei problemi. |
| 0 | **Corretto:** Lo strumento di risoluzione dei problemi ha identificato e risolto almeno una causa radice e nessuna causa radice rimane in uno stato non fisso. |
| 1 | **Presente, ma non corretto:** Lo strumento di risoluzione dei problemi ha identificato una o più cause principali che rimangono in uno stato non fisso. Questo codice viene restituito anche se è stata risolta un'altra causa radice. |
| 2 | **Non trovato:** Lo strumento di risoluzione dei problemi non ha identificato alcuna causa radice. |

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [Pacchetti di risoluzione dei problemi disponibili](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/ee424379(v=ws.11)#available-troubleshooting-packs)

- [Informazioni di riferimento su PowerShell per TroubleshootingPack](https://docs.microsoft.com/powershell/module/troubleshootingpack/?view=win10-ps)