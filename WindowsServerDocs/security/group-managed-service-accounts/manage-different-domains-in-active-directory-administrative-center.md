---
title: Gestire domini diversi in Centro di amministrazione di Active Directory
ms.prod: windows-server
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
ms.openlocfilehash: 71edf6bb38cc665fe5c780ce986d0c0b8807d6ab
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386935"
---
# <a name="manage-different-domains-in-active-directory-administrative-center"></a>Gestire domini diversi in Centro di amministrazione di Active Directory

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

  Quando si apre Active Directory amministratore, il dominio al quale si è attualmente connessi in questo computer \(il\) di dominio locale viene visualizzato nel riquadro di spostamento Centro di amministrazione di Active Directory \(riquadro sinistro\). A seconda dei diritti del set corrente di credenziali di accesso, è possibile visualizzare o gestire gli oggetti Active Directory in questo dominio locale.

 È inoltre possibile utilizzare lo stesso set di credenziali di accesso e la stessa istanza di Centro di amministrazione di Active Directory per visualizzare o gestire Active Directory oggetti in qualsiasi altro dominio nella stessa foresta o un dominio in un'altra foresta con una relazione di trust stabilita con il locale dominio. Sono supportati entrambi i trust\-Way e due trust\-Way.

> [!NOTE]
>  Se esiste\-una relazione di trust tra il dominio A e il dominio B tramite il quale gli utenti del dominio A possono accedere alle risorse nel dominio B, ma gli utenti del dominio B non possono accedere alle risorse nel dominio A, se si esegue Centro di amministrazione di Active Directory nel computer in cui il dominio A è il dominio locale, è possibile connettersi al dominio B con il set corrente di credenziali di accesso e nella stessa istanza Centro di amministrazione di Active Directory di Tuttavia, se si esegue Centro di amministrazione di Active Directory nel computer in cui il dominio B è il dominio locale, non è possibile connettersi al dominio A con lo stesso set di credenziali nella stessa istanza del Centro di amministrazione di Active Directory.

 Per eseguire questa procedura, non è richiesta l'appartenenza ad alcun gruppo.

### <a name="windows-server-2012-to-manage-a-foreign-domain-in-the-selected-instance-of-active-directory-administrative-center-using-the-current-set-of-logon-credentials"></a>Windows Server 2012: per gestire un dominio esterno nell'istanza selezionata di Centro di amministrazione di Active Directory utilizzando il set corrente di credenziali di accesso

1.  Per aprire Centro di amministrazione di Active Directory, in **Server Manager**fare clic su **strumenti**, quindi fare clic su **centro di amministrazione di Active Directory**.

    > [!NOTE]
    >  Un altro modo per aprire Centro di amministrazione di Active Directory consiste nel fare clic sul pulsante **Start**, quindi digitare **dsac. exe**.

2.  Per aprire **Aggiungi nodi di spostamento**, fare clic su **Gestisci**, quindi su **Aggiungi nodi di spostamento** , come illustrato nella figura seguente.

     ![Screenshot che mostra * * Aggiungi nodi di spostamento * * interfaccia utente](media/ADDS_ADACAddNavNode.gif)

3.  In **Aggiungi nodi di spostamento**fare clic su **Connetti ad altri domini** , come illustrato nella figura seguente.

     ![Screenshot che mostra * * Aggiungi nodi di spostamento * * interfaccia utente](media/ADDS_ADACConnectToDomain.gif)

4.  In **Connetti a**Digitare il nome del dominio esterno che si desidera gestire \(ad esempio, **contoso.com**\), quindi fare clic su **OK**.

5.  Una volta stabilita la connessione al dominio esterno, esplorare le colonne nella finestra **Aggiungi nodi di spostamento** , selezionare il contenitore o i contenitori da aggiungere al riquadro di spostamento centro di amministrazione di Active Directory, quindi fare clic su **OK**.

### <a name="windows-server-2008-r2-to-manage-a-foreign-domain-in-the-selected-instance-of-active-directory-administrative-center-using-the-current-set-of-logon-credentials"></a>Windows Server 2008 R2: per gestire un dominio esterno nell'istanza selezionata di Centro di amministrazione di Active Directory utilizzando il set corrente di credenziali di accesso

1. Per aprire Centro di amministrazione di Active Directory, fare clic sul pulsante **Start**, scegliere **strumenti di amministrazione**, quindi fare clic su **centro di amministrazione di Active Directory**.

   > [!NOTE]
   >  Un altro modo per aprire Centro di amministrazione di Active Directory consiste nel fare clic sul pulsante **Start**, scegliere **Esegui**, quindi digitare **dsac. exe**.

2. Per aprire **Aggiungi nodi di spostamento**, nella parte superiore della finestra di centro di amministrazione di Active Directory, fare clic su **Aggiungi nodi di spostamento** , come illustrato nella figura seguente.

    ![Screenshot che mostra * * Aggiungi nodi di spostamento * * interfaccia utente](media/click_add_nav_nodes.gif)

   > [!NOTE]
   >  Per aprire **Aggiungi nodi di spostamento** è possibile fare clic con il pulsante destro del mouse\-fare clic in un punto qualsiasi dello spazio vuoto nel riquadro di spostamento centro di amministrazione di Active Directory, quindi fare clic su **Aggiungi nodi di spostamento**.

3. In **Aggiungi nodi di spostamento**fare clic su **Connetti ad altri domini** , come illustrato nella figura seguente.

    ![Screenshot che mostra * * Aggiungi nodi di spostamento * * * * Connetti ad altri domini * * interfaccia utente](media/add_nav_nodes.gif)

4. In **Connetti a**Digitare il nome del dominio esterno che si desidera gestire \(ad esempio, **contoso.com**\), quindi fare clic su **OK**.

5. Una volta stabilita la connessione al dominio esterno, esplorare le colonne nella finestra **Aggiungi nodi di spostamento** , selezionare il contenitore o i contenitori da aggiungere al riquadro di spostamento centro di amministrazione di Active Directory, quindi fare clic su **OK**.

   Per ulteriori informazioni sulla personalizzazione del riquadro di spostamento Centro di amministrazione di Active Directory, vedere [personalizzare il centro di amministrazione di Active Directory riquadro di spostamento](customize-the-active-directory-administrative-center-navigation-pane.md).

   È inoltre possibile aprire Centro di amministrazione di Active Directory utilizzando un set di credenziali di accesso diverso dal set corrente di credenziali di accesso. Il comando nella procedura seguente può essere utile se si è connessi al computer in cui è in esecuzione Centro di amministrazione di Active Directory con credenziali utente normali, ma si desidera utilizzare Centro di amministrazione di Active Directory sul computer per gestire il dominio locale come amministratore. \(questo comando può essere utile anche se si vuole usare Centro di amministrazione di Active Directory per gestire in remoto un dominio esterno diverso dal dominio locale con un set di credenziali diverso dal set corrente di credenziali di accesso. Tuttavia, il dominio esterno deve avere una relazione di trust stabilita con il dominio locale.\)

   Per eseguire questa procedura, non è richiesta l'appartenenza ad alcun gruppo.

### <a name="to-manage-a-domain-using-logon-credentials-that-are-different-from-the-current-set-of-logon-credentials"></a>Per gestire un dominio utilizzando credenziali di accesso diverse dall'insieme attivo di credenziali di accesso

1.  Per aprire Centro di amministrazione di Active Directory, al prompt dei comandi digitare il comando seguente e quindi premere INVIO:

     `runas /user:<domain\user> dsac`

     Dove `<domain\user>` è il set di credenziali che si desidera aprire Centro di amministrazione di Active Directory con e `dsac` è il nome del file eseguibile Centro di amministrazione di Active Directory \(dsac. exe\).

     Digitare ad esempio il comando seguente e quindi premere INVIO:

     `runas /user:contoso\administrator dsac`

2.  Quando Centro di amministrazione di Active Directory è aperto, sfogliare il riquadro di spostamento per visualizzare o gestire il dominio di Active Directory.

  

