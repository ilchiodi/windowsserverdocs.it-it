---
title: WSUS e il sito del catalogo
description: Argomento di Windows Server Update Service (WSUS) - procedura importare hotfix in Windows Server Update Services accedendo al sito del catalogo Microsoft Update
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f19a8659-5a96-4fdd-a052-29e4547fe51a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7cced9a930b63baa79b4addb429c562c38d6da01
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843882"
---
# <a name="wsus-and-the-catalog-site"></a>WSUS e il sito del catalogo

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Il sito del catalogo è il percorso di Microsoft da cui è possibile importare i driver hardware e aggiornamenti rapidi.

## <a name="the-microsoft-update-catalog-site"></a>Il sito del catalogo Microsoft Update
Per importare hotfix in WSUS, è necessario accedere il sito del catalogo Microsoft Update da un computer Windows Server Update Services. Qualsiasi computer con la console di amministrazione di WSUS installata, se si tratta di un server WSUS, è utilizzabile per importare hotfix dal sito del catalogo. Per poter importare gli hotfix, è necessario effettuare l'accesso al computer come amministratore.

#### <a name="to-access-the-microsoft-update-catalog-site"></a>Per accedere al sito del catalogo di Microsoft Update

1.  Nella console di amministrazione di WSUS, selezionare il nodo server principale o **aggiornamenti**e il **azioni** riquadro fare clic su **importare gli aggiornamenti**. Nel sito Web Catalogo di Microsoft Update si apre una finestra del browser.

2.  Per poter accedere gli aggiornamenti in questo sito, è necessario installare il controllo activeX Microsoft Update Catalog.

3.  È possibile esplorare questo sito per i driver hardware e gli hotfix di Windows. Quando è stata trovata quelli desiderati, aggiungerli al carrello selezioni.

4.  Quando completata l'esplorazione, passare al carrello e fare clic su Importa per importare gli aggiornamenti. Per scaricare gli aggiornamenti senza importarli, deselezionare il **importare direttamente in Windows Server Update Services** casella di controllo.

Gli aggiornamenti approvati importati dal sito Microsoft Update Catalog vengono scaricati la volta successiva che il server WSUS Sincronizza. Non vengono scaricati al momento dell'importazione dal sito Microsoft Update Catalog.

Si noti che deve accedere il sito del catalogo Microsoft Update tramite la console di WSUS per garantire che gli aggiornamenti vengono importati in un formato compatibile con Windows Server Update Services. Se si accede manualmente il sito Web Microsoft Update Catalog, tutti gli aggiornamenti scaricati non vengono importati nel server WSUS, ma vengono invece scaricati come singole *. File MSU. Windows Server Update Services non dispone attualmente di un meccanismo supportato per l'importazione di file nei \*. Formato di pacchetto autonomo Microsoft Update.

Se si esegue una pulizia del Server la procedura guidata, gli aggiornamenti importati da Microsoft Update Catalog impostati come non approvato o rifiutato potrebbero essere rimossi dal server WSUS. Se vengono rimossi, possono essere importati nuovamente da Microsoft Update Catalog.

> [!NOTE]
> È possibile rimuovere gli aggiornamenti che vengono importati da Microsoft Update Catalog impostati come non approvato o rifiutato, eseguendo la procedura guidata di pulizia Server WSUS. È possibile reimportare gli aggiornamenti che sono stati rimossi in precedenza dai sistemi Windows Server Update Services tramite Microsoft Update Catalog.

## <a name="restricting-access-to-hotfixes"></a>Limitazione dell'accesso per gli aggiornamenti rapidi
Gli amministratori WSUS potrebbero prendere in considerazione la limitazione dell'accesso per gli aggiornamenti rapidi scaricati dal sito Microsoft Update Catalog. Per rendere questa restrizione attenersi alla procedura seguente:

#### <a name="to-restrict-access-to-hotfixes"></a>Per limitare l'accesso per gli aggiornamenti rapidi

1.  Abilitare l'autenticazione di Windows nella radice virtuale IIS contenuto.

    -   Avviare Gestione IIS.

    -   Passare al nodo contenuto nel sito web di amministrazione di WSUS.

    -   Nel **contenuto home page** riquadro, fare doppio clic nel **autenticazione** opzione.

    -   Selezionare **l'autenticazione anonima** e fare clic su **disabilitare** nel **azioni** riquadro a destra.

    -   Selezionare **autenticazione di Windows** e fare clic su **consentono** nel **azioni** riquadro a destra.

2.  Creare un gruppo di destinazione WSUS per il computer che richiedono l'aggiornamento rapido e aggiungerli al gruppo. Per altre informazioni sui computer e gruppi, vedere [computer di gestione dei Client WSUS e gruppi di computer WSUS](managing-wsus-client-computers-and-wsus-computer-groups.md) in questa Guida e sezione [3.3. Configurare gruppi di computer WSUS](../deploy/2-configure-wsus.md#BKMK_ConfigcomputerGroups) del passaggio 3: Configurazione di WSUS, nella Guida alla distribuzione di WSUS.

3.  Scaricare i file per l'aggiornamento rapido.

4.  Impostare le autorizzazioni di questi file in modo che possano essere letti solo gli account computer di tali macchine. È necessario anche consentire il servizio di rete account l'accesso completo ai file.

5.  Approvare l'aggiornamento rapido per il gruppo di destinazione WSUS creata nel passaggio 2.

## <a name="importing-updates-in-different-languages"></a>Importazione di aggiornamenti in lingue diverse
Il sito Web Microsoft Update Catalog include gli aggiornamenti che supportano più lingue. È molto **importanti** corrispondere le lingue supportate dal server WSUS con le lingue supportate da questi aggiornamenti. Se il server WSUS non supporta tutte le lingue incluse nell'aggiornamento, l'aggiornamento non verrà distribuito ai computer client. Analogamente, se un aggiornamento che supporta più lingue è stato scaricato il server WSUS, ma non ancora distribuito ai computer client e un amministratore deseleziona uno dei linguaggi inclusi l'aggiornamento, l'aggiornamento non verrà distribuito ai client.
