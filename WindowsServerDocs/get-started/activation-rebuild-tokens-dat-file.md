---
title: Ricompilare il file Tokens.dat
description: Questo articolo descrive come ricompilare il file Tokens.dat durante la risoluzione dei problemi di attivazione di Windows
ms.topic: troubleshooting
ms.date: 09/18/2019
ms.technology: server-general
author: Teresa-Motiv
ms.author: v-tea
manager: dcscontentpm
ms.localizationpriority: medium
ms.openlocfilehash: 8a5835cd601b2eb327c8605d70bf075e6c8e8414
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "71962997"
---
# <a name="rebuild-the-tokensdat-file"></a>Ricompilare il file Tokens.dat

Durante la risoluzione dei problemi di attivazione di Windows, può essere necessario ricompilare il file Tokens.dat. Questo articolo descrive in dettaglio come eseguire questa operazione.

## <a name="resolution"></a>Risoluzione

Per ricompilare il file Tokens.dat, segui questi passaggi:

1. Apri una finestra del prompt dei comandi con privilegi elevati:  
   **Per Windows 10**

   1. Apri il menu **Start** e digita **cmd**.
   1. Nei risultati della ricerca fai clic con il pulsante destro del mouse su **Prompt dei comandi** e quindi seleziona **Esegui come amministratore**.  

   **Per Windows 8.1**
   1. Scorri rapidamente dal bordo destro della schermata e quindi tocca **Ricerca**. In alternativa, se usi un mouse, posiziona il puntatore nell'angolo inferiore destro della schermata e quindi seleziona **Ricerca**.
   1. Nella casella di ricerca digita **cmd**.
   1. Scorri o fai clic con il pulsante destro del mouse sull'icona **Prompt dei comandi** visualizzata.
   1. Tocca o fai clic su **Esegui come amministratore**.

   **Per Windows 7**
   1. Apri il menu **Start** e digita **cmd**.
   1. Nei risultati della ricerca fai clic con il pulsante destro del mouse su **cmd.exe** e quindi seleziona **Esegui come amministratore**.

1. Immetti l'elenco dei comandi appropriato per il sistema operativo.  

   Per Windows 10, Windows Server 2016 e versioni successive di Windows, immetti i comandi seguenti in sequenza:
   ```cmd
   net stop sppsvc
   cd %windir%\system32\spp\store\2.0
   ren tokens.dat tokens.bar
   net start sppsvc
   cscript.exe %windir%\system32\slmgr.vbs /rilc
   ```
   Per Windows 8.1, Windows Server 2012 e Windows Server 2012 R2, immetti i comandi seguenti in sequenza:
   ```cmd
   net stop sppsvc
   cd %windir%\ServiceProfiles\LocalService\AppData\Local\Microsoft\WSLicense
   ren tokens.dat tokens.bar
   net start sppsvc
   cscript.exe %windir%\system32\slmgr.vbs /rilc
   ```
   Per Windows 7, Windows Server 2008 e Windows Server 2008 R2, immetti i comandi seguenti in sequenza:
   ```cmd
   net stop sppsvc
   cd %windir%\ServiceProfiles\NetworkService\AppData\Roaming\Microsoft\SoftwareProtectionPlatform
   ren tokens.dat tokens.bar
   net start sppsvc
   cscript.exe %windir%\system32\slmgr.vbs /rilc
   ```
1. Riavviare il computer.

## <a name="more-information"></a>Altre informazioni

Dopo aver ricompilato il file Tokens.dat, devi reinstallare il codice Product Key usando uno dei metodi seguenti:

- Apri il prompt dei comandi con privilegi elevati, digita il comando seguente e quindi premi INVIO:

   ```cmd
   cscript.exe %windir%\system32\slmgr.vbs /ipk <Product key>
   ```

  > [!IMPORTANT]
  > Non usare l'opzione **/upk** per disinstallare un codice Product Key. Per installare un codice Product Key sovrascrivendo un codice esistente, usa l'opzione **/ipk**.
- Fai clic con il pulsante destro del mouse su **Risorse del computer**, scegli **Proprietà** e quindi seleziona **Cambia codice Product Key**.

Per altre informazioni sulle chiavi di configurazione di client del Servizio di gestione delle chiavi, vedi [Chiavi di configurazione di client del Servizio di gestione delle chiavi](kmsclientkeys.md).
