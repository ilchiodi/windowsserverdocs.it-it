---
title: Client desktop di Windows per amministratori
description: Informazioni sul client desktop di Windows utili principalmente per gli amministratori.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: heidilohr
manager: lizross
ms.author: helohr
ms.date: 09/16/2019
ms.localizationpriority: medium
ms.openlocfilehash: c0a9f11aea9e24cc1a2bab275aae8dc0a81e9cba
ms.sourcegitcommit: 0a0a45bec6583162ba5e4b17979f0b5a0c179ab2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/13/2020
ms.locfileid: "79323663"
---
# <a name="windows-desktop-client-for-admins"></a>Client desktop di Windows per amministratori

>Si applica a: Windows 10 e Windows 7

Questo argomento contiene informazioni aggiuntive sul client desktop di Windows utili per gli amministratori. Per informazioni di base sull'utilizzo, vedi [Introduzione al client desktop di Windows](windowsdesktop.md).

## <a name="installation-options"></a>Opzioni di installazione

Benché gli utenti possano installare il client direttamente dopo averlo scaricato, se esegui la distribuzione in più dispositivi, puoi distribuire il client anche in altri modi. La distribuzione tramite criteri di gruppo o Microsoft Endpoint Configuration Manager consente di eseguire il programma di installazione in modo invisibile all'utente dalla riga di comando. Esegui questi comandi per distribuire il client per dispositivo o per utente.

### <a name="per-device-installation"></a>Installazione per dispositivo

```
msiexec.exe /I <path to the MSI> /qn ALLUSERS=1
```

### <a name="per-user-installation"></a>Installazione per utente

```
msiexec.exe /i `<path to the MSI>` /qn ALLUSERS=2 MSIINSTALLPERUSER=1
```

## <a name="configuration-options"></a>Opzioni di configurazione

Questa sezione illustra le nuove opzioni di configurazione per il client.

### <a name="configure-update-notifications"></a>Configurare le notifiche di aggiornamento

Per impostazione predefinita, il client invia una notifica ogni volta che è disponibile un aggiornamento. Per disattivare le notifiche, imposta le informazioni del Registro di sistema seguenti:

- **Chiave:** HKLM\Software\Microsoft\MSRDC\Policies
- **Tipo:** REG_DWORD
- **Nome:** AutomaticUpdates
- **Dati:** 0 = Disabilita le notifiche. 1 = Mostra le notifiche.

### <a name="configure-user-groups"></a>Configurare i gruppi di utenti

Puoi configurare il client per uno dei tipi di gruppi di utenti seguenti determinando in questo modo quando vengono ricevuti gli aggiornamenti dal client.

#### <a name="insider-group"></a>Gruppo Insider

Il gruppo Insider è per la convalida preliminare ed è costituito da amministratori e da utenti selezionati. Prima che il client venga rilasciato al gruppo Public, questo gruppo esegue test in modo da rilevare eventuali problemi nell'aggiornamento che possono avere impatto sulle prestazioni.

> [!NOTE]
> È consigliabile che ogni organizzazione abbia alcuni utenti nel gruppo Insider per testare gli aggiornamenti e rilevare tempestivamente i problemi.

Nel gruppo Insider una nuova versione del client viene rilasciata agli utenti il secondo martedì di ogni mese per la convalida preliminare. Se l'aggiornamento non presenta problemi, viene rilasciato al gruppo Public due settimane dopo. Gli utenti del gruppo Insider riceveranno automaticamente le notifiche di aggiornamento ogni volta che sono pronti aggiornamenti. Per informazioni più dettagliate sulle modifiche del client desktop di Windows, vedi [Introduzione al client desktop di Windows](windowsdesktop-whatsnew.md).

Per configurare il client per il gruppo Insider, imposta le informazioni del Registro di sistema seguenti:

- **Chiave:** HKLM\Software\Microsoft\MSRDC\Policies
- **Tipo:** REG_SZ
- **Nome:** ReleaseRing
- **Dati:** insider

#### <a name="public-group"></a>Gruppo Public

Questo gruppo è per tutti gli utenti ed è la versione più stabile. Non è necessario eseguire alcuna operazione per configurare questo gruppo.

Il gruppo Public riceve la versione del client testata dal gruppo Insider ogni quarto martedì del mese. Se questa impostazione è abilitata, tutti gli utenti del gruppo Public riceveranno una notifica di aggiornamento.
