---
title: Al momento della connessione l'utente riceve un messaggio Servizi Desktop remoto è attualmente occupato
description: Risoluzione del problema per cui il servizio Desktop remoto è occupato quando gli utenti avviano una connessione Desktop remoto.
ms.reviewer: rklemen
ms.topic: troubleshooting
author: kaushika-msft
manager: dcscontentpm
ms.author: delhan
ms.date: 07/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: c345833ee63a1286a5615998649e8aa9d25896a6
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "80857164"
---
# <a name="on-connecting-user-receives-remote-desktop-service-is-currently-busy-message"></a>Al momento della connessione l'utente riceve un messaggio "Servizi Desktop remoto è attualmente occupato"

Per determinare la risposta appropriata per questo problema, segui queste indicazioni:

- Servizi Desktop remoto smette di rispondere (ad esempio, il client Desktop remoto sembra "bloccarsi" sulla schermata iniziale)?  
   - Se il servizio non risponde più, vedi [Problema di memoria del server Host sessione Desktop remoto](#rdsh-server-memory-issue).
   - Se il client sembra interagire normalmente con il servizio, prosegui con il passaggio successivo.
- Se uno o più utenti disconnettono le rispettive sessioni Desktop remoto, possono riconnettersi?  
   - Se il servizio continua a negare le connessioni indipendentemente da quanti utenti disconnettono le loro sessioni, vedi [Problema del listener Desktop remoto](#rd-listener-issue).
   - Se il servizio inizia ad accettare di nuovo le connessioni dopo che un determinato numero di utenti si è disconnesso dalle rispettive sessioni, [verifica il criterio relativo al limite di connessioni](#check-the-connection-limit-policy).

## <a name="rdsh-server-memory-issue"></a>Problema di memoria del server Host sessione Desktop remoto

È stata rilevata una perdita di memoria in alcuni server Host sessione Desktop remoto Windows Server 2012 R2. Con il passare del tempo questi server iniziano a rifiutare sia le connessioni Desktop remoto sia gli accessi alla console locale con messaggi simili al seguente:

> Impossibile completare l'attività che si sta tentando di eseguire perché Servizi Desktop remoto è attualmente occupato. Riprovare tra alcuni minuti. È possibile che altri utenti riescano comunque a connettersi.

Anche i client Desktop remoto che provano a connettersi smettono di rispondere.

Per ovviare a questo problema, riavvia il server Host sessione Desktop remoto.

Per risolvere questo problema, applica l'aggiornamento KB 4093114, [10 aprile 2018 - KB4093114 (rollup mensile)](https://support.microsoft.com/help/4093114/) ai server Host sessione Desktop remoto.

## <a name="rd-listener-issue"></a>Problema del listener Desktop remoto

In alcuni server Host sessione Desktop remoto aggiornati direttamente da Windows Server 2008 R2 a Windows Server 2012 R2 o Windows Server 2016 è stato rilevato un problema. Quando un client Desktop remoto si connette al server Host sessione Desktop remoto, tale server crea un listener Desktop remoto per la sessione utente. I server interessati effettuano un conteggio dei listener Remote Desktop che aumenta man mano che gli utenti si connettono, ma che non diminuisce mai.

Puoi ovviare a questo problema con le soluzioni seguenti:

  - Riavvia il server Host sessione Desktop remoto per reimpostare il conteggio dei listener Desktop remoto.
  - Modifica il criterio relativo al limite di connessioni impostandolo su un valore molto alto. Per altre informazioni sulla gestione di tale criterio, vedi [Verificare il criterio relativo al limite di connessioni](#check-the-connection-limit-policy).

Per risolvere questo problema, applica gli aggiornamenti seguenti ai server Host sessione Desktop remoto:

  - Windows Server 2012 R2: KB 4343891, [30 agosto 2018 - KB4343891 (anteprima del rollup mensile)](https://support.microsoft.com/help/4343891/windows-81-update-kb4343891)
  - Windows Server 2016: KB 4343884, [30 agosto 2018 - KB4343884 (build sistema operativo 14393.2457)](https://support.microsoft.com/help/4343884/windows-10-update-kb4343884)

## <a name="check-the-connection-limit-policy"></a>Verificare il criterio relativo al limite di connessioni

Puoi impostare il limite relativo al numero di connessioni Desktop remoto simultanee a livello del singolo computer o configurando un oggetto Criteri di gruppo. Per impostazione predefinita, questo limite non è configurato.

Per verificare le impostazioni correnti e identificare gli eventuali oggetti Criteri di gruppo esistenti nel server Host sessione Desktop remoto, apri una finestra del prompt dei comandi come amministratore e immetti il comando seguente:
  
```cmd
gpresult /H c:\gpresult.html
```
   
Al termine dell'esecuzione di questo comando, apri **gpresult.html**. In **Configurazione computer\\Modelli amministrativi\\Componenti di Windows\\Servizi Desktop remoto\\Host sessione Desktop remoto\\Connessioni** trova il criterio **Limita il numero di connessioni**.

  - Se l'impostazione di questo criterio è **Disabilitato**, i criteri di gruppo non limitano le connessioni RDP.
  - Se l'impostazione di questo criterio è **Abilitato**, controlla **Oggetto Criteri di gruppo dominante**. Se devi rimuovere o modificare il limite relativo alle connessioni, modifica questo oggetto Criteri di gruppo.

Per applicare le modifiche apportate al criterio, apri una finestra del prompt dei comandi nel computer interessato e immetti il comando seguente:
  
```cmd
gpupdate /force
```
  
