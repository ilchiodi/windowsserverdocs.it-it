---
title: Utilizzo di PowerShell nell'estensione
description: Uso di PowerShell nella propria estensione Windows Admin Center SDK (progetto Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: ae5004104150c510a56c06161c9280e029968298
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867602"
---
# <a name="using-powershell-in-your-extension"></a>Utilizzo di PowerShell nell'estensione #

>Si applica a: Windows Admin Center, Windows Admin Center anteprima

Apriamo più approfondita delle estensioni SDK di Windows Admin Center, esaminiamo l'aggiunta di comandi di PowerShell per l'estensione.

## <a name="powershell-in-typescript"></a>PowerShell in TypeScript ##

Il processo di compilazione gulp dispone di un passaggio di generazione che sarà eseguite eventuali "ps1" che viene inserito nella cartella "/ src/risorse/scripts" e compilarle nella classe "script di powershell" nella cartella "/ src/generato".

>[!NOTE] 
> Non aggiornare manualmente i file "strings.ts" né "powershell-scripts.ts". Eventuali modifiche apportate verranno sovrascritte durante la generazione successiva.

### <a name="adding-your-own-powershell-script"></a>Aggiunta di script di PowerShell ##

Si aggiungerà ulteriori informazioni sull'aggiunta a breve lo script di PowerShell.

### <a name="managing-powershell-sessions"></a>Gestione delle sessioni di PowerShell ###

Quando si lavora con PowerShell in Windows Admin Center, le sessioni sono un componente obbligatorio per il processo di esecuzione dello script. Per eseguire script su server remoti gestiti, PowerShell Usa spazi di esecuzione. Windows Admin Center esegue il wrapping della creazione dello spazio di esecuzione e gestione in un oggetto PowerShellSession per gestire la durata e consentire il riutilizzo dello spazio di esecuzione per l'esecuzione di script sequenziale.

Ogni componente deve creare un riferimento a un oggetto di sessione che viene creato dalla classe AppContextService utilizzando tre diverse opzioni:
<!-- I don't 100% get this part - it looks like you're adding 3 arguments - nodeName, <session key>, and <PowerShellSessionRequestOptions>. I got that from looking at the examples, not the text. We need to rework those paras explaining. -->
``` ts
this.psSession = this.appContextService.powerShell.createSession(this.appContextService.activeConnection.nodeName);
```

Fornendo il nome del nodo per il metodo createSession, una nuova sessione di PowerShell viene creata, utilizzata e quindi viene eliminata immediatamente dopo il completamento della chiamata di PowerShell. Questa funzionalità è utile per le chiamate non standard, ma l'uso ripetuto può influire sulle prestazioni. Una sessione richiede circa 1 secondo per creare, quindi, in modo continuo il riciclo sessioni può provocare rallentamenti.

``` ts
this.psSession = this.appContextService.powerShell.createSession(this.appContextService.activeConnection.nodeName, '<session key>');
```

Il primo parametro facoltativo è il \<chiave di sessione\> parametro. In questo modo viene creata una sessione con chiave che può essere cercata e riutilizzata, persino tra componenti (ossia Component1 può creare una sessione con la chiave "SME-ROCKS" e Component2 può usare la stessa sessione).  

``` ts
this.psSession = this.appContextService.powerShell.createSession(this.appContextService.activeConnection.nodeName, '<session key>', <PowerShellSessionRequestOptions>);
```
<!-- The second optional parameter is \<PowerShellSessionRequestOptions\> that does ... ? -->
### <a name="common-patterns"></a>Modelli comuni ###

Nella maggior parte dei casi, creare una sessione con chiave nel **ngOnInit** metodo e quindi eliminarla in un **ngOnDestroy**. Seguire questo modello quando sono presenti più script di PowerShell in un componente, ma la sessione sottostante che non viene condivisa tra i componenti.

Per ottenere risultati ottimali, assicurarsi che la creazione della sessione viene gestito all'interno di componenti invece services: questo contribuisce a garantire tale durata e la pulitura può essere gestita correttamente.