---
title: Registrare un Server dei criteri di rete in un dominio Active Directory
description: È possibile utilizzare questo argomento per registrare un server che esegue Server dei criteri di rete in Windows Server 2016 nel dominio predefinito server dei criteri di rete o in un altro dominio.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2de954fd-a7d8-4cc6-85b1-b0c3c06f788f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7ba18f6c994e8b15da3a07a3e37550d5dbff24af
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="register-an-nps-server-in-an-active-directory-domain"></a>Registrare un Server dei criteri di rete in un dominio Active Directory

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per registrare un server che esegue Server dei criteri di rete in Windows Server 2016 nel dominio predefinito server dei criteri di rete o in un altro dominio.

## <a name="register-an-nps-server-in-its-default-domain"></a>Registrare un Server dei criteri di rete nel dominio predefinito

È possibile utilizzare questa procedura per registrare un server dei criteri di rete nel dominio in cui il server è un membro del dominio. 

Server dei criteri di rete devono essere registrati in Active Directory in modo che dispongano dell'autorizzazione per leggere le proprietà di connessione remota degli account utente durante il processo di autorizzazione. La registrazione di un server dei criteri di rete consente di aggiungere il server per il **server RAS e IAS** gruppo in Active Directory.

Appartenenza al gruppo **amministratori**, o equivalente è il requisito minimo necessario per eseguire queste procedure.

### <a name="to-register-an-nps-server-in-its-default-domain"></a>Per registrare un server dei criteri di rete nel dominio predefinito


1. Nel server dei criteri di rete, in Server Manager, fare clic su **strumenti**, quindi fare clic su **Server dei criteri di rete**. Apre la console di Server dei criteri di rete.

2. Fare doppio clic su **dei criteri di rete (locale)**, quindi fare clic su **registra Server in Active Directory**. Il **Server dei criteri di rete** apre la finestra di dialogo.

3. In **Server dei criteri di rete**, fare clic su **OK**, quindi fare clic su **OK** nuovamente.

## <a name="register-an-nps-server-in-another-domain"></a>Registrare un Server dei criteri di rete in un altro dominio

Per fornire un server dei criteri di rete con l'autorizzazione per leggere le proprietà di connessione dell'account utente in Active Directory, il server dei criteri di rete deve essere registrato nel dominio in cui risiedono gli account.

È possibile utilizzare questa procedura per registrare un server dei criteri di rete in un dominio in cui il server dei criteri di rete non è un membro del dominio.

Appartenenza al gruppo **amministratori**, o equivalente è il requisito minimo necessario per eseguire queste procedure.

### <a name="to-register-an-nps-server-in-another-domain"></a>Per registrare un server dei criteri di rete in un altro dominio

1. Nel controller di dominio, in Server Manager, fare clic su **strumenti**, quindi fare clic su **Active Directory Users and Computers**. Apre la console Active Directory Users and Computers.

2. Nell'albero della console, selezionare il dominio in cui si desidera che il server dei criteri di rete per la lettura delle informazioni sull'account utente, quindi fare clic su di **utenti** cartella. 

3. Nel riquadro dei dettagli fare doppio clic su **server RAS e IAS**, quindi fare clic su **proprietà**. Il **proprietà RAS e IAS Server** apre la finestra di dialogo.

4. Nel **proprietà RAS e IAS Server** la finestra di dialogo, fare clic su di **membri** scheda, aggiungere ogni server dei criteri di rete che si desidera registrare nel dominio, quindi fare clic su **OK**.


### <a name="to-register-an-nps-server-in-another-domain-by-using-netsh-commands-for-nps"></a>Per registrare un server dei criteri di rete in un altro dominio utilizzando i comandi Netsh per criteri di rete

1. Aprire il prompt dei comandi o windows PowerShell. 

2. Digitare il comando seguente al prompt dei comandi: **netsh nps set registeredserver**&nbsp;*dominio*&nbsp;*server*, quindi premere INVIO.

>[!NOTE]
>Nel comando precedente, *dominio* è il nome di dominio DNS del dominio in cui si desidera registrare il server dei criteri di rete, e *server* è il nome del computer del server dei criteri di rete.

