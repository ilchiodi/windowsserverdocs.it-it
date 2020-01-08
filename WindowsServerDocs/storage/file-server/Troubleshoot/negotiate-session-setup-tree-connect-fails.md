---
title: Errori di negoziazione, configurazione della sessione e connessione ad albero
description: Viene illustrato come risolvere i problemi relativi a negoziazione, configurazione della sessione ed errori di connessione ad albero.
author: Deland-Han
manager: dcscontentpm
audience: ITPro
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: 0ccd8d882060432dcfc27ee47b82d0c61e3aad4d
ms.sourcegitcommit: 8cf04db0bc44fd98f4321dca334e38c6573fae6c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/03/2020
ms.locfileid: "75654372"
---
# <a name="negotiate-session-setup-and-tree-connect-failures"></a>Errori di negoziazione, configurazione della sessione e connessione ad albero

Questo articolo descrive come risolvere gli errori che si verificano durante un'operazione di negoziazione SMB, configurazione della sessione e richiesta di connessione ad albero.

## <a name="negotiate-fails"></a>Negoziazione non riuscita

Il server SMB riceve una richiesta di negoziazione SMB da un client SMB. Si verifica il timeout della connessione e viene reimpostato dopo 60 secondi. Potrebbe essere presente un messaggio ACK dopo circa 200 microsecondi.

Questo problema è spesso causato dal programma antivirus.

Se si usa Windows Server 2008 R2, sono presenti hotfix per questo problema. Verificare che il client SMB e il server SMB siano aggiornati.

## <a name="session-setup-fails"></a>Errore di installazione della sessione

Il server SMB riceve una sessione SMB\_richiesta di installazione da un client SMB, ma non è riuscita a rispondere.

Se il nome di dominio completo (FQDN) o il nome NetBIOS (Network Basic Input/Output System) del server viene utilizzato nel percorso Universal Naming Convention (UNC), Windows utilizzerà Kerberos per l'autenticazione.

Dopo la risposta di negoziazione, verrà effettuato un tentativo di ottenere un ticket Kerberos per il nome dell'entità servizio (SPN) Common Internet file System (CIFS) del server. Esaminare il traffico Kerberos sulla porta TCP 88 per assicurarsi che non siano presenti errori Kerberos quando il client SMB sta ottenendo il token.

> [!NOTE]
> Gli errori che si verificano durante la pre-autenticazione Kerberos sono OK. Gli errori che si verificano dopo la pre-autenticazione Kerberos (istanze in cui l'autenticazione non funziona) sono gli errori che hanno causato il problema SMB.

Inoltre, effettuare i controlli seguenti:

- Esaminare il BLOB di sicurezza nella sessione SMB\_richiesta di installazione per assicurarsi che vengano inviate le credenziali corrette.

- Provare a disabilitare la protezione avanzata del nome del server SMB (**SmbServerNameHardeningLevel = 0**).

- Verificare che il server SMB disponga di un nome SPN quando si accede tramite un record DNS CNAME.

- Assicurarsi che la firma SMB sia funzionante. Questa operazione è particolarmente importante per i dispositivi di terze parti precedenti.

## <a name="tree-connect-fails"></a>Connessione albero non riuscita

Assicurarsi che le credenziali dell'account utente dispongano delle autorizzazioni di condivisione e di NT file system (NTFS) per la cartella.

La richiesta di errori comuni di Connect Tree si trova in [3.3.5.7 che riceve un albero SMB2\_richiesta Connect](https://docs.microsoft.com/openspecs/windows_protocols/ms-smb2/652e0c14-5014-4470-999d-b174d7b2da87). Di seguito sono riportate le soluzioni per due codici di stato comuni.

STATO \[\_nome\_rete\_non valido\]

Verificare che la condivisione esista nel server e che sia stata digitata correttamente nella richiesta client SMB.

STATO \[\_accesso\_negato\]

Verificare che il disco e la cartella utilizzati dalla condivisione esistano e siano accessibili.

Se si usa SMBv3 o versione successiva, verificare se il server e la condivisione richiedono la crittografia, ma il client non supporta la crittografia. Per effettuare questa operazione, eseguire le azioni seguenti:

- Controllare il server eseguendo il comando seguente.

  ```PowerShell
  Get-SmbServerConfiguration | select Encrypt*
  ```

  Se EncryptData e RejectUnencryptedAccess sono true, il server richiede la crittografia.

- Controllare la condivisione eseguendo il comando seguente:

  ```PowerShell
  Get-SmbShare | select name, EncryptData  
  ```

  Se EncryptData è true nella condivisione e RejectUnencryptedAccess è true nel server, la crittografia è richiesta dalla condivisione

Seguire queste linee guida durante la risoluzione dei problemi:

- Windows 8, Windows Server 2012 e versioni successive di Windows supportano la crittografia lato client (SMBv3 e versioni successive).

- Windows 7, Windows Server 2008 R2 e le versioni precedenti di Windows non supportano la crittografia lato client.

- Samba e dispositivi di terze parti potrebbero non supportare la crittografia. Per ulteriori informazioni, potrebbe essere necessario consultare la documentazione del prodotto.

## <a name="references"></a>Riferimenti

Per altre informazioni, vedere gli articoli seguenti.

[3.3.5.4 che riceve una richiesta di negoziazione SMB2](https://docs.microsoft.com/openspecs/windows_protocols/ms-smb2/b39f253e-4963-40df-8dff-2f9040ebbeb1)

[3.3.5.5 ricezione di una sessione di SMB2\_richiesta di installazione](https://docs.microsoft.com/openspecs/windows_protocols/ms-smb2/e545352b-9f2b-4c5e-9350-db46e4f6755e)

[3.3.5.7 ricezione di un albero SMB2\_richiesta CONNECT](https://docs.microsoft.com/openspecs/windows_protocols/ms-smb2/652e0c14-5014-4470-999d-b174d7b2da87?redirectedfrom=MSDN)
