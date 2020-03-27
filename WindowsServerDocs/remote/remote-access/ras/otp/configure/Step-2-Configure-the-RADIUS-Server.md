---
title: Passaggio 2 configurare il server RADIUS
description: Questo argomento fa parte della Guida distribuire accesso remoto con l'autenticazione OTP in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0326818f-9144-496c-b946-f82be4eefbd3
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 83c63e8d11b4f93b87e1f49f487342d3955f6372
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80313640"
---
# <a name="step-2-configure-the-radius-server"></a>Passaggio 2 configurare il server RADIUS

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Prima di configurare il server di accesso remoto per supportare DirectAccess con supporto OTP, configurare il server RADIUS.  
  
|Attività|Descrizione|  
|----|--------|  
|[2,1. configurare i token di distribuzione del software RADIUS](#BKMK_1.1)|Nel server RADIUS configurare i token di distribuzione software.|  
|[2,2. configurare le informazioni di sicurezza RADIUS](#BKMK_1.2)|Nel server RADIUS configurare le porte e il segreto condiviso da usare.|  
|[2,3 aggiunta dell'account utente per il probe OTP](#BKMK_Probe)|Nel server RADIUS creare un nuovo account utente per il probe OTP.|  
|[2,4 sincronizzare con Active Directory](#BKMK_Active)|Nel server RADIUS creare gli account utente sincronizzati con Active Directory account.|  
|[2,5 configurare l'agente di autenticazione RADIUS](#BKMK_AuthAgent)|Configurare il server di accesso remoto come agente di autenticazione RADIUS.|  
  
## <a name="21-configure-the-radius-software-distribution-tokens"></a><a name="BKMK_1.1"></a>2,1 configurare i token di distribuzione del software RADIUS  
Il server RADIUS deve essere configurato con i token di licenza e software e/o di distribuzione hardware necessari per l'uso da parte di DirectAccess con OTP. Questo processo sarà specifico per ogni implementazione del fornitore RADIUS.  
  
## <a name="22-configure-the-radius-security-information"></a><a name="BKMK_1.2"></a>2,2 configurare le informazioni di sicurezza RADIUS  
Il server RADIUS utilizza porte UDP per finalità di comunicazione e ogni fornitore RADIUS dispone di porte UDP predefinite per le comunicazioni in ingresso e in uscita. Affinché il server RADIUS funzioni con il server di accesso remoto, assicurarsi che tutti i firewall nell'ambiente siano configurati per consentire il traffico UDP tra i server DirectAccess e OTP sulle porte richieste in base alle esigenze.  
  
Il server RADIUS utilizza un segreto condiviso a scopo di autenticazione. Configurare il server RADIUS con una password complessa per il segreto condiviso e tenere presente che verrà usato quando si configura la configurazione del computer client del server DirectAccess per l'uso con DirectAccess con OTP.  
  
## <a name="23-adding-user-account-for-otp-probing"></a><a name="BKMK_Probe"></a>2,3 aggiunta dell'account utente per il probe OTP  
Nel server RADIUS creare un nuovo account utente denominato **DAProbeUser** e assegnargli la password **DAProbePass**.  
  
## <a name="24-synchronize-with-active-directory"></a><a name="BKMK_Active"></a>2,4 sincronizzare con Active Directory  
Il server RADIUS deve avere account utente che corrispondono agli utenti in Active Directory che utilizzeranno DirectAccess con OTP.  
  
#### <a name="to-synchronize-the-radius-and-active-directory-users"></a>Per sincronizzare gli utenti RADIUS e Active Directory  
  
1.  Registrare le informazioni utente da Active Directory per tutti i DirectAccess con gli utenti OTP.  
  
2.  Utilizzare la procedura specifica del fornitore per creare account **DOMINIO\nomeutente** utente identici nel server RADIUS registrato.  
  
## <a name="25-configure-the-radius-authentication-agent"></a><a name="BKMK_AuthAgent"></a>2,5 configurare l'agente di autenticazione RADIUS  
Il server di accesso remoto deve essere configurato come un agente di autenticazione RADIUS per l'implementazione di DirectAccess con OTP. Seguire le istruzioni del fornitore RADIUS per configurare il server di accesso remoto come agente di autenticazione RADIUS.  
  


