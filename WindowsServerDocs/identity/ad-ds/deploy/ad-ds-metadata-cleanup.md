---
title: La pulizia dei metadati del server Active Directory Domain Services
description: Usare gli strumenti predefiniti per pulire i metadati da controller di dominio rimossi
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 11/14/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: fbb6e720c9289c608d71d3c36695ba623a9df5f6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818082"
---
# <a name="clean-up-active-directory-domain-controller-server-metadata"></a>La pulizia dei metadati del server Controller di dominio Active Directory

Si applica a: Windows Server

Pulizia dei metadati è una procedura necessaria dopo una rimozione forzata di servizi di dominio Active Directory (AD DS). Eseguire la pulizia dei metadati in un controller di dominio nel dominio del controller di dominio che rimossa forzatamente. Pulizia dei metadati rimuove i dati da Active Directory Domain Services che identifica un controller di dominio per il sistema di replica. Pulizia dei metadati anche rimuove le connessioni del servizio Replica File (FRS) e replica del File System distribuito (DFS) e tenta di trasferire o assegnare i ruoli di master (noto anche come FSMO FSMO, Flexible) di operazioni che il dominio ritirato controller contiene.

Sono disponibili tre opzioni per la pulizia dei metadati del server:

- Pulizia dei metadati del server con gli strumenti di interfaccia utente grafica
- La pulizia dei metadati del server tramite la riga di comando
- Pulizia dei metadati del server tramite uno script

> [!NOTE]
> Se si riceve un errore "Accesso negato" quando si usa uno di questi metodi per eseguire la pulizia dei metadati, assicurarsi che l'oggetto computer e l'oggetto impostazioni NTDS per il controller di dominio non sono protetti da eliminazioni accidentali. Per verificare il pulsante destro del mouse dell'oggetto computer o l'oggetto impostazioni NTDS, fare clic su **delle proprietà**, fare clic su **oggetto**e deselezionare il **Proteggi oggetto da eliminazioni accidentali** casella di controllo. In Active Directory Users and Computers, il **oggetto** verrà visualizzata la scheda di un oggetto se si fa clic **View** e quindi fare clic su **Advanced Features**.

## <a name="clean-up-server-metadata-using-gui-tools"></a>La pulizia dei metadati del server utilizzando gli strumenti di interfaccia utente grafica

Quando si usano strumenti di amministrazione remota Server (RSAT) o di Active Directory Users e computer console (DSA. msc) inclusa in Windows Server per eliminare un account computer controller di dominio nell'unità organizzativa (OU), i controller di dominio di la pulizia dei metadati del server viene eseguita automaticamente. Prima di Windows Server 2008, era necessario eseguire una procedura di pulizia di metadati separati.

È anche possibile usare i siti di Active Directory e servizi di console (Dssite. msc) per eliminare l'account computer del controller di dominio, che anche completa automaticamente la pulizia dei metadati. Tuttavia, servizi e siti di Active Directory rimuove i metadati automaticamente solo quando si elimina innanzitutto l'oggetto impostazioni NTDS sotto l'account del computer in Dssite. msc.

Fino a quando si utilizza il Windows Server 2008 o versioni più recenti di amministrazione remota del server di DSA. msc o Dssite. msc, è possibile pulire i metadati automaticamente per i controller di dominio che eseguono versioni precedenti dei sistemi operativi Windows.

L'appartenenza a **Domain Admins**, o equivalente è il requisito minimo necessario per completare queste procedure.

## <a name="clean-up-server-metadata-using-activedirectory-users-and-computers"></a>La pulizia dei metadati del server usando Active Directory Users and Computers

1. Aprire **Utenti e computer di Active Directory**.
2. Se si sono identificati i partner di replica in preparazione per questa procedura e se non si è connessi a un partner di replica del controller di dominio rimossi i cui metadati da eliminare, fare doppio clic su **Active Directory Users e I computer** nodo e quindi fare clic su **cambia Controller di dominio**. Fare clic sul nome del controller di dominio da cui si desidera rimuovere i metadati e quindi fare clic su **OK**.
3. Espandere il dominio del controller di dominio che è stato rimosso forzatamente e quindi fare clic su **controller di dominio**.
4. Nel riquadro dei dettagli, fare doppio clic su oggetto computer del controller di dominio i cui metadati da pulire e quindi fare clic su **Elimina**.
5. Nel **Active Directory Domain Services** finestra di dialogo confermare il nome del controller di dominio che si desidera eliminare viene visualizzata e fare clic su **Yes** per confermare l'eliminazione di oggetti computer.
6. Nel **Controller di dominio di eliminazione** finestra di dialogo **questo Controller di dominio è definitivamente offline e non può non è più essere abbassata di livello con l'Active Directory Domain Services installazione guidata (DCPROMO)**, quindi fare clic su **eliminare**.
7. Se il controller di dominio è un server di catalogo globale, nel **eliminare Controller di dominio** finestra di dialogo, fare clic su **Sì** per continuare con l'eliminazione.
8. Se il controller di dominio contiene una o più operazioni attualmente i ruoli di master, fare clic su **OK** per spostare uno o più ruoli di controller di dominio che viene visualizzato. Non è possibile modificare questo controller di dominio. Se si desidera spostare il ruolo in un controller di dominio diverso, è necessario spostare il ruolo dopo aver completato la procedura di pulizia dei metadati di server.

## <a name="clean-up-server-metadata-using-activedirectory-sites-and-services"></a>La pulizia dei metadati del server usando Active Directory Sites and Services

1. Aprire Siti e servizi di Active Directory.
2. Se si sono identificati i partner di replica in preparazione per questa procedura e se non si è connessi a un partner di replica del controller di dominio rimossi i cui metadati da eliminare, fare doppio clic su **servizi e siti di Active Directory** , quindi fare clic su **cambia Controller di dominio**. Fare clic sul nome del controller di dominio da cui si desidera rimuovere i metadati e quindi fare clic su **OK**.
3. Espandere il sito del controller di dominio che è stato rimosso forzatamente, espandere **i server**espandere il nome del controller di dominio, fare doppio clic su oggetto impostazioni NTDS e quindi fare clic su **eliminare**.
4. Nel **Active Directory Sites and Services** finestra di dialogo, fare clic su **Sì** per confermare l'eliminazione di impostazioni NTDS.
5. Nel **Controller di dominio di eliminazione** finestra di dialogo **questo Controller di dominio è definitivamente offline e non può non è più essere abbassata di livello con l'Active Directory Domain Services installazione guidata (DCPROMO)**, quindi fare clic su **eliminare**.
6. Se il controller di dominio è un server di catalogo globale, nel **eliminare Controller di dominio** finestra di dialogo, fare clic su **Sì** per continuare con l'eliminazione.
7. Se il controller di dominio contiene una o più operazioni attualmente i ruoli di master, fare clic su **OK** per spostare uno o più ruoli di controller di dominio che viene visualizzato.
8. Fare doppio clic su controller di dominio che è stato rimosso forzatamente e quindi fare clic su Elimina.
9. Nel **Active Directory Domain Services** finestra di dialogo, fare clic su **Yes** per confermare l'eliminazione di controller di dominio.

## <a name="clean-up-server-metadata-using-the-command-line"></a>La pulizia dei metadati del server tramite la riga di comando

In alternativa, è possibile pulire i metadati utilizzando Ntdsutil.exe, uno strumento da riga di comando che viene installato automaticamente su tutti i controller di dominio e i server con Active Directory Lightweight Directory Services (AD LDS) installato. Ntdsutil.exe disponibile anche su computer che dispongono di amministrazione remota del server installato.

## <a name="to-clean-up-server-metadata-by-using-ntdsutil"></a>Per pulire i metadati del server utilizzando lo strumento Ntdsutil

1. Aprire un prompt dei comandi come amministratore: A tale scopo, fare clic sul pulsante **Start**, fare clic con il pulsante destro del mouse su **Prompt dei comandi** e quindi scegliere **Esegui come amministratore**. Se il **User Account Control** verrà visualizzata la finestra di dialogo, fornire le credenziali di amministratore dell'organizzazione se necessario e quindi fare clic su **continua**.
2. Al prompt dei comandi digitare il comando seguente e quindi premere INVIO:

   `ntdsutil`

3. Al prompt `ntdsutil:` digitare il comando seguente e quindi premere INVIO:

   `metadata cleanup`

4. Al prompt `metadata cleanup:` digitare il comando seguente e quindi premere INVIO:

   `remove selected server <ServerName>`

5. Nelle **Server rimuovere al finestra di dialogo di configurazione**, esaminare le informazioni e un avviso e quindi fare clic su **Yes** per rimuovere l'oggetto server e i metadati.

   A questo punto, verrà confermata che il controller di dominio è stato rimosso correttamente. Se si riceve un messaggio di errore che indica che l'oggetto non può essere trovato, il controller di dominio sia stato rimosso in precedenza.

6. Nel `metadata cleanup:` e `ntdsutil:` prompt, digitare `quit`, quindi premere INVIO.

7. Per confermare la rimozione del controller di dominio:

   Aprire utenti e computer Active Directory. Nel dominio del controller di dominio rimossi, fare clic su **controller di dominio**. Nel riquadro dei dettagli, un oggetto per il controller di dominio che sono stati rimossi non deve essere visualizzati.

   Aprire servizi e siti di Active Directory. Passare il **server** contenitore e verificare che l'oggetto server per il controller di dominio che sono stati rimossi non contiene un oggetto impostazioni NTDS. Se nessun oggetto figlio viene visualizzato sotto l'oggetto server, è possibile eliminare l'oggetto server. Se viene visualizzato un oggetto figlio, non eliminare l'oggetto server perché l'oggetto è utilizzato da un'altra applicazione.

## <a name="see-also"></a>Vedere anche

* [Abbassamento di livello i controller di dominio](Demoting-Domain-Controllers-and-Domains--Level-200-.md)
* [Riferimento ai comandi di Ntdsutil](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753343(v=ws.10))
