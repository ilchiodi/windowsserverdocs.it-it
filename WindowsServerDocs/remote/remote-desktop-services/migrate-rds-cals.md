---
title: Eseguire la migrazione delle licenze CAL Servizi Desktop remoto
description: Questo articolo descrive come eseguire la migrazione di licenze di accesso Client di Servizi Desktop remoto al nuovo server licenze di Windows Server 2016.
ms.custom: na
ms.prod: windows-server-threshold
msreviewer: ''
nams.suite: ''
nams.technology: remote-desktop-services
ms.author: chrimo
ms.date: 11/01/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 91bdedce-6145-469f-b72e-7e113c4391e9
author: christianmontoya
manager: scottman
ms.openlocfilehash: 3b809d4d624f21eaeae5a66277db683c95c79bdd
ms.sourcegitcommit: 21165734a0f37c4cd702c275e85c9e7c42d6b3cb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/03/2019
ms.locfileid: "65034447"
---
# <a name="migrate-your-remote-desktop-services-client-access-licenses-rds-cals"></a>Eseguire la migrazione delle licenze CAL Servizi Desktop remoto

Sono disponibili tre opzioni per eseguire la migrazione delle licenze CAL Servizi Desktop remoto:
1. Metodo di connessione automatica: Questo metodo consigliato comunica tramite internet direttamente a Microsoft Clearinghouse in uscita sulla porta TCP 443.  
2. Usando un web browser: Questo metodo consente la migrazione quando il server che esegue lo strumento Gestione licenze Desktop remoto non dispone di connettività internet, ma l'amministratore abbia la connettività internet in un dispositivo separato. In Gestione guidata licenze CAL Servizi Desktop remoto viene visualizzato l'URL per il metodo di migrazione di Web. 
3. Utilizzo di un telefono: Questo metodo consente all'amministratore completare il processo di migrazione tramite telefono con un rappresentante Microsoft. Il numero di telefono appropriato viene determinato dal paese/area geografica che è stato scelto l'attivazione guidata Server e viene visualizzato in Gestione guidata licenze CAL Servizi Desktop remoto.

In questo articolo, il [metodo di migrazione di servizi desktop remoto stabilire CAL](#establish-rds-cal-migration-method) evidenzia i passaggi generali comuni tra qualsiasi metodo di migrazione CAL RDS, mentre [licenze CAL Servizi Desktop remoto di eseguire la migrazione](#migrate-rds-cals) evidenzia i passaggi specifici per ogni migrazione metodo.

Indipendentemente dal metodo di migrazione, è necessario, come minimo, essere un membro del gruppo Administrators locale per eseguire i passaggi della migrazione.

## <a name="establish-rds-cal-migration-method"></a>Stabilire il metodo di migrazione di licenze CAL Servizi Desktop remoto

1. Nel server licenze aprire **gestione licenze Desktop remoto**. (Fare clic su **Start > Strumenti di amministrazione**. Immettere il **Servizi Desktop remoto** directory e avviare **gestione licenze Desktop remoto**.)
2. Verificare il metodo di connessione per il server licenze Desktop remoto: pulsante destro del mouse il server licenze a cui si desidera eseguire la migrazione di licenze CAL Servizi Desktop remoto e quindi fare clic su **proprietà**. Nel **Connection Method** scheda, verificare le **Connection method** -è possibile modificarlo nel menu a discesa. Fare clic su **OK**.
3. Fare clic sul server licenze a cui si desidera eseguire la migrazione di licenze CAL Servizi Desktop remoto e quindi fare clic su **gestire le licenze CAL Servizi Desktop remoto**.
4. Seguire i passaggi della procedura guidata per la **selezione azione** pagina. Fare clic su **eseguire la migrazione di licenze CAL da un altro server licenze a questo server licenze**.
6. Scegliere il motivo per la migrazione di licenze CAL Servizi Desktop remoto e quindi fare clic su **successivo**. Sono disponibili le opzioni seguenti:
    - Server licenze di origine viene sostituito da questo server licenze.
    - Server licenze di origine non funziona più.
7. La pagina successiva della procedura guidata dipende dal motivo migrazione scelto.
    - Se si è scelto **server licenze di origine viene sostituito da questo server licenze** come il motivo per la migrazione le licenze CAL Servizi Desktop remoto, il **informazioni Server licenze di origine** viene visualizzata la pagina.
    
       Nella pagina informazioni sul Server licenze di origine, immettere il nome o indirizzo IP del server licenze di origine.

       Se il server licenze di origine è disponibile in rete, fare clic su **successivo**. La procedura guidata contatta il server licenze di origine. Se il server licenze di origine è in esecuzione un sistema operativo precedente a Windows Server 2008 R2 o server licenze di origine è disattivato, si ricevono un promemoria che è necessario rimuovere manualmente le licenze CAL Servizi Desktop remoto dal server licenze di origine dopo la procedura guidata è stata completata. Dopo la conferma di aver compreso questo requisito, il **ottenere il Client licenze Key Pack** verrà visualizzata la pagina.

       Se il server licenze di origine non è disponibile in rete, selezionare **server licenze di origine specificato non è disponibile in rete**. Specificare il sistema operativo che esegue il server licenze di origine e quindi specificare l'ID server licenze per il server licenze di origine. Dopo aver fatto clic **successivo**, si ricevono un promemoria che è necessario rimuovere manualmente le licenze CAL Servizi Desktop remoto dal server licenze di origine dopo la procedura guidata è stata completata. Dopo la conferma di aver compreso questo requisito, il **ottenere il Client licenze Key Pack** verrà visualizzata la pagina.

    - Se si è scelto **server licenze di origine non funziona più** come il motivo per la migrazione di licenze CAL Servizi Desktop remoto, si ricevono un promemoria che è necessario rimuovere manualmente le licenze CAL Servizi Desktop remoto dal server licenze di origine dopo la procedura guidata è stata completata. Dopo la conferma di aver compreso questo requisito, il **ottenere il Client licenze Key Pack** verrà visualizzata la pagina.

Il passaggio successivo è eseguire la migrazione di licenze CAL: usare le informazioni seguenti per completare la procedura guidata. Si noti che ciò che viene visualizzato nella procedura guidata dipende il metodo di connessione che è stata identificata nel passaggio 2 precedente.

## <a name="migrate-rds-cals"></a>Eseguire la migrazione di licenze CAL Servizi Desktop remoto

Esistono tre meccanismi per eseguire la migrazione delle licenze per il server licenze di destinazione. continuare la procedura corrispondente per il **metodo di connessione** verificati nel passaggio 2:
  - [Metodo di connessione automatica](#automatic-connection-method)
  - [Usando un web browser](#using-a-web-browser)
  - [Telefono.](#using-a-telephone)

### <a name="automatic-connection-method"></a>Metodo di connessione automatica

1. Nel **programma di licenza** pagina, selezionare il programma appropriato tramite cui sono state acquistate le licenze CAL Servizi Desktop remoto, quindi fare clic su **successivo**.
2. Immettere le informazioni necessarie (in genere un codice di licenza o un numero di contratto, a seconda il **programma di licenza**), quindi fare clic su **successivo**. Consultare la documentazione fornita al momento dell'acquisto delle licenze CAL Servizi Desktop remoto.
4. Selezionare la versione appropriata del prodotto, il tipo di licenza e quantità di licenze CAL Servizi Desktop remoto per l'ambiente in base al contratto di acquisto CAL RDS e quindi fare clic su **successivo**.
5. Microsoft verrà automaticamente contattata e la richiesta verrà elaborata. Le licenze CAL Servizi Desktop remoto viene quindi eseguita la migrazione nel server licenze.
6. Fare clic su **fine** per completare il processo di migrazione CAL RDS.

### <a name="using-a-web-browser"></a>Usando un web browser
1. Nel **ottenere il Client licenze Key Pack** fare clic sul collegamento ipertestuale per connettersi al sito Web licenze Servizi Desktop remoto.
Se si esegue Gestione licenze Desktop remoto in un computer che non hanno connettività Internet, prendere nota dell'indirizzo per il sito Web licenze Servizi Desktop remoto e quindi connettersi al sito Web da un computer con connettività Internet. 
2. Nella pagina Web licenze Servizi Desktop remoto sotto **selezionare un'opzione**, selezionare **gestire le licenze CAL**e quindi fare clic su **Next**.
3. Specificare le informazioni richieste, quindi fare clic su **successivo**:
    - **ID del Server licenze di destinazione**: Un numero di cifre 35, nei gruppi di 5, che viene visualizzato nel **ottenere il Client licenze Key Pack** pagina della procedura guidata di gestire le licenze CAL Servizi Desktop remoto.
    - **Motivo per il ripristino**: Scegliere il motivo per la migrazione di licenze CAL Servizi Desktop remoto.
    - **Programma di licenza**: Scegliere il programma tramite cui sono state acquistate le licenze CAL Servizi Desktop remoto.
4. Specificare le informazioni richieste, quindi fare clic su **successivo**:
    - Ultimo nome o cognome
    - Nome o il nome specificato
    - Nome della società
    - Paese/area geografica

    È anche possibile fornire le informazioni facoltative, ad esempio l'indirizzo della società, indirizzo di posta elettronica e numero di telefono. Nel campo unità organizzativa, è possibile descrivere l'unità all'interno dell'organizzazione che viene utilizzato questo server licenze.

5. Il programma di licenza che nella pagina precedente è stato selezionato determina quali informazioni è necessario specificare nella pagina successiva. Nella maggior parte dei casi, è necessario fornire un codice di licenza o un numero di contratto. Consultare la documentazione fornita al momento dell'acquisto delle licenze CAL Servizi Desktop remoto. Inoltre, è necessario specificare quale tipo di licenze CAL Servizi Desktop remoto e la quantità che si desidera eseguire la migrazione al server licenze.
6. Dopo avere immesso le informazioni necessarie, fare clic su **Avanti**.
7. Verificare che tutte le informazioni che è stato immesso sia corretto, quindi fare clic su **successivo** per inviare la richiesta a Clearinghouse Microsoft. La pagina web Visualizza quindi un ID del key pack licenza generato da Microsoft Clearinghouse.

   > [!IMPORTANT] 
   > Conservare una copia dell'ID del key pack per le licenze. Queste informazioni si facilita le comunicazioni con Microsoft Clearinghouse, dovrebbe occorre assistenza per il recupero delle licenze CAL Servizi Desktop remoto.

8. Nella stessa **ottenere il Client licenze Key Pack** pagina, immettere l'ID del key pack per le licenze e quindi fare clic su **successivo** per eseguire la migrazione di licenze CAL Servizi Desktop remoto per il server licenze.
9. Fare clic su **fine** per completare il processo di migrazione CAL RDS.

### <a name="using-a-telephone"></a>Telefono.
1. Nel **ottenere il Client licenze Key Pack** pagina, utilizzare il numero di telefono visualizzati per chiamare Microsoft Clearinghouse. Fornire al rappresentante l'ID del server licenze Desktop remoto e le informazioni necessarie per il programma di licenza tramite cui sono state acquistate le licenze CAL Servizi Desktop remoto. Il rappresentante elaborerà la richiesta di eseguire la migrazione di licenze CAL Servizi Desktop remoto e ti offre un ID univoco per le licenze CAL Servizi Desktop remoto. Questo ID univoco viene indicato come la **licenza di ID del key pack**.

   > [!IMPORTANT]
   > Conservare una copia dell'ID del key pack per le licenze. Queste informazioni si facilita le comunicazioni con Microsoft Clearinghouse deve occorre assistenza per il recupero delle licenze CAL Servizi Desktop remoto.

2. Nella stessa **ottenere il Client licenze Key Pack** pagina, immettere l'ID del key pack per le licenze e quindi fare clic su **successivo** per eseguire la migrazione di licenze CAL Servizi Desktop remoto per il server licenze.
3. Fare clic su **fine** per completare il processo di migrazione CAL RDS.
