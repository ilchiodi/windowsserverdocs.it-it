---
title: Preparare il File CAPolicy. inf
description: Il CAPolicy.inf contiene varie impostazioni che vengono utilizzate quando si installa il servizio di certificazione di Active Directory (AD CS) o durante il rinnovo della CA certificato.
manager: alanth
ms.topic: article
ms.assetid: 65b36794-bb09-4c1b-a2e7-8fc780893d97
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9618d4abe512b487f4f22ffde85a052c1c52ef22
ms.sourcegitcommit: fb4e2ace2e0a29e0f6b028f1cb945cab6aa6ee2c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/05/2018
---
# <a name="capolicyinf-syntax"></a>Sintassi di CAPolicy.inf
>   Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Il CAPolicy.inf è un file di configurazione che definisce le estensioni, vincoli e altre impostazioni di configurazione che vengono applicate a un certificato della CA radice e tutti i certificati rilasciati dalla CA radice. Il file CAPolicy.inf deve essere installato in un server host prima la routine di installazione per la radice di che autorità di certificazione cresce. Quando vengono modificati le restrizioni di sicurezza in una CA radice, deve essere rinnovato il certificato radice e un file CAPolicy.inf aggiornato deve essere installato nel server prima di iniziare il processo di rinnovo.

Il CAPolicy.inf è:

-   Creato e definiti manualmente dall'amministratore

-   Utilizzati durante la creazione di certificati radice e subordinati CA

-   Definito nella CA firma in cui accedi e rilasciare il certificato (non l'autorità di certificazione in cui la richiesta viene accettata)

Dopo aver creato il file CAPolicy.inf, è necessario copiare i dati nel **% systemroot %** cartella del server prima di installare Servizi certificati Active Directory o rinnovare il certificato della CA.

Il CAPolicy.inf consente di specificare e configurare una vasta gamma di opzioni e gli attributi di autorità di certificazione. Nella sezione seguente descrive tutte le opzioni per la creazione di un file. inf personalizzata in base alle proprie esigenze specifiche.

## <a name="capolicyinf-file-structure"></a>Struttura del File CAPolicy.inf

Per descrivere la struttura dei file. inf vengono utilizzati i termini seguenti:

-   _Sezione_ – è un'area del file che descrive un gruppo logico delle chiavi. I nomi di sezione nel file con estensione inf vengono identificati visualizzati tra parentesi quadre. Sezioni di molti, ma non tutte, vengono utilizzate per configurare le estensioni del certificato.

-   _Chiave_ : è il nome di una voce e viene visualizzato a sinistra del segno di uguale.

-   _Valore_ : è il parametro e viene visualizzato a destra del segno di uguale.

Nell'esempio seguente, **[versione]** è la sezione **firma** è la chiave, e **"\ $ Windows NT \ $"** è il valore.

Esempio:

```PowerShell
[Version]                     #section
Signature="$Windows NT$"      #key=value
```

###  <a name="version"></a>Versione

Identifica il file con estensione inf. Versione è l'unica questa sezione obbligatoria e deve essere all'inizio del file CAPolicy.inf.

###  <a name="policystatementextension"></a>PolicyStatementExtension

Sono elencati i criteri che sono stati definiti dall'organizzazione, e sono obbligatori o facoltativi. Più criteri sono separati da virgole. I nomi hanno un significato nel contesto di una distribuzione specifica, o in relazione a applicazioni personalizzate per verificare la presenza di questi criteri.

Per ogni criterio definito, deve essere presente una sezione che definisce le impostazioni per tale criterio specifico. Per ogni criterio, è necessario fornire un identificatore di oggetto definito dall'utente (OID) e il testo che si desidera visualizzare come l'istruzione dei criteri o URL puntatore all'istruzione di criteri. L'URL può essere sotto forma di un URL di LDAP, HTTP o FTP.

Se si intende disporre testo descrittivo nell'istruzione dei criteri, le tre righe successive di CAPolicy.inf sarà simile:

```
[InternalPolicy]
OID=1.1.1.1.1.1.1
Notice=”Legal policy statement text”
```

Se si intende utilizzare un URL per ospitare la dichiarazione della CA, quindi tre righe sarebbe invece simile:

```
[InternalPolicy]
OID=1.1.1.1.1.1.2
URL=http://pki.wingtiptoys.com/policies/legalpolicy.asp
```

Inoltre:

-   Sono supportate più chiavi URL e l'avviso.

-   Le chiavi di avviso e l'URL nella stessa sezione criteri sono supportate.

-   Gli URL contenenti spazi o testo con spazi deve essere racchiuso tra virgolette. Ciò vale per il **URL** chiave, indipendentemente dalla sezione in cui è presente.

Un esempio di più avvisi e gli URL in una sezione criteri sarà simile a:

```
[InternalPolicy]
OID=1.1.1.1.1.1.1
URL=http://pki.wingtiptoys.com/policies/legalpolicy.asp
URL=ftp://ftp.wingtiptoys.com/pki/policies/legalpolicy.asp
Notice=”Legal policy statement text”
```

### <a name="crldistributionpoint"></a>CRLDistributionPoint

È possibile specificare i punti di distribuzione CRL (CDP) per un certificato della CA radice nel CAPolicy.inf. Dopo aver installato l'autorità di certificazione è possibile configurare gli URL CDP che include l'autorità di certificazione in ogni certificato rilasciato. Sono inclusi gli URL specificati in questa sezione del file CAPolicy.inf nel certificato della CA radice stesso.

```
[CRLDistributionPoint]
URL=http://pki.wingtiptoys.com/cdp/WingtipToysRootCA.crl
```

Alcune informazioni aggiuntive in questa sezione:

-   Sono supportati più URL.

-   Sono supportati HTTP, FTP e URL LDAP. URL HTTPS non sono supportati.

-   In questa sezione viene utilizzata solo se si desidera configurare una CA radice o rinnovare il certificato CA radice. Le estensioni CDP CA subordinate sono determinate dall'autorità di certificazione che emette il certificato della CA subordinata.

-   Gli URL contenenti spazi devono essere racchiusi tra virgolette.

-   Se non vengono specificati URL, ovvero, se il **[CRLDistributionPoint]** sezione presente nel file, ma è vuota, l'estensione punto di distribuzione CRL viene specificato il certificato CA radice. Si tratta in genere è preferibile durante la configurazione di una CA radice. Windows non esegue revoche su un certificato della CA radice, quindi l'estensione CDP è superfluo in un certificato della CA radice.

### <a name="authorityinformationaccess"></a>AuthorityInformationAccess

È possibile specificare i punti di accesso di informazioni autorità nel CAPolicy.inf per il certificato CA radice.

```
[AuthorityInformationAccess]
URL=http://pki.wingtiptoys.com/Public/myCA.crt
```

Note aggiuntive nella sezione autorità informazioni accesso:

-   Sono supportati più URL.

-   HTTP, FTP, LDAP e gli URL di FILE supportati. URL HTTPS non sono supportati.

-   In questa sezione viene utilizzata solo se si esegue l'impostazione di una CA radice o rinnovare il certificato CA radice. Estensioni AIA CA subordinate sono determinate dall'autorità di certificazione che ha rilasciato il certificato della CA subordinata.

-   Gli URL contenenti spazi devono essere racchiusi tra virgolette.

-   Se non vengono specificati URL, ovvero, se il **[AuthorityInformationAccess]** sezione presente nel file, ma è vuota, l'estensione punto di distribuzione CRL viene specificato il certificato CA radice. Nuovamente, questa funzionalità è l'impostazione preferita nel caso di un certificato della CA radice perché non esiste alcuna autorità superiore di una CA radice che devi fare riferimento tramite un collegamento per il certificato.

### <a name="certsrvserver"></a>certsrv_Server

Un'altra sezione facoltativa di CAPolicy.inf è [certsrv_server], che viene utilizzato per specificare il periodo di validità certificato revoche di certificati (CRL) di elenco, il periodo di validità di rinnovo e lunghezza della chiave rinnovo per un'autorità di certificazione è viene rinnovato o installato. Nessuno dei tasti in questa sezione sono necessari. Molte di queste impostazioni presentano valori predefiniti sono sufficienti per la maggior parte delle esigenze e possono essere omesso semplicemente dal file CAPolicy.inf. In alternativa, molte di queste impostazioni possono essere modificate dopo aver installato l'autorità di certificazione.

Sarà simile a un esempio:

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

**RenewalKeyLength** imposta la dimensione della chiave solo per il rinnovo. Viene utilizzato solo quando una nuova coppia di chiavi viene generata durante il rinnovo del certificato CA. La dimensione della chiave per il certificato della CA iniziale è impostata quando viene installato l'autorità di certificazione.

Quando il rinnovo di un certificato della CA con una nuova coppia di chiavi, la lunghezza della chiave può essere aumentata o ridotto. Ad esempio, se è stata impostata una dimensione di chiave della CA o superiore a 4096 byte radice e quindi individuare che hai Java App o dispositivi di rete in grado di supportare solo dimensioni della chiave di 2048 byte. Se si aumenta o diminuisce le dimensioni, è necessario eseguire nuovamente tutti i certificati rilasciati dalla CA.

**RenewalValidityPeriod** e **RenewalValidityPeriodUnits** stabilire la durata del nuovo certificato CA radice quando rinnovare il certificato CA radice precedente. Si applica solo a una CA radice. Il ciclo di vita di un'autorità di certificazione subordinata è quella di livello superiore. RenewalValidityPeriod può avere i seguenti valori: anni, mesi, settimane, giorni e ore.

**Parametri CRLPeriod** e **CRLPeriodUnits** stabilire il periodo di validità per il CRL di base. **Parametri CRLPeriod** può avere i seguenti valori: anni, mesi, settimane, giorni e ore.

**CRLDeltaPeriod** e **CRLDeltaPeriodUnits** stabilire il periodo di validità dei delta CRL. **CRLDeltaPeriod** può avere i seguenti valori: anni, mesi, settimane, giorni e ore.

Ognuna di queste impostazioni può essere configurata dopo aver installato l'autorità di certificazione:

```
Certutil -setreg CACRLPeriod Weeks
Certutil -setreg CACRLPeriodUnits 1
Certutil -setreg CACRLDeltaPeriod Days
Certutil -setreg CACRLDeltaPeriodUnits 1
```

Ricordare di riavviare Servizi certificati Active Directory per tutte le modifiche abbiano effetto.

**LoadDefaultTemplates** si applica solo durante l'installazione di una CA dell'organizzazione. Questa impostazione, uno True o False (1 o 0), determina se la CA sia configurata con uno qualsiasi dei modelli predefiniti.

In un'installazione predefinita dell'autorità di certificazione, viene aggiunto un sottoinsieme di modelli di certificato predefiniti nella cartella modelli di certificato nello snap-in Autorità di certificazione. Ciò significa che non appena il servizio Servizi certificati Active Directory viene avviato dopo aver installato il ruolo di un utente o computer con autorizzazioni sufficienti può immediatamente registrare un certificato.

Potrebbe non voler emettere eventuali certificati immediatamente dopo l'installazione di un'autorità di certificazione, in modo è possibile utilizzare l'impostazione LoadDefaultTemplates per impedire l'aggiunta all'autorità di certificazione dell'organizzazione i modelli predefiniti. Se non sono presenti modelli configurati nell'autorità di certificazione è non possibile emettere alcun certificato.

**AlternateSignatureAlgorithm** consente di configurare l'autorità di certificazione per supportare il formato di firma PKCS\ #1 v 2.1 per il certificato della CA e le richieste di certificati. Se impostato su 1 in una radice CA il certificato della CA includerà il formato di firma PKCS\ #1 v 2.1. Se impostato su subordinata autorità di certificazione, la CA subordinata creerà una richiesta di certificato che include il formato di firma PKCS\ #1 v 2.1.

**ForceUTF8** cambia il valore predefinito di codifica dei nomi distinti (dominio) nell'oggetto e autorità emittente nomi UTF-8 distinti. Solo i nomi RDN che supportano UTF-8, ad esempio quelli che sono definite come tipi di stringa della Directory di un cambiamento, sono interessati. Ad esempio, il nome distinto relativo componente per dominio (DC) supporta la codifica come IA5 o UTF-8, mentre il Country RDN (C) supporta solo la codifica come stringa stampabile. La direttiva ForceUTF8 avrà pertanto RDN un controller di dominio ma non avrà effetto su un RDN C.

**EnableKeyCounting** consente di configurare l'autorità di certificazione per incrementare un contatore ogni volta che viene utilizzata chiave di firma della CA. Non abilitare questa impostazione a meno che non si dispone di un modulo di protezione Hardware (HSM) e i provider associato del servizio di crittografia (CSP) che supporta il conteggio chiave. Il provider CSP sicuro Microsoft né il Microsoft Provider di archiviazione chiavi (KSP) Software supporto chiave conteggio.


## <a name="create-the-capolicyinf-file"></a>Creare il file CAPolicy.inf

Prima di installare Servizi certificati Active Directory, configurare file CAPolicy. inf con impostazioni specifiche per la distribuzione.

**Prerequisito:** è necessario essere un membro del gruppo Administrators.

1.  Nel computer in cui si prevede di installare Servizi certificati Active Directory, aprire Windows PowerShell, digitare **blocco note c:.inf** e premere INVIO.

2.  Quando viene richiesto di creare un nuovo file, fare clic su **Sì**.

3.  Immettere quanto segue come contenuto del file:
   ```
   [Version]  
    Signature="$Windows NT$"  
    [PolicyStatementExtension]  
    Policies=InternalPolicy  
    [InternalPolicy]  
    OID=1.2.3.4.1455.67.89.5  
    Notice="Legal Policy Statement"  
    URL=http://pki.corp.contoso.com/pki/cps.txt  
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
1.  Fare clic su **File**, quindi fare clic su **Salva con nome**.

2.  Passare alla cartella % systemroot %.

3.  Verificare quanto segue:

    -   **Nome del file** è impostato su **file CAPolicy. inf**

    -   **Salva come** è impostato su **tutti i file**

    -   **Codifica** è **ANSI**

4.  Fare clic su **salvare**.

5.  Quando viene chiesto di sovrascrivere il file, fare clic su **Sì**.

    ![Salva come percorso per il file CAPolicy.inf](../../../media/Prepare-the-CAPolicy-inf-File/001-SaveCAPolicyORCA1.gif)

    >   [!CAUTION]  
    >   Assicurarsi di salvare il CAPolicy.inf con l'estensione inf. Se non digita specificamente **. inf** alla fine del nome file e selezionare le opzioni, come descritto, il file verrà salvato come file di testo e non verrà utilizzato durante l'installazione della CA.

6.  Chiudere Blocco note.

>   [!IMPORTANT]  
>   Nel CAPolicy.inf, è possibile visualizzare una riga che specifica l'URL http://pki.corp.contoso.com/pki/cps.txt. La sezione criteri interni di CAPolicy.inf è riportata solo come esempio del modo in cui specificare il percorso di un'istruzione di pratica di certificato (CPS). In questa Guida non viene richiesto di creare l'istruzione di pratica di certificazione (CPS).
