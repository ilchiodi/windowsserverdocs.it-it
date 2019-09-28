---
title: PASSAGGIO 5 configurare DC1
description: 'Questo argomento fa parte della Guida al Lab di test: illustra una distribuzione multisito di DirectAccess per Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 70357156-fcb0-4346-a61e-4ea963e3ffb0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: aa251ccc0cc48e3805667a247047711c2ae4fcf6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388296"
---
# <a name="step-5-configure-dc1"></a>PASSAGGIO 5 configurare DC1

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

DC1 funge da controller di dominio, server DNS e server DHCP per il dominio corp.contoso.com.  
  
Per configurare l'accesso remoto per l'utilizzo di una topologia multisito, è necessario aggiungere un ulteriore sito di Active Directory Domain Services (AD DS) per il secondo controller di dominio 2-DC1 e per configurare il routing tra le subnet.  
  
1. Per configurare il gateway predefinito nel controller di dominio. Configurare il gateway predefinito in DC1.  
  
2. Creare gruppi di sicurezza per i client DirectAccess di Windows 7 in DC1. Quando DirectAccess è configurato, vengono creati automaticamente oggetti Criteri di gruppo (GPO) e impostazioni dell'oggetto Criteri di gruppo che vengono applicati ai server e ai client DirectAccess. L'oggetto Criteri di gruppo del client DirectAccess viene applicato a gruppi di sicurezza Active Directory specifici.  
  
3. Per aggiungere un nuovo sito di servizi di dominio Active Directory. Creare un secondo sito di servizi di dominio Active Directory.  
  
## <a name="to-configure-the-default-gateway-on-the-domain-controller"></a>Per configurare il gateway predefinito nel controller di dominio  
  
1.  Nella console di Server Manager fare clic su **server locale**e quindi nell'area **Proprietà** , accanto a **connessione Ethernet cablata**, fare clic sul collegamento.  
  
2.  Nella finestra connessioni di rete fare clic con il pulsante destro del mouse su **connessione Ethernet cablata**e quindi scegliere **Proprietà**.  
  
3.  Fare clic su **Protocollo Internet versione 4 (TCP/IPv4)** e quindi su **Proprietà**.  
  
4.  In **gateway predefinito**Digitare **10.0.0.254**e in **server DNS alternativo**digitare **10.2.0.1**e quindi fare clic su **OK**.  
  
5.  Fare clic su **Protocollo Internet versione 6 (TCP/IPv6)** , quindi fare clic su **Proprietà**.  
  
6.  In **gateway predefinito**Digitare **2001: DB8:1:: Fe**e in **server DNS alternativo**digitare **2001: DB8:2:: 1**, quindi fare clic su **OK**.  
  
7.  Nella finestra di dialogo **Proprietà connessione Ethernet cablata** fare clic su **Chiudi**.  
  
8.  Chiudere la finestra **Connessioni di rete**.  
  
## <a name="create-security-groups-for-windows-7-directaccess-clients-on-dc1"></a>Creare gruppi di sicurezza per i client DirectAccess di Windows 7 in DC1  
Creare i gruppi di sicurezza DirectAccess per Windows 7 con la procedura riportata di seguito.  
  
 I computer client Windows 7 devono essere membri di gruppi di sicurezza separati perché sono in grado di connettersi alle risorse interne tramite un solo punto di ingresso. Quando si Abilita il supporto multisito o si aggiungono punti di ingresso, se è richiesto il supporto per Windows 7, un oggetto Criteri di gruppo separato verrà creato automaticamente da DirectAccess per i client Windows 7 per ogni punto di ingresso.  
  
### <a name="create-security-groups"></a>Creazione di gruppi di sicurezza  
  
1.  Nel **avviare** digitare**DSA. msc**, quindi premere INVIO.  
  
2.  Nel riquadro sinistro espandere **Corp.contoso.com**, fare clic su **utenti**, quindi fare clic con il pulsante destro del mouse su **utenti**, scegliere **nuovo**, quindi fare clic su **gruppo**.  
  
3.  Nella finestra di dialogo **nuovo oggetto-gruppo** , in **nome gruppo**, immettere **Win7_Clients_Site1**.  
  
4.  In **ambito del gruppo**, fare clic su **globale**, in **tipo di gruppo**, fare clic su **sicurezza**, quindi fare clic su **OK**.  
  
5.  Fare doppio clic sul gruppo di sicurezza **Win7_Clients_Site1** , quindi nella finestra di dialogo **Proprietà Win7_Clients_Site1** fare clic sulla scheda **membri** .  
  
6.  Nella scheda **Membri** fare clic su **Aggiungi**.  
  
7.  Nella finestra di dialogo **Seleziona utenti, contatti, computer o account di servizio** fare clic su **tipi di oggetto**. Nella finestra di dialogo **tipi di oggetto** selezionare **computer**e quindi fare clic su **OK**.  
  
8.  In **immettere i nomi degli oggetti da selezionare**Digitare **CLIENT2**, quindi fare clic su **OK**, quindi nella finestra di dialogo **Proprietà Win7_Clients_Site1** fare clic su **OK**.  
  
9. Nel riquadro sinistro della console **utenti e computer Active Directory** fare clic con il pulsante destro del mouse su **utenti**, scegliere **nuovo**, quindi fare clic su **gruppo**.  
  
10. Nella finestra di dialogo **nuovo oggetto-gruppo** , in **nome gruppo**, immettere **Win7_Clients_Site2**.  
  
11. In **ambito del gruppo**, fare clic su **globale**, in **tipo di gruppo**, fare clic su **sicurezza**, quindi fare clic su **OK**.  
  
12. Chiudere la console **Utenti e computer di Active Directory** .  
  
## <a name="to-add-a-new-ad-ds-site"></a>Per aggiungere un nuovo sito di servizi di dominio Active Directory  
  
1.  Nella schermata **Start** Digitare**Dssite. msc**, quindi premere INVIO.  
  
2.  Nella console di Active Directory Sites and Services, nell'albero della console, fare doppio clic su **siti**, quindi fare clic su **nuovo sito**.  
  
3.  Nella finestra di dialogo **nuovo oggetto-sito** digitare **Second-site**nella casella **nome** .  
  
4.  Nella casella di riepilogo fare clic su **DEFAULTIPSITELINK**, quindi fare clic su **OK** due volte.  
  
5.  Nell'albero della console, espandere **siti**, fare doppio clic su **subnet**, quindi fare clic su **nuova Subnet**.  
  
6.  Nella finestra di dialogo **nuovo oggetto-subnet** , in **prefisso**, digitare **10.0.0.0/24**nell'elenco **selezionare un oggetto sito per questo prefisso** , fare clic su **Default-First-Site-Name**, quindi fare clic su **OK**.  
  
7.  Nell'albero della console fare clic con il pulsante destro del mouse su **subnet**, quindi scegliere **Nuova Subnet**.  
  
8.  Nella finestra di dialogo **nuovo oggetto-subnet** , in **prefisso**, digitare **2001: DB8:1::/64**, nell'elenco **selezionare un oggetto sito per questo prefisso** , fare clic su **Default-First-Site-Name**, quindi fare clic su **OK**.  
  
9. Nell'albero della console fare clic con il pulsante destro del mouse su **subnet**, quindi scegliere **Nuova Subnet**.  
  
10. Nella finestra di dialogo **nuovo oggetto-subnet** , in **prefisso**, digitare **10.2.0.0/24**nell'elenco **selezionare un oggetto sito per questo prefisso** , fare clic su **secondo-sito**, quindi fare clic su **OK**.  
  
11. Nell'albero della console fare clic con il pulsante destro del mouse su **subnet**, quindi scegliere **Nuova Subnet**.  
  
12. Nella finestra di dialogo **nuovo oggetto-subnet** , in **prefisso**, digitare **2001: DB8:2::/64**, nell'elenco **selezionare un oggetto sito per il prefisso** , fare clic su **secondo-sito**, quindi fare clic su **OK**.  
  
13. Chiudere siti e servizi Active Directory.  
  


