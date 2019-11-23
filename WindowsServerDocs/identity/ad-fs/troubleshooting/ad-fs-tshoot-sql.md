---
title: Risoluzione dei problemi di AD FS-connettività SQL
description: In questo documento viene descritto come risolvere i diversi aspetti di AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/12/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 09d61292b91c83466f9770184d431b3e6d627dca
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385445"
---
# <a name="ad-fs-troubleshooting---sql-connectivity"></a>Risoluzione dei problemi di AD FS-connettività SQL
AD FS offre la possibilità di utilizzare SQL Server remoto per i dati della farm di AD FS.  Si noterà un problema se i server AD FS della farm non possono comunicare con i server SQL back-end.  Il documento seguente fornirà alcuni passaggi di base per il test della comunicazione con i server back-end.

## <a name="acquire-the-sql-database-connection-string"></a>Acquisire la stringa di connessione al database SQL
La prima cosa da verificare quando si verifica la connettività SQL è, se AD FS ha le informazioni di connessione SQL corrette.  Questa operazione può essere eseguita tramite PowerShell.

### <a name="to-acquire-the-sql-connection-string"></a>Per acquisire la stringa di connessione SQL
1.  Apri Windows PowerShell
2. Immettere quanto segue: `$adfs = gwmi -Namespace root/ADFS -Class SecurityTokenService` e premere INVIO
3. Immettere quanto segue: `$adfs.ConfigurationDatabaseConnectionString` e premere INVIO.
4. Verranno visualizzate le informazioni sulla stringa di connessione.
![](media/ad-fs-tshoot-sql/sql2.png)

## <a name="create-a-universal-data-link-udl-file-to-test-connectivity"></a>Creare un file di Universal Data Link (UDL) per testare la connettività
Un file Universal Data Link o UDL è fondamentalmente un file di testo che contiene la stringa di connessione al database.  Utilizzando le informazioni ottenute in precedenza, è possibile verificare se SQL Server sta rispondendo alle connessioni.

### <a name="to-create-a-udl-file-to-test-connectivity"></a>Per creare un file UDL per testare la connettività

1. Aprire il blocco note e salvare il file come test. udl.  Assicurarsi di disporre di **tutti i file** selezionati nell'elenco a discesa per **Salva come tipo**.
2. Fare doppio clic su test. UDL
3. Immettere le informazioni seguenti: a. **Selezionare o immettere il nome di un server:**  Utilizzare l'origine dati dalla stringa di connessione sopra b. **Immettere le informazioni per l'accesso al server:**  Utilizzare l'account del servizio AD FS o un account che dispone delle autorizzazioni per accedere in remoto.  Se l'account è un account di Windows, utilizzare l'autenticazione integrata in caso contrario immettere il nome utente e la password.
    c. **Selezionare il database nel server:** Usare il catalogo iniziale dalla stringa sopra indicata.  Esempio: AdfsConfigurationV3.
   ![Test connessione](media/ad-fs-tshoot-sql/sql4.png)
1. Fare clic su **Test connessione**.</br>
![esito positivo](media/ad-fs-tshoot-sql/sql3.png)

## <a name="use-sql-server-management-studio-to-test-connectivity"></a>Usare SQL Server Management Studio per testare la connettività
È anche possibile [scaricare](https://go.microsoft.com/fwlink/?linkid=864329) e installare SSMS per testare la connettività del database.

### <a name="to-test-connectivity-with-ssms"></a>Per testare la connettività con SSMS
1. Scaricare e installare SQL Server Management Studio.
![Installa](media/ad-fs-tshoot-sql/sql5.png)
1. Aprire SSMS, immettere il nome del server.  Origine dati precedente.
2. Utilizzare l'account del servizio AD FS o un account che dispone delle autorizzazioni per accedere in remoto.  Se l'account è un account di Windows, utilizzare l'autenticazione integrata in caso contrario immettere il nome utente e la password.
Connessione ![](media/ad-fs-tshoot-sql/sql6.png)
1. Si noterà che il lato sinistro è popolato.  Espandere database e verificare che siano visualizzati i database di AD FS.
![AD FS database](media/ad-fs-tshoot-sql/sql7.png)

## <a name="next-steps"></a>Passaggi successivi

- [Risoluzione dei problemi relativi ad AD FS](ad-fs-tshoot-overview.md)