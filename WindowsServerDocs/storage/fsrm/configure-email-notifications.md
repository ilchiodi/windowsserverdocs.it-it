---
title: Configurare le notifiche tramite posta elettronica
description: Questo articolo descrive come configurare le notifiche tramite posta elettronica
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: d53be34d04edfac9f30b6e269833be74a6ebcf22
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820352"
---
# <a name="configure-e-mail-notifications"></a>Configurare le notifiche tramite posta elettronica

> Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Quando si creano quote e screening dei file, è possibile inviare notifiche tramite posta elettronica agli utenti quando stanno per raggiungere il limite di quota o quando tentano di salvare un file bloccato. Quando si generano i rapporti di archiviazione, è possibile inviare i rapporti a destinatari specifici tramite posta elettronica. Se si vuole avvisare regolarmente alcuni amministratori in merito a eventi di quota e screening dei file o inviare rapporti di archiviazione, è possibile configurare uno o più destinatari predefiniti.

Per inviare queste notifiche e i rapporti di archiviazione, è necessario specificare il server SMTP da usare per inoltrare i messaggi di posta elettronica.

## <a name="to-configure-e-mail-options"></a>Per configurare le opzioni di posta elettronica

1.  Nell'albero della console, fare clic con il pulsante destro del mouse su **Gestione risorse file server**, quindi scegliere **Configura opzioni**. Verrà visualizzata la finestra di dialogo **Gestione risorse file server**.

2.  Nella scheda **Notifiche posta elettronica**, in **Nome server SMTP o indirizzo IP**, digitare il nome host o l'indirizzo IP del server SMTP che inoltrerà le notifiche tramite posta elettronica e i rapporti di archiviazione.

3.  Se si vuole avvisare regolarmente alcuni amministratori in merito a eventi di quota o screening dei file o rapporti di archiviazione tramite posta elettronica, in **Destinatari amministratori predefiniti** digitare l'indirizzo di posta elettronica di ognuno.

    Usare il formato *account@domain*. Utilizzare il segno di punto e virgola per separare più account.

4.  Per specificare un diverso indirizzo "Da" per le notifiche tramite posta elettronica e i rapporti di archiviazione inviati da Gestione risorse file server, in **"Indirizzo di posta elettronica "Da" predefinito**, digitare l'indirizzo di posta elettronica che si desidera venga visualizzato nel messaggio.

5.  Per testare le impostazioni, fare clic su **Invia messaggio di prova**.

6.  Fare clic su **OK**.


## <a name="see-also"></a>Vedere anche

-   [Opzioni di gestione risorse di impostazione File Server](setting-file-server-resource-manager-options.md)