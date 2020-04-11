---
title: Eseguire la migrazione delle licenze CAL Servizi Desktop remoto
description: Questo articolo descrive come eseguire la migrazione delle licenze CAL di Servizi Desktop remoto ai nuovi server licenze di Windows Server 2016.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: chrimo
ms.date: 11/01/2016
ms.topic: article
ms.assetid: 91bdedce-6145-469f-b72e-7e113c4391e9
author: christianmontoya
manager: scottman
ms.openlocfilehash: 5d95c2bc3a92a8cdcba4b308c88d94cb9af6d2a5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855994"
---
# <a name="migrate-your-remote-desktop-services-client-access-licenses-rds-cals"></a>Eseguire la migrazione delle licenze CAL Servizi Desktop remoto

Sono disponibili tre opzioni per eseguire la migrazione delle licenze CAL di Servizi Desktop remoto:
1. Metodo di connessione automatica: Questo metodo consigliato comunica tramite internet direttamente con Microsoft Clearinghouse in uscita sulla porta TCP 443.  
2. Da un Web browser: Questo metodo consente la migrazione quando il server che esegue lo strumento Gestione licenze Desktop remoto non dispone di connettività internet, ma l'amministratore abbia la connettività internet in un dispositivo separato. In Gestione guidata licenze CAL Servizi Desktop remoto viene visualizzato l'URL per il metodo di migrazione Web. 
3. Da un telefono: Questo metodo consente all'amministratore di completare il processo di migrazione tramite telefono con un rappresentante Microsoft. Il numero di telefono appropriato dipende dal paese/area geografica che scelti nell'attivazione guidata del server e viene visualizzato in Gestione guidata licenze CAL Servizi Desktop remoto.

In questo articolo, [Stabilire il metodo di migrazione delle licenze CAL Servizi Desktop remoto ](#establish-rds-cal-migration-method) evidenzia i passaggi generali comuni di qualsiasi metodo di migrazione CAL Servizi Desktop remoto, mentre [Eseguire la migrazione delle licenze CAL Servizi Desktop remoto](#migrate-rds-cals) evidenzia i passaggi specifici per ogni metodo di migrazione.

Indipendentemente dal metodo di migrazione, è necessario, come minimo, essere un membro del gruppo Administrators locale per eseguire i passaggi della migrazione.

## <a name="establish-rds-cal-migration-method"></a>Stabilire il metodo di migrazione delle licenze CAL Servizi Desktop remoto

1. Nel server licenze, aprire **Gestione licenze Desktop remoto**. (Fare clic su**Start >Strumenti di amministrazione**. Immettere la directory **Servizi Desktop remoto** e avviare **Gestione licenze Desktop remoto**.)
2. Verificare il metodo di connessione per il server licenze Desktop remoto:fare clic con il pulsante destro del mouse sul server licenze su cui si desidera eseguire la migrazione delle licenze CAL Servizi Desktop remoto e quindi fare clic su **Proprietà**. Nella scheda **Metodo di connessione**, verificare il **Metodo di connessione** (è possibile modificarlo nel menu a discesa). Fare clic su **OK**.
3. Fare clic con il pulsante destro del mouse sul server licenze su cui si desidera eseguire la migrazione delle licenze CAL Servizi Desktop remoto, quindi fare clic su **Gestione licenze CAL Servizi Desktop remoto**.
4. Seguire i passaggi della procedura guidata per la pagina **Selezione azione**. Fare clic su **Eseguire la migrazione di licenze CAL Servizi Desktop remoto da un altro server licenze a questo server licenze**.
6. Scegliere il motivo per la migrazione delle licenze CAL Servizi Desktop remoto e quindi fare clic su **Avanti**. Sono disponibili le seguenti opzioni:
    - Il server licenze di origine viene sostituito da questo server licenze.
    - Il server licenze di origine non è più funzionante.
7. La pagina successiva della procedura guidata dipende dal motivo di migrazione scelto.
    - Se si è scelto **Il server licenze di origine viene sostituito da questo server licenze** come motivo per la migrazione delle licenze CAL Servizi Desktop remoto, viene visualizzata la pagina **Informazioni sul Server licenze di origine**.
    
       Nella pagina informazioni sul Server licenze di origine, immettere il nome o indirizzo IP del server licenze di origine.

       Se il server licenze di origine è disponibile in rete, fare clic su **Avanti**. La procedura guidata contatta il server licenze di origine. Se sul server licenze di origine è in esecuzione un sistema operativo precedente a Windows Server 2008 R2 o se il server licenze di origine è disattivato, si riceve un promemoria che è necessario rimuovere manualmente le licenze CAL Servizi Desktop remoto dal server licenze di origine alla fine della procedura guidata. Dopo la conferma di aver compreso questo requisito, viene visualizzata la pagina **Ottenere il key pack per le licenze client**.

       Se il server licenze di origine non è disponibile in rete, selezionare **Il server licenze di origine specificato non è disponibile in rete**. Specificare il sistema operativo in esecuzione sul server licenze di origine e quindi specificare l'ID del server licenze per il server licenze di origine. Dopo aver fatto clic su **Avanti**, si riceve un promemoria che è necessario rimuovere manualmente le licenze CAL Servizi Desktop remoto dal server licenze di origine alla fine della procedura guidata. Dopo la conferma di aver compreso questo requisito, viene visualizzata la pagina **Ottenere il key pack per le licenze client**.

    - Se si è scelto **Il server licenze di origine non funziona più** come motivo per la migrazione delle licenze CAL Servizi Desktop remoto, si riceve un promemoria che è necessario rimuovere manualmente le licenze CAL Servizi Desktop remoto dal server licenze di origine alla fine della procedura guidata. Dopo la conferma di aver compreso questo requisito, viene visualizzata la pagina **Ottenere il key pack per le licenze client**.

Il passaggio successivo è eseguire la migrazione delle licenze CAL: usare le informazioni seguenti per completare la procedura guidata. Si noti che ciò che viene visualizzato nella procedura guidata dipende dal metodo di connessione identificato nel passaggio 2 precedente.

## <a name="migrate-rds-cals"></a>Eseguire la migrazione delle licenze CAL Servizi Desktop remoto

Esistono tre meccanismi per eseguire la migrazione delle licenze sul server licenze di destinazione. continuare la procedura corrispondente per il **Metodo di connessione** verificato nel passaggio 2:
  - [Metodo di connessione automatico](#automatic-connection-method)
  - [Da un Web browser](#using-a-web-browser)
  - [Da un telefono](#using-a-telephone)

### <a name="automatic-connection-method"></a>Metodo di connessione automatico

1. Nella pagina **Programma di licenza**, selezionare il programma appropriato mediante il quale sono state acquistate le licenze CAL di Servizi Desktop remoto, quindi fare clic su **Avanti**.
2. Immettere le informazioni necessarie (in genere un codice di licenza o un numero di contratto, a seconda il **programma di licenza**), quindi fare clic su **Avanti**. Consultare la documentazione fornita al momento dell'acquisto delle licenze CAL Desktop remoto.
4. Selezionare la versione del prodotto, il tipo di licenza e la quantità appropriate delle licenze CAL di Servizi Desktop remoto per il proprio ambiente in base al contratto di acquisto delle licenze CAL di Servizi Desktop remoto, quindi fare clic su **Avanti**.
5. Microsoft verrà automaticamente contattata e la richiesta verrà elaborata. Le licenze CAL di Servizi Desktop remoto vengono installate sul server licenze.
6. Fare clic su **Fine** per completare il processo di migrazione delle licenze CAL di Servizi Desktop remoto.

### <a name="using-a-web-browser"></a>Da un Web browser
1. Nella pagina **Ottenere il key pack per le licenze client** fare clic sul collegamento ipertestuale per connettersi al sito Web per la gestione delle licenze di Servizi Desktop remoto.
   Se si esegue Gestione licenze Desktop remoto su un computer che non ha connettività Internet, prendere nota dell'indirizzo per il sito Web di Gestione licenze Servizi Desktop remoto e quindi connettersi al sito Web da un computer con connettività Internet. 
2. Nella pagina Web Gestione licenze Servizi Desktop remoto sotto **Selezionare un'opzione**, selezionare **Gestire le licenze CAL**e quindi fare clic su **Avanti**.
3. Specificare le informazioni richieste, quindi fare clic su **Avanti**:
    - **ID del server licenze**: Un numero di 35 cifre, a gruppi di 5, che viene visualizzato nella pagina **Ottenere il key pack per le licenze client** della procedura guidata per la gestione delle licenze CAL di Servizi Desktop remoto.
    - **Motivo per il ripristino**: Scegliere il motivo per la migrazione delle licenze CAL di Servizi Desktop remoto.
    - **Programma di licenza**: Scegliere il programma tramite cui sono state acquistate le licenze CAL di Servizi Desktop remoto.
4. Specificare le informazioni richieste, quindi fare clic su **Avanti**:
   - Cognome
   - Nome
   - Nome della società
   - Paese

     È anche possibile fornire le informazioni facoltative, ad esempio indirizzo della società, indirizzo di posta elettronica e numero di telefono. Nel campo unità organizzativa, è possibile descrivere l'unità all'interno dell'organizzazione in cui viene utilizzato questo server licenze.

5. Le informazioni che occorre fornire nella pagina successiva dipendono dal programma di licenza selezionato nella pagina precedente. Nella maggior parte dei casi è necessario fornire un codice licenza o un numero di contratto. Consultare la documentazione fornita al momento dell'acquisto delle licenze CAL Desktop remoto. Inoltre, è necessario specificare il tipo e la quantità di licenze CAL di Servizi Desktop remoto per cui si desidera eseguire la migrazione al server licenze.
6. Dopo avere immesso le informazioni necessarie, fare clic su **Avanti**.
7. Verificare che tutte le informazioni immesse siano corrette, quindi fare clic su **Avanti** per inviare la richiesta a Clearinghouse Microsoft. Quindi, la pagina web visualizza un ID del key pack per la licenza generato da Microsoft Clearinghouse.

   > [!IMPORTANT] 
   > Conservare una copia dell'ID del key pack per le licenze. Questa informazione semplificherà le comunicazioni con Microsoft Clearinghouse nel caso fosse necessario richiedere assistenza per il recupero delle licenze CAL di Servizi Desktop remoto.

8. Nella stessa pagina **ottenere il Client licenze Key Pack**, immettere l'ID del key pack per le licenze e quindi fare clic su **Avanti** per eseguire la migrazione delle licenze CAL di Servizi Desktop remoto sul server licenze.
9. Fare clic su **Fine** per completare il processo di migrazione delle licenze CAL di Servizi Desktop remoto.

### <a name="using-a-telephone"></a>Da un telefono
1. Nella pagina **Ottenere il key pack per le licenze client**, utilizzare il numero di telefono visualizzato per chiamare Microsoft Clearinghouse. Fornire al rappresentante l'ID del server licenze Desktop remoto e le informazioni necessarie per il programma di gestione delle licenze tramite cui sono state acquistate le licenze CAL di Servizi Desktop remoto. Il rappresentante elabora la richiesta di migrazione delle licenze CAL di Desktop remoto e fornisce un ID univoco per ciascuna licenza CAL. Tale ID univoco viene indicato come **ID del key pack per le licenze**.

   > [!IMPORTANT]
   > Conservare una copia dell'ID del key pack per le licenze. Questa informazione semplificherà le comunicazioni con Microsoft Clearinghouse nel caso fosse necessario richiedere assistenza per il recupero delle licenze CAL di Servizi Desktop remoto.

2. Nella stessa pagina **ottenere il Client licenze Key Pack**, immettere l'ID del key pack per le licenze e quindi fare clic su **Avanti** per eseguire la migrazione delle licenze CAL di Servizi Desktop remoto sul server licenze.
3. Fare clic su **Fine** per completare il processo di migrazione delle licenze CAL di Servizi Desktop remoto.
