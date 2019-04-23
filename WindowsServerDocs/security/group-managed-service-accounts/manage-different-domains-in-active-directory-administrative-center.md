---
title: Gestire domini diversi nel centro di amministrazione di Active Directory
ms.prod: windows-server-threshold
description: Sicurezza di Windows Server
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.assetid: 166351c3-4076-48be-aa8f-797adf1e9d68
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 5f253bd4952d8a347e97eafdb38d86fa98024b8d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839942"
---
# <a name="manage-different-domains-in-active-directory-administrative-center"></a>Gestire domini diversi nel centro di amministrazione di Active Directory

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

  Quando si apre amministrativi di Active Directory, il dominio che attualmente connessi al computer \(il dominio locale\) viene visualizzato nel riquadro di spostamento di centro di amministrazione di Active Directory \(riquadroasinistra\). In base ai diritti relativi all'insieme attivo di credenziali di accesso, è possibile visualizzare o gestire gli oggetti di Active Directory in tale dominio locale.

 È anche possibile usare lo stesso insieme di credenziali di accesso e la stessa istanza del centro di amministrazione di Active Directory per visualizzare o gestire oggetti Active Directory in qualsiasi altro dominio nella stessa foresta o in un dominio in un'altra foresta che abbia una relazione di trust esistente con locale dominio. Entrambi uno\-relazioni di trust bidirezionali e due\-relazioni di trust bidirezionali sono supportati.

> [!NOTE]
>  Se è una\-di trust bidirezionale tra il dominio e il dominio B tramite il quale gli utenti nel dominio A possono accedere alle risorse nel dominio B ma gli utenti nel dominio B non possono accedere alle risorse del dominio, se si esegue Centro di amministrazione di Active Directory nel computer dove il dominio è il dominio locale, è possibile connettersi al dominio B con il set di credenziali di accesso e nella stessa istanza del centro di amministrazione di Active Directory corrente. Ma se si esegue il centro di amministrazione di Active Directory nel computer in cui il dominio B costituisce il dominio locale, è possibile connettersi al dominio con lo stesso insieme di credenziali nella stessa istanza del centro di amministrazione di Active Directory.

 Per eseguire questa procedura, non è richiesta l'appartenenza ad alcun gruppo.

### <a name="windows-server-2012-to-manage-a-foreign-domain-in-the-selected-instance-of-active-directory-administrative-center-using-the-current-set-of-logon-credentials"></a>Windows Server 2012: Per gestire un dominio esterno nell'istanza selezionata del centro di amministrazione di Active Directory utilizzando l'insieme attivo di credenziali di accesso

1.  Per aprire Centro di amministrazione di Active Directory, in **Server Manager**, fare clic su **Tools**, quindi fare clic su **centro di amministrazione di Active Directory**.

    > [!NOTE]
    >  Un altro modo per aprire Centro di amministrazione di Active Directory è necessario fare clic su **avviare**, quindi digitare **dsac.exe**.

2.  Per aprire **Aggiungi nodi di spostamento**, fare clic su **Gestisci**, quindi fare clic su **Aggiungi nodi di spostamento** come illustrato nella figura seguente.

     ![Screenshot che mostra * * Aggiungi navigazione nodi * * dell'interfaccia utente](media/ADDS_ADACAddNavNode.gif)

3.  Nelle **Aggiungi nodi di spostamento**, fare clic su **Connetti ad altri domini** come illustrato nella figura seguente.

     ![Screenshot che mostra * * Aggiungi navigazione nodi * * dell'interfaccia utente](media/ADDS_ADACConnectToDomain.gif)

4.  Nella **connettersi a**, digitare il nome del dominio esterno che si desidera gestire \(ad esempio, **contoso.com**\), quindi fare clic su **OK**.

5.  Quando si è connessi correttamente al dominio esterno, esplorare le colonne nel **Aggiungi nodi di spostamento** finestra, selezionare il contenitore o contenitori da aggiungere al riquadro di spostamento di centro di amministrazione di Active Directory, e quindi fare clic su **OK**.

### <a name="windows-server-2008-r2-to-manage-a-foreign-domain-in-the-selected-instance-of-active-directory-administrative-center-using-the-current-set-of-logon-credentials"></a>Windows Server 2008 R2: Per gestire un dominio esterno nell'istanza selezionata del centro di amministrazione di Active Directory utilizzando l'insieme attivo di credenziali di accesso

1.  Per aprire Centro di amministrazione di Active Directory, fare clic su **avviare**, fare clic su **strumenti di amministrazione**, quindi fare clic su **centro di amministrazione di Active Directory**.

    > [!NOTE]
    >  Un altro modo per aprire Centro di amministrazione di Active Directory è necessario fare clic su **avviare**, fare clic su **eseguito**, quindi digitare **dsac.exe**.

2.  Per aprire **Aggiungi nodi di spostamento**, nella parte superiore della finestra di centro di amministrazione di Active Directory, fare clic su **Aggiungi nodi di spostamento** come illustrato nella figura seguente.

     ![Screenshot che mostra * * Aggiungi navigazione nodi * * dell'interfaccia utente](media/click_add_nav_nodes.gif)

    > [!NOTE]
    >  Un altro modo per aprire **Aggiungi nodi di spostamento** è a destra\-fare clic in qualsiasi punto nello spazio vuoto nel riquadro di spostamento di centro di amministrazione di Active Directory e quindi fare clic su **Aggiungi nodi di spostamento**.

3.  Nelle **Aggiungi nodi di spostamento**, fare clic su **Connetti ad altri domini** come illustrato nella figura seguente.

     ![Screenshot che mostra * * Aggiungi navigazione nodi * * * * Connetti a altri domini * * dell'interfaccia utente](media/add_nav_nodes.gif)

4.  Nella **connettersi a**, digitare il nome del dominio esterno che si desidera gestire \(ad esempio, **contoso.com**\), quindi fare clic su **OK**.

5.  Quando si è connessi correttamente al dominio esterno, esplorare le colonne nel **Aggiungi nodi di spostamento** finestra, selezionare il contenitore o contenitori da aggiungere al riquadro di spostamento di centro di amministrazione di Active Directory, e quindi fare clic su **OK**.

 Per altre informazioni sulla personalizzazione del riquadro di spostamento di centro di amministrazione di Active Directory, vedere [personalizzare Active Directory amministrativi centro riquadro di spostamento](customize-the-active-directory-administrative-center-navigation-pane.md).

 È anche possibile aprire Centro di amministrazione di Active Directory utilizzando un insieme di credenziali di accesso diverso dall'insieme attivo di credenziali di accesso. Il comando nella procedura seguente può essere utile se si è connessi al computer che esegue il centro di amministrazione di Active Directory con le credenziali dell'utente normale, ma si vuole usare il centro di amministrazione di Active Directory nel computer per la gestione di dominio locale come amministratore. \(Questo comando può anche essere utile se si desidera utilizzare il centro di amministrazione di Active Directory per gestire in remoto un dominio esterno che è diverso dal dominio locale con un insieme di credenziali diverso dall'insieme attivo di credenziali di accesso. Tuttavia, il dominio esterno deve avere una relazione di trust stabilita con il dominio locale.\)

 Per eseguire questa procedura, non è richiesta l'appartenenza ad alcun gruppo.

### <a name="to-manage-a-domain-using-logon-credentials-that-are-different-from-the-current-set-of-logon-credentials"></a>Per gestire un dominio utilizzando credenziali di accesso diverse dall'insieme attivo di credenziali di accesso

1.  Per aprire Centro di amministrazione di Active Directory, un prompt dei comandi, digitare il comando seguente e quindi premere INVIO:

     `runas /user:<domain\user> dsac`

     In cui `<domain\user>` è un insieme di credenziali che si desidera aprire Centro di amministrazione di Active Directory con e `dsac` è il nome di file eseguibile del centro di amministrazione di Active Directory \(Dsac.exe\).

     Digitare ad esempio il comando seguente e quindi premere INVIO:

     `runas /user:contoso\administrator dsac`

2.  Quando il centro di amministrazione di Active Directory è aperto, esplorare il riquadro di spostamento per visualizzare o gestire il dominio di Active Directory.

  

