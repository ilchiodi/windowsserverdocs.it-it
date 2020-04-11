---
title: L'aggiornamento delle distribuzioni di Servizi Desktop remoto a Windows Server 2016
description: In questo articolo viene descritto come aggiornare le distribuzioni di Servizi Desktop remoto esistente a Windows Server 2016.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: spatnaik
ms.date: 03/20/2018
ms.topic: article
ms.assetid: f7b1f1f6-57c8-40ab-a235-e36240dcc1f8
author: spatnaik
manager: scottman
ms.openlocfilehash: 8980f7941a997e24ea5c5a9c1f39b4588b234b75
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857134"
---
# <a name="upgrading-your-remote-desktop-services-deployments-to-windows-server-2016"></a>L'aggiornamento delle distribuzioni di Servizi Desktop remoto a Windows Server 2016

>Si applica a: Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016

## <a name="supported-os-upgrades-with-rds-role-installed"></a>Aggiornamenti del sistema operativo è supportato con ruolo Servizi Desktop REMOTO
Gli aggiornamenti per Windows Server 2016 sono supportati solo da Windows Server 2012 R2 e Windows Server 2016.

## <a name="flow-for-deployment-upgrades"></a>Flusso per gli aggiornamenti di distribuzione
Per ridurre al minimo i tempi di inattività, è consigliabile attenersi alla procedura seguente:

1. **Server Gestore connessione desktop remoto** dovrebbe essere il primo a essere aggiornato. Se nella distribuzione è presente una configurazione attivo/attivo, rimuovere tutti i server tranne uno dalla distribuzione ed eseguire un aggiornamento sul posto. Eseguire aggiornamenti in modalità offline negli altri server Gestore connessione Desktop remoto e quindi aggiungerli nuovamente alla distribuzione. La distribuzione non sarà disponibile durante l'aggiornamento di server Gestore connessione desktop remoto.

   > [!NOTE] 
   > È necessario aggiornare i server Gestore connessione desktop remoto. Server Gestore connessione desktop remoto di Windows Server 2012 R2 non è supportata in una distribuzione mista con i server Windows Server 2016. Quando il server Gestore connessione desktop remoto sono in esecuzione Windows Server 2016 la distribuzione sarà funzionante, anche se il resto dei server nella distribuzione sono ancora in esecuzione Windows Server 2012 R2.

2. I **server licenze Desktop remoto** devono essere aggiornati prima di aggiornare i server Host sessione Desktop remoto.
   > [!NOTE] 
   > I server licenze Desktop remoto Windows Server 2012 e 2012 R2 possono essere utilizzati per le distribuzioni di Windows Server 2016, ma possono elaborare solo le licenze CAL da Windows Server 2012 R2 e versioni precedenti. Non possono usare le licenze CAL di Windows Server 2016. Per altre informazioni sui server licenza Desktop remoto, consultare [La distribuzione di servizi Desktop remoto di licenza con licenze di accesso client (CAL)](rds-client-access-license.md).

3. **I server Host sessione Desktop remoto** possono essere aggiornati successivamente. Per evitare di inattività durante l'aggiornamento l'amministratore può suddividere i server da aggiornare in 2 passaggi come indicato di seguito. Tutti sarà funzioni dopo l'aggiornamento. Per eseguire l'aggiornamento, utilizzare i passaggi descritti [server di aggiornamento di Host sessione Desktop remoto per Windows Server 2016](upgrade-to-rdsh.md).

4. **Server Host di virtualizzazione Desktop remoto** possono essere aggiornati successivamente. Per eseguire l'aggiornamento, utilizzare i passaggi descritti [server Host di virtualizzazione Desktop remoto l'aggiornamento a Windows Server 2016](upgrade-to-rdvh.md).

5. **Server accesso Web desktop remoto** possono essere aggiornati in qualsiasi momento.
   > [!NOTE]
   > L'aggiornamento Web desktop remoto può reimpostare le proprietà IIS (ad esempio i file di configurazione). Per evitare la perdita delle modifiche, inserire note o copie delle personalizzazioni apportate al sito Web di desktop remoto in IIS.

   > [!NOTE] 
   > Server accesso Web desktop remoto R2 2012 e Windows Server 2012 funziona con le distribuzioni di Windows Server 2016.

6. **I server Gateway Desktop remoto** possono essere aggiornati in qualsiasi momento.
   > [!NOTE]
   > Windows Server 2016 non include i criteri di protezione accesso rete (NAP), dovranno essere rimossi. Il modo più semplice per rimuovere i criteri corretti è eseguendo l'aggiornamento guidato. Se sono presenti eventuali criteri di protezione accesso alla RETE che è necessario eliminare, l'aggiornamento verrà bloccata e creare un file di testo sul desktop che include i criteri specifici. Per gestire i criteri di protezione accesso alla RETE, aprire lo strumento Server dei criteri di rete. Dopo l'eliminazione di loro, fare clic su **aggiornamento** nello strumento di installazione per continuare il processo di aggiornamento. 

   > [!NOTE] 
   > Windows Server 2012 e i server Gateway Desktop remoto R2 2012 funziona con le distribuzioni di Windows Server 2016.

## <a name="vdi-deployment--supported-guest-os-upgrade"></a>Distribuzione VDI: aggiornamento del sistema operativo guest supportato
Gli amministratori saranno disponibili le opzioni seguenti per eseguire l'aggiornamento di raccolte di macchina Virtuale:

### <a name="upgrade-managed-shared-vm-collections"></a>Aggiornamento raccolte gestito VM condivisi 
Gli amministratori dovranno creare modelli di macchina Virtuale con la versione desiderata del sistema operativo e usarlo per tutte le macchine virtuali nel pool di patch. 

Sono supportati i seguenti scenari di applicazione delle patch:
- Può essere corretto, Windows 7 SP1 per Windows 8 o Windows 8.1
- Windows 8 può essere corretto in Windows 8.1
- Windows 8.1 può essere corretto per Windows 10

### <a name="upgrade-unmanaged-shared-vm-collections"></a>Aggiornamento raccolte di macchine Virtuali condivise non gestite 
Gli utenti finali non può aggiornare i propri desktop personali. Gli amministratori devono eseguire l'aggiornamento. La procedura esatta ancora devono essere determinate.