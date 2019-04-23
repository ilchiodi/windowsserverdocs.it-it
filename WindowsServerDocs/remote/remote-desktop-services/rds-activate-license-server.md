---
title: Attivare il server licenze di Servizi Desktop remoto
description: Installare e attivare il server licenze Desktop remoto
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eb24ddd2-0361-41fe-bd6b-c7c63427cb71
author: lizap
ms.author: elizapo
ms.date: 09/20/2016
manager: dongill
ms.openlocfilehash: d28ceac9cde0ee2d4c92867bdd90d5c463a8cd4c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888012"
---
# <a name="activate-the-remote-desktop-services-license-server"></a>Attivare il server licenze di Servizi Desktop remoto

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Remote Desktop Services licenza problemi licenze di accesso client (CAL) a utenti e dispositivi quando accedono a Host sessione Desktop remoto. È possibile attivare il server licenze tramite Gestione licenze Desktop remoto. 

## <a name="install-the-rd-licensing-role"></a>Installare il ruolo Servizio licenze Desktop remoto

1. Accedere al server di cui che si desidera utilizzare come server licenze utilizzando un account amministratore.
2. In Server Manager fare clic su **Riepilogo ruoli**, quindi fare clic su **Aggiungi ruoli**.
   Fare clic su **successivo** nella prima pagina della procedura guidata dei ruoli.
3. Selezionare **Servizi Desktop remoto**, quindi fare clic su **successiva**e quindi **Next** nella pagina Servizi Desktop remoto.
4. Selezionare **servizio licenze Desktop remoto**, quindi fare clic su **successivo**.
5. Configurare il dominio - selezionare **configurare un ambito di individuazione per questo server licenze**, fare clic su **questo dominio**, quindi fare clic su **Avanti**.
6. Fare clic su **Installa**.

## <a name="activate-the-license-server"></a>Attivare il server licenze

1. Aprire Gestione licenze Desktop remoto: fare clic su **Start > Strumenti di amministrazione > Servizi Desktop remoto > Gestione licenze Desktop remoto**.
2. Fare clic sul server licenze e quindi fare clic su **attiva Server**.
3. Fare clic su **successivo** nella pagina di benvenuto.
4. Per il metodo di connessione, selezionare **connessione automatica (consigliata)**, quindi fare clic su **successivo**.
5. Immettere le informazioni aziendali (il nome, il nome della società, l'area geografica) e quindi fare clic su **successivo**.
6. Facoltativamente, immettere le informazioni aziendali (ad esempio, gli indirizzi di posta elettronica e la società) e quindi fare clic su **successivo**. 
7. Verificare che l'opzione **Avvia installazione guidata licenze** non è selezionata (si installeranno le licenze in un passaggio successivo), quindi fare clic su **successivo**.

Il server licenze è ora pronto per iniziare il rilascio e gestione delle licenze. 