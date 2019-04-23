---
title: Ripristino della foresta Active Directory - requisizione un ruolo di Master operazioni
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 7e6bb370-f840-4416-b5e2-86b0ba715f4f
ms.technology: identity-adds
ms.openlocfilehash: 1994d49652ee9eb10f6afc73cf5b4630b4718e77
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858462"
---
# <a name="ad-forest-recovery---seizing-an-operations-master-role"></a>Ripristino della foresta Active Directory - requisizione un ruolo di master operazioni  

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Utilizzare la procedura seguente per assegnare un ruolo di master operazioni (noto anche come un ruolo FSMO flexible single master operations ()). È possibile usare Ntdsutil.exe, uno strumento da riga di comando che viene installato automaticamente in tutti i controller di dominio.  
  
## <a name="to-seize-an-operations-master-role"></a>Per assegnare un ruolo di master operazioni  
  
1. Al prompt dei comandi digitare il comando seguente e quindi premere INVIO:  

   ```  
   ntdsutil  
   ```  

2. Nel **ntdsutil:** prompt, digitare il comando seguente e quindi premere INVIO:  

   ```  
   roles  
   ```  

3. Nel **manutenzione FSMO:** prompt, digitare il comando seguente e quindi premere INVIO:  

   ```  
   connections  
   ```  

4. Nel **le connessioni al server:** prompt, digitare il comando seguente e quindi premere INVIO:  

   ```  
   Connect to server ServerFQDN  
   ```  

   In cui *ServerFQDN* è il nome di dominio completo (FQDN) di questo controller di dominio, ad esempio: **connettersi al server nycdc01.example.com**.  

   Se *ServerFQDN* non abbia esito positivo, utilizzare il nome NetBIOS del controller di dominio.  

5. Nel **le connessioni al server:** prompt, digitare il comando seguente e quindi premere INVIO:  

   ```  
   quit  
   ```  

6. A seconda del ruolo che si desidera riassegnare, nelle **manutenzione FSMO:** prompt, digitare il comando appropriato, come descritto nella tabella seguente e quindi premere INVIO.  
  
|Ruolo|Credenziali|Comando|  
|----------|-----------------|-------------|  
|Master denominazione domini|Enterprise Admins|**Riassegnare master denominazione**|  
|Master schema|Schema Admins|**Riassegnare master schema**|  
|Master infrastrutture **Nota:**  Dopo aver requisire il ruolo di master infrastrutture, si verifichi un errore in un secondo momento se è necessario eseguire Adprep /Rodcprep. Per altre informazioni, vedere l'articolo della KB [949257](https://support.microsoft.com/kb/949257).|Domain Admins|**Riassegnare master infrastrutture**|  
|Master emulatore PDC|Domain Admins|**Seize pdc**|  
|Master RID di|Domain Admins|**Riassegnare master rid di**|  

Dopo aver confermato la richiesta, Active Directory o Active Directory Domain Services prova a trasferire il ruolo. Quando il trasferimento non riesce, vengono visualizzate alcune informazioni di errore e Active Directory o Active Directory Domain Services procede con la requisizione. Dopo aver completata la requisizione, viene visualizzato un elenco dei ruoli e il nome di Lightweight Directory Access Protocol (LDAP) del server che attualmente contiene ogni ruolo. È anche possibile eseguire **Netdom Query FSMO** a un prompt dei comandi con privilegi elevati per verificare i proprietari del ruolo corrente.  
  
> [!NOTE]
> Se il computer non era un master RID prima dell'errore e si tenta di assegnare il ruolo di master RID, il computer tenta di sincronizzare con un partner di replica prima di accettare questo ruolo. Tuttavia, poiché questo passaggio viene eseguito quando il computer è isolato, non riuscirà in sincronizzazione con un partner. Pertanto, una finestra di dialogo viene visualizzata in cui viene richiesto se si desidera continuare con l'operazione nonostante questo computer non riesca a eseguire la sincronizzazione con un partner. Scegliere **Sì**.  
  
## <a name="next-steps"></a>Passaggi successivi

- [Guida al ripristino della foresta AD](AD-Forest-Recovery-Guide.md)
- [Ripristino della foresta Active Directory - procedure](AD-Forest-Recovery-Procedures.md)
