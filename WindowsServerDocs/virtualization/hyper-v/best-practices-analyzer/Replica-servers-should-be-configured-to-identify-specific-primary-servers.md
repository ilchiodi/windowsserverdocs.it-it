---
title: Server di replica devono essere configurati per identificare i server primari specifici è autorizzato a inviare il traffico di replica
description: Fornisce le istruzioni per risolvere il problema segnalato da questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 0aeb1f4b-2e75-430b-9557-fe64738c4992
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 47b215d4c84e68d93ae1189ddd370358e2781eff
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822242"
---
# <a name="replica-servers-should-be-configured-to-identify-specific-primary-servers-authorized-to-send-replication-traffic"></a>Server di replica devono essere configurati per identificare i server primari specifici è autorizzato a inviare il traffico di replica

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**/ Funzionalità del prodotto**|Hyper-V|  
|**Severity**|Avviso|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
*In base alla configurazione, server di Replica accetta il traffico di replica da tutti i server primari e li archivia in un'unica posizione.*  
  
### <a name="impact"></a>Impatto  
*Tutte le repliche dal server primario vengono archiviati in un'unica posizione, questo potrebbe comportare problemi di sicurezza o privacy.*  
  
## <a name="resolution"></a>Risoluzione  
*Utilizzare Hyper-V Manager per creare nuove voci di autorizzazione per i server primari specifici e specificare i percorsi di archiviazione separato per ognuno di essi. È possibile usare caratteri jolly per raggruppare i server primari in set per ogni voce di autorizzazione.*  
  
#### <a name="create-authorization-entries-using-hyper-v-manager"></a>Creare le voci di autorizzazione utilizzando Gestione di Hyper-V  
  
1.  Aprire la console di gestione di Hyper-V. (Da Server Manager, fare clic su **Strumenti** > **Hyper-V Manager**.)  
  
2.  Dall'elenco di host, fare doppio clic su quello desiderato, quindi fare clic su **Impostazioni Hyper-V**.  
  
3.  Nel riquadro di spostamento, fare clic su **configurazione della replica**.  
  
4.  In **autorizzazione e archiviazione**, fare clic su **consentire la replica da server specificato**.  
  
5.  Sotto l'elenco dei server, fare clic su **Aggiungi**.  
  
6.  In **aggiungere una voce di autorizzazione**:  
  
    -   Digitare il nome completo del primo server.  
  
    -   Specificare un percorso per archiviare solo i file del server dedicato.  
  
7.  Fare clic su **OK**.  
  
8.  Ripetere per ogni server primario.  
  
9. Fare clic su **OK** nuovamente per completare e chiudere la finestra.  
  
### <a name="create-authorization-entries-using-windows-powershell"></a>Creare le voci di autorizzazione con Windows PowerShell  
  
1.  Aprire Windows PowerShell. (Dal desktop fare clic sul pulsante Start e iniziare a digitare **Windows PowerShell**.)  
  
2.  Fare doppio clic su **Windows PowerShell** e fare clic su **Esegui come amministratore**.  
  
3.  Eseguire un comando simile al seguente, sostituendo:  
  
    -   Il nome del server primario di server01.domain01.contoso.com con il nome di dominio completo del server.  
  
    -   Il percorso del D:\ReplicaVMStorage con il percorso.  
  
    -   Il gruppo di trust denominato PREDEFINITO con nome del gruppo, se è stato creato uno. In caso contrario, utilizzare l'impostazione PREDEFINITA.  
  
```  
New-VMReplicationAuthorizationEntry server01.domain01.contoso.com D:\ReplicaVMStorage DEFAULT  
```  
  
## <a name="see-also"></a>Vedere anche  
[New-VMReplicationAuthorizationEntry](https://technet.microsoft.com/library/hh848606.aspx)  
  


