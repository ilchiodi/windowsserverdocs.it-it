---
title: Passaggio 2 configurare il Server RADIUS
description: Questo argomento fa parte della Guida alla distribuzione di accesso remoto con autenticazione OTP in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0326818f-9144-496c-b946-f82be4eefbd3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9c111ce52f2cca0cc37ea4d5b873c5fde12bce18
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282452"
---
# <a name="step-2-configure-the-radius-server"></a>Passaggio 2 configurare il Server RADIUS

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Prima di configurare il server di accesso remoto per supportare il supporto di DirectAccess con OTP, si configura il server RADIUS.  
  
|Attività|Descrizione|  
|----|--------|  
|[2.1. Configurare i token di distribuzione software RADIUS](#BKMK_1.1)|Configurare i token di distribuzione software del server RADIUS.|  
|[2.2. Configurare le informazioni di sicurezza RADIUS](#BKMK_1.2)|Configurare le porte e il segreto condiviso da usare nel server RADIUS.|  
|[2.3 aggiunta di account utente per l'individuazione tramite probe OTP](#BKMK_Probe)|Creare un nuovo account utente per l'individuazione tramite probe OTP sul server RADIUS.|  
|[2.4 eseguire la sincronizzazione con Active Directory](#BKMK_Active)|Creare account utente sincronizzati con gli account Active Directory del server RADIUS.|  
|[2.5 configurare l'agente di autenticazione RADIUS](#BKMK_AuthAgent)|Configurare il server di accesso remoto come un agente di autenticazione RADIUS.|  
  
## <a name="BKMK_1.1"></a>2.1 configurare i token di distribuzione software RADIUS  
Il server RADIUS deve essere configurato con la licenza necessaria e i token di distribuzione software e/o hardware utilizzabile da DirectAccess con OTP. Questo processo sarà specifico per ogni implementazione del fornitore RADIUS.  
  
## <a name="BKMK_1.2"></a>2.2 configurare le informazioni di sicurezza RADIUS  
Il server RADIUS utilizza porte UDP per scopi di comunicazione e ogni fornitore RADIUS ha una proprio le porte UDP predefinito per le comunicazioni in ingresso e in uscita. Per il server RADIUS lavorare con il server di accesso remoto, assicurarsi che tutti i firewall nell'ambiente vengono configurati per consentire il traffico UDP tra il server DirectAccess e OTP tramite le porte necessarie in base alle esigenze.  
  
Il server RADIUS utilizza una chiave privata condivisa per scopi di autenticazione. Configurare il server RADIUS con una password complessa per il segreto condiviso e si noti che verrà usato durante la configurazione di configurazione dei computer client del server DirectAccess per l'uso con DirectAccess con OTP.  
  
## <a name="BKMK_Probe"></a>2.3 aggiunta di account utente per l'individuazione tramite probe OTP  
Creare un nuovo account utente denominato del server RADIUS **DAProbeUser** e specificare la password **DAProbePass**.  
  
## <a name="BKMK_Active"></a>2.4 eseguire la sincronizzazione con Active Directory  
Il server RADIUS deve avere gli account utente che corrispondono agli utenti in Active Directory che verrà usato DirectAccess con OTP.  
  
#### <a name="to-synchronize-the-radius-and-active-directory-users"></a>Per sincronizzare gli utenti di Active Directory e RADIUS  
  
1.  Registrare le informazioni utente da Active Directory per DirectAccess tutti con gli utenti OTP.  
  
2.  Utilizzare la procedura specifico fornitore per creare un utente identico **dominio\nomeutente** gli account nel server RADIUS che sono stati registrati.  
  
## <a name="BKMK_AuthAgent"></a>2.5 configurare l'agente di autenticazione RADIUS  
Il server di accesso remoto deve essere configurato come un agente di autenticazione RADIUS per il client DirectAccess con l'implementazione di OTP. Seguire le istruzioni del fornitore RADIUS per configurare il server di accesso remoto come un agente di autenticazione RADIUS.  
  


