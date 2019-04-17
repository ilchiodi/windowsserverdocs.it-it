---
title: Disabilitare i file Offline per le singole cartelle reindirizzate
description: Come disabilitare la memorizzazione nella cache i file Offline per le singole cartelle che vengono reindirizzate a condivisioni di rete utilizzando il reindirizzamento delle cartelle.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: adc93906cb7ff958fc1db7b00abdc557623e764e
ms.sourcegitcommit: 9ed4c9fe04ebf3ef488170503c9a354c992b6fde
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/03/2018
ms.locfileid: "4339309"
---
# Disabilitare i file Offline per le singole cartelle reindirizzate

>Si applica a: Windows 10, Windows 8, Windows 8.1, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016

Questo argomento descrive come disabilitare la memorizzazione nella cache i file Offline per le singole cartelle che vengono reindirizzate a condivisioni di rete utilizzando il reindirizzamento delle cartelle. Questo offre la possibilità di specificare le cartelle da escludere della memorizzazione nella cache in locale, riducendo la cache di file Offline dimensioni e tempo necessario per sincronizzare i file Offline.

>[!NOTE]
>Questo argomento include cmdlet Windows PowerShell di esempio che puoi usare per automatizzare alcune delle procedure descritte. Per ulteriori informazioni, vedere [Nozioni di base di Windows PowerShell](https://docs.microsoft.com/powershell/scripting/getting-started/fundamental/windows-powershell-basics?view=powershell-6).

## Prerequisiti

Per disabilitare i file Offline memorizzazione nella cache delle cartelle reindirizzate specifiche, l'ambiente deve soddisfare i prerequisiti seguenti.

- Un dominio di Active Directory Domain Services (AD DS), con i computer client aggiunto al dominio. Non esistono requisiti al livello di funzionalità della foresta o un dominio o requisiti dello schema.
- Computer client che eseguono Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.
- Un computer con installato Gestione criteri di gruppo.

## La disabilitazione di file Offline per le singole cartelle reindirizzate

Per disabilitare la memorizzazione nella cache delle cartelle reindirizzate specifiche di file Offline, utilizzare criteri di gruppo per abilitare l'impostazione dei criteri **non rendere offline automaticamente cartelle reindirizzate specifiche disponibili** per oggetto Criteri di gruppo (GPO) appropriato. Configurare questa impostazione dei criteri **disabilitata** o **Non configurata** rende reso disponibile offline tutte le cartelle reindirizzate.

>[!NOTE]
>Solo gli amministratori di dominio, gli amministratori aziendali e i membri del gruppo proprietari creatore di criteri di gruppo possono creare oggetti Criteri di gruppo.

### Per disabilitare i file Offline per le cartelle reindirizzate specifiche.

1. Apri **Gestione criteri di gruppo**.
2. Per creare facoltativamente un nuovo oggetto Criteri di gruppo che specifica gli utenti che devono avere reindirizzate cartelle escluse da essere rese disponibili offline, fai il dominio appropriato o unità organizzativa (UO) e quindi seleziona **Crea un oggetto Criteri di gruppo in questo dominio e collegarlo qui **.
3. Nell'albero della console, fai clic sull'oggetto Criteri di gruppo per cui si desidera configurare le impostazioni di cartelle di reindirizzamento cartelle e quindi seleziona **Modifica**. Viene visualizzata l'Editor Gestione criteri di gruppo.
4. Nell'albero della console, in **Configurazione utente**, espandere **i criteri**, modelli **Amministrativi**, espandere **sistema**e **Il reindirizzamento delle cartelle**.
5. Fai **automaticamente rendere specifica le cartelle reindirizzate disponibili offline** e quindi seleziona **Modifica**. Viene visualizzata la finestra **non automaticamente rendere specifica le cartelle reindirizzate disponibili offline** .
6. Seleziona **Attivata**. Nel riquadro **Opzioni** selezionare le cartelle che non devono essere reso disponibile offline selezionando le caselle di controllo. Seleziona **OK**.

### Comandi equivalenti di Windows PowerShell

Il seguente cmdlet Windows PowerShell o i cmdlet di eseguono la stessa funzione la procedura descritta in [Disabilitando i file Offline per le singole cartelle reindirizzate](#disabling-offline-files-on-individual-redirected-folders). Immetti ogni cmdlet su una singola riga, anche se possano essere visualizzati a capo tra più righe qui a causa di vincoli di formattazione.

Questo esempio crea un nuovo oggetto Criteri di gruppo denominato *Le impostazioni di file Offline* nell'unità organizzativa *MyOu* nel dominio *contoso.com dell'azienda* (il nome distinto LDAP è "ou = MyOU, dc = contoso, dc = com"). Disabilita quindi i file Offline per la cartella video reindirizzate.

```PowerShell
New-GPO -Name "Offline Files Settings" | New-Gplink -Target "ou=MyOu,dc=contoso,dc=com" -LinkEnabled Yes

Set-GPRegistryValue –Name "Offline Files Settings" –Key
"HKCU\Software\Policies\Microsoft\Windows\NetCache\{18989B1D-99B5-455B-841C-AB7C74E4DDFC}" -ValueName DisableFRAdminPinByFolder –Type DWORD –Value 1
```

Vedi la tabella seguente per un elenco di nomi chiave del Registro di sistema (cartella GUID) da utilizzare per ogni cartella reindirizzata.

|Cartella reindirizzata|Nome della chiave del Registro di sistema (cartella GUID)|
|---|---|
|AppData|{3EB685DB-65F9-4CF6-A03A-E3EF65729F3D}|
|Desktop|{B4BFCC3A-DB2C-424C-B029-7FE99A87C641}|
|Menu Start|{625B53C3-AB48-4EC1-BA1F-A1EF4146FC19}|
|Documenti|{FDD39AD0-238F-46AF-ADB4-6C85480369C7}|
|Immagini|{33E28130-4E1E-4676-835A-98395C3BC3BB}|
|Musica|{4BD8D571-6D19-48D3-BE97-422220080E43}|
|Video|{18989B1D-99B5-455B-841C-AB7C74E4DDFC}|
|Preferiti|{1777F761-68AD-4D8A-87BD-30B759FA33DD}|
|Contatti|{56784854-C6CB-462b-8169-88E350ACB882}|
|Download|{374DE290-123F-4565-9164-39C4925E467B}|
|Collegamenti|{BFB9D5E0-C6A9-404C-B2B2-AE6DB6AF4968}|
|Ricerche|{7D1D3A04-DEBB-4115-95CF-2F29DA2920DA}|
|Giochi salvati|{4C5C32FF-BB9D-43B0-B5B4-2D72E54EAAA4}|

## Ulteriori informazioni

- [Panoramica di cartelle di reindirizzamento cartelle, file Offline e profili utente mobili](folder-redirection-rup-overview.md)
- [Distribuire cartelle di reindirizzamento cartelle con i file Offline](deploy-folder-redirection.md)