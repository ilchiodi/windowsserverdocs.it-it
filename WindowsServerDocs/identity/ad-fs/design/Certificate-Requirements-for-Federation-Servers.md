---
ms.assetid: 9831b421-8fb7-4e15-ac27-c013cbca6d05
title: Requisiti dei certificati per i server federativi
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 7eeff82ef3311f18c8252c44c96310fcf3c18217
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359178"
---
# <a name="certificate-requirements-for-federation-servers"></a>Requisiti dei certificati per i server federativi

In qualsiasi Active Directory Federation Services progettazione \(AD FS @ no__t-1, è necessario usare vari certificati per proteggere le comunicazioni e facilitare le autenticazioni utente tra client Internet e server federativi. Ogni server federativo deve disporre di un certificato di comunicazione del servizio e di un token @ no__t-0signing prima di poter partecipare alle comunicazioni AD FS. Nella tabella seguente vengono descritti i tipi di certificato associati al server federativo.  
  
|Tipo di certificato|Descrizione|  
|--------------------|---------------|  
|Token @ no__t-certificato 0signing|Un token @ no__t-0signing certificate è un certificato X509. I server federativi usano le coppie di chiavi public @ no__t-0Private associate per firmare digitalmente tutti i token di sicurezza che producono. tra cui metadati federativi pubblicati e richieste di risoluzione di elementi.<br /><br />È possibile avere più certificati @ no__t-0signing configurati nello snap-in di gestione AD FS @ no__t-1in per consentire il rollover dei certificati quando un certificato è prossimo alla scadenza. Per impostazione predefinita, vengono pubblicati tutti i certificati dell'elenco, ma solo il certificato primary token @ no__t-0signing viene usato da AD FS per firmare effettivamente i token. Tutti i certificati selezionati devono avere una chiave privata corrispondente.<br /><br />Per ulteriori informazioni, consultare [Token-Signing Certificates](Token-Signing-Certificates.md) e [Add a Token-Signing Certificate](../../ad-fs/deployment/Add-a-Token-Signing-Certificate.md).|  
|Certificato per le comunicazioni di servizi|I server federativi usano un certificato di autenticazione server, noto anche come comunicazione del servizio per la sicurezza dei messaggi Windows Communication Foundation \(WCF @ no__t-1. Per impostazione predefinita, si tratta dello stesso certificato usato da un server federativo come certificato Secure Sockets Layer \(SSL @ no__t-1 in Internet Information Services \(IIS @ no__t-3. **Nota:** Lo snap-in di gestione AD FS @ no__t-0cm fa riferimento ai certificati di autenticazione server per i server federativi come certificati di comunicazione del servizio.<br /><br />Per ulteriori informazioni, vedere [Service Communications Certificates](Service-Communications-Certificates.md) e [set a Service Communications Certificate](../../ad-fs/deployment/Set-a-Service-Communications-Certificate.md).<br /><br />Poiché il certificato di comunicazione del servizio deve essere considerato attendibile dai computer client, è consigliabile utilizzare un certificato firmato da un'autorità di certificazione attendibile \(CA @ no__t-1. Tutti i certificati selezionati devono avere una chiave privata corrispondente.|  
|Secure Sockets Layer \(SSL @ no__t-1 certificate|I server federativi usano un certificato SSL per proteggere il traffico dei servizi Web per le comunicazioni SSL con client Web e proxy server federativi.<br /><br />Poiché il certificato SSL deve essere considerato attendibile dai computer client, è consigliabile usarne uno firmato da una CA attendibile. Tutti i certificati selezionati devono avere una chiave privata corrispondente.|  
|Token @ no__t-certificato 0decryption|Questo certificato viene usato per decrittografare i token ricevuti da questo server federativo.<br /><br />È possibile avere più certificati di decrittografia. In questo modo un server federativo di risorsa può decrittografare i token emessi con un certificato meno recente dopo che un nuovo certificato è stato impostato come certificato di decrittografia primario. Tutti i certificati possono essere usati per la decrittografia, ma solo il certificato primary token @ no__t-0decrypting è effettivamente pubblicato nei metadati federativi. Tutti i certificati selezionati devono avere una chiave privata corrispondente.<br /><br />Per altre informazioni, vedere [aggiungere un certificato per la decrittografia di token](../../ad-fs/deployment/Add-a-Token-Decrypting-Certificate.md).|  
  
È possibile richiedere e installare un certificato SSL o un certificato di comunicazione del servizio richiedendo un certificato di comunicazione del servizio tramite Microsoft Management Console \(MMC @ no__t-1 snap @ no__t-2in per IIS. Per informazioni generali sull'uso di certificati SSL, vedi [IIS 7.0: Configurazione di Secure Sockets Layer in IIS 7.0 @ no__t-0 e [IIS 7,0: Configurazione dei certificati del server in IIS 7.0 @ no__t-0.  
  
> [!NOTE]  
> In ADFS è possibile modificare l'algoritmo Hash sicuro \(SHA\) livello utilizzato per le firme digitali entrambi SHA\-1 o SHA\-256 \(più sicuro\). AD FSdoes non supporta l'uso di certificati con altri metodi hash, ad esempio MD5 \(The default hash Algorithm usato con il comando Makecert. exe @ no__t-1line Tool @ no__t-2. Per una protezione ottimale, è consigliabile utilizzare SHA\-256 \(che per impostazione predefinita\) per tutte le firme. SHA @ no__t-01 è consigliato per l'uso solo in scenari in cui è necessario interoperare con un prodotto che non supporta le comunicazioni tramite SHA @ no__t-1256, ad esempio un prodotto non @ no__t-2Microsoft o AD FS 1. *x*.  
  
## <a name="determining-your-ca-strategy"></a>Scelta della strategia per la CA  
AD FS non richiede che i certificati vengano emessi da un'autorità di certificazione. Tuttavia, il certificato SSL \(The certificato usato per impostazione predefinita come il certificato di comunicazione del servizio @ no__t-1 deve essere considerato attendibile dai client AD FS. Si consiglia di non usare i certificati self @ no__t-0signed per questi tipi di certificato.  
  
> [!IMPORTANT]  
> L'uso di self @ no__t-0signed, i certificati SSL in un ambiente di produzione possono consentire a un utente malintenzionato di un'organizzazione partner account di assumere il controllo dei server federativi in un'organizzazione partner risorse. Questo rischio di sicurezza esiste perché i certificati self @ no__t-0signed sono certificati radice. Devono essere aggiunti all'archivio radice attendibile di un altro server federativo \(per esempio, il server federativo di risorsa @ no__t-1, che può lasciare tale server vulnerabile agli attacchi.  
  
Dopo aver ricevuto un certificato da una CA, assicurarsi che tutti i certificati vengano importati nell'archivio certificati personali del computer locale. È possibile importare i certificati nell'archivio personale con lo snap MMC certificati\-in.  
  
In alternativa all'utilizzo dello snap-in certificati @ no__t-0cm, è anche possibile importare il certificato SSL con lo snap-in Gestione IIS @ no__t-1in al momento dell'assegnazione del certificato SSL al sito Web predefinito. Per ulteriori informazioni, vedere [importare un certificato di autenticazione server nel sito Web predefinito](../../ad-fs/deployment/Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md).  
  
> [!NOTE]  
> Prima di installare il software AD FS nel computer che diventerà il server federativo, verificare che entrambi i certificati si trovino nell'archivio certificati personali del computer locale e che il certificato SSL sia assegnato al sito Web predefinito. Per ulteriori informazioni sull'ordine delle attività necessarie per configurare un server federativo, vedere [Checklist: Configurazione di un server federativo @ no__t-0.  
  
In base ai requisiti di sicurezza e di budget, valutare con attenzione quali certificati verranno ottenuti da una CA pubblica o da una CA aziendale. La figura seguente mostra le autorità di certificazione consigliate per l'emissione di specifici tipi di certificati. Questa raccomandazione riflette il migliore approccio @ no__t-0practice per la sicurezza e i costi.  
  
![requisiti del certificato](media/adfs2_fedserver_certstory_1.png)  
  
## <a name="certificate-revocation-lists"></a>Elenchi di revoche di certificati  
Se per uno dei certificati usati esistono elenchi di revoca dei certificati, il server con il certificato configurato deve essere in grado di contattare il server che distribuisce gli elenchi di revoca di certificati.  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
