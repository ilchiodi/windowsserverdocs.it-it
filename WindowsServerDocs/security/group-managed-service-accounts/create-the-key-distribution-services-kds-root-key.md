---
title: Creare la chiave radice KDS dei servizi distribuzione chiavi
description: Sicurezza di Windows Server
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
ms.openlocfilehash: 3d5f7b46b28e6a2fbfafb664b69aebc8d34886fe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867212"
---
# <a name="create-the-key-distribution-services-kds-root-key"></a>Creare la chiave radice KDS dei servizi distribuzione chiavi

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

In questo argomento destinato ai professionisti IT descrive come creare una chiave radice del servizio distribuzione chiavi Microsoft (kdssvc. dll) sul controller di dominio tramite Windows PowerShell per generare le password Account del servizio gestito di gruppo in Windows Server 2012.

 Windows Server 2012 controller di dominio (DC) richiedono una chiave radice per avviare la generazione di password gMSA. I controller di dominio aspetteranno fino a 10 ore dal momento della creazione per permettere a tutti i controller di dominio di far convergere le repliche di Active Directory, prima di consentire la creazione di un account del servizio gestito del gruppo. L'intervallo di 10 ore è una misura di sicurezza che impedisce la generazione di password prima che tutti i controller di dominio nell'ambiente siano in grado di rispondere alle richieste di account del servizio gestito del gruppo.  Se si prova a usare un account gMSA troppo presto la chiave potrebbe non essere stata replicata i controller di dominio di tutti i Windows Server 2012 e pertanto il recupero della password potrebbe avere esito negativo quando l'host gMSA tenta di recuperare la password. errori di recupero di password gMSA possono verificarsi anche quando si usano controller di dominio con pianificazioni di replica limitate o se si verifica un problema di replica.

Per eseguire questa procedura, è richiesta almeno l'appartenenza al gruppo **Domain Admins** o **Enterprise Admins** oppure a un gruppo equivalente. Per altre informazioni sull'uso degli account appropriati e sull'appartenenza a gruppi, vedere [Gruppi predefiniti locali e di dominio](https://technet.microsoft.com/library/dd728026(WS.10).aspx).

> [!NOTE]
> Per eseguire i comandi di Windows PowerShell necessari per amministrare account del servizio gestito del gruppo, è richiesta un'architettura a 64 bit.

#### <a name="to-create-the-kds-root-key-using-the-add-kdsrootkey-cmdlet"></a>Per creare la chiave radice KDS usando il cmdlet Add-KdsRootKey

1.  Nel controller di dominio di Windows Server 2012, eseguire il comando di Windows PowerShell dalla barra delle applicazioni.

2.  Al prompt dei comandi del modulo Active Directory per Windows PowerShell, digitare i comandi seguenti, quindi premere INVIO:

    **Aggiungere-KdsRootKey - EffectiveImmediately**

    > [!TIP]
    > È possibile usare il parametro Tempo di validità per consentire la propagazione delle chiavi a tutti i controller di dominio prima dell'uso. Con Add-KdsRootKey - EffectiveImmediately aggiungerà una chiave radice alla destinazione controller di dominio che verrà usata immediatamente dal servizio distribuzione CHIAVI. Tuttavia, altri controller di dominio di Windows Server 2012 non sarà in grado di usare la chiave radice fino a quando la replica ha esito positivo.

Negli ambienti di test con un solo controller di dominio, è possibile creare una chiave radice del Servizio distribuzione chiavi e impostare un'ora di avvio già superata per evitare l'attesa dovuta alla generazione della chiave, applicando la procedura seguente. Convalidare la registrazione di un evento 4004 nel registro eventi del Servizio distribuzione chiavi.

#### <a name="to-create-the-kds-root-key-in-a-test-environment-for-immediate-effectiveness"></a>Per creare una chiave radice del Servizio distribuzione chiavi pronta per l'uso in un ambiente di test

1.  Nel controller di dominio di Windows Server 2012, eseguire il comando di Windows PowerShell dalla barra delle applicazioni.

2.  Al prompt dei comandi del modulo Active Directory per Windows PowerShell, digitare i comandi seguenti, quindi premere INVIO:

    **$a=Get-Date**

    **$b=$a.AddHours(-10)**

    **Add-KdsRootKey -EffectiveTime $b**

    Oppure usare un singolo comando

    **Add-KdsRootKey -EffectiveTime ((get-date).addhours(-10))**

## <a name="see-also"></a>Vedere anche
[Introduzione a gruppo di account del servizio gestiti](getting-started-with-group-managed-service-accounts.md)


