---
title: Kerberos con nome dell'entità servizio (SPN)
description: Il controller di rete supporta più metodi di autenticazione per la comunicazione con i client di gestione. È possibile usare l'autenticazione basata su Kerberos, l'autenticazione basata su certificati X509. È anche possibile usare nessuna autenticazione per le distribuzioni di test.
manager: grcusanz
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: bc625de9-ee31-40a4-9ad2-7448bfbfb6e6
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/23/2018
ms.openlocfilehash: 12ad2b27d275c074e0d8baacccd864e8926f405f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854374"
---
# <a name="kerberos-with-service-principal-name-spn"></a>Kerberos con nome dell'entità servizio (SPN)

>Si applica a: Windows Server 2019

Il controller di rete supporta più metodi di autenticazione per la comunicazione con i client di gestione. È possibile usare l'autenticazione basata su Kerberos, l'autenticazione basata su certificati X509. È anche possibile usare nessuna autenticazione per le distribuzioni di test.

System Center Virtual Machine Manager utilizza l'autenticazione basata su Kerberos. Se si usa l'autenticazione basata su Kerberos, è necessario configurare un nome dell'entità servizio (SPN) per il controller di rete in Active Directory. Il nome SPN è un identificatore univoco per l'istanza del servizio di controller di rete, che viene utilizzata dall'autenticazione Kerberos per associare un'istanza del servizio a un account di accesso al servizio. Per ulteriori informazioni, vedere [nomi dell'entità servizio](https://docs.microsoft.com/windows/desktop/ad/service-principal-names).

## <a name="configure-service-principal-names-spn"></a>Configurare i nomi dell'entità servizio (SPN)

Il controller di rete configura automaticamente il nome SPN. È sufficiente fornire le autorizzazioni per la registrazione e la modifica del nome SPN da parte dei computer del controller di rete.

1.  Nel computer controller di dominio avviare **Active Directory utenti e computer**.

2.  Selezionare **visualizza \> avanzate**.

3.  In **computer**individuare uno degli account del computer del controller di rete, quindi fare clic con il pulsante destro del mouse e scegliere **Proprietà**.

4.  Selezionare la scheda **sicurezza** e fare clic su **Avanzate**.

5.  Nell'elenco, se non sono elencati tutti gli account del computer del controller di rete o un gruppo di sicurezza con tutti gli account del computer del controller di rete, fare clic su **Aggiungi** per aggiungerlo.

6.  Per ogni account computer del controller di rete o un singolo gruppo di sicurezza contenente gli account del computer del controller di rete:

    a.  Selezionare l'account o il gruppo e fare clic su **modifica**.

    b.  In autorizzazioni selezionare **convalida scrittura servicePrincipalName**.

    d.  Scorrere verso il basso e sotto **Proprietà** selezionare:

       -  **Leggi servicePrincipalName**

       -  **Scrivi servicePrincipalName**

    e.  Fare clic su **OK** due volte.

7.  Ripetere il passaggio 3-6 per ogni computer del controller di rete.

8.  Chiudere **Utenti e computer di Active Directory**.

## <a name="failure-to-provide-permissions-for-spn-registrationmodification"></a>Impossibile fornire le autorizzazioni per la registrazione o la modifica del nome SPN

In una **nuova** distribuzione di Windows Server 2019, se si sceglie Kerberos per l'autenticazione client REST e non si concede l'autorizzazione per i nodi del controller di rete per la registrazione o la modifica del nome SPN, le operazioni REST sul controller di rete non riescono a impedire la gestione di Sdn.

Per un aggiornamento da Windows Server 2016 a Windows Server 2019 e si è scelto Kerberos per l'autenticazione client REST, le operazioni REST non vengono bloccate, garantendo la trasparenza per le distribuzioni di produzione esistenti. 

Se SPN non è registrato, l'autenticazione client REST usa NTLM, che è meno sicuro. Si riceve anche un evento critico nel canale di amministrazione del canale degli eventi **NetworkController-Framework,** in cui viene chiesto di fornire le autorizzazioni per i nodi del controller di rete per registrare il nome SPN. Una volta fornita l'autorizzazione, il controller di rete registra automaticamente il nome SPN e tutte le operazioni client utilizzano Kerberos.


>[!TIP]
>In genere, è possibile configurare il controller di rete per usare un indirizzo IP o un nome DNS per le operazioni basate su REST. Tuttavia, quando si configura Kerberos, non è possibile usare un indirizzo IP per le query REST per il controller di rete. È ad esempio possibile utilizzare \<https://networkcontroller.consotso.com\>, ma non è possibile utilizzare \<https://192.34.21.3\>. I nomi dell'entità servizio non possono funzionare se si usano indirizzi IP.
>
>Se si usa l'indirizzo IP per le operazioni REST insieme all'autenticazione Kerberos in Windows Server 2016, la comunicazione effettiva sarebbe stata rispetto all'autenticazione NTLM. Quando si esegue l'aggiornamento a Windows Server 2019 in una distribuzione di questo tipo, si continua a usare l'autenticazione basata su NTLM. Per passare all'autenticazione basata su Kerberos, è necessario usare il nome DNS del controller di rete per le operazioni REST e fornire l'autorizzazione per i nodi del controller di rete per la registrazione del nome SPN.

---