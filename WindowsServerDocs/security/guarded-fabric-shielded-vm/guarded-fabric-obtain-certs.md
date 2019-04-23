---
title: Ottenere i certificati per HGS
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: f4b4d1a8-bf6d-4881-9150-ddeca8b48038
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 14ee8eb6431a266d05897160d241d63e8cdb09a4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887732"
---
# <a name="obtain-certificates-for-hgs"></a>Ottenere i certificati per HGS

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

Quando si distribuisce HGS, verrà richiesto di fornire un certificato di firma e crittografia che viene usato per proteggere le informazioni riservate necessarie per avviare una macchina virtuale schermata.
Questi certificati mai lasciare HGS e vengono usate solo per le chiavi di decrittografia schermata della macchina virtuale quando si è dimostrata l'host in cui sono in esecuzione che è integro.
I tenant (proprietari della macchina virtuale) usare pubblico metà dei certificati per autorizzare il tuo Data Center per eseguire le proprie macchine virtuali schermate.
In questa sezione illustra i passaggi necessari per ottenere i certificati di firma e crittografia compatibili per HGS.

## <a name="request-certificates-from-your-certificate-authority"></a>Richiedere i certificati da un'autorità di certificazione

Sebbene non sia necessario, è consigliabile ottenere i certificati da un'autorità di certificazione attendibile.
In questo modo consente ai proprietari della macchina virtuale verificare che si autorizza il server HGS corretto (ad esempio provider di servizi o datacenter) per eseguire le proprie macchine virtuali schermate.
In uno scenario aziendale, è possibile usare la propria CA dell'organizzazione per emettere i certificati.
Provider di servizi e hoster deve provare a utilizzare un'autorità di certificazione ben noto e pubblico.

Certificati di firma e crittografia devono essere rilasciati con le seguenti proprietà di certificato (a meno che non contrassegnato come "consigliati"):

Proprietà modello di certificato | Valore obbligatorio 
------------------------------|----------------
Provider di crittografia               | Qualsiasi Provider di archiviazione chiavi (KSP). Legacy Cryptographic Service Provider (CSP) sono **non** supportato.
Algoritmo della chiave                 | RSA
Dimensione minima della chiave              | 2048 bit
Algoritmo di firma           | Consigliato: SHA256
Key usage                     | Firma digitale *e* crittografia dati
Utilizzo chiavi avanzato            | Autenticazione server
Criteri di rinnovo della chiave            | Rinnova con la stessa chiave. Rinnovo dei certificati di servizio HGS con chiavi diverse impedirà le macchine virtuali schermate di avvio.
Nome oggetto                  | Consigliato: l'indirizzo della società nome o web. Queste informazioni verranno visualizzate ai proprietari della macchina virtuale della creazione guidata file di dati schermatura.

Questi requisiti valgono se si utilizzano certificati supportati da hardware o software.
Per motivi di sicurezza, è consigliabile creare chiavi HGS in un modulo di protezione Hardware (HSM) per impedire che le chiavi private vengono copiati disattivato il sistema.
Seguire le indicazioni fornite dal fornitore del modulo di protezione hardware per richiedere certificati con attributi indicati sopra e assicurarsi di installare e autorizzare il KSP HSM in ogni nodo del servizio HGS.

Ogni nodo del servizio HGS richiederanno l'accesso per la stessa firma e i certificati di crittografia.
Se si utilizzano certificati basate su software, è possibile esportare i certificati in un file PFX con una password e consentire HGS gestire i certificati per l'utente.
È anche possibile scegliere installare i certificati nell'archivio certificati del computer locale in ogni nodo del servizio HGS e fornire l'identificazione personale di HGS.
Entrambe le opzioni sono illustrate nel [inizializzare il Cluster del servizio HGS](guarded-fabric-initialize-hgs.md) argomento.

## <a name="create-self-signed-certificates-for-test-scenarios"></a>Creare certificati autofirmati per scenari di test

Se si sta creando un ambiente di laboratorio HGS e non hanno o non desiderano usare un'autorità di certificazione, è possibile creare certificati autofirmati.
Si riceverà un avviso quando si importano le informazioni del certificato della creazione guidata file di dati schermatura, ma tutte le funzionalità subiranno modifiche.

Per creare certificati autofirmati e li Esporta in un file PFX, eseguire i comandi seguenti in PowerShell:

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

Tutte le chiavi e le informazioni riservate trasmessi tra gli host Hyper-V e HGS vengono crittografati a livello di messaggio, vale a dire, le informazioni vengono crittografate con chiavi note in HGS o Hyper-V, impedendo ad altri utenti di analisi del traffico di rete e rubi chiavi per le macchine virtuali.
Tuttavia, se si hanno reqiurements conformità o preferiscono semplicemente crittografare tutte le comunicazioni tra Hyper-V e HGS, è possibile configurare HGS con un certificato SSL che consente di crittografare tutti i dati a livello di trasporto.

I nodi HGS sia l'host Hyper-V saranno necessario considerare attendibile il certificato SSL che è fornire, pertanto si consiglia di richiedere il certificato SSL dall'autorità di certificazione dell'organizzazione. Quando si richiede il certificato, assicurarsi di specificare quanto segue:

Proprietà del certificato SSL | Valore obbligatorio
-------------------------|---------------
Nome oggetto             | Nome del servizio HGS cluster (nome rete distribuita). Ciò corrisponderà alla concatenazione del nome del servizio HGS fornito a `Initialize-HgsServer` e il nome di dominio del servizio HGS.
Nome alternativo del soggetto | Se si utilizzerà un nome DNS diverso per raggiungere il cluster del servizio HGS (ad esempio, se lo si trova dietro un bilanciamento del carico), assicurarsi di includere i nomi DNS nel campo della richiesta di certificato SAN.

Le opzioni per specificare questo certificato durante l'inizializzazione server HGS sono illustrate nella [configurare il primo nodo del servizio HGS](guarded-fabric-initialize-hgs.md).
È anche possibile aggiungere o modificare il certificato SSL in un secondo momento usando il [Set-HgsServer](https://docs.microsoft.com/powershell/module/hgsserver/set-hgsserver?view=win10-ps) cmdlet.

## <a name="next-step"></a>Passaggio successivo

>[!div class="nextstepaction"]
[Installare servizio HGS](guarded-fabric-choose-where-to-install-hgs.md)