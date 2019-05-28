---
ms.assetid: 9831b421-8fb7-4e15-ac27-c013cbca6d05
title: Requisiti dei certificati per i server federativi
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ce301f6320ed3347b1ee802f57c2b2ebd4394970
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191645"
---
# <a name="certificate-requirements-for-federation-servers"></a>Requisiti dei certificati per i server federativi

In qualsiasi Active Directory Federation Services \(ADFS\) progettazione, è necessario usare vari certificati per proteggere le comunicazioni e facilitare le autenticazioni utente tra client Internet e i server federativi. Ogni server federativo deve essere un certificato di comunicazione del servizio e un token\-firma del certificato prima che possa partecipare le comunicazioni di AD FS. Nella tabella seguente vengono descritti i tipi di certificati che sono associati server federativo.  
  
|Tipo di certificato|Descrizione|  
|--------------------|---------------|  
|Token\-certificato di firma|Un token\-certificato di firma è un X509 certificato. I server federativi usano pubblico associato\/coppie di chiavi private per firmare digitalmente tutti i token di sicurezza che producono. tra cui metadati federativi pubblicati e richieste di risoluzione di elementi.<br /><br />È possibile avere più token\-firma dei certificati configurati nello snap di gestione di AD FS\-per consentire rollover del certificato quando si è prossimi alla scadenza di un certificato. Per impostazione predefinita, vengono pubblicati tutti i certificati nell'elenco, ma solo il token primario\-certificato di firma viene utilizzato da ADFS per firmare effettivamente i token. Tutti i certificati selezionati devono avere una chiave privata corrispondente.<br /><br />Per ulteriori informazioni, consultare [Token-Signing Certificates](Token-Signing-Certificates.md) e [Add a Token-Signing Certificate](../../ad-fs/deployment/Add-a-Token-Signing-Certificate.md).|  
|Certificato per le comunicazioni di servizi|I server federativi usano un certificato di autenticazione server, noto anche come un servizio di comunicazione per Windows Communication Foundation \(WCF\) sicurezza dei messaggi. Per impostazione predefinita, questo è lo stesso certificato che utilizza un server federativo come Secure Sockets Layer \(SSL\) certificato in Internet Information Services \(IIS\). **Nota:** Lo snap di gestione di AD FS\-in si riferisce ai certificati di autenticazione server per i server federativi come certificati per le comunicazioni del servizio.<br /><br />Per altre informazioni, vedere [certificati per le comunicazioni del servizio](Service-Communications-Certificates.md) e [impostare un certificato di comunicazioni di servizi](../../ad-fs/deployment/Set-a-Service-Communications-Certificate.md).<br /><br />Poiché il certificato di comunicazione del servizio deve essere considerato attendibile dai computer client, è consigliabile usare un certificato firmato da un'autorità di certificazione attendibile \(CA\). Tutti i certificati selezionati devono avere una chiave privata corrispondente.|  
|Secure Sockets Layer \(SSL\) certificato|I server federativi usano un certificato SSL per proteggere il traffico dei servizi Web per le comunicazioni SSL con client Web e proxy server federativi.<br /><br />Poiché il certificato SSL deve essere considerato attendibile dai computer client, è consigliabile usarne uno firmato da una CA attendibile. Tutti i certificati selezionati devono avere una chiave privata corrispondente.|  
|Token\-certificato di decrittografia|Questo certificato viene usato per decrittografare i token ricevuti dal server federativo.<br /><br />È possibile avere più certificati di decrittografia. Questo rende possibile per un server federativo di risorsa essere in grado di decrittografare i token emessi con un certificato meno recente dopo aver impostato un nuovo certificato come certificato di decrittografia primario. Tutti i certificati possono essere usati per la decrittografia, ma solo il token primario\-la decrittografia di certificati viene effettivamente pubblicata nei metadati della federazione. Tutti i certificati selezionati devono avere una chiave privata corrispondente.<br /><br />Per altre informazioni, vedere [aggiungere un certificato di decrittografia di Token](../../ad-fs/deployment/Add-a-Token-Decrypting-Certificate.md).|  
  
È possibile richiedere e installare un certificato SSL o un certificato di comunicazione del servizio richiedendo un certificato di comunicazione del servizio tramite la Console di gestione di Microsoft \(MMC\) blocca\-in per IIS. Per informazioni generali sull'uso di certificati SSL, vedi [IIS 7.0: Configurazione di Secure Sockets Layer in IIS 7.0](https://go.microsoft.com/fwlink/?LinkID=108544) e [IIS 7.0: Configurazione dei certificati del Server in IIS 7.0](https://go.microsoft.com/fwlink/?LinkID=108545) .  
  
> [!NOTE]  
> In ADFS è possibile modificare l'algoritmo Hash sicuro \(SHA\) livello utilizzato per le firme digitali entrambi SHA\-1 o SHA\-256 \(più sicuro\). AD FSdoes non supporta l'uso di certificati con altri metodi hash, ad esempio MD5 \(l'algoritmo hash predefinito usato con il comando Makecert.exe\-strumento da riga di\). Per una protezione ottimale, è consigliabile utilizzare SHA\-256 \(che per impostazione predefinita\) per tutte le firme. SHA\-1 è consigliato per l'uso solo in scenari in cui è necessario interoperare con un prodotto che non supporta le comunicazioni tramite SHA\-256, ad esempio non\-prodotti Microsoft o ADFS 1. *x*.  
  
## <a name="determining-your-ca-strategy"></a>Scelta della strategia per la CA  
ADFS non richiede che i certificati vengano emessi da un'autorità di certificazione. Tuttavia, il certificato SSL \(il certificato che viene usato anche per impostazione predefinita come certificato per le comunicazioni del servizio\) deve essere considerato attendibile dai client di AD FS. È consigliabile non usare Auto\-certificati autofirmati per questi tipi di certificato.  
  
> [!IMPORTANT]  
> Uso di se stesso\-firmato, a certificati SSL in un ambiente di produzione possono consentire a utenti malintenzionati di un'organizzazione partner account di assumere il controllo dei server federativi in un'organizzazione partner risorse. Questo rischio di sicurezza esiste perché self\-i certificati autofirmati sono certificati radice. Devono essere aggiunti all'archivio radice attendibile di un altro server federativo \(ad esempio, il server federativo di risorsa\), che può rendere vulnerabile agli attacchi di tale server.  
  
Dopo aver ricevuto un certificato da una CA, assicurarsi che tutti i certificati vengano importati nell'archivio certificati personali del computer locale. È possibile importare i certificati nell'archivio personale con lo snap MMC certificati\-in.  
  
Come alternativa all'uso di certificati di allineamento\-, è anche possibile importare il certificato SSL con lo snap di gestione IIS\-nel momento in cui si assegna il protocollo SSL è certificato per il sito Web predefinito. Per altre informazioni, vedere [importare un certificato di autenticazione Server nel sito Web predefinito](../../ad-fs/deployment/Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md).  
  
> [!NOTE]  
> Prima di installare il software ADFS nel computer che diventerà il server federativo, assicurarsi che siano entrambi i certificati nell'archivio certificati personali del Computer locale e che il certificato SSL sia assegnato al sito Web predefinito. Per altre informazioni sull'ordine delle attività necessarie per configurare un server federativo, vedere [elenco di controllo: Configurazione di un Server federativo](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md).  
  
In base ai requisiti di sicurezza e di budget, valutare con attenzione quali certificati verranno ottenuti da una CA pubblica o da una CA aziendale. La figura seguente mostra le autorità di certificazione consigliate per l'emissione di specifici tipi di certificati. Queste indicazioni riflettono una migliore\-fare pratica approccio relative alla sicurezza e costi.  
  
![requisiti del certificato](media/adfs2_fedserver_certstory_1.png)  
  
## <a name="certificate-revocation-lists"></a>Elenchi di revoche di certificati  
Se per uno dei certificati usati esistono elenchi di revoca dei certificati, il server con il certificato configurato deve essere in grado di contattare il server che distribuisce gli elenchi di revoca di certificati.  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
