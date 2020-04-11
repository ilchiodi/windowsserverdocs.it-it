---
title: Attivare il server licenze di Servizi Desktop remoto
description: Installare e attivare il server licenze di Desktop remoto
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
ms.assetid: eb24ddd2-0361-41fe-bd6b-c7c63427cb71
author: lizap
ms.author: elizapo
ms.date: 09/20/2016
manager: dongill
ms.openlocfilehash: 3eaa999c03c97ad3188d4dcd8514b2705bf0a3b1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852974"
---
# <a name="activate-the-remote-desktop-services-license-server"></a>Attivare il server licenze di Servizi Desktop remoto

>Si applica a: Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016

Il server licenze di Servizi Desktop remoto invia licenze di accesso client (CAL) a utenti e dispositivi quando accedono all'host sessione Desktop remoto. Per attivare il server licenze, usare Gestione licenze Desktop remoto. 

## <a name="install-the-rd-licensing-role"></a>Installare il ruolo Servizio licenze Desktop remoto

1. Accedere al server da utilizzare come server licenze utilizzando un account amministratore.
2. In Server Manager fare clic su **Riepilogo ruoli**, quindi su **Aggiungi ruoli**.
   Fare clic su **Avanti** nella prima pagina della procedura guidata dei ruoli.
3. Selezionare **Servizi Desktop remoto**, fare clic su **Avanti** e ancora su **Avanti** nella pagina Servizi Desktop remoto.
4. Selezionare **Servizio licenze Desktop remoto**, quindi fare clic su **Avanti**.
5. Configurare il dominio: selezionare **Configurare un ambito di individuazione per questo server licenze**, fare clic su **Questo dominio**, quindi fare clic su **Avanti**.
6. Fare clic su **Installa**.

## <a name="activate-the-license-server"></a>Attivare il server licenze

1. Aprire Gestione licenze Desktop remoto: fare clic su **Start > Strumenti di amministrazione > Servizi Desktop remoto > Gestione licenze Desktop remoto**.
2. Fare clic con il pulsante destro del mouse sul server licenze, quindi su **Attiva server**.
3. Fare clic su **Avanti** nella pagina iniziale.
4. Per il metodo di connessione, selezionare **Connessione automatica (consigliata)** , quindi fare clic su **Avanti**.
5. Immettere le informazioni aziendali (il nome, il nome della società, l'area geografica), quindi fare clic su **Avanti**.
6. Facoltativamente, immettere altre informazioni aziendali (ad esempio, gli indirizzi di posta elettronica e della società), quindi fare clic su **Avanti**. 
7. Verificare che l'opzione **Avvia Installazione guidata licenze** non sia selezionata (le licenze verranno installate in un passaggio successivo), quindi fare clic su **Avanti**.

Il server licenze è ora pronto per iniziare a inviare e a gestire le licenze. 