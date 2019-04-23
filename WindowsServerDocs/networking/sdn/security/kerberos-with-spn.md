---
title: Kerberos con nome dell'entità servizio (SPN)
description: Controller di rete supporta più metodi di autenticazione per la comunicazione con i client di gestione. È possibile usare l'autenticazione basata su Kerberos, X509 l'autenticazione basata su certificato. È anche possibile usare senza autenticazione per le distribuzioni di test.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: bc625de9-ee31-40a4-9ad2-7448bfbfb6e6
ms.author: pashort
author: shortpatti
ms.date: 08/23/2018
ms.openlocfilehash: 2459cfa8dfec3de4aa23da7aba192d6eeed7f8ec
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828972"
---
# <a name="kerberos-with-service-principal-name-spn"></a>Kerberos con nome dell'entità servizio (SPN)

>Si applica a: Windows Server 2019

Controller di rete supporta più metodi di autenticazione per la comunicazione con i client di gestione. È possibile usare l'autenticazione basata su Kerberos, X509 l'autenticazione basata su certificato. È anche possibile usare senza autenticazione per le distribuzioni di test.

System Center Virtual Machine Manager utilizza l'autenticazione basata su Kerberos. Se si usa l'autenticazione basata su Kerberos, è necessario configurare un nome dell'entità servizio (SPN) per il Controller di rete in Active Directory. Il nome SPN è un identificatore univoco per l'istanza del servizio Controller di rete, che viene usato dall'autenticazione Kerberos per associare un'istanza del servizio con un account di accesso del servizio. Per altre informazioni, vedere [Service Principal Names](https://docs.microsoft.com/windows/desktop/ad/service-principal-names).

## <a name="configure-service-principal-names-spn"></a>Configurare nomi dell'entità servizio (SPN)

Il Controller di rete configura automaticamente il nome SPN. È sufficiente eseguire consiste nel fornire le autorizzazioni per le macchine di Controller di rete registrare e modificare il nome SPN.

1.  Nel computer del Controller di dominio, avviare **Active Directory Users and Computers**.

2.  Selezionare **View \> Advanced**.

3.  Sotto **computer**, uno degli account computer del Controller di rete, individuare e quindi fare doppio clic e selezionare **proprietà**.

4.  Selezionare il **sicurezza** scheda e fare clic su **avanzate**.

5.  Nell'elenco, se tutti gli account computer del Controller di rete o sicurezza gruppo con tutti gli account computer del Controller di rete non è elencato, fare clic su **Add** per aggiungerlo.

6.  Per ogni account del computer Controller di rete o un singolo gruppo di sicurezza contenenti gli account computer Controller di rete:

    a.  Selezionare l'account o gruppo e fare clic su **modifica**.

    b.  In selezionare le autorizzazioni **convalidare Write servicePrincipalName**.

    d.  Scorrere verso il basso e sotto **proprietà** selezionare:

       -  **Lettura servicePrincipalName**

       -  **Write servicePrincipalName**

    e.  Fare clic su **OK** due volte.

7.  Ripetere il passaggio 3 a 6 per ogni computer Controller di rete.

8.  Chiudere **Utenti e computer di Active Directory**.

## <a name="failure-to-provide-permissions-for-spn-registrationmodification"></a>Se non vengono fornite le autorizzazioni per la registrazione SPN/modifica

In un **NEW** si verifica un errore di distribuzione di Windows Server 2019, se si sceglie di Kerberos per l'autenticazione di client REST e non concedere l'autorizzazione per i nodi di Controller di rete registrare o modificare il nome SPN, operazioni REST sui Controller di rete consente la gestione di rete SDN.

Per un aggiornamento da Windows Server 2016 per Windows Server 2019 e si sceglie Kerberos per l'autenticazione di client REST, le operazioni REST non bloccarsi, garantendo trasparenza per le distribuzioni di produzione esistenti. 

Se il SPN non è registrato, l'autenticazione del client REST utilizza NTLM, che è meno sicuro. Otterrai anche un evento critico nel canale di amministrazione di **NetworkController-Framework** canale degli eventi in cui viene chiesto di fornire le autorizzazioni per i nodi di Controller di rete di registrare SPN. Dopo aver specificato l'autorizzazione, Controller di rete registra automaticamente il nome SPN e Kerberos di usare tutte le operazioni client.


>[!TIP]
>In genere, è possibile configurare il Controller di rete per usare un indirizzo IP o nome DNS per operazioni basate su REST. Tuttavia, quando si configura Kerberos, è possibile utilizzare un indirizzo IP per query REST nel Controller di rete. Ad esempio, è possibile usare \< https://networkcontroller.consotso.com\>, ma non è possibile usare \< https://192.34.21.3\>. Nomi dell'entità servizio non può funzionare se vengono usati gli indirizzi IP.
>
>Se si Usa indirizzo IP per le operazioni REST con l'autenticazione Kerberos in Windows Server 2016, la comunicazione effettiva sarebbe stato rispetto all'autenticazione NTLM. In questo tipo di distribuzione, dopo l'aggiornamento a Windows Server 2019, continuare a utilizzare l'autenticazione basata su NTLM. Per spostare l'autenticazione basata su Kerberos, è necessario usare il nome DNS di Controller di rete per le operazioni REST e fornire le autorizzazioni per i nodi di Controller di rete di registrare SPN.

---