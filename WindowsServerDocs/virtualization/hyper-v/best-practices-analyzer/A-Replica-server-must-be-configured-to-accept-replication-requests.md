---
title: Un server di Replica deve essere configurato per accettare le richieste di replica
description: Vengono fornite istruzioni per risolvere il problema segnalato da questa regola di Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 09bb573bbb091d1b167f3c354be4d6448476e26d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857954"
---
# <a name="a-replica-server-must-be-configured-to-accept-replication-requests"></a>Un server di Replica deve essere configurato per accettare le richieste di replica

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Errore|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.
  
## <a name="issue"></a>Problema  
*Questo computer è designato come server di replica Hyper-V, ma non è configurato per accettare i dati di replica in ingresso dai server primari.*  
  
## <a name="impact"></a>Impatto  
*Questo server non è in grado di accettare il traffico di replica dai server primari.*  
  
## <a name="resolution"></a>Risoluzione  
*Utilizzare la console di gestione di Hyper-V per specificare i server primari da cui il server di replica deve accettare i dati di replica.*  
  
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
  


