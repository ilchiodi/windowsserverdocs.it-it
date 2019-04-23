---
title: Registrare un Server dei criteri di rete in un Dominio di Active Directory
description: È possibile utilizzare questo argomento per registrare un server che esegue Server dei criteri di rete in Windows Server 2016 nel dominio predefinito dei criteri di rete o in un altro dominio.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2de954fd-a7d8-4cc6-85b1-b0c3c06f788f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4a289ec519e5107576becf2905cd881cf9def190
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877652"
---
# <a name="register-an-nps-in-an-active-directory-domain"></a>Registrare un Server dei criteri di rete in un Dominio di Active Directory

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per registrare un server che esegue Server dei criteri di rete in Windows Server 2016 nel dominio predefinito dei criteri di rete o in un altro dominio.

## <a name="register-an-nps-in-its-default-domain"></a>Registrare un criteri di rete nel dominio predefinito

È possibile utilizzare questa procedura per iscrivere un criteri di rete nel dominio in cui il server è un membro del dominio. 

NPSs deve essere registrato in Active Directory in modo che abbiano l'autorizzazione per leggere le proprietà di connessione remota degli account utente durante il processo di autorizzazione. La registrazione di un NPS aggiunge il server per il **server RAS e IAS** gruppo in Active Directory.

L'appartenenza a **amministratori**, o equivalente è il requisito minimo necessario per eseguire queste procedure.

### <a name="to-register-an-nps-in-its-default-domain"></a>Per registrare un criteri di rete nel dominio predefinito


1. Su NPS, in Server Manager fare clic su **degli strumenti**, quindi fare clic su **Server dei criteri di rete**. Verrà visualizzata la console di Server dei criteri di rete.

2. Fare doppio clic su **dei criteri di rete (locale)**, quindi fare clic su **registra Server in Active Directory**. Il **Server dei criteri di rete** verrà visualizzata la finestra di dialogo.

3. In **Server dei criteri di rete**, fare clic su **OK**, quindi fare clic su **OK** nuovamente.

## <a name="register-an-nps-in-another-domain"></a>Registrare un NPS in un altro dominio

Per fornire un NPS con l'autorizzazione per leggere le proprietà di connessione remota degli account utente in Active Directory, i criteri di rete devono essere registrati nel dominio in cui si trovano gli account.

È possibile utilizzare questa procedura per registrare un NPS in un dominio in cui i criteri di rete non è un membro del dominio.

L'appartenenza a **amministratori**, o equivalente è il requisito minimo necessario per eseguire queste procedure.

### <a name="to-register-an-nps-in-another-domain"></a>Per registrare un NPS in un altro dominio

1. Nel controller di dominio, in Server Manager fare clic su **degli strumenti**, quindi fare clic su **Active Directory Users and Computers**. Verrà aperta la console Utenti e computer di Active Directory.

2. Nell'albero della console, passare al dominio di cui i criteri di rete per la lettura delle informazioni sull'account utente e quindi scegliere il **utenti** cartella. 

3. Nel riquadro dei dettagli, fare doppio clic su **server RAS e IAS**, quindi fare clic su **proprietà**. Il **IAS proprietà server RAS e** verrà visualizzata la finestra di dialogo.

4. Nel **IAS proprietà server RAS e** finestra di dialogo, fare clic sul **membri** scheda, aggiungere ogni NPSs che si desidera registrare nel dominio e quindi fare clic su **OK**.


### <a name="to-register-an-nps-in-another-domain-by-using-netsh-commands-for-nps"></a>Per registrare un NPS in un altro dominio utilizzando i comandi Netsh per criteri di rete

1. Aprire il prompt dei comandi o windows PowerShell. 

2. Digitare il comando seguente al prompt dei comandi: **netsh nps aggiungere registeredserver** &nbsp; *dominio* &nbsp; *server*, quindi premere INVIO.

>[!NOTE]
>Nel comando precedente, *domain* è il nome di dominio DNS del dominio in cui si vuole registrare i criteri di rete, e *server* è il nome del computer dei criteri di rete.

