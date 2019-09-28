---
title: Preparare il file CAPolicy.inf
description: Il file CAPolicy. inf contiene varie impostazioni che vengono usate durante l'installazione del servizio di certificazione Active Directory (AD CS) o durante il rinnovo del certificato della CA.
manager: alanth
ms.topic: article
ms.assetid: 65b36794-bb09-4c1b-a2e7-8fc780893d97
ms.prod: windows-server
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 810f6f8ba9e33f1f26f49f542ad6d23819deb463
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406288"
---
# <a name="capolicyinf-syntax"></a>Sintassi di CAPolicy. inf
>   Si applica a: Windows Server (Canale semestrale), Windows Server 2016

CAPolicy. inf è un file di configurazione che definisce le estensioni, i vincoli e altre impostazioni di configurazione applicate a un certificato CA radice e a tutti i certificati emessi dalla CA radice. Il file CAPolicy. inf deve essere installato in un server host prima dell'inizio della routine di installazione per la CA radice. Quando è necessario modificare le restrizioni di sicurezza in una CA radice, è necessario rinnovare il certificato radice e installare nel server un file CAPolicy. inf aggiornato prima che il processo di rinnovo venga avviato.

Il file CAPolicy. inf è:

-   Creato e definito manualmente da un amministratore

-   Utilizzato durante la creazione di certificati della CA radice e subordinata

-   Definito nella CA di firma dove si firma ed emette il certificato (non la CA a cui viene concessa la richiesta)

Dopo aver creato il file CAPolicy. inf, è necessario copiarlo nella cartella **% systemroot%** del server prima di installare gli ADC o rinnovare il certificato della CA.

Il file CAPolicy. inf consente di specificare e configurare un'ampia gamma di attributi e opzioni della CA. Nella sezione seguente vengono descritte tutte le opzioni per la creazione di un file con estensione inf adattato a specifiche esigenze.

## <a name="capolicyinf-file-structure"></a>Struttura del file CAPolicy. inf

I termini seguenti vengono usati per descrivere la struttura dei file. inf:

-   _Section_ : area del file che copre un gruppo logico di chiavi. I nomi delle sezioni nei file con estensione inf sono identificati da parentesi quadre. Molte, ma non tutte, le sezioni vengono usate per configurare le estensioni di certificato.

-   _Chiave_ : è il nome di una voce e viene visualizzato a sinistra del segno di uguale.

-   _Value_ : è il parametro e viene visualizzato a destra del segno di uguale.

Nell'esempio seguente **[Version]** è la sezione, **Signature** è la chiave e **"\$Windows NT @ no__t-4"** è il valore.

Esempio:

```PowerShell
[Version]                     #section
Signature="$Windows NT$"      #key=value
```

###  <a name="version"></a>Versione

Identifica il file come file con estensione inf. Version è l'unica sezione obbligatoria e deve trovarsi all'inizio del file CAPolicy. inf.

###  <a name="policystatementextension"></a>PolicyStatementExtension

Elenca i criteri definiti dall'organizzazione e indica se sono facoltativi o obbligatori. Più criteri sono separati da virgole. I nomi hanno un significato nel contesto di una distribuzione specifica o in relazione alle applicazioni personalizzate che verificano la presenza di questi criteri.

Per ogni criterio definito, è necessario che sia presente una sezione che definisce le impostazioni per quel particolare criterio. Per ogni criterio, è necessario fornire un identificatore di oggetto definito dall'utente (OID) e il testo che si desidera visualizzare come istruzione del criterio o un puntatore URL all'istruzione del criterio. L'URL può essere sotto forma di URL HTTP, FTP o LDAP.

Se si prevede un testo descrittivo nell'istruzione Policy, le tre righe successive di CAPolicy. inf avranno un aspetto simile al seguente:

```
[InternalPolicy]
OID=1.1.1.1.1.1.1
Notice=”Legal policy statement text”
```

Se si prevede di usare un URL per ospitare l'istruzione dei criteri di autorità di certificazione, le tre righe successive avranno un aspetto simile al seguente:

```
[InternalPolicy]
OID=1.1.1.1.1.1.2
URL=https://pki.wingtiptoys.com/policies/legalpolicy.asp
```

Inoltre:

-   Sono supportate più chiavi URL e di avviso.

-   Sono supportate le chiavi di avviso e URL nella stessa sezione dei criteri.

-   Gli URL con spazi o testo con spazi devono essere racchiusi tra virgolette. Questa condizione è valida per la chiave **URL** , indipendentemente dalla sezione in cui viene visualizzata.

Un esempio di più notifiche e URL in una sezione di criteri avrà un aspetto simile al seguente:

```
[InternalPolicy]
OID=1.1.1.1.1.1.1
URL=https://pki.wingtiptoys.com/policies/legalpolicy.asp
URL=ftp://ftp.wingtiptoys.com/pki/policies/legalpolicy.asp
Notice=”Legal policy statement text”
```

### <a name="crldistributionpoint"></a>CRLDistributionPoint

È possibile specificare i punti di distribuzione CRL (CDP) per un certificato CA radice nel file CAPolicy. inf.  Dopo aver installato la CA, è possibile configurare gli URL CDP inclusi nell'autorità di certificazione in ogni certificato emesso. Il certificato CA radice Mostra gli URL specificati in questa sezione del file CAPolicy. inf. 

```
[CRLDistributionPoint]
URL=http://pki.wingtiptoys.com/cdp/WingtipToysRootCA.crl
```

Altre informazioni su questa sezione:
-   Supporta
    - HTTP 
    - URL file
    - URL LDAP 
    - Più URL
   
    >[!IMPORTANT]
    >Non supporta gli URL HTTPS.

-   Le virgolette devono racchiudere gli URL con spazi.

-   Se non viene specificato alcun URL, ovvero se la sezione **[CRLDistributionPoint]** esiste nel file ma è vuota, l'estensione di accesso alle informazioni dell'autorità viene omessa dal certificato CA radice. Questa operazione è in genere preferibile quando si configura una CA radice. Windows non esegue il controllo delle revoche su un certificato CA radice, quindi l'estensione CDP è superflua in un certificato CA radice.

-    CA può pubblicare nel FILE UNC, ad esempio, in una condivisione che rappresenta la cartella di un sito Web in cui un client recupera tramite HTTP.

-   Usare questa sezione solo se si configura una CA radice o si rinnova il certificato CA radice. La CA determina le estensioni della CDP della CA subordinata.
   

### <a name="authorityinformationaccess"></a>AuthorityInformationAccess

È possibile specificare i punti di accesso alle informazioni sull'autorità nel file CAPolicy. inf per il certificato CA radice.

```
[AuthorityInformationAccess]
URL=http://pki.wingtiptoys.com/Public/myCA.crt
```

Altre note sulla sezione accesso alle informazioni sull'autorità:

-   Sono supportati più URL.

-   Gli URL HTTP, FTP, LDAP e FILE sono supportati. Gli URL HTTPS non sono supportati.

-   Questa sezione viene usata solo se si configura una CA radice o si rinnova il certificato CA radice. Le estensioni dell'AIA CA subordinate sono determinate dalla CA che ha emesso il certificato della CA subordinata.

-   Gli URL con spazi devono essere racchiusi tra virgolette.

-   Se non vengono specificati URL, ovvero se la sezione **[AuthorityInformationAccess]** esiste nel file ma è vuota, l'estensione del punto di distribuzione CRL viene omessa dal certificato CA radice. Anche in questo caso, si tratta dell'impostazione preferita nel caso di un certificato CA radice, poiché non esiste un'autorità superiore a una CA radice a cui deve essere fatto riferimento da un collegamento al relativo certificato.

### <a name="certsrv_server"></a>certsrv_Server

Un'altra sezione facoltativa di CAPolicy. inf è [certsrv_server], che consente di specificare la lunghezza della chiave di rinnovo, il periodo di validità del rinnovo e il periodo di validità dell'elenco di revoche di certificati (CRL) per un'autorità di certificazione che viene rinnovata o installata. Nessuna delle chiavi di questa sezione è obbligatoria. Molte di queste impostazioni includono valori predefiniti sufficienti per la maggior parte delle esigenze e possono essere semplicemente omessi dal file CAPolicy. inf. In alternativa, molte di queste impostazioni possono essere modificate dopo l'installazione dell'autorità di certificazione.

Un esempio potrebbe essere simile al seguente:

```
[certsrv_server]
RenewalKeyLength=2048
RenewalValidityPeriod=Years
RenewalValidityPeriodUnits=5
CRLPeriod=Days
CRLPeriodUnits=2
CRLDeltaPeriod=Hours
CRLDeltaPeriodUnits=4
ClockSkewMinutes=20
LoadDefaultTemplates=True
AlternateSignatureAlgorithm=0
ForceUTF8=0
EnableKeyCounting=0
```

**RenewalKeyLength** imposta la dimensione della chiave solo per il rinnovo. Viene usato solo quando viene generata una nuova coppia di chiavi durante il rinnovo del certificato della CA. La dimensione della chiave per il certificato della CA iniziale viene impostata durante l'installazione dell'autorità di certificazione.

Quando si rinnova un certificato CA con una nuova coppia di chiavi, la lunghezza della chiave può essere aumentata o diminuita. Ad esempio, se è stata impostata una dimensione della chiave della CA radice di 4096 byte o superiore, quindi si scopre di avere app Java o dispositivi di rete che possono supportare solo le dimensioni delle chiavi di 2048 byte. Se si aumentano o si riducono le dimensioni, è necessario eseguire nuovamente tutti i certificati emessi da tale CA.

**RenewalValidityPeriod** e **RenewalValidityPeriodUnits** stabiliscono la durata del nuovo certificato CA radice quando si rinnova il vecchio certificato CA radice. Si applica solo a una CA radice. La durata del certificato di una CA subordinata è determinata dal livello superiore. RenewalValidityPeriod può avere i valori seguenti: Hours, Days, weeks, months e years.

**CRLPeriod** e **CRLPeriodUnits** stabiliscono il periodo di validità per il CRL di base. **CRLPeriod** può avere i valori seguenti: Hours, Days, weeks, months e years.

**CRLDeltaPeriod** e **CRLDeltaPeriodUnits** stabiliscono il periodo di validità del Delta CRL. **CRLDeltaPeriod** può avere i valori seguenti: Hours, Days, weeks, months e years.

Ognuna di queste impostazioni può essere configurata dopo l'installazione dell'autorità di certificazione:

```
Certutil -setreg CACRLPeriod Weeks
Certutil -setreg CACRLPeriodUnits 1
Certutil -setreg CACRLDeltaPeriod Days
Certutil -setreg CACRLDeltaPeriodUnits 1
```

Ricordarsi di riavviare Active Directory Servizi certificati per rendere effettive le modifiche.

**LoadDefaultTemplates** si applica solo durante l'installazione di una CA globale (Enterprise). Questa impostazione, true o false (o 1 o 0), determina se la CA è configurata con uno dei modelli predefiniti.

In un'installazione predefinita della CA, un subset dei modelli di certificato predefiniti viene aggiunto alla cartella modelli di certificato nello snap-in autorità di certificazione. Ciò significa che non appena viene avviato il servizio Servizi certificati Active Directory dopo l'installazione del ruolo, un utente o un computer con autorizzazioni sufficienti può eseguire immediatamente la registrazione per un certificato.

È possibile che non si desideri emettere certificati immediatamente dopo l'installazione di un'autorità di certificazione, quindi è possibile usare l'impostazione LoadDefaultTemplates per impedire che i modelli predefiniti vengano aggiunti alla CA globale (Enterprise). Se non sono configurati modelli sulla CA, è possibile che non vengano rilasciati certificati.

**AlternateSignatureAlgorithm** configura la CA per supportare il formato di\#firma PKCS 1 v 2.1 sia per il certificato della CA che per le richieste di certificati. Se impostato su 1 in una CA radice, il certificato della CA includerà\#il formato di firma PKCS 1 v 2.1. Se impostata su una CA subordinata, la CA subordinata creerà una richiesta di certificato\#che include il formato di firma PKCS 1 v 2.1.

**ForceUTF8** modifica la codifica predefinita dei nomi distinti relativi (RDNS) nei nomi distinti del soggetto e dell'autorità emittente in UTF-8. Sono interessati solo i RDNs che supportano UTF-8, ad esempio quelli definiti come tipi di stringa di directory da una RFC. Ad esempio, l'RDN per il componente di dominio (DC) supporta la codifica come IA5 o UTF-8, mentre il paese RDN (C) supporta solo la codifica come stringa stampabile. La direttiva ForceUTF8 influirà quindi su un RDN del controller di dominio, ma non influirà su un RDN C.

**EnableKeyCounting** configura la CA per incrementare un contatore ogni volta che viene usata la chiave di firma della CA. Non abilitare questa impostazione a meno che non si disponga di un modulo di protezione hardware (HSM) e del provider del servizio di crittografia (CSP) associato che supporta il conteggio delle chiavi. Né il CSP Microsoft Strong né il provider di archiviazione chiavi del software Microsoft (KSP) supportano il conteggio delle chiavi.


## <a name="create-the-capolicyinf-file"></a>Creare il file CAPolicy. inf

Prima di installare Servizi certificati Active Directory, si configura il file CAPolicy. inf con impostazioni specifiche per la distribuzione.

**Prerequisiti** È necessario essere un membro del gruppo Administrators.

1. Nel computer in cui si prevede di installare Servizi certificati Active Directory, aprire Windows PowerShell, digitare **notepad c:\CAPolicy.inf** e premere INVIO.

2. Quando viene richiesto di creare un nuovo file, fare clic su **Sì**.

3. Immettere quanto segue come contenuto del file:
   ```
   [Version]  
   Signature="$Windows NT$"  
   [PolicyStatementExtension]  
   Policies=InternalPolicy  
   [InternalPolicy]  
   OID=1.2.3.4.1455.67.89.5  
   Notice="Legal Policy Statement"  
   URL=https://pki.corp.contoso.com/pki/cps.txt  
   [Certsrv_Server]  
   RenewalKeyLength=2048  
   RenewalValidityPeriod=Years  
   RenewalValidityPeriodUnits=5  
   CRLPeriod=weeks  
   CRLPeriodUnits=1  
   LoadDefaultTemplates=0  
   AlternateSignatureAlgorithm=1  
   [CRLDistributionPoint]  
   [AuthorityInformationAccess]
   ```
4. Fare clic su **file**, quindi su **Salva con nome**.

5. Passare alla cartella% SystemRoot%.

6. Verificare quanto segue:

   -   **Nome file** è impostato su **CAPolicy.inf**

   -   **Salva come** impostato su **Tutti i file**

   -   **Codifica** impostata su **ANSI**

7. Fare clic su **Salva**.

8. Quando viene richiesto di sovrascrivere il file, fare clic su **Sì**.

   ![Salva come percorso per il file CAPolicy. inf](../../../media/Prepare-the-CAPolicy-inf-File/001-SaveCAPolicyORCA1.gif)

   > [!CAUTION]
   >   Assicurarsi di salvare il file CAPolicy.inf con l'estensione inf. Se non digita specificamente **.inf** alla fine del nome del file e si selezionano le opzioni nel modo indicato, il file verrà salvato come file di testo e non verrà usato durante l'installazione della CA.

9. Chiudere Blocco note.

> [!IMPORTANT]
>   Nel file CAPolicy. inf, è possibile notare che è presente una riga che specifica https://pki.corp.contoso.com/pki/cps.txt l'URL. La sezione del criterio interno di CAPolicy.inf è riportata solo come esempio del modo in cui specificare il percorso di un'istruzione di pratica di certificazione (CPS). In questa guida non viene richiesto di creare l'istruzione per la procedura di certificazione (CPS).
