---
title: Registrare un Server dei criteri di rete in un Dominio di Active Directory
description: È possibile utilizzare questo argomento per registrare un server che esegue Server dei criteri di rete in Windows Server 2016 nel dominio predefinito server dei criteri di rete o in un altro dominio.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 2de954fd-a7d8-4cc6-85b1-b0c3c06f788f
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 63d630250b0b24937a3dfc01bcba7ec63faa3c3e
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315968"
---
# <a name="register-an-nps-in-an-active-directory-domain"></a>Registrare un Server dei criteri di rete in un Dominio di Active Directory

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per registrare un server che esegue Server dei criteri di rete in Windows Server 2016 nel dominio predefinito server dei criteri di rete o in un altro dominio.

## <a name="register-an-nps-in-its-default-domain"></a>Registrare un server dei criteri di dominio nel dominio predefinito

È possibile utilizzare questa procedura per registrare un server dei criteri di dominio nel dominio in cui il server è un membro di dominio. 

NPSs deve essere registrato in Active Directory in modo che disponga delle autorizzazioni per leggere le proprietà di connessione di account utente durante il processo di autorizzazione. La registrazione di un server dei criteri di gruppo aggiunge il server al gruppo di **server RAS e IAS** in Active Directory.

Per eseguire queste procedure è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente.

### <a name="to-register-an-nps-in-its-default-domain"></a>Per registrare un server dei criteri di dominio nel dominio predefinito


1. Nel server dei criteri di rete, in Server Manager, fare clic su **strumenti**e quindi su **Server dei criteri di rete**. Verrà visualizzata la console del server dei criteri di rete.

2. Fare clic con il pulsante destro del mouse su **NPS (locale)** e quindi scegliere **registra server in Active Directory**. Viene visualizzata la finestra di dialogo **Server dei criteri di rete**.

3. In **Server dei criteri di rete** fare clic su **OK** e quindi di nuovo su **OK**.

## <a name="register-an-nps-in-another-domain"></a>Registrare un server dei criteri di dominio in un altro dominio

Per fornire a un server dei criteri di accesso con l'autorizzazione per leggere le proprietà di connessione degli account utente in Active Directory, è necessario che il server dei criteri di dominio sia registrato nel dominio in cui si trovano gli account.

È possibile utilizzare questa procedura per registrare un server dei criteri di dominio in un dominio in cui NPS non è un membro di dominio.

Per eseguire queste procedure è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente.

### <a name="to-register-an-nps-in-another-domain"></a>Per registrare un server dei criteri di dominio in un altro dominio

1. Nel controller di dominio, in Server Manager, fare clic su **strumenti**, quindi fare clic su **Active Directory utenti e computer**. Verrà aperta la console Utenti e computer di Active Directory.

2. Nell'albero della console passare al dominio in cui si desidera che il server dei criteri di dominio legga le informazioni sull'account utente e quindi fare clic sulla cartella **utenti** . 

3. Nel riquadro dei dettagli fare clic con il pulsante destro del mouse su **server RAS e IAS**, quindi scegliere **Proprietà**. Verrà visualizzata la finestra di dialogo **Proprietà server RAS e IAS** .

4. Nella finestra di dialogo **Proprietà server RAS e IAS** fare clic sulla scheda **membri** , aggiungere ogni NPSs che si desidera registrare nel dominio, quindi fare clic su **OK**.


### <a name="to-register-an-nps-in-another-domain-by-using-netsh-commands-for-nps"></a>Per registrare un server dei criteri di dominio in un altro dominio usando i comandi Netsh per NPS

1. Aprire il prompt dei comandi o Windows PowerShell. 

2. Al prompt dei comandi digitare quanto segue: **netsh NPS add registeredserver** &nbsp;*Domain* &nbsp;*server*, quindi premere INVIO.

>[!NOTE]
>Nel comando precedente, *Domain* è il nome di dominio DNS del dominio in cui si vuole registrare il server dei criteri di servizio e *Server* è il nome del computer NPS.

