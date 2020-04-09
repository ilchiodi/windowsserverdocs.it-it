---
title: Ottenere i certificati per HGS
ms.prod: windows-server
ms.topic: article
ms.assetid: f4b4d1a8-bf6d-4881-9150-ddeca8b48038
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 09/25/2019
ms.openlocfilehash: da1ae4bacd5a6b2e38b22930aacf06f65b16bb29
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856534"
---
# <a name="obtain-certificates-for-hgs"></a>Ottenere i certificati per HGS

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

Quando si distribuisce HGS, verrà richiesto di fornire i certificati di firma e crittografia che vengono usati per proteggere le informazioni riservate necessarie per avviare una macchina virtuale schermata.
Questi certificati non lasciano mai HGS e vengono usati solo per decrittografare le chiavi della VM schermate quando l'host in cui sono in esecuzione ha dimostrato che è integro.
I tenant (proprietari di macchine virtuali) usano la metà pubblica dei certificati per autorizzare il Data Center a eseguire le macchine virtuali schermate.
Questa sezione illustra i passaggi necessari per ottenere i certificati di firma e crittografia compatibili per HGS.

## <a name="request-certificates-from-your-certificate-authority"></a>Richiedere certificati dall'autorità di certificazione

Sebbene non sia necessario, è consigliabile ottenere i certificati da un'autorità di certificazione attendibile.
In questo modo, i proprietari delle macchine virtuali verificano che stiano autorizzando il server HGS corretto, ad esempio il provider di servizi o il Data Center, per eseguire le VM schermate.
In uno scenario aziendale, è possibile scegliere di usare la CA globale (Enterprise) per emettere questi certificati.
I provider di servizi di hosting e i provider di servizi devono prendere in considerazione l'uso di una CA pubblica e nota.

I certificati di firma e di crittografia devono essere rilasciati con le seguenti proprietà di Certificiate (a meno che non sia contrassegnato "Recommended"):

Proprietà del modello di certificato | Valore obbligatorio 
------------------------------|----------------
Provider di crittografia               | Qualsiasi provider di archiviazione chiavi (KSP). I provider del servizio di crittografia (CSP) legacy **non** sono supportati.
Algoritmo chiave                 | RSA
Dimensioni minime chiave              | 2048 bit
Algoritmo di firma           | Consigliato: SHA256
Utilizzo chiavi                     | Firma digitale *e* crittografia dei dati
Utilizzo chiavi avanzato            | Autenticazione server
Criteri di rinnovo della chiave            | Rinnovare con la stessa chiave. Il rinnovo dei certificati HGS con chiavi diverse impedisce l'avvio delle macchine virtuali schermate.
Nome oggetto                  | Consigliato: il nome o l'indirizzo Web della società. Queste informazioni verranno visualizzate ai proprietari delle macchine virtuali nella creazione guidata file di dati di schermatura.

Questi requisiti si applicano se si utilizzano certificati supportati da hardware o software.
Per motivi di sicurezza, è consigliabile creare le chiavi di HGS in un modulo di protezione hardware (HSM) per impedire che le chiavi private vengano copiate fuori dal sistema.
Seguire le istruzioni del fornitore del modulo di protezione hardware per richiedere certificati con gli attributi indicati in precedenza e assicurarsi di installare e autorizzare il provider di archiviazione chiavi HSM in ogni nodo HGS.

Ogni nodo HGS richiederà l'accesso agli stessi certificati di firma e crittografia.
Se si usano certificati supportati da software, è possibile esportare i certificati in un file PFX con una password e consentire a HGS di gestire i certificati.
È anche possibile scegliere di installare i certificati nell'archivio certificati del computer locale in ogni nodo HGS e fornire l'identificazione personale a HGS.
Entrambe le opzioni sono descritte nell'argomento [inizializzare il cluster HGS](guarded-fabric-initialize-hgs.md) .

## <a name="create-self-signed-certificates-for-test-scenarios"></a>Creare certificati autofirmati per gli scenari di test

Se si sta creando un ambiente lab HGS e non si vuole usare un'autorità di certificazione, è possibile creare certificati autofirmati.
Si riceverà un avviso quando si importano le informazioni sul certificato nella creazione guidata file di dati di schermatura, ma tutte le funzionalità rimarranno invariate.

Per creare certificati autofirmati ed esportarli in un file PFX, eseguire i comandi seguenti in PowerShell:

```powershell
$certificatePassword = Read-Host -AsSecureString -Prompt "Enter a password for the PFX file"

$signCert = New-SelfSignedCertificate -Subject "CN=HGS Signing Certificate"
Export-PfxCertificate -FilePath .\signCert.pfx -Password $certificatePassword -Cert $signCert
Remove-Item $signCert.PSPath

$encCert = New-SelfSignedCertificate -Subject "CN=HGS Encryption Certificate"
Export-PfxCertificate -FilePath .\encCert.pfx -Password $certificatePassword -Cert $encCert
Remove-Item $encCert.PSPath
```

## <a name="request-an-ssl-certificate"></a>Richiedere un certificato SSL

Tutte le chiavi e le informazioni riservate trasmesse tra gli host Hyper-V e HGS vengono crittografate a livello di messaggio, ovvero le informazioni vengono crittografate con le chiavi note a HGS o Hyper-V, impedendo a un utente di sniffare il traffico di rete e di rubare le chiavi alle macchine virtuali.
Tuttavia, se si dispone di reqiurements di conformità o si preferisce semplicemente crittografare tutte le comunicazioni tra Hyper-V e HGS, è possibile configurare HGS con un certificato SSL che consentirà di crittografare tutti i dati a livello di trasporto.

Sia gli host Hyper-V che i nodi HGS devono considerare attendibile il certificato SSL fornito, quindi è consigliabile richiedere il certificato SSL dall'autorità di certificazione dell'organizzazione. Quando si richiede il certificato, assicurarsi di specificare quanto segue:

Proprietà certificato SSL | Valore obbligatorio
-------------------------|---------------
Nome oggetto             | Nome del cluster HGS, noto come nome di rete distribuita o FQDN dell'oggetto computer virtuale. Questa sarà la concatenazione del nome del servizio HGS fornito per `Initialize-HgsServer` e il nome di dominio HGS.
Nome alternativo oggetto | Se si usa un nome DNS diverso per raggiungere il cluster HGS (ad esempio, se si trova dietro un servizio di bilanciamento del carico), assicurarsi di includere i nomi DNS nel campo SAN della richiesta di certificato.

Le opzioni per specificare questo certificato durante l'inizializzazione del server HGS sono descritte in [configurare il primo nodo HGS](guarded-fabric-initialize-hgs.md).
È anche possibile aggiungere o modificare il certificato SSL in un secondo momento usando il cmdlet [set-HgsServer](https://docs.microsoft.com/powershell/module/hgsserver/set-hgsserver?view=win10-ps) .

## <a name="next-step"></a>Passaggio successivo

> [!div class="nextstepaction"]
> [Installare HGS](guarded-fabric-choose-where-to-install-hgs.md)