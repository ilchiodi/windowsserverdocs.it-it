---
ms.assetid: 6e711a96-9055-4508-b6d4-318d6aa95fd1
title: "Quando usare la delega dell'identità"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: af227d9e87ddb73f194dd46c8ce45fcdf12a34cf
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="when-to-use-identity-delegation"></a>Quando usare la delega dell'identità

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
  
## <a name="what-is-identity-delegation"></a>Che cos'è la delega dell'identità?  
Delega dell'identità è una funzionalità di Active Directory Federation Services \(AD FS\) che consente agli account specificati administrator\ rappresentare gli utenti. L'account che rappresenta l'utente viene chiamato il *delegare*. Questa funzionalità di delega è fondamentale per molte applicazioni distribuite in cui sono presente una serie di accesso verifiche del controllo che devono essere eseguite in sequenza per ogni applicazione, database o servizio che fa parte della catena di autorizzazione per la richiesta di origine. Esistono molti scenari di reali in cui un'applicazione "front-end Web" deve recuperare dati da un "back-end" più sicuro, ad esempio un servizio Web che è connesso a un database Microsoft SQL Server.  
  
Ad esempio, un sito Web esistente ordinamento parts\ può essere migliorato a livello di codice in modo che consente alle organizzazioni di visualizzare il proprio stato di cronologia e account di acquisto al partner. Per motivi di sicurezza tutti i dati finanziari di partner è archiviato in un database protetto in un server dedicato Structured Query Language \(SQL\). In questo caso, il codice dell'applicazione front\-end non riconosce i dati finanziari dell'organizzazione partner. Pertanto, è necessario recuperare i dati da un altro computer in un' posizione nella rete che ospita \ (in questo) il servizio Web per il database di parti \(the back end\).  
  
Per questo processo data\ recupero abbia esito positivo, alcuni successione di autorizzazione "hand\-agitazione" deve accettare posizionate tra l'applicazione Web e il servizio Web per il database delle parti, come illustrato nella figura seguente.  
  
![Delega dell'identità](media/adfs2_identitydelegationconcept.gif)  
  
Poiché è stata effettuata la richiesta originale al server Web stesso, che è probabile che si trova in un'organizzazione completamente diversa dall'organizzazione dell'utente che sta tentando di accedere al server Web, il token di sicurezza che viene inviato insieme alla richiesta non soddisfa i criteri di autorizzazione necessari per accedere a qualsiasi altro computer oltre il server Web. Pertanto, l'unico metodo che consente di soddisfare la richiesta di utente di origine è l'inserimento di un server federativo intermedio nell'organizzazione partner risorse per una nuova emissione di un token di sicurezza che dispone di privilegi di accesso appropriati.  
  
## <a name="how-does-identity-delegation-work"></a>Come funziona la delega dell'identità?  
Le applicazioni Web nelle architetture di applicazioni multilivello spesso chiamano servizi Web per accedere a dati o funzionalità comuni. È importante che tali servizi Web conoscano l'identità dell'utente originale in modo che il servizio possa prendere decisioni di autorizzazione e facilitare il controllo. In questo caso, l'applicazione Web front\-end rappresenta l'utente al servizio Web come un delegato. ADFS facilita questo scenario consentendo l'account di Active Directory di agire come un utente a un altro componente. Nella figura seguente è illustrato uno scenario di delega di identità.  
  
![Delega dell'identità](media/adfs2_identitydelegationsteps.gif)  
  
1.  Frank tenta di accedere a cronologia ordini part\ da un'applicazione Web in un'altra organizzazione. Il computer client richiede e riceve un token da ADFS per l'applicazione Web ordinamento part\ front\-end.  
  
2.  Il computer client invia una richiesta all'applicazione Web, includendo il token ottenuto nel passaggio 1, per dimostrare l'identità del client.  
  
3.  L'applicazione Web deve comunicare con il servizio Web per completare la transazione per il client. L'applicazione Web contatta ADFS per ottenere un token di delega per interagire con il servizio Web. Token di delega sono token di sicurezza rilasciati affinché un delegato possa agire come un utente. ADFS restituisce un token di delega con attestazioni relative al client, destinato al servizio Web.  
  
4.  L'applicazione Web utilizza il token ottenuto da ADFS nel passaggio 3 per accedere al servizio Web che funge da client. Esaminare il token di delega, il servizio Web può stabilire che l'applicazione Web funge da client. Il servizio Web esegue proprio criterio di autorizzazione, registra la richiesta e fornisce i dati di cronologia che originariamente richiesti da Frank all'applicazione Web sulle parti necessarie e quindi a Frank.  
  
Per un determinato delegato, ADFS può limitare i servizi Web per cui l'applicazione Web potrebbe richiedere un token di delega. Il computer client non dispone di un account di Active Directory per eseguire questa operazione. Infine, come indicato in precedenza, il servizio Web può facilmente determinare l'identità del delegato che opera come utente. In questo modo i servizi Web possono presentare un comportamento diverso in base a che comunichino direttamente al computer client o tramite un delegato.  
  
## <a name="configuring-ad-fs-for-identity-delegation"></a>Configurazione di ADFS per la delega dell'identità  
È possibile utilizzare la gestione di ADFS snap-in per configurare ADFS per la delega dell'identità ogni volta che è necessario semplificare il processo di recupero dati. Dopo la configurazione, ADFS può generare nuovi token di sicurezza che includeranno il contesto di autorizzazione che il servizio back\-end può richiedere prima di poter fornire accesso ai dati protetti.  
  
ADFS non limitano gli utenti che possono essere rappresentati. Dopo aver configurato ADFS per la delega dell'identità, esegue le seguenti:  
  
-   Determina quali server è possibile delegare l'autorità di richiedere token per rappresentare un utente.  
  
-   Stabilisce e mantiene separati sia contesto dell'identità per l'account del client delegato e il server che agisce come un delegato.  
  
È possibile configurare la delega dell'identità mediante l'aggiunta di regole di autorizzazione di delega per un trust della relying party in snap-in di gestione di ADFS. Per ulteriori informazioni su come eseguire questa operazione, vedere [elenco di controllo: creazione di regole attestazione per un Trust della Relying Party](../../ad-fs/deployment/Checklist--Creating-Claim-Rules-for-a-Relying-Party-Trust.md).  
  
## <a name="configuring-the-front-end-web-application-for-identity-delegation"></a>Configurazione dell'applicazione Web front\-end per la delega dell'identità  
Gli sviluppatori sono disponibili diverse opzioni che possono utilizzare per programmare in modo appropriato l'applicazione front\-end Web o il servizio per reindirizzare le richieste di delega in un computer di ADFS. Per ulteriori informazioni su come personalizzare un'applicazione Web per funzionare con la delega dell'identità, vedere il [Windows Identity Foundation SDK](https://go.microsoft.com/fwlink/?LinkId=122266).  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di ADFS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
