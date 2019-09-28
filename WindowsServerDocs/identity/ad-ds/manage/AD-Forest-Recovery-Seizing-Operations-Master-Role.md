---
title: 'Ripristino della foresta di Active Directory: requisizione di un ruolo di master operazioni'
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 7e6bb370-f840-4416-b5e2-86b0ba715f4f
ms.technology: identity-adds
ms.openlocfilehash: 672dc119845acbe9cf38f82c793bd377d31db3b2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390281"
---
# <a name="ad-forest-recovery---seizing-an-operations-master-role"></a>Ripristino della foresta di Active Directory: requisizione di un ruolo di master operazioni  

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Utilizzare la procedura seguente per requisire un ruolo di master operazioni, noto anche come ruolo FSMO (Flexible Single Master Operations). È possibile utilizzare Ntdsutil. exe, uno strumento da riga di comando che viene installato automaticamente in tutti i controller di dominio.  
  
## <a name="to-seize-an-operations-master-role"></a>Per requisire un ruolo di master operazioni  
  
1. Al prompt dei comandi digitare il comando seguente e quindi premere INVIO:  

   ```  
   ntdsutil  
   ```  

2. Al prompt di **Ntdsutil:** Digitare il comando seguente e quindi premere INVIO:  

   ```  
   roles  
   ```  

3. Al prompt **manutenzione FSMO** Digitare il comando seguente e quindi premere INVIO:  

   ```  
   connections  
   ```  

4. Al prompt **Server Connections:** Digitare il comando seguente e quindi premere INVIO:  

   ```  
   Connect to server ServerFQDN  
   ```  

   Dove *serverfqdn* è il nome di dominio completo (FQDN) del controller di dominio, ad esempio: **connetti al server nycdc01.example.com**.  

   Se *serverfqdn* ha esito negativo, usare il nome NetBIOS del controller di dominio.  

5. Al prompt **Server Connections:** Digitare il comando seguente e quindi premere INVIO:  

   ```  
   quit  
   ```  

6. A seconda del ruolo che si desidera requisire, al prompt di **manutenzione FSMO** , digitare il comando appropriato come descritto nella tabella seguente, quindi premere INVIO.  
  
|Role|Credenziali|Comando|  
|----------|-----------------|-------------|  
|Master per la denominazione dei domini|Enterprise Admins|**Requisire il master di denominazione**|  
|Master schema|Schema Admins|**Cogli schema master**|  
|Nota master infrastrutture **:**  Dopo aver ripreso il ruolo di master infrastrutture, è possibile che venga visualizzato un errore in un secondo momento se è necessario eseguire adprep/rodcprep. Per ulteriori informazioni, vedere l'articolo KB [949257](https://support.microsoft.com/kb/949257).|Domain Admins|**Requisire il master infrastrutture**|  
|Master emulatore PDC|Domain Admins|**Requisire PDC**|  
|Master RID di|Domain Admins|**Requisire il master RID**|  

Una volta confermata la richiesta, Active Directory o servizi di dominio Active Directory tenta di trasferire il ruolo. Quando il trasferimento non riesce, vengono visualizzate alcune informazioni sull'errore e Active Directory o servizi di dominio Active Directory procede con il grippaggio. Una volta completato il grippaggio, viene visualizzato un elenco dei ruoli e il nome LDAP (Lightweight Directory Access Protocol) del server che attualmente include ogni ruolo. È anche possibile eseguire **netdom query FSMO** a un prompt dei comandi con privilegi elevati per verificare i titolari dei ruoli correnti.  
  
> [!NOTE]
> Se il computer non è un master RID prima dell'errore e si tenta di riassegnare il ruolo di master RID, il computer tenterà di eseguire la sincronizzazione con un partner di replica prima di accettare questo ruolo. Tuttavia, poiché questo passaggio viene eseguito quando il computer è isolato, non riuscirà a eseguire la sincronizzazione con un partner. Viene quindi visualizzata una finestra di dialogo in cui viene chiesto se si desidera continuare l'operazione nonostante il computer non sia in grado di eseguire la sincronizzazione con un partner. Scegliere **Sì**.  
  
## <a name="next-steps"></a>Passaggi successivi

- [Guida al ripristino della foresta di Active Directory](AD-Forest-Recovery-Guide.md)
- [Ripristino della foresta di Active Directory - Procedure](AD-Forest-Recovery-Procedures.md)
