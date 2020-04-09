---
title: WSUS e il sito del catalogo
description: 'Argomento Windows Server Update Service (WSUS): come importare gli hotfix in WSUS accedendo al sito del catalogo Microsoft Update'
ms.prod: windows-server
ms.technology: manage-wsus
ms.topic: article
ms.assetid: f19a8659-5a96-4fdd-a052-29e4547fe51a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 44c5ff9ffe793160b0d378a753c3f4c35e40f282
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80828324"
---
# <a name="wsus-and-the-catalog-site"></a>WSUS e il sito del catalogo

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Il sito del catalogo è il percorso Microsoft da cui è possibile importare gli hotfix e i driver hardware.

## <a name="the-microsoft-update-catalog-site"></a>Il sito del catalogo Microsoft Update
Per importare gli hotfix in WSUS, è necessario accedere al sito del catalogo Microsoft Update da un computer WSUS. Tutti i computer in cui è installata la console di amministrazione di WSUS, indipendentemente dal fatto che si tratti di un server WSUS, possono essere utilizzati per importare gli hotfix dal sito del catalogo. Per poter importare gli hotfix, è necessario effettuare l'accesso al computer come amministratore.

#### <a name="to-access-the-microsoft-update-catalog-site"></a>Per accedere al sito del catalogo Microsoft Update

1.  Nella console di amministrazione di WSUS selezionare il nodo del server principale o **gli aggiornamenti**, quindi nel riquadro **azioni** fare clic su **Importa aggiornamenti**. Nel sito Web Catalogo di Microsoft Update si apre una finestra del browser.

2.  Per accedere agli aggiornamenti in questo sito, è necessario installare il controllo activeX catalogo Microsoft Update.

3.  È possibile esplorare questo sito per gli hotfix e i driver hardware di Windows. Una volta trovati quelli desiderati, aggiungerli al carrello.

4.  Al termine dell'esplorazione, passare al cestino e fare clic su Importa per importare gli aggiornamenti. Per scaricare gli aggiornamenti senza importarli, deselezionare la casella di controllo **Importa direttamente in Windows Server Update Services** .

Gli aggiornamenti approvati importati dal sito del catalogo Microsoft Update vengono scaricati alla successiva sincronizzazione del server WSUS. Non vengono scaricati al momento dell'importazione dal sito del catalogo Microsoft Update.

Si noti che è necessario accedere al sito del catalogo di Microsoft Update anche se la console di WSUS per assicurarsi che gli aggiornamenti vengano importati in un formato compatibile con WSUS. Se si accede manualmente al sito Web del catalogo di Microsoft Update, gli eventuali aggiornamenti scaricati non vengono importati nel server WSUS, ma vengono invece scaricati come singoli *. File MSU. WSUS attualmente non dispone di un meccanismo supportato per l'importazione di file nel \*. Formato MSU.

Se si esegue la pulitura guidata del server, gli aggiornamenti importati dal catalogo Microsoft Update impostati come non approvati o rifiutati potrebbero essere rimossi dal server WSUS. Se vengono rimossi, possono essere reimportati dal catalogo Microsoft Update.

> [!NOTE]
> È possibile rimuovere gli aggiornamenti importati dal catalogo Microsoft Update impostati come non approvati o rifiutati, eseguendo la pulitura guidata del server WSUS. È possibile reimportare gli aggiornamenti rimossi in precedenza dai sistemi WSUS tramite il catalogo Microsoft Update.

## <a name="restricting-access-to-hotfixes"></a>Limitazione dell'accesso agli hotfix
Gli amministratori WSUS potrebbero considerare la restrizione dell'accesso agli hotfix scaricati dal sito del catalogo Microsoft Update. Per applicare questa restrizione, attenersi alla procedura seguente:

#### <a name="to-restrict-access-to-hotfixes"></a>Per limitare l'accesso agli hotfix

1.  Abilitare l'autenticazione di Windows sul contenuto IIS vroot.

    -   Avviare Gestione IIS.

    -   Passare al nodo contenuto nel sito Web di amministrazione di WSUS.

    -   Nel riquadro **Home del contenuto** fare doppio clic sull'opzione **Authentication** .

    -   Selezionare **autenticazione anonima** e fare clic su **Disabilita** nel riquadro **azioni** a destra.

    -   Selezionare **autenticazione di Windows** e fare clic su **Abilita** nel riquadro **azioni** a destra.

2.  Creare un gruppo di destinazione WSUS per i computer che richiedono l'hotfix e aggiungerli al gruppo. Per ulteriori informazioni su computer e gruppi, vedere [gestione dei computer client WSUS e gruppi di computer WSUS](managing-wsus-client-computers-and-wsus-computer-groups.md) in questa guida e sezione [3,3. Configurare i gruppi di computer WSUS](../deploy/2-configure-wsus.md#23-configure-wsus-computer-groups) del passaggio 3: configurare WSUS nella Guida alla distribuzione di WSUS.

3.  Scaricare i file per l'hotfix.

4.  Impostare le autorizzazioni di questi file in modo che solo gli account computer di tali computer siano in grado di leggerli. Sarà inoltre necessario consentire all'account servizio di rete di accedere in modo completo ai file.

5.  Approvare l'hotfix per il gruppo di destinazione WSUS creato nel passaggio 2.

## <a name="importing-updates-in-different-languages"></a>Importazione di aggiornamenti in lingue diverse
Il sito Web del catalogo Microsoft Update include aggiornamenti che supportano più lingue. È molto **importante** abbinare le lingue supportate dal server WSUS con le lingue supportate da questi aggiornamenti. Se il server WSUS non supporta tutte le lingue incluse nell'aggiornamento, l'aggiornamento non verrà distribuito nei computer client. Analogamente, se un aggiornamento che supporta più lingue è stato scaricato nel server WSUS ma non ancora distribuito ai computer client e un amministratore deseleziona una delle lingue incluse nell'aggiornamento, l'aggiornamento non verrà distribuito ai client.
