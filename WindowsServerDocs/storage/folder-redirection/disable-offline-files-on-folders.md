---
title: Disabilitare File offline sulle singole cartelle reindirizzate
description: Come disabilitare File offline caching in singole cartelle reindirizzate alle condivisioni di rete tramite Reindirizzamento cartelle.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: c2614c0180b32a0215454f2d725d6a962986ef1f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394400"
---
# <a name="disable-offline-files-on-individual-redirected-folders"></a>Disabilitare File offline sulle singole cartelle reindirizzate

>Si applica a: Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2, Windows (canale semestrale)

In questo argomento viene descritto come disabilitare File offline caching in singole cartelle reindirizzate alle condivisioni di rete tramite Reindirizzamento cartelle. In questo modo è possibile specificare le cartelle da escludere dalla memorizzazione nella cache localmente, riducendo le dimensioni della cache File offline e il tempo necessario per sincronizzare File offline.

>[!NOTE]
>Questo argomento include cmdlet di esempio di Windows PowerShell che è possibile usare per automatizzare alcune delle procedure descritte. Per altre informazioni, vedere [nozioni di base su Windows PowerShell](https://docs.microsoft.com/powershell/scripting/getting-started/fundamental/windows-powershell-basics?view=powershell-6).

## <a name="prerequisites"></a>Prerequisiti

Per disabilitare File offline caching di cartelle reindirizzate specifiche, l'ambiente deve soddisfare i prerequisiti seguenti.

- Un dominio Active Directory Domain Services (AD DS) con computer client aggiunti al dominio. Non sono previsti requisiti a livello di funzionalità o di foresta o requisiti di schema.
- Computer client che eseguono Windows 10, Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 o Windows (canale semestrale).
- Un computer in cui è installata la gestione Criteri di gruppo.

## <a name="disabling-offline-files-on-individual-redirected-folders"></a>Disabilitazione File offline sulle singole cartelle reindirizzate

Per disabilitare File offline caching di cartelle reindirizzate specifiche, usare Criteri di gruppo per abilitare l'impostazione di criteri non in linea per le **cartelle reindirizzate specifiche** per l'oggetto Criteri di gruppo appropriato (GPO). La configurazione di questa impostazione di criteri su **disabilitato** o **non configurato** rende tutte le cartelle reindirizzate disponibili offline.

>[!NOTE]
>Solo gli amministratori di dominio, gli amministratori dell'organizzazione e i membri del gruppo Criteri di gruppo Creator Owners possono creare oggetti Criteri di gruppo.

### <a name="to-disable-offline-files-on-specific-redirected-folders"></a>Per disabilitare File offline in cartelle reindirizzate specifiche

1. Aprire **gestione criteri di gruppo**.
2. Per creare facoltativamente un nuovo oggetto Criteri di gruppo che specifichi quali utenti devono avere le cartelle reindirizzate escluse da rese disponibili offline, fare clic con il pulsante destro del mouse sul dominio o sull'unità organizzativa appropriata, quindi selezionare **Crea un oggetto Criteri di gruppo in questo dominio e collegarlo qui.** .
3. Nell'albero della console fare clic con il pulsante destro del mouse sull'oggetto Criteri di gruppo per il quale si desidera configurare le impostazioni di Reindirizzamento cartelle e quindi scegliere **modifica**. Viene visualizzato il Editor Gestione Criteri di gruppo.
4. Nell'albero della console, in **Configurazione utente**, espandere **criteri**, espandere **modelli amministrativi**, espandere **sistema**ed espandere **Reindirizzamento cartelle**.
5. Fare clic con il pulsante destro del mouse su **non rendere automaticamente disponibili offline le cartelle reindirizzate specifiche** , quindi scegliere **modifica**. Viene visualizzata la finestra non **eseguire automaticamente le cartelle reindirizzate specifiche disponibili offline** .
6. Seleziona **Attivata**. Nel riquadro **Opzioni** selezionare le cartelle che non devono essere rese disponibili offline selezionando le caselle di controllo appropriate. Scegliere **OK**.

### <a name="windows-powershell-equivalent-commands"></a>Comandi equivalenti di Windows PowerShell

Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura descritta in [disabilitazione di file offline sulle singole cartelle reindirizzate](#disabling-offline-files-on-individual-redirected-folders). Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.

In questo esempio viene creato un nuovo oggetto Criteri di gruppo denominato *file offline impostazioni* nell'unità organizzativa *unitàorganizzativa* nel dominio *contoso.com* (il nome distinto LDAP è "OU = UnitàOrganizzativa, DC = contoso, DC = com"). Quindi Disabilita File offline per la cartella video reindirizzati.

```PowerShell
New-GPO -Name "Offline Files Settings" | New-Gplink -Target "ou=MyOu,dc=contoso,dc=com" -LinkEnabled Yes

Set-GPRegistryValue –Name "Offline Files Settings" –Key
"HKCU\Software\Policies\Microsoft\Windows\NetCache\{18989B1D-99B5-455B-841C-AB7C74E4DDFC}" -ValueName DisableFRAdminPinByFolder –Type DWORD –Value 1
```

Vedere la tabella seguente per un elenco dei nomi delle chiavi del registro di sistema (GUID di cartella) da usare per ogni cartella reindirizzata.

|Cartella reindirizzata|Nome della chiave del registro di sistema (GUID cartella)|
|---|---|
|AppData (roaming)|{3EB685DB-65F9-4CF6-A03A-E3EF65729F3D}|
|Desktop|B4BFCC3A-DB2C-424C-B029-7FE99A87C641|
|Menu Start|{625B53C3-AB48-4EC1-BA1F-A1EF4146FC19}|
|Documenti|{FDD39AD0-238F-46AF-ADB4-6C85480369C7}|
|Immagini|{33E28130-4E1E-4676-835A-98395C3BC3BB}|
|Musica|4BD8D571-6D19-48D3-BE97-422220080E43|
|Video|{18989B1D-99B5-455B-841C-AB7C74E4DDFC}|
|Preferiti|{1777F761-68AD-4D8A-87BD-30B759FA33DD}|
|Contatti|{56784854-C6CB-462b-8169-88E350ACB882}|
|Download|{374DE290-123F-4565-9164-39C4925E467B}|
|Collegamenti|{BFB9D5E0-C6A9-404C-B2B2-AE6DB6AF4968}|
|Ricerche|{7D1D3A04-DEBB-4115-95CF-2F29DA2920DA}|
|Partite salvate|{4C5C32FF-BB9D-43B0-B5B4-2D72E54EAAA4}|

## <a name="more-information"></a>Altre informazioni

- [Panoramica di Reindirizzamento cartelle, File offline e profili utente mobili](folder-redirection-rup-overview.md)
- [Distribuire il reindirizzamento cartelle con File offline](deploy-folder-redirection.md)