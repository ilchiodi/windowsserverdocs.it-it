---
title: Preparare il file CAPolicy.inf
description: Il file CAPolicy. inf contiene varie impostazioni che vengono usate quando si installa il servizio di certificazione Active Directory (AD CS) o durante il rinnovo del certificato CA.
manager: alanth
ms.topic: article
ms.assetid: 65b36794-bb09-4c1b-a2e7-8fc780893d97
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 19a87df7c4f165d3b0e6c5add4bc40ff97cc87cb
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446467"
---
# <a name="capolicyinf-syntax"></a>Sintassi di CAPolicy. inf
>   Si applica a: Windows Server (canale semestrale), Windows Server 2016

Il file CAPolicy. inf è un file di configurazione che definisce le estensioni, vincoli e altre impostazioni di configurazione che vengono applicate a un certificato CA radice e tutti i certificati rilasciati dalla CA radice. Il file CAPolicy. inf deve essere installato in un server host prima la routine di installazione per la radice che della CA viene avviata. Quando vengono modificati le restrizioni di sicurezza in una CA radice, deve essere rinnovato il certificato radice e un file CAPolicy. inf aggiornato deve essere installato nel server prima che inizi il processo di rinnovo.

Il file CAPolicy. inf è:

-   Creata e definita manualmente dall'amministratore

-   Utilizzati durante la creazione di radice e certificati della CA subordinata

-   Definiti nella firma CA in cui si accedere ed emettere il certificato (non la CA in cui viene concesso le richiesta)

Dopo aver creato il file CAPolicy. inf, è necessario copiarlo nel **% systemroot %** cartella del server prima di installare Servizi certificati Active Directory o rinnovare il certificato della CA.

Il file CAPolicy. inf rende possibile specificare e configurare un'ampia gamma di opzioni e gli attributi di autorità di certificazione. La sezione seguente descrive tutte le opzioni per creare un file. inf personalizzate in base a esigenze specifiche.

## <a name="capolicyinf-file-structure"></a>Struttura del File CAPolicy. inf

I termini seguenti vengono utilizzati per descrivere la struttura di file. inf:

-   _Sezione_ – è un'area del file che descrive un gruppo logico di chiavi. I nomi delle sezioni nel file con estensione inf sono identificati da visualizzati tra parentesi quadre. Le sezioni molte, ma non tutte, vengono utilizzate per configurare le estensioni del certificato.

-   _Chiave_ : è il nome di una voce e viene visualizzato a sinistra del segno di uguale.

-   _Valore_ : è il parametro e viene visualizzato a destra del segno di uguale.

Nell'esempio riportato di seguito **[versione]** è la sezione **firma** è la chiave, e **"\$Windows NT\$"** è il valore.

Esempio:

```PowerShell
[Version]                     #section
Signature="$Windows NT$"      #key=value
```

###  <a name="version"></a>Version

Identifica il file con estensione inf. Versione è l'unica sezione obbligatorio e deve trovarsi all'inizio del file CAPolicy. inf.

###  <a name="policystatementextension"></a>PolicyStatementExtension

Elenca i criteri che sono stati definiti dall'organizzazione, e se sono obbligatori o facoltativi. Più criteri sono separati da virgole. I nomi hanno un significato nel contesto di una distribuzione specifica, o in relazione ad applicazioni personalizzate che consentono di controllare la presenza di questi criteri.

Per ogni criterio definita, deve esistere una sezione che definisce le impostazioni per il criterio specifico. Per ogni criterio, è necessario fornire un identificatore di oggetto definito dall'utente (OID) e il testo da visualizzare come l'istruzione del criterio o un puntatore di URL per l'istruzione del criterio. L'URL può essere sotto forma di un HTTP, FTP o URL LDAP.

Se si intende avere un testo descrittivo nell'istruzione del criterio, le tre righe successive del file CAPolicy. inf si presenta come:

```
[InternalPolicy]
OID=1.1.1.1.1.1.1
Notice=”Legal policy statement text”
```

Se si intende usare un URL per ospitare l'istruzione del criterio della CA, quindi tre righe successive sarà invece simile:

```
[InternalPolicy]
OID=1.1.1.1.1.1.2
URL=https://pki.wingtiptoys.com/policies/legalpolicy.asp
```

Inoltre:

-   Sono supportate più chiavi di URL e l'avviso.

-   Le chiavi di avviso e l'URL nella sezione criteri stessi sono supportate.

-   Testo con spazi o gli URL contenenti spazi deve essere racchiuso tra virgolette. Questo vale per il **URL** chiave, indipendentemente dalla sezione in cui è presente.

Un esempio di più avvisi e gli URL in una sezione di criteri si presenta come:

```
[InternalPolicy]
OID=1.1.1.1.1.1.1
URL=https://pki.wingtiptoys.com/policies/legalpolicy.asp
URL=ftp://ftp.wingtiptoys.com/pki/policies/legalpolicy.asp
Notice=”Legal policy statement text”
```

### <a name="crldistributionpoint"></a>CRLDistributionPoint

È possibile specificare i punti di distribuzione CRL (CDP) per un certificato CA radice nel file CAPolicy. inf.  Dopo aver installato l'autorità di certificazione, è possibile configurare gli URL CDP che include l'autorità di certificazione in ogni certificato rilasciato. Il certificato CA radice vengono visualizzati gli URL specificati in questa sezione del file CAPolicy. inf. 

```
[CRLDistributionPoint]
URL=http://pki.wingtiptoys.com/cdp/WingtipToysRootCA.crl
```

Informazioni aggiuntive su questa sezione:
-   supporta:
    - HTTP 
    - URL di file
    - URL LDAP 
    - Più URL
   
    >[!IMPORTANT]
    >Non supporta URL HTTPS.

-   Le virgolette devono racchiudere gli URL con spazi.

-   Se è stato specificato alcun URL, vale a dire, se il **[CRLDistributionPoint]** sezione nel file è presente ma è vuota – l'estensione AIA viene omesso dal certificato radice CA. Si tratta in genere preferibile quando si configura una CA radice. Windows non eseguirà revoche su un certificato CA radice, in modo che l'estensione CDP è superfluo a partire da un certificato CA radice.

-    Autorità di certificazione può pubblicare al FILE UNC, ad esempio, in una condivisione che rappresenta la cartella di un sito Web in cui un client recupera tramite HTTP.

-   Usare questa sezione solo se si desidera configurare una CA radice o rinnovare il certificato CA radice. L'autorità di certificazione determina le estensioni CDP CA subordinate.
   

### <a name="authorityinformationaccess"></a>AuthorityInformationAccess

È possibile specificare i punti di accesso di informazioni autorità nel file CAPolicy. inf per il certificato CA radice.

```
[AuthorityInformationAccess]
URL=http://pki.wingtiptoys.com/Public/myCA.crt
```

Alcune note aggiuntive sulla sezione di accesso di informazioni autorità:

-   Sono supportati più URL.

-   HTTP, FTP, LDAP e gli URL di FILE sono supportati. Gli URL HTTPS non sono supportati.

-   Questa sezione viene usata solo se si configurare una CA radice o rinnovare il certificato CA radice. Le estensioni AIA CA subordinate sono determinate dall'autorità di certificazione che ha emesso il certificato della CA subordinata.

-   Gli URL contenenti spazi devono essere racchiuso tra virgolette.

-   Se è stato specificato alcun URL, vale a dire, se il **[AuthorityInformationAccess]** sezione nel file è presente ma è vuota, l'estensione punto di distribuzione CRL viene omesso dal certificato radice della CA. Anche in questo caso, sarebbe l'impostazione preferita in caso di un certificato CA radice come nessuna autorità superiore a una CA radice che sarà necessario farvi riferimento da un collegamento al relativo certificato.

### <a name="certsrvserver"></a>certsrv_Server

Un'altra sezione facoltativa del file CAPolicy. inf è [certsrv_server], che consente di specificare la lunghezza della chiave rinnovo, il periodo di validità di rinnovo e il periodo di validità certificato revoche di certificati (CRL) elenco per un'autorità di certificazione viene rinnovato o installato. Le chiavi presenti in questa sezione non sono più necessari. Molte di queste impostazioni hanno valori predefiniti sono sufficienti per la maggior parte delle esigenze e possono essere omesse semplicemente dal file CAPolicy. inf. In alternativa, molte di queste impostazioni può essere modificati dopo aver installato l'autorità di certificazione.

Un esempio sarebbe simile:

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

**RenewalKeyLength** imposta la dimensione della chiave solo per il rinnovo. Viene utilizzato solo quando una nuova coppia di chiavi viene generata durante il rinnovo del certificato della CA. La dimensione della chiave per il certificato della CA iniziale viene impostata quando viene installato l'autorità di certificazione.

Durante il rinnovo di un certificato della CA con una nuova coppia di chiavi, la lunghezza della chiave può essere aumentata o diminuita. Ad esempio, se è stata impostata una radice di dimensioni della chiave della CA di 4096 byte o versione successiva e quindi scoprire che è necessario App Java o dispositivi di rete che possono supportare solo dimensioni della chiave di 2048 byte. Se si aumentano o riducono le dimensioni, è necessario eseguire nuovamente tutti i certificati rilasciati dalla CA in questione.

**RenewalValidityPeriod** e **RenewalValidityPeriodUnits** stabilire la durata del nuovo certificato CA radice quando rinnovo il certificato CA radice precedente. Si applica solo a una CA radice. La durata del certificato della CA subordinata è determinata da quella di livello superiore. RenewalValidityPeriod può avere i valori seguenti: Ore, giorni, settimane, mesi e anni.

**Parametri CRLPeriod** e **CRLPeriodUnits** stabilire il periodo di validità per il CRL di base. **Parametri CRLPeriod** può avere i valori seguenti: Ore, giorni, settimane, mesi e anni.

**CRLDeltaPeriod** e **CRLDeltaPeriodUnits=0** stabilire il periodo di validità dei delta CRL. **CRLDeltaPeriod** può avere i valori seguenti: Ore, giorni, settimane, mesi e anni.

Ognuna di queste impostazioni può essere configurata dopo aver installato l'autorità di certificazione:

```
Certutil -setreg CACRLPeriod Weeks
Certutil -setreg CACRLPeriodUnits 1
Certutil -setreg CACRLDeltaPeriod Days
Certutil -setreg CACRLDeltaPeriodUnits 1
```

Ricordarsi di riavviare Servizi certificati Active Directory per tutte le modifiche per rendere effettive.

**LoadDefaultTemplates=0** si applica solo durante l'installazione di una CA dell'organizzazione. Questa impostazione, entrambi True o False (o 1 o 0), determina se l'autorità di certificazione è configurata con uno qualsiasi dei modelli predefiniti.

In un'installazione predefinita dell'autorità di certificazione, un subset dei modelli di certificato predefinito viene aggiunto alla cartella dei modelli di certificato nello snap-in Autorità di certificazione. Ciò significa che non appena viene avviato il servizio Servizi certificati Active Directory dopo aver installato il ruolo di un utente o un computer dotato di autorizzazioni sufficienti possono registrarsi immediatamente per un certificato.

Non è possibile emettere alcun certificato immediatamente dopo l'installazione di un'autorità di certificazione, quindi è possibile usare l'impostazione LoadDefaultTemplates=0 per impedire l'aggiunta all'autorità di certificazione dell'organizzazione di modelli predefiniti. Se non sono disponibili modelli configurati nell'autorità di certificazione è non possibile emettere alcun certificato.

**Alternatesignaturealgorithm=1** consente di configurare l'autorità di certificazione per supportare il PKCS\#1 V2.1 formato della firma per il certificato della CA e le richieste di certificati. Se impostato su 1 in una radice della CA il certificato della CA includerà il PKCS\#1 V2.1 formato della firma. Se impostato su una CA subordinata, l'autorità di certificazione subordinata verrà creata una richiesta di certificato che include il PKCS\#1 V2.1 formato della firma.

**ForceUTF8** modifica l'impostazione predefinita la codifica dei nomi distinti relativi (nomi RDN) nell'oggetto e autorità emittente di nomi in UTF-8 distinti. Solo i nomi RDN che supporta UTF-8, ad esempio quelli che vengono definiti come tipi di stringa di Directory da una richiesta di modifica, sono interessate. Ad esempio, il nome distinto relativo componente di dominio (DC) supporta la codifica come IA5 o UTF-8, mentre il Country RDN (C) supporta solo la codifica sotto forma di stringa stampabili. La direttiva ForceUTF8 avrà pertanto effetto su RDN un controller di dominio, ma non avrà effetto su un RDN C.

**EnableKeyCounting** consente di configurare la CA per incrementare un contatore ogni volta che viene usata chiave di firma della CA. Non abilitare questa impostazione se non si dispone di un modulo di protezione Hardware (HSM) e provider associato del servizio di crittografia (CSP) che supporta il conteggio di chiavi. Microsoft sicuro CSP né il Provider archiviazione chiavi (KSP) di Microsoft Software supporto chiave il conteggio.


## <a name="create-the-capolicyinf-file"></a>Creare il file CAPolicy. inf

Prima di installare Servizi certificati Active Directory, si configura il file CAPolicy. inf con impostazioni specifiche per la distribuzione.

**Prerequisito:** È necessario essere un membro del gruppo Administrators.

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
4. Fare clic su **File**, quindi fare clic su **Salva con nome**.

5. Passare alla cartella % systemroot %.

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
>   Nel file CAPolicy. inf, è possibile visualizzare una riga specificando l'URL https://pki.corp.contoso.com/pki/cps.txt. La sezione del criterio interno di CAPolicy.inf è riportata solo come esempio del modo in cui specificare il percorso di un'istruzione di pratica di certificazione (CPS). In questa Guida non viene richiesto per creare l'istruzione di pratica di certificato (CPS).
