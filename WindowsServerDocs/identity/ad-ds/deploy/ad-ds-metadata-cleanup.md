---
title: Pulire i metadati del server Servizi di dominio Active Directory
description: Usare gli strumenti predefiniti per pulire i metadati dai controller di dominio rimossi
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 11/14/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 622ff33437a3aef14a185c9a4157dba68db0a2ee
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80824786"
---
# <a name="clean-up-active-directory-domain-controller-server-metadata"></a>Pulire i metadati del server Dominio di Active Directory controller

Si applica a: Windows Server

La pulizia dei metadati è una procedura obbligatoria dopo una rimozione forzata di Active Directory Domain Services (AD DS). È possibile eseguire la pulizia dei metadati in un controller di dominio nel dominio del controller di dominio che è stato rimosso forzatamente. La pulizia dei metadati rimuove i dati da servizi di dominio Active Directory che identificano un controller di dominio per il sistema di replica. Con la pulizia dei metadati vengono inoltre rimosse le connessioni di replica del servizio Replica file (FRS) e file system distribuito (DFS) e viene effettuato il tentativo di trasferire o requisire i ruoli di master operazioni (noti anche come FSMO, Flexible Single Master Operation) che il controller di dominio ritirato include.

Sono disponibili tre opzioni per la pulizia dei metadati del server:

- Pulire i metadati del server usando gli strumenti dell'interfaccia utente grafica
- Pulire i metadati del server usando la riga di comando
- Pulire i metadati del server usando uno script

> [!NOTE]
> Se si riceve un errore di accesso negato quando si usa uno di questi metodi per eseguire la pulizia dei metadati, assicurarsi che l'oggetto computer e l'oggetto Impostazioni NTDS per il controller di dominio non siano protetti da eliminazioni accidentali. Per verificare questa operazione, fare clic con il pulsante destro del mouse sull'oggetto computer o sull'oggetto Impostazioni NTDS, scegliere **Proprietà**, fare clic su **oggetto**e deselezionare la casella **di controllo Proteggi oggetto da eliminazioni accidentali** . In Active Directory utenti e computer la scheda **oggetto** di un oggetto viene visualizzata se si fa clic su **Visualizza** e quindi su **funzionalità avanzate**.

## <a name="clean-up-server-metadata-using-gui-tools"></a>Pulire i metadati del server usando gli strumenti dell'interfaccia utente grafica

Quando si usa Strumenti di amministrazione remota del server (strumenti di amministrazione remota) o la console di utenti e computer Active Directory (DSA. msc) inclusa in Windows Server per eliminare un account computer del controller di dominio dall'unità organizzativa dei controller di dominio, la pulizia dei metadati del server viene eseguita automaticamente. Prima di Windows Server 2008, era necessario eseguire una procedura di pulizia dei metadati separata.

È inoltre possibile utilizzare la console dei siti e dei servizi Active Directory (Dssite. msc) per eliminare un account computer di un controller di dominio, che completa anche la pulizia dei metadati automaticamente. Tuttavia, Active Directory siti e servizi rimuove automaticamente i metadati solo quando si elimina per la prima volta l'oggetto Impostazioni NTDS sotto l'account computer in Dssite. msc.

Se si usano Windows Server 2008 o versioni più recenti di strumenti di amministrazione di Windows (DSA. msc) o Dssite. msc, è possibile pulire automaticamente i metadati per i controller di dominio che eseguono versioni precedenti dei sistemi operativi Windows.

L'appartenenza a **Domain Admins**, o equivalente è il requisito minimo necessario per completare queste procedure.

## <a name="clean-up-server-metadata-using-activedirectory-users-and-computers"></a>Pulire i metadati del server usando Active Directory utenti e computer

1. Aprire **Utenti e computer di Active Directory**.
2. Se sono stati identificati i partner di replica in preparazione per questa procedura e se non si è connessi a un partner di replica del controller di dominio rimosso di cui si desidera pulire i metadati, fare clic con il pulsante destro del mouse su **Active Directory nodo utenti e computer** , quindi fare clic su **Cambia controller di dominio**. Fare clic sul nome del controller di dominio da cui si desidera rimuovere i metadati, quindi fare clic su **OK**.
3. Espandere il dominio del controller di dominio che è stato rimosso forzatamente, quindi fare clic su **controller di dominio**.
4. Nel riquadro dei dettagli fare clic con il pulsante destro del mouse sull'oggetto computer del controller di dominio di cui si desidera eseguire la pulizia dei metadati, quindi scegliere **Elimina**.
5. Nella finestra di dialogo **Active Directory Domain Services** verificare che sia visualizzato il nome del controller di dominio che si desidera eliminare e fare clic su **Sì** per confermare l'eliminazione dell'oggetto computer.
6. Nella finestra di dialogo **eliminazione controller di dominio** selezionare il **controller di dominio è in modo permanente offline e non può più essere abbassato di grado utilizzando il installazione guidata di Active Directory Domain Services (Dcpromo)** , quindi fare clic su **Elimina**.
7. Se il controller di dominio è un server di catalogo globale, nella finestra di dialogo **Elimina controller di dominio** fare clic su **Sì** per continuare con l'eliminazione.
8. Se il controller di dominio dispone attualmente di uno o più ruoli di master operazioni, fare clic su **OK** per spostare il ruolo o i ruoli nel controller di dominio visualizzato. Non è possibile modificare il controller di dominio. Se si desidera spostare il ruolo in un controller di dominio diverso, è necessario spostare il ruolo dopo aver completato la procedura di pulizia dei metadati del server.

## <a name="clean-up-server-metadata-using-activedirectory-sites-and-services"></a>Pulire i metadati del server usando Active Directory siti e servizi

1. Aprire Siti e servizi di Active Directory.
2. Se sono stati identificati i partner di replica in preparazione per questa procedura e se non si è connessi a un partner di replica del controller di dominio rimosso di cui si desidera pulire i metadati, fare clic con il pulsante destro del mouse su **Active Directory siti e servizi**, quindi scegliere **Cambia controller di dominio**. Fare clic sul nome del controller di dominio da cui si desidera rimuovere i metadati, quindi fare clic su **OK**.
3. Espandere il sito del controller di dominio che è stato rimosso forzatamente, espandere **Server**, espandere il nome del controller di dominio, fare clic con il pulsante destro del mouse sull'oggetto Impostazioni NTDS, quindi fare clic su **Elimina**.
4. Nella finestra di dialogo **Active Directory siti e servizi** , fare clic su **Sì** per confermare l'eliminazione delle impostazioni NTDS.
5. Nella finestra di dialogo **eliminazione controller di dominio** selezionare il **controller di dominio è in modo permanente offline e non può più essere abbassato di grado utilizzando il installazione guidata di Active Directory Domain Services (Dcpromo)** , quindi fare clic su **Elimina**.
6. Se il controller di dominio è un server di catalogo globale, nella finestra di dialogo **Elimina controller di dominio** fare clic su **Sì** per continuare con l'eliminazione.
7. Se il controller di dominio dispone attualmente di uno o più ruoli di master operazioni, fare clic su **OK** per spostare il ruolo o i ruoli nel controller di dominio visualizzato.
8. Fare clic con il pulsante destro del mouse sul controller di dominio che è stato rimosso forzatamente, quindi scegliere Elimina.
9. Nella finestra di dialogo **Active Directory Domain Services** fare clic su **Sì** per confermare l'eliminazione del controller di dominio.

## <a name="clean-up-server-metadata-using-the-command-line"></a>Pulire i metadati del server usando la riga di comando

In alternativa, è possibile eliminare i metadati utilizzando Ntdsutil. exe, uno strumento da riga di comando installato automaticamente in tutti i controller di dominio e i server in cui è installato Active Directory Lightweight Directory Services (AD LDS). Ntdsutil. exe è disponibile anche nei computer in cui è installato strumenti di amministrazione remota del server.

## <a name="to-clean-up-server-metadata-by-using-ntdsutil"></a>Per pulire i metadati del server tramite Ntdsutil

1. Aprire un prompt dei comandi come amministratore: nel menu **Start** , fare clic con il pulsante destro del mouse su **prompt dei comandi**e quindi scegliere **Esegui come amministratore**. Se viene visualizzata la finestra di dialogo **controllo account utente** , fornire le credenziali di un amministratore dell'organizzazione, se necessario, e quindi fare clic su **continua**.
2. Al prompt dei comandi digitare il comando seguente, quindi premere INVIO:

   `ntdsutil`

3. Al prompt `ntdsutil:` digitare il comando seguente e quindi premere INVIO:

   `metadata cleanup`

4. Al prompt `metadata cleanup:` digitare il comando seguente e quindi premere INVIO:

   `remove selected server <ServerName>`

5. Nella **finestra di dialogo Rimuovi configurazione del server**esaminare le informazioni e gli avvisi, quindi fare clic su **Sì** per rimuovere l'oggetto server e i metadati.

   A questo punto, Ntdsutil conferma che il controller di dominio è stato rimosso correttamente. Se viene visualizzato un messaggio di errore che indica che l'oggetto non è stato trovato, è possibile che il controller di dominio sia stato rimosso in precedenza.

6. Al prompt dei comandi `metadata cleanup:` e `ntdsutil:` digitare `quit`, quindi premere INVIO.

7. Per confermare la rimozione del controller di dominio:

   Aprire Active Directory utenti e computer. Nel dominio del controller di dominio rimosso fare clic su **controller di dominio**. Nel riquadro dei dettagli, un oggetto per il controller di dominio rimosso non dovrebbe essere visualizzato.

   Aprire Active Directory siti e servizi. Passare al contenitore **Server** e verificare che l'oggetto server per il controller di dominio rimosso non contenga un oggetto Impostazioni NTDS. Se non vengono visualizzati oggetti figlio sotto l'oggetto server, è possibile eliminare l'oggetto server. Se viene visualizzato un oggetto figlio, non eliminare l'oggetto server perché l'oggetto viene utilizzato da un'altra applicazione.

## <a name="see-also"></a>Vedi anche

* [Abbassamento di livello dei controller di dominio](Demoting-Domain-Controllers-and-Domains--Level-200-.md)
* [Riferimento al comando Ntdsutil](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753343(v=ws.10))
