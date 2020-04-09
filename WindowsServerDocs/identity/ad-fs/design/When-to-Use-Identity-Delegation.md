---
ms.assetid: 6e711a96-9055-4508-b6d4-318d6aa95fd1
title: Quando usare la delega di identità
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 7a594332900c8b3afb95c139bcde8458a10f186b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858464"
---
# <a name="when-to-use-identity-delegation"></a>Quando usare la delega di identità
  
## <a name="what-is-identity-delegation"></a>Che cos'è la delega di identità?  
La delega dell'identità è una funzionalità di Active Directory Federation Services \(AD FS\) che consente all'amministratore di\-account specificati di rappresentare gli utenti. L'account che rappresenta l'utente viene chiamato *delegato*. Questa funzionalità di delega è fondamentale per molte applicazioni distribuite per le quali è disponibile una serie di verifiche del controllo di accesso che devono essere eseguite in sequenza per ogni applicazione, database o servizio che fa parte della catena di autorizzazione per la richiesta di origine. Esistono molti scenari\-reali in cui il "front-end" di un'applicazione Web deve recuperare i dati da un "back-end" più sicuro, ad esempio un servizio Web connesso a un database Microsoft SQL Server.  
  
Ad esempio, un parti esistenti\-ordinamento può essere migliorato a livello di codice del sito Web in modo che consente alle organizzazioni di visualizzare il proprio stato di cronologia e account di acquisto al partner. Per motivi di sicurezza tutti i dati finanziari di partner è archiviato in un database protetto in dedicato Structured Query Language \(SQL\) server. In questa situazione, il codice nell'applicazione front\-end non riconosce i dati finanziari dell'organizzazione partner. Pertanto, è necessario recuperare i dati da un altro computer in un punto della rete che ospita \(in questo caso\) il servizio Web per il database di parti \(back-end\).  
  
Per il corretto completamento del processo di recupero dei dati\-, è necessario che venga eseguita una certa successione di autorizzazione "mano\-agitazione" tra l'applicazione Web e il servizio Web per il database delle parti, come illustrato nella figura seguente.  
  
![delega dell'identità](media/adfs2_identitydelegationconcept.gif)  
  
Poiché la richiesta originale è stata effettuata al server Web stesso, che con maggiore probabilità si trova in un'organizzazione completamente diversa da quella dell'utente che sta provando ad accedere al server Web, il token di sicurezza che viene inviato insieme alla richiesta non soddisfa i criteri di autorizzazione necessari per accedere a qualsiasi altro computer oltre il server Web. Quindi, l'unico metodo che consente di soddisfare la richiesta dell'utente di origine è l'inserimento di un server federativo intermedio nell'organizzazione partner risorse per il nuovo rilascio di un token di sicurezza con privilegi di accesso appropriati.  
  
## <a name="how-does-identity-delegation-work"></a>Come funziona la delega di identità?  
Le applicazioni Web nelle architetture di applicazioni multilivello spesso chiamano i servizi Web per accedere a dati o funzionalità comuni. È importante che tali servizi Web conoscano l'identità utente originale in modo che il servizio possa prendere decisioni di autorizzazione e facilitare il controllo. In questo caso, all'inizio\-end dell'applicazione Web rappresenta l'utente al servizio Web come un delegato. ADFS facilita questo scenario, concedendo agli account di Active Directory come un utente a un altro componente. Nella figura seguente è illustrato uno scenario di delega di identità.  
  
![delega dell'identità](media/adfs2_identitydelegationsteps.gif)  
  
1.  Frank tenta di accedere a parte\-ordinamento cronologia da un'applicazione Web in un'altra organizzazione. Il computer client richiede e riceve un token da ADFS per la parte anteriore\-terminare parte\-ordinamento applicazione Web.  
  
2.  Il computer client invia una richiesta all'applicazione Web, incluso il token ottenuto nel passaggio 1, per dimostrare l'identità del client.  
  
3.  L'applicazione Web deve comunicare con il servizio Web per completare la transazione per il client. L'applicazione Web contatta ADFS per ottenere un token di delega per interagire con il servizio Web. I token di delega sono token di sicurezza rilasciati affinché un delegato possa agire come un utente. ADFS restituisce un token di delega con attestazioni relative al client, per il servizio Web di destinazione.  
  
4.  L'applicazione Web usa il token ottenuto da AD FS nel passaggio 3 per accedere al servizio Web che funge da client. Esaminando il token di delega, il servizio Web può stabilire che l'applicazione Web funge da client. Il servizio Web esegue il proprio criterio di autorizzazione, registra la richiesta e fornisce i dati di cronologia sulle parti necessarie originariamente richiesti da Frank all'applicazione Web e quindi a Frank.  
  
Per un determinato delegato, ADFS può limitare i servizi Web per cui l'applicazione Web potrebbe richiedere un token di delega. Il computer client non ha un account di Active Directory per eseguire questa operazione. Infine, come indicato in precedenza, il servizio Web può facilmente determinare l'identità del delegato che opera come utente. In questo modo i servizi Web possono presentare un comportamento diverso a seconda che comunichino direttamente con il computer client o tramite un delegato.  
  
## <a name="configuring-ad-fs-for-identity-delegation"></a>Configurazione di AD FS per la delega di identità  
È possibile utilizzare lo snap di gestione di ADFS\-per configurare ADFS per la delega dell'identità ogni volta che è necessario facilitare il processo di recupero dati. Dopo la configurazione, ADFS può generare nuovi token di sicurezza che verranno inclusi nel contesto di autorizzazione che il back\-fine servizio può essere necessario prima che possa fornire accesso ai dati protetti.  
  
ADFS non limitano gli utenti che possono essere rappresentati. Dopo aver configurato ADFS per la delega di identità, esegue le seguenti:  
  
-   Determina i server ai quali è possibile delegare l'autorità di richiedere token per rappresentare un utente.  
  
-   Stabilisce e mantiene separati sia il contesto dell'identità per l'account del client delegato che per il server che agisce come delegato.  
  
È possibile configurare la delega dell'identità mediante l'aggiunta di regole di autorizzazione di delega per una relying party trust nello snap di gestione di ADFS\-in. Per ulteriori informazioni su come eseguire questa operazione, vedere [elenco di controllo: creazione di regole attestazione per un Trust della Relying Party](../../ad-fs/deployment/Checklist--Creating-Claim-Rules-for-a-Relying-Party-Trust.md).  
  
## <a name="configuring-the-front-end-web-application-for-identity-delegation"></a>Configurazione di primo piano\-terminare l'applicazione Web per la delega dell'identità  
Gli sviluppatori sono disponibili diverse opzioni che consentono di programmare in modo appropriato all'inizio di Web\-terminare l'applicazione o un servizio per reindirizzare le richieste di delega in un computer di ADFS. Per altre informazioni su come personalizzare un'applicazione Web in modo che funzioni con la delega dell'identità, vedere [Windows Identity Foundation SDK](https://go.microsoft.com/fwlink/?LinkId=122266).  
  
## <a name="see-also"></a>Vedi anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
