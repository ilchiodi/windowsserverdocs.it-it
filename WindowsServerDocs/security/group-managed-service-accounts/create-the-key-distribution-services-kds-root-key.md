---
title: Creare la chiave radice di distribuzione di chiavi servizi KDS
description: Protezione di Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-gmsa
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 42e5db8f-1516-4d42-be0a-fa932f5588e9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 30075e56f3ca8e90a0655508efeacfcf2aaa0337
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="create-the-key-distribution-services-kds-root-key"></a>Creare la chiave radice di distribuzione di chiavi servizi KDS

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

This topic for the IT professional describes how to create a Microsoft Key Distribution Service (kdssvc.dll) root key on the domain controller using Windows PowerShell to generate group Managed Service Account passwords in Windows Server 2012.

 Windows Server 2012 controller di dominio (DC) richiedono una chiave radice per avviare la generazione di password gestito. I controller di dominio attenderà fino a 10 ore dal momento della creazione per permettere a tutti i controller di dominio far convergere la replica di Active Directory prima di consentire la creazione di un account. 10 ore è una misura di sicurezza per evitare la generazione di password prima che tutti i controller di dominio nell'ambiente sono in grado di rispondere alle richieste gestito.  Se si tenta di utilizzare un gMSA troppo presto la chiave potrebbe non essere stata replicata i controller di dominio di tutti Windows Server 2012 e pertanto il recupero della password potrebbe non riuscire se l'host gestito tenta di recuperare la password. Errori di recupero password gestito possono inoltre verificarsi quando si utilizzano i controller di dominio con pianificazioni di replica limitate o se esiste un problema di replica.

Appartenenza al gruppo di **Domain Admins** o **Enterprise Admins** gruppi, o equivalente è il requisito minimo necessario per completare questa procedura. For detailed information about using the appropriate accounts and group memberships, see [Local and Domain Default Groups](https://technet.microsoft.com/library/dd728026(WS.10).aspx).

> [!NOTE]
> Un'architettura a 64 bit è necessario per eseguire i comandi di Windows PowerShell che vengono utilizzati per amministrare account del servizio gestito del gruppo.

#### <a name="to-create-the-kds-root-key-using-the-new-kdsrootkey-cmdlet"></a>Per creare una chiave radice utilizzando il cmdlet New-KdsRootKey

1.  Nel controller di dominio di Windows Server 2012, eseguire Windows PowerShell dalla barra delle applicazioni.

2.  Al prompt dei comandi per il modulo Active Directory per Windows PowerShell, digitare i comandi seguenti e quindi premere INVIO:

    **Add-KdsRootKey -EffectiveImmediately**

    > [!TIP]
    > Il parametro tempo efficace utilizzabile per dare tempo per le chiavi saranno propagate a tutti i controller di dominio prima dell'uso. Using Add-KdsRootKey -EffectiveImmediately will add a root key to the target DC which will be used by the KDS service immediately. Tuttavia, altri controller di dominio di Windows Server 2012 non sarà in grado di utilizzare la chiave radice finché la replica ha esito positivo.

Negli ambienti di test con un solo controller di dominio, è possibile creare un'una chiave radice e impostare l'ora di inizio in passato per evitare l'attesa per la generazione di chiavi utilizzando la procedura seguente. Convalida che è stato registrato un evento 4004 nel registro eventi kds.

#### <a name="to-create-the-kds-root-key-in-a-test-environment-for-immediate-effectiveness"></a>Per creare una chiave radice in un ambiente di test per l'uso

1.  Nel controller di dominio di Windows Server 2012, eseguire Windows PowerShell dalla barra delle applicazioni.

2.  Al prompt dei comandi per il modulo Active Directory per Windows PowerShell, digitare i comandi seguenti e quindi premere INVIO:

    **$un = Get-Date**

    **$b=$a.AddHours(-10)**

    **Add-KdsRootKey -EffectiveTime $b**

    Oppure utilizzare un singolo comando

    **Add-KdsRootKey -EffectiveTime ((get-date).addhours(-10))**

## <a name="see-also"></a>Vedere anche
[Guida introduttiva a gruppo di account dei servizi gestiti](getting-started-with-group-managed-service-accounts.md)


