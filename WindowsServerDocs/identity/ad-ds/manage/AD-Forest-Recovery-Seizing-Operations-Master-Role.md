---
title: Ripristino della foresta Active Directory - requisizione un ruolo di Master operazioni
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 7e6bb370-f840-4416-b5e2-86b0ba715f4f
ms.technology: identity-adfs
ms.openlocfilehash: 7ca6e3746586feeb3573b1ad6ba02831d1f5addd
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---seizing-an-operations-master-role"></a>Ripristino della foresta Active Directory - requisizione un ruolo di master operazioni  

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

 Utilizzare la procedura seguente per assegnare un ruolo di master operazioni (noto anche come un ruolo (FSMO FLEXIBLE) single master operations). È possibile utilizzare Ntdsutil.exe, uno strumento da riga di comando che viene installato automaticamente in tutti i controller di dominio.  
  
## <a name="to-seize-an-operations-master-role"></a>Per assegnare un ruolo di master operazioni  
  
1.  Al prompt dei comandi, digitare il comando seguente e quindi premere INVIO:  
  
    ```  
    ntdsutil  
    ```  
  
2.  Nel **ntdsutil:** prompt, digitare il comando seguente e quindi premere INVIO:  
  
    ```  
    roles  
    ```  
  
3.  Nel **FSMO maintenance:** prompt, digitare il comando seguente e quindi premere INVIO:  
  
    ```  
    connections  
    ```  
  
4.  Nel **connessioni server:** prompt, digitare il comando seguente e quindi premere INVIO:  
  
    ```  
    Connect to server ServerFQDN  
    ```  
  
     Dove *ServerFQDN* è il nome di dominio completo (FQDN) di questo controller di dominio, ad esempio: **connettersi al server nycdc01.example.com**.  
  
     Se *ServerFQDN* non riuscita, utilizzare il nome NetBIOS del controller di dominio.  
  
5.  Nel **connessioni server:** prompt, digitare il comando seguente e quindi premere INVIO:  
  
    ```  
    quit  
    ```  
  
6.  A seconda del ruolo che si desidera assegnare, nel **FSMO maintenance:** prompt, digitare il comando appropriato, come descritto nella tabella seguente e quindi premere INVIO.  
  
    |Ruolo|Credenziali|Comando|  
    |----------|-----------------|-------------|  
    |Master denominazione domini|Enterprise Admins|**Riassegnare master denominazione**|  
    |Master schema|Schema Admins|**Riassegnare master schema**|  
    |Master infrastrutture **Nota:** dopo requisire il ruolo di master infrastrutture, si verifichi un errore in un secondo momento se è necessario eseguire Adprep /Rodcprep. Per ulteriori informazioni, vedere l'articolo [949257](https://support.microsoft.com/kb/949257).|Domain Admins|**Riassegnare master infrastrutture**|  
    |Master dell'emulatore PDC|Domain Admins|**Riassegnare pdc**|  
    |Master RID|Domain Admins|**Riassegnare master rid**|  
  
     Dopo aver confermato la richiesta, Active Directory o dominio Active Directory tenta di trasferire il ruolo. Quando il trasferimento non riesce, vengono visualizzate alcune informazioni di errore e di dominio Active Directory o Active Directory procede con la requisizione. Una volta completata la requisizione, viene visualizzato un elenco di ruoli e il nome di Lightweight Directory Access Protocol (LDAP) del server che svolge ogni ruolo. È inoltre possibile eseguire **Netdom Query FSMO** in un prompt dei comandi con privilegi elevati per verificare i titolari del ruolo corrente.  
  
    > [!NOTE]
    >  Se il computer non è un ruolo di master RID prima dell'errore e si tenta di requisire il ruolo di master RID, il computer tenta di sincronizzare con un partner di replica prima di accettare questo ruolo. Tuttavia, poiché questo passaggio viene eseguito quando il computer è isolato, non avrà successo la sincronizzazione con un partner. Di conseguenza, una finestra di dialogo viene visualizzata in cui viene richiesto se si desidera continuare l'operazione anche se il computer non è in grado di sincronizzare con un partner. Fare clic su **Sì**.  
  
## <a name="next-steps"></a>Passaggi successivi

- [Guida di ripristino della foresta Active Directory](AD-Forest-Recovery-Guide.md)
- [Ripristino della foresta Active Directory - procedure](AD-Forest-Recovery-Procedures.md)
