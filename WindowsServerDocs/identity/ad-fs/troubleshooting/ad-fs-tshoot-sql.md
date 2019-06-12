---
title: Risoluzione dei problemi di AD FS - connettività SQL
description: Questo documento descrive come risolvere i vari aspetti di AD FS.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/12/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4b09094b6e305bc85b38e94d11fbc8845d555437
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66443930"
---
# <a name="ad-fs-troubleshooting---sql-connectivity"></a>Risoluzione dei problemi di AD FS - connettività SQL
ADFS offre la possibilità di usare remote SQL Server per i dati della farm AD FS.  Noterete problemi se i server AD FS nella farm non possono comunicare con il server SQL back-end.  Il seguente documento riporta alcuni passaggi di base per la comunicazione con i server back-end di test.

## <a name="acquire-the-sql-database-connection-string"></a>Acquisire la stringa di connessione del database SQL
È la prima cosa da verificare quando controllare la connettività di SQL, se AD FS offre le informazioni di connessione SQL corrette.  Questa operazione può essere eseguita tramite PowerShell.

### <a name="to-acquire-the-sql-connection-string"></a>Per acquisire la stringa di connessione SQL
1.  Aprire Windows PowerShell
2. Immettere le informazioni seguenti: `$adfs = gwmi -Namespace root/ADFS -Class SecurityTokenService` e premere INVIO.
3. Immettere le informazioni seguenti: `$adfs.ConfigurationDatabaseConnectionString` e premere INVIO.
4. Si dovrebbero vedere le informazioni sulla stringa di connessione.
![](media/ad-fs-tshoot-sql/sql2.png)

## <a name="create-a-universal-data-link-udl-file-to-test-connectivity"></a>Creare un file Universal Data Link (UDL) per testare la connettività
Un file UDL o il file Universal Data Link è fondamentalmente un file di testo che contiene la stringa di connessione del database.  Usando le informazioni che abbiamo ottenuti in precedenza è possibile testare se il server SQL risponde alle connessioni.

### <a name="to-create-a-udl-file-to-test-connectivity"></a>Per creare un file udl per testare la connettività

1. Aprire Blocco note e salvare il file test. udl.  Assicurarsi di aver **tutti i file** selezionato dall'elenco a discesa per **Salva come tipo**.
2. Fare doppio clic sul test. udl
3. Immettere le informazioni seguenti: una. **Selezionare o immettere un nome server:**  Usare l'origine dati dalla stringa di connessione sopra b. **Immettere le informazioni per l'accesso al server:**  Usare l'account del servizio AD FS o un account che disponga delle autorizzazioni per l'accesso remoto.  Se l'account è un utilizzo di account di windows integrato l'autenticazione in caso contrario, immettere il nome utente e password.
    c. **Selezionare il database nel server:** Usare il catalogo iniziale della stringa precedente.  Esempio:  AdfsConfigurationV3.
   ![Test della connessione](media/ad-fs-tshoot-sql/sql4.png)
1. Fare clic su **Test connessione**.</br>
![Operazione riuscita](media/ad-fs-tshoot-sql/sql3.png)

## <a name="use-sql-server-management-studio-to-test-connectivity"></a>Usare SQL Server Management Studio per testare la connettività
È anche possibile [scaricare](https://go.microsoft.com/fwlink/?linkid=864329) e installare SSMS per testare la connettività del database.

### <a name="to-test-connectivity-with-ssms"></a>Per testare la connettività con SSMS
1. Scaricare e installare SQL Server Management Studio.
![Installa](media/ad-fs-tshoot-sql/sql5.png)
1. Aprire SQL Server Management Studio, immettere il nome del Server.  L'origine dati dall'esempio precedente.
2. Usare l'account del servizio AD FS o un account che disponga delle autorizzazioni per l'accesso remoto.  Se l'account è un utilizzo di account di windows integrato l'autenticazione in caso contrario, immettere il nome utente e password.
![La connessione](media/ad-fs-tshoot-sql/sql6.png)
1. Verrà visualizzato sul lato sinistro popolato.  Espandere database e verificare che venga visualizzato i database di ADFS.
![Database di AD FS](media/ad-fs-tshoot-sql/sql7.png)

## <a name="next-steps"></a>Passaggi successivi

- [Risoluzione dei problemi relativi ad AD FS](ad-fs-tshoot-overview.md)