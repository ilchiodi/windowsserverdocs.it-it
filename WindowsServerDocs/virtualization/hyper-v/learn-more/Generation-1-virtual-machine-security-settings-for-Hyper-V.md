---
title: Impostazioni di sicurezza di generazione 1 macchina virtuale per Hyper-V
description: Descrive le impostazioni di sicurezza disponibili nella console di gestione di Hyper-V per macchine virtuali di seconda generazione
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.topic: article
ms.assetid: f8f8c569-8b74-4c19-876e-1c7d00cce308
author: larsiwer
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: f86c4fe9222f08b3ef3719080deeb4fbda6edd33
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860814"
---
# <a name="generation-1-virtual-machine-security-settings"></a>Impostazioni di sicurezza di generazione 1 macchina virtuale

>Si applica a: Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

Utilizzare le impostazioni di sicurezza di generazione 1 macchina virtuale in Hyper-V Manager per proteggere i dati e lo stato di una macchina virtuale.

## <a name="encryption-support-settings-in-hyper-v-manager"></a>Impostazioni di supporto di crittografia nella gestione di Hyper-V

È possibile proteggere i dati e lo stato della macchina virtuale selezionando l'opzione di supporto di crittografia seguenti.

- **Crittografare il traffico di migrazione dello stato e la macchina virtuale** -consente di crittografare lo stato della macchina virtuale salvata quando viene scritta su disco e il traffico di migrazione in tempo reale.

Per abilitare questa opzione, è necessario aggiungere un'unità di archiviazione chiavi per la macchina virtuale.

## <a name="key-storage-drive-in-hyper-v-manager"></a>Unità di archiviazione delle chiavi nella gestione di Hyper-V

Un'unità di archiviazione chiavi fornisce un'unità di piccole dimensioni per la macchina virtuale per una chiave di BitLocker da archiviare. In questo modo la macchina virtuale crittografare il disco del sistema operativo senza richiedere un chip Trusted Platform Module (TPM) virtualizzate. Il contenuto dell'unità di archiviazione chiavi crittografato con una protezione con chiave. L'host di protezione con chiave authories Hyper-V per eseguire la macchina virtuale. Sia il contenuto dell'unità di archiviazione delle chiavi e la protezione con chiave viene archiviato come parte dello stato di runtime della macchina virtuale.

Per decrittografare il contenuto dell'unità di archiviazione delle chiavi e avviare la macchina virtuale, l'host Hyper-V deve essere:

- Parte di un'infrastruttura protetta autorizzata per questa macchina virtuale, o
- Dispone della chiave privata da uno dei tutori della macchina virtuale.

Per ulteriori informazioni sulle infrastrutture protetta, vedere la sezione Introduzione a macchine virtuali schermati [sicurezza e Assurance](../../../security/Security-and-Assurance.md).

È possibile aggiungere un'unità di archiviazione delle chiavi in uno slot vuoto in uno dei controller IDE della macchina virtuale. A tale scopo, fare clic su **aggiungere unità di archiviazione chiave** per aggiungere un'unità di archiviazione chiavi per il primo slot di controller IDE gratuito di questa macchina virtuale.

## <a name="see-also"></a>Vedere anche

- [Impostazioni di sicurezza di macchine virtuali di seconda generazione nella console di gestione di Hyper-V](Generation-2-virtual-machine-security-settings-for-hyper-v.md)
- [Sicurezza e controllo](../../../security/Security-and-Assurance.md)