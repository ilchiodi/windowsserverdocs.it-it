---
title: I portatili remoti si disconnettono dalla rete wireless
description: Risoluzione di un problema per cui un portatile remoto si disconnette dalla rete wireless.
ms.reviewer: rklemen
ms.topic: troubleshooting
author: kaushika-msft
manager: dcscontentpm
ms.author: delhan
ms.date: 07/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: 72bf482512ff3bb0a678ae59cd6ac20b947a54d9
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "80857154"
---
# <a name="remote-laptop-disconnects-from-wireless-network"></a>I portatili remoti si disconnettono dalla rete wireless

Questo problema può verificarsi quando un client Desktop remoto si connette a un portatile tramite una rete wireless 802.1x. Il portatile si disconnette dalla rete wireless a intermittenza e non si riconnette automaticamente.

Si tratta di un problema noto che si verifica quando l'impostazione di autenticazione di rete per la connessione di rete wireless è **Autenticazione utente**.

Per ovviare a questo problema, configura l'impostazione di autenticazione di rete su **Autenticazione utente o computer** oppure su **Autenticazione computer**.

 > [!NOTE]  
> Per modificare le impostazioni di autenticazione di rete in un singolo computer, potresti dover usare il pannello di controllo Centro connessioni di rete e condivisione per creare una nuova connessione wireless con le nuove impostazioni.

Per una descrizione completa di come configurare le impostazioni di rete wireless mediante oggetti Criteri di gruppo, vedi [Configurare criteri di rete wireless (IEEE 802.11)](../../../networking/core-network-guide/cncg/wireless/e-wireless-access-deployment.md#bkmk_policies).
