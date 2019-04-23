---
title: Disabilita file Offline su singole cartelle reindirizzate
description: Come disabilitare la memorizzazione nella cache di file non in linea per le singole cartelle che vengono reindirizzate alle condivisioni di rete tramite reindirizzamento cartelle.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: adc93906cb7ff958fc1db7b00abdc557623e764e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834202"
---
# <a name="disable-offline-files-on-individual-redirected-folders"></a>Disabilita file Offline su singole cartelle reindirizzate

>Si applica a: Windows 10, Windows 8, Windows 8.1, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016

Questo argomento descrive come disabilitare la memorizzazione nella cache di file non in linea per le singole cartelle che vengono reindirizzate alle condivisioni di rete tramite reindirizzamento cartelle. Questo offre la possibilità di specificare le cartelle da escludere dalla memorizzazione nella cache in locale, riducendo la cache dei file Offline le dimensioni e il tempo necessario per sincronizzare i file Offline.

>[!NOTE]
>Questo argomento include cmdlet di esempio di Windows PowerShell che è possibile usare per automatizzare alcune delle procedure descritte. Per altre informazioni, vedere [nozioni di base di Windows PowerShell](https://docs.microsoft.com/powershell/scripting/getting-started/fundamental/windows-powershell-basics?view=powershell-6).

## <a name="prerequisites"></a>Prerequisiti

Per disabilitare la memorizzazione nella cache di file Offline delle cartelle reindirizzate specifiche, l'ambiente deve soddisfare i prerequisiti seguenti.

- Un dominio di Active Directory Domain Services (AD DS), con i computer client aggiunti al dominio. Non esistono requisiti a livello funzionale di foresta o dominio o requisiti dello schema.
- Computer client che eseguono Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.
- Un computer di gestione di criteri di gruppo installata.

## <a name="disabling-offline-files-on-individual-redirected-folders"></a>La disabilitazione di file Offline su singole cartelle reindirizzate

Per disabilitare la memorizzazione nella cache di file Offline delle cartelle reindirizzate specifiche, usare criteri di gruppo per abilitare la **non rendere automaticamente le cartelle reindirizzate specifiche disponibili offline** impostazione dei criteri per oggetto Criteri di gruppo (GPO) appropriato. Questa impostazione dei criteri di configurazione **disabilitati** o **non configurato** rende disponibili tutte le cartelle reindirizzate offline.

>[!NOTE]
>Solo gli amministratori di dominio, gli amministratori dell'organizzazione e i membri del gruppo Proprietari autori criteri di gruppo è possono creare oggetti Criteri di gruppo.

### <a name="to-disable-offline-files-on-specific-redirected-folders"></a>Per disabilitare i file Offline su specifiche cartelle reindirizzate

1. Aprire **Gestione criteri di gruppo**.
2. Per creare, facoltativamente, un nuovo oggetto Criteri di gruppo che specifica gli utenti che devono avere reindirizzato cartelle escluse dalla viene reso disponibile in modalità offline, fare doppio clic il dominio appropriato o unità organizzativa (OU) e quindi selezionare **crea un oggetto Criteri di gruppo in questo dominio e crea un collegamento In questo caso IT**.
3. Nell'albero della console, fare doppio clic su oggetto Criteri di gruppo per cui si desidera configurare le impostazioni di reindirizzamento cartelle e quindi selezionare **modifica**. Viene visualizzato l'Editor Gestione criteri di gruppo.
4. Nell'albero della console, sotto **User Configuration**, espandere **criteri**, espandere **modelli amministrativi**, espandere **sistema**, e Espandere **reindirizzamento cartelle**.
5. Fare doppio clic su **non rendere automaticamente le cartelle reindirizzate specifiche disponibili non in linea** e quindi selezionare **modificare**. Il **non rendere automaticamente le cartelle reindirizzate specifiche disponibili offline** verrà visualizzata la finestra.
6. Seleziona **Attivata**. Nel **opzioni** riquadro selezionare le cartelle che non devono essere reso disponibile non in linea selezionando le caselle di controllo appropriate. Scegliere **OK**.

### <a name="windows-powershell-equivalent-commands"></a>Comandi equivalenti di Windows PowerShell

Il file o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione, come la procedura descritta in [la disabilitazione di file Offline su singole cartelle reindirizzate](#disabling-offline-files-on-individual-redirected-folders). Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.

Questo esempio viene creato un nuovo oggetto Criteri di gruppo denominato *impostazioni file Offline* nel *UnitàOrganizzativa* unità organizzativa nel *contoso.com* dominio (il nome distinto LDAP è "ou = UnitàOrganizzativa, dc = Contoso, dc = com "). Quindi disabilita file Offline per la cartella video reindirizzati.

```PowerShell
New-GPO -Name "Offline Files Settings" | New-Gplink -Target "ou=MyOu,dc=contoso,dc=com" -LinkEnabled Yes

Set-GPRegistryValue –Name "Offline Files Settings" –Key
"HKCU\Software\Policies\Microsoft\Windows\NetCache\{18989B1D-99B5-455B-841C-AB7C74E4DDFC}" -ValueName DisableFRAdminPinByFolder –Type DWORD –Value 1
```

Vedere la tabella seguente per un elenco di nomi chiave del Registro di sistema (cartella GUID) da usare per ogni cartella reindirizzata.

|Cartella reindirizzata|Nome della chiave del Registro di sistema (GUID della cartella)|
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
|Partite salvate|{4C5C32FF-BB9D-43B0-B5B4-2D72E54EAAA4}|

## <a name="more-information"></a>Altre informazioni

- [Panoramica di reindirizzamento cartelle, file Offline e profili utente mobili](folder-redirection-rup-overview.md)
- [Distribuire reindirizzamento cartelle con i file Offline](deploy-folder-redirection.md)