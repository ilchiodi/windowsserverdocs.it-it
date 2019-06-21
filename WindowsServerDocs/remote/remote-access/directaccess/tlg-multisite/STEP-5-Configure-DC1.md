---
title: PASSAGGIO 5 configurare DC1
description: 'Questo argomento fa parte della Guida al Lab di Test: dimostrare una distribuzione multisito DirectAccess per Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 70357156-fcb0-4346-a61e-4ea963e3ffb0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 108e517923c75f685d817cdf9fad9b14132e3bb0
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281438"
---
# <a name="step-5-configure-dc1"></a>PASSAGGIO 5 configurare DC1

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

DC1 agisce come un controller di dominio, server DNS e server DHCP per il dominio corp.contoso.com.  
  
Per configurare accesso remoto per usare una topologia multisita, è necessario aggiungere un sito di Active Directory Domain Services (AD DS) aggiuntivo per il secondo dominio controller 2-DC1 e configurare il routing tra le subnet.  
  
1. Per configurare il gateway predefinito sul controller di dominio. Configurare il gateway predefinito in DC1.  
  
2. Creare gruppi di sicurezza per i client DirectAccess di Windows 7 in DC1. Quando DirectAccess è configurato, vengono creati automaticamente oggetti Criteri di gruppo (GPO) e le impostazioni oggetto Criteri di gruppo che vengono applicate ai client DirectAccess e server. Il client DirectAccess oggetti Criteri di gruppo viene applicato a specifici gruppi di sicurezza di Active Directory.  
  
3. Per aggiungere un nuovo sito di Active Directory Domain Services. Creare un secondo sito Active Directory Domain Services.  
  
## <a name="to-configure-the-default-gateway-on-the-domain-controller"></a>Per configurare il gateway predefinito sul controller di dominio  
  
1.  Nella console di Server Manager, fare clic su **Server locale**e quindi il **delle proprietà** area, accanto a **connessione Ethernet cablata**, fare clic sul collegamento.  
  
2.  Nella finestra di connessioni di rete, fare doppio clic su **connessione Ethernet cablata**, quindi fare clic su **proprietà**.  
  
3.  Fare clic su **Protocollo Internet versione 4 (TCP/IPv4)** e quindi su **Proprietà**.  
  
4.  In **gateway predefinito**, digitare **10.0.0.254**e in **server DNS alternativo**, tipo **10.2.0.1**, quindi fare clic su **OK** .  
  
5.  Fare clic su **Protocollo Internet versione 6 (TCP/IPv6)** , quindi fare clic su **Proprietà**.  
  
6.  Nella **gateway predefinito**, digitare **2001:db8:1::fe**e in **server DNS alternativo**, digitare **2001:db8:2::1**e quindi fare clic su **OK**.  
  
7.  Nel **proprietà di connessione Ethernet cablata** finestra di dialogo, fare clic su **Chiudi**.  
  
8.  Chiudere la finestra **Connessioni di rete**.  
  
## <a name="create-security-groups-for-windows-7-directaccess-clients-on-dc1"></a>Creare gruppi di sicurezza per i client DirectAccess di Windows 7 su DC1  
Creare i gruppi di sicurezza di DirectAccess per Windows 7 con la procedura seguente.  
  
 I computer client Windows 7 devono essere membri dei gruppi di sicurezza separati in quanto sono in grado di connettersi alle risorse interne tramite un singolo punto di ingresso solo. Quando l'attivazione del supporto multisito o l'aggiunta di voce punta, se è richiesto il supporto di Windows 7, quindi un oggetto Criteri di gruppo separati verrà automaticamente creato dai client DirectAccess per Windows 7 per ogni punto di ingresso.  
  
### <a name="create-security-groups"></a>Creare gruppi di sicurezza  
  
1.  Nel **avviare** digitare**DSA. msc**, quindi premere INVIO.  
  
2.  Nel riquadro sinistro, espandere **corp.contoso.com**, fare clic su **Users**, quindi fare doppio clic su **utenti**, scegliere **New**e quindi fare clic su **Gruppo**.  
  
3.  Nel **nuovo oggetto - gruppo** nella finestra di dialogo **nome gruppo**, immettere **Win7_Clients_Site1**.  
  
4.  In **ambito del gruppo**, fare clic su **globale**, in **tipo di gruppo**, fare clic su **sicurezza**, quindi fare clic su **OK**.  
  
5.  Fare doppio clic sul **Win7_Clients_Site1** gruppo di sicurezza e nella **proprietà Win7_Clients_Site1** nella finestra di dialogo fare clic sul **membri** scheda.  
  
6.  Nella scheda **Membri** fare clic su **Aggiungi**.  
  
7.  Nel **Seleziona utenti, contatti, computer o gli account del servizio** finestra di dialogo, fare clic su **tipi di oggetto**. Nel **tipi di oggetti** finestra di dialogo **computer**e quindi fare clic su **OK**.  
  
8.  In **immettere i nomi degli oggetti da selezionare**, digitare **client2**e quindi fare clic su **OK**, quindi nel **Win7_Clients_Site1 proprietà** finestra di dialogo Fare clic su casella **OK**.  
  
9. Nel **Active Directory Users and Computers** nel riquadro sinistro della console fare doppio clic su **utenti**, scegliere **New**e quindi fare clic su **gruppo** .  
  
10. Nel **nuovo oggetto - gruppo** nella finestra di dialogo **nome gruppo**, immettere **Win7_Clients_Site2**.  
  
11. In **ambito del gruppo**, fare clic su **globale**, in **tipo di gruppo**, fare clic su **sicurezza**, quindi fare clic su **OK**.  
  
12. Chiudere la console **Utenti e computer di Active Directory** .  
  
## <a name="to-add-a-new-ad-ds-site"></a>Per aggiungere un nuovo sito di Active Directory Domain Services  
  
1.  Nel **avviare** digitare**Dssite. msc**, quindi premere INVIO.  
  
2.  Nella console di Active Directory Sites and Services, nell'albero della console, fare doppio clic su **siti**, quindi fare clic su **nuovo sito**.  
  
3.  Nel **nuovo oggetto - sito** nella finestra di dialogo il **Name** , digitare **secondo sito**.  
  
4.  Nella casella di riepilogo, fare clic su **DEFAULTIPSITELINK**, quindi fare clic su **OK** due volte.  
  
5.  Nell'albero della console, espandere **siti**, fare doppio clic su **subnet**, quindi fare clic su **nuova Subnet**.  
  
6.  Nel **nuovo oggetto - Subnet** nella finestra di dialogo **prefisso**, tipo **10.0.0.0/24**, nel **selezionare un oggetto sito per il prefisso** elenco, fare clic su **Nome-predefinito-primo-sito**, quindi fare clic su **OK**.  
  
7.  Nell'albero della console, fare doppio clic su **subnet**, quindi fare clic su **nuova Subnet**.  
  
8.  Nel **nuovo oggetto - Subnet** nella finestra di dialogo **prefisso**, tipo **2001:db8:1:: 64**, nel **selezionare un oggetto sito per il prefisso** elenco, Fare clic su **nome-predefinito-primo-sito**, quindi fare clic su **OK**.  
  
9. Nell'albero della console, fare doppio clic su **subnet**, quindi fare clic su **nuova Subnet**.  
  
10. Nel **nuovo oggetto - Subnet** nella finestra di dialogo **prefisso**, tipo **10.2.0.0/24**, nel **selezionare un oggetto sito per il prefisso** elenco, fare clic su **Sito secondo**, quindi fare clic su **OK**.  
  
11. Nell'albero della console, fare doppio clic su **subnet**, quindi fare clic su **nuova Subnet**.  
  
12. Nel **nuovo oggetto - Subnet** nella finestra di dialogo **prefisso**, tipo **2001:DB8:2:::/:: 64**, nel **selezionare un oggetto sito per il prefisso** elenco, Fare clic su **sito secondo**, quindi fare clic su **OK**.  
  
13. Chiudere siti e servizi Active Directory.  
  


