---
title: Disabilitare File offline su singole cartelle reindirizzate
description: Come disabilitare il caching di File offline in singole cartelle reindirizzate alle condivisioni di rete tramite Reindirizzamento cartelle.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: c2614c0180b32a0215454f2d725d6a962986ef1f
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "71394400"
---
# <a name="disable-offline-files-on-individual-redirected-folders"></a>Disabilitare File offline su singole cartelle reindirizzate

>Si applica a: Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2 e Windows (canale semestrale)

Questo argomento descrive come disabilitare il caching di File offline in singole cartelle reindirizzate alle condivisioni di rete tramite Reindirizzamento cartelle. In questo modo, puoi specificare le cartelle da escludere dal caching localmente, riducendo le dimensioni della cache di File offline e il tempo necessario per sincronizzare File offline.

>[!NOTE]
>Questo argomento include cmdlet di esempio di Windows PowerShell che è possibile usare per automatizzare alcune delle procedure descritte. Per altre informazioni, vedi [Nozioni di base su Windows PowerShell](https://docs.microsoft.com/powershell/scripting/getting-started/fundamental/windows-powershell-basics?view=powershell-6).

## <a name="prerequisites"></a>Prerequisiti

Per disabilitare il caching di File offline per specifiche cartelle reindirizzate, l'ambiente deve soddisfare i prerequisiti seguenti.

- Un dominio Active Directory Domain Services con computer client aggiunti al dominio. Non sono previsti requisiti a livello di funzionalità della foresta o del dominio o requisiti di schema.
- Computer client che eseguono Windows 10, Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 o Windows (canale semestrale).
- Un computer in cui è installato lo strumento Gestione Criteri di gruppo.

## <a name="disabling-offline-files-on-individual-redirected-folders"></a>Disabilitazione di File offline su singole cartelle reindirizzate

Per disabilitare il caching di File offline per specifiche cartelle reindirizzate, usa Criteri di gruppo per abilitare l'impostazione criterio **Non rendere automaticamente disponibili offline specifiche cartelle reindirizzate** per l'oggetto Criteri di gruppo appropriato. La configurazione di questa impostazione criterio su **Disabilitato** o **Non configurato** rende tutte le cartelle reindirizzate disponibili offline.

>[!NOTE]
>Solo gli amministratori di dominio, gli amministratori dell'organizzazione e i membri del gruppo Proprietari autori Criteri di gruppo possono creare oggetti Criteri di gruppo.

### <a name="to-disable-offline-files-on-specific-redirected-folders"></a>Per disabilitare File offline su specifiche cartelle reindirizzate

1. Apri **Gestione Criteri di gruppo**.
2. Per creare facoltativamente un nuovo oggetto Criteri di gruppo che specifichi per quali utenti le cartelle reindirizzate devono essere escluse dalla disponibilità offline, fai clic con il pulsante destro del mouse sul dominio o sull'unità organizzativa appropriata e quindi seleziona **Crea un oggetto Criteri di gruppo in questo dominio e crea qui un collegamento**.
3. Nell'albero della console fai clic con il pulsante destro del mouse sull'oggetto Criteri di gruppo per cui vuoi configurare le impostazioni di Reindirizzamento cartelle e quindi seleziona **Modifica**. Viene visualizzato l'Editor Gestione Criteri di gruppo.
4. Nell'albero della console, in **Configurazione utente**, espandi **Criteri**, **Modelli amministrativi**, **Sistema** e **Reindirizzamento cartelle**.
5. Fai clic con il pulsante destro del mouse su **Non rendere automaticamente disponibili offline specifiche cartelle reindirizzate** e quindi seleziona **Modifica**. Viene visualizzata la finestra **Non rendere automaticamente disponibili offline specifiche cartelle reindirizzate**.
6. Seleziona **Attivata**. Nel riquadro **Opzioni** seleziona le cartelle che non devono essere rese disponibili offline selezionando le caselle di controllo appropriate. Seleziona **OK**.

### <a name="windows-powershell-equivalent-commands"></a>Comandi equivalenti di Windows PowerShell

Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura descritta in [Disabilitazione di File offline su singole cartelle reindirizzate](#disabling-offline-files-on-individual-redirected-folders). Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.

Questo esempio crea un nuovo oggetto Criteri di gruppo denominato *Impostazioni File offline* nell'unità organizzativa *UnitàOrganizzativa* nel dominio *contoso.com* (il nome distinto del protocollo LDAP è "ou=UnitàOrganizzativa,dc=contoso,dc=com"). Quindi disabilita File offline per la cartella reindirizzata Video.

```PowerShell
New-GPO -Name "Offline Files Settings" | New-Gplink -Target "ou=MyOu,dc=contoso,dc=com" -LinkEnabled Yes

Set-GPRegistryValue –Name "Offline Files Settings" –Key
"HKCU\Software\Policies\Microsoft\Windows\NetCache\{18989B1D-99B5-455B-841C-AB7C74E4DDFC}" -ValueName DisableFRAdminPinByFolder –Type DWORD –Value 1
```

Per un elenco dei nomi delle chiavi del Registro di sistema (GUID della cartella) da usare per ogni cartella reindirizzata, vedi la tabella seguente.

|Cartella reindirizzata|Nome della chiave del Registro di sistema (GUID della cartella)|
|---|---|
|AppData(Roaming)|{3EB685DB-65F9-4CF6-A03A-E3EF65729F3D}|
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
|Partite salvate|{4C5C32FF-BB9D-43B0-B5B4-2D72E54EAAA4}|

## <a name="more-information"></a>Altre informazioni

- [Panoramica di reindirizzamento cartelle, file offline e profili utente mobili](folder-redirection-rup-overview.md)
- [Distribuire il reindirizzamento cartelle con File offline](deploy-folder-redirection.md)