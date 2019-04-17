---
ms.assetid: 9831b421-8fb7-4e15-ac27-c013cbca6d05
title: Requisiti dei certificati per i server federativi
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 369c0e9e7ab1ef25baee1c35379cc66b886f20d8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="certificate-requirements-for-federation-servers"></a>Requisiti dei certificati per i server federativi

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In qualsiasi progettazione \(AD FS\) Active Directory Federation Services, è necessario usare vari certificati per proteggere le comunicazioni e facilitare le autenticazioni utente tra client Internet e i server federativi. Ogni server federativo deve avere un certificato di comunicazione del servizio e un certificato di firma token\ prima di poter partecipare alle comunicazioni ADFS. La tabella seguente descrive i tipi di certificato associati al server federativo.  
  
|Tipo di certificato|Descrizione|  
|--------------------|---------------|  
|Certificato di firma Token\|Un certificato di firma token\ è un X509 certificato. I server federativi usano coppie di chiavi public\/private associate per firmare digitalmente tutti i token di sicurezza che producono. Ciò include la firma di richieste di risoluzione artefatto e metadati federativi pubblicati.<br /><br />È possibile avere più certificati di firma token\ configurati nella gestione di AD FS snap-in per consentire il rollover dei certificati prossimi alla scadenza. Per impostazione predefinita, vengono pubblicati tutti i certificati nell'elenco, ma solo il primario token\ certificato di firma viene utilizzato da ADFS per firmare effettivamente i token. Tutti i certificati selezionato devono avere una chiave privata corrispondente.<br /><br />Per ulteriori informazioni, vedere [certificati di firma di Token](Token-Signing-Certificates.md) e [aggiungere un certificato di firma di Token](../../ad-fs/deployment/Add-a-Token-Signing-Certificate.md).|  
|Certificato di comunicazione del servizio|I server federativi usano un certificato di autenticazione server, anche noto come comunicazione di servizi per Windows Communication Foundation \(WCF\) sicurezza dei messaggi. Per impostazione predefinita, questo è lo stesso certificato che utilizza un server federativo come certificato Secure Sockets Layer \(SSL\) in \(IIS\) Internet Information Services. **Nota:** snap-in di gestione di ADFS si riferisce ai certificati di autenticazione server per i server federativi come certificati per le comunicazioni del servizio.<br /><br />Per ulteriori informazioni, vedere [certificati per le comunicazioni di servizi](Service-Communications-Certificates.md) e [impostare un certificato di comunicazioni di servizi](../../ad-fs/deployment/Set-a-Service-Communications-Certificate.md).<br /><br />Poiché il certificato di comunicazione del servizio deve essere considerato attendibile dai computer client, è consigliabile utilizzare un certificato firmato da un'autorità di certificazione attendibili \(CA\). Tutti i certificati selezionato devono avere una chiave privata corrispondente.|  
|Secure Sockets Layer \(SSL\) certificato|I server federativi usano un certificato SSL per proteggere il traffico dei servizi Web per le comunicazioni SSL con client Web e i proxy server federativi.<br /><br />Poiché il certificato SSL deve essere considerato attendibile dai computer client, è consigliabile utilizzare un certificato firmato da un'autorità di certificazione attendibili. Tutti i certificati selezionato devono avere una chiave privata corrispondente.|  
|Certificato di decrittografia Token\|Questo certificato viene utilizzato per decrittografare i token ricevuti dal server federativo.<br /><br />È possibile avere più certificati di decrittografia. Questo rende possibile per un server federativo di risorsa essere in grado di decrittografare i token emessi con un certificato meno recente dopo che un nuovo certificato viene impostato come certificato di decrittografia primario. Tutti i certificati possono essere utilizzati per la decrittografia, ma solo il decrittografia token\ certificato primario viene effettivamente pubblicato nei metadati federativi. Tutti i certificati selezionato devono avere una chiave privata corrispondente.<br /><br />Per ulteriori informazioni, vedere [aggiungere un certificato di decrittografia di Token](../../ad-fs/deployment/Add-a-Token-Decrypting-Certificate.md).|  
  
È possibile richiedere e installare un certificato SSL o un certificato di comunicazione del servizio richiedendo un certificato di comunicazione del servizio tramite il \(MMC\) snap\ Microsoft Management Console-in di IIS. Per ulteriori informazioni generali sull'uso dei certificati SSL, vedere [IIS 7.0: configurazione di Secure Sockets Layer in IIS 7.0](https://go.microsoft.com/fwlink/?LinkID=108544) e [IIS 7.0: configurazione dei certificati Server in IIS 7.0](https://go.microsoft.com/fwlink/?LinkID=108545) .  
  
> [!NOTE]  
> In ADFS è possibile modificare il livello di Secure Hash Algorithm \(SHA\) che viene utilizzato per le firme digitali \(more secure\) SHA\-1 o SHA\-256. FSdoes Active Directory non supporta l'uso di certificati con altri metodi hash, ad esempio MD5 \ (l'algoritmo hash predefinito usato con il tool\ della riga di comando Makecert.exe). Per una protezione ottimale, ti consigliamo di usare SHA\-256 \ (che per impostazione default\) per tutte le firme. SHA\-1 è consigliato per l'uso solo in scenari in cui è necessario interoperare con un prodotto che non supporta le comunicazioni tramite SHA\-256, ad esempio un prodotto di fornitori non Microsoft o ADFS 1. *x*.  
  
## <a name="determining-your-ca-strategy"></a>Definire la strategia di autorità di certificazione  
ADFS non richiede che i certificati vengano emessi da un'autorità di certificazione. Tuttavia, il certificato SSL \ (il certificato viene utilizzato anche per impostazione predefinita come il certificate\ le comunicazioni di servizio) deve essere considerato attendibile dai client ADFS. Si consiglia di utilizzare i certificati firmati e per questi tipi di certificati.  
  
> [!IMPORTANT]  
> Uso di firma e, i certificati SSL autofirmati in un ambiente di produzione possono consentire un utente malintenzionato di un'organizzazione partner account di assumere il controllo di server federativi in un'organizzazione partner risorse. Questo rischio di sicurezza esiste perché i certificati firmati e sono certificati radice. Devono essere aggiunti all'archivio radice attendibile di un altro server federativo \ (ad esempio, la risorsa federation server \), che possono lasciare il server vulnerabile agli attacchi.  
  
Dopo aver ricevuto un certificato da una CA, assicurarsi che tutti i certificati vengono importati nell'archivio certificati personali del computer locale. È possibile importare i certificati nell'archivio personale con snap-in MMC certificati.  
  
Come alternativa all'utilizzo di certificati snap-in, è anche possibile importare il certificato SSL con lo snap di gestione IIS-in al momento in cui si assegna il certificato SSL al sito Web predefinito. Per ulteriori informazioni, vedere [importare un certificato di autenticazione Server nel sito Web predefinito](../../ad-fs/deployment/Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md).  
  
> [!NOTE]  
> Prima di installare il software ADFS nel computer che diventerà il server federativo, assicurarsi che entrambi i certificati siano nell'archivio certificati personali del Computer locale e che il certificato SSL sia assegnato al sito Web predefinito. Per ulteriori informazioni sull'ordine delle attività necessarie per configurare un server federativo, vedere [Checklist: Setting Up a Federation Server](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md).  
  
A seconda di sicurezza e i requisiti di budget, valutare con attenzione quali certificati verranno ottenuti da una pubblica CA o un'autorità di certificazione aziendale. Nella figura seguente mostra le autorità emittenti di CA consigliati per un tipo di certificato specificato. Queste indicazioni riflettono un approccio best\ consigliate sulla sicurezza e i costi.  
  
![requisiti del certificato](media/adfs2_fedserver_certstory_1.png)  
  
## <a name="certificate-revocation-lists"></a>Elenchi di revoche di certificati  
Se tutti i certificati che usi ha CRL, il server con il certificato configurato deve essere in grado di contattare il server che distribuisce i CRL.  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di ADFS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
