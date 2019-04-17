---
ms.assetid: 16a344a9-f9a6-4ae2-9bea-c79a0075fd04
title: Attestazione chiave TPM
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9d08efe825e10d526763a1654c7e090427719c51
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="tpm-key-attestation"></a>Attestazione chiave TPM

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

**Autore**: Justin Turner, Senior Support Escalation Engineer con il gruppo di Windows  
  
> [!NOTE]  
> Questo contenuto è stato redatto da un ingegnere del supporto tecnico Microsoft ed è destinato ad amministratori esperti e architetti di sistemi che desiderano per una spiegazione tecnica più approfondita delle funzionalità e le soluzioni in Windows Server 2012 R2 che solitamente disponibili su TechNet. Tuttavia, non è stata sottoposta stessi passaggi redazionali, in modo che il linguaggio potrebbe sembrare che meno accurato della documentazione che cosa si trova in genere su TechNet.  
  
## <a name="overview"></a>Panoramica  
Mentre il supporto per le chiavi protette da TPM esistente dal Windows 8, vi sono alcun meccanismo per le CA a livello di crittografia attestare che la chiave privata del certificato richiedente è effettivamente protetto da un modulo TPM (Trusted Platform). Questo aggiornamento consente a un'autorità di certificazione per eseguire tale attestazione e per riflettere tale attestazione nel certificato emesso.  
  
> [!NOTE]  
> Questo articolo si presuppone che il lettore abbia familiarità con il concetto di modello di certificato (per riferimento, vedere [modelli di certificato](https://technet.microsoft.com/library/cc730705.aspx)). Si presuppone inoltre che il lettore abbia familiarità con le procedure configurare le CA dell'organizzazione per rilasciare certificati basati su modelli di certificato (per riferimento, vedere [elenco di controllo: configurare le CA di rilascio e gestione certificati](https://technet.microsoft.com/library/cc771533.aspx)).  
  
### <a name="terminology"></a>Terminologia  
  
|Termine|Definizione|  
|--------|--------------|  
|VERIFICA DELL'AUTENTICITÀ|Chiave di verifica dell'autenticità. Si tratta di una chiave asimmetrica contenuta all'interno del TPM (introdotto in fase di produzione). Verifica dell'autenticità è univoco per ogni TPM e possibile identificarlo. Verifica dell'autenticità non può essere modificata o rimossa.|  
|EKpub|Si riferisce a chiave pubblica della verifica dell'autenticità.|  
|EKPriv|Fa riferimento alla chiave privata della verifica dell'autenticità.|  
|EKCert|Certificato di verifica dell'autenticità. Un TPM produttore certificato rilasciato per EKPub. TPM non tutti sono EKCert.|  
|TPM|Trusted Platform Module. Un TPM è progettato per fornire funzioni correlate alla sicurezza basata su hardware. Un chip TPM è un cryptoprocessor sicuro progettato per eseguire le operazioni di crittografia. Il chip include diversi meccanismi di sicurezza fisica per renderlo di manomissione e software dannoso è in grado di alterare le funzioni di protezione del TPM.|  
  
### <a name="background"></a>Sfondo  
A partire da Windows 8, un modulo TPM (Trusted Platform) può essere utilizzato per proteggere la chiave privata di un certificato. Il Microsoft Platform Crypto Provider di archiviazione Provider chiavi (KSP) abilita questa funzionalità. Sono presenti due problemi con l'implementazione:  

-   Vi è alcuna garanzia che una chiave è effettivamente protetto da un TPM (un utente può facilmente lo spoofing KSP di software come un KSP TPM con credenziali di amministratore locale).

-   Non è stato possibile limitare l'elenco dei moduli TPM che sono autorizzati a proteggere l'azienda ha rilasciato i certificati (nel caso in cui l'amministratore dell'infrastruttura PKI desidera controllare i tipi di dispositivi che possono essere utilizzati per ottenere i certificati nell'ambiente).  

### <a name="tpm-key-attestation"></a>Attestazione chiave TPM  
Attestazione chiave TPM è la possibilità di entità che richiede un certificato di crittografia dimostrare un'autorità di certificazione che la chiave RSA nella richiesta di certificato è protetta da "a" o "the" TPM che considera attendibile l'autorità di certificazione. Il modello di trust TPM verrà discusso più il [Cenni preliminari sulla distribuzione](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_DeploymentOverview) più avanti in questo argomento.  
  
### <a name="why-is-tpm-key-attestation-important"></a>Perché è importante attestazione chiave TPM?  
Un certificato utente con una chiave attestata TPM garantisce maggiore sicurezza, backup esportabilità, anti-hammering, isolamento e delle chiavi fornite dal TPM.  
  
Con attestazione chiave TPM, un nuovo paradigma di gestione è ora possibile: un amministratore può definire il set di dispositivi che gli utenti possono usare per accedere alle risorse aziendali (ad esempio, VPN o punto di accesso wireless) e **sicuro** garantisce altri dispositivi non possono essere utilizzati per accedervi. Questo nuovo paradigma di controllo di accesso è **sicuro** perché è associato a un *associate all'hardware* identità utente, che è più sicuro di una credenziale basata su software.
  
### <a name="how-does-tpm-key-attestation-work"></a>Come funziona l'attestazione chiave TPM?  
In generale, attestazione chiave TPM si basa sulle aree seguenti:  
  
1.  Ogni TPM viene fornito con una chiave asimmetrica univoca, denominata la *chiave di verifica dell'autenticità* autenticità, copiato dal produttore. Si fa riferimento alla parte pubblica della chiave come *EKPub* e la chiave privata associata come *EKPriv*. Alcuni chip TPM dispone anche di un certificato di verifica dell'autenticità rilasciato dal produttore per il EKPub. Si fa riferimento a questo certificato come *EKCert*.  
  
2.  Un'autorità di certificazione stabilisce relazioni di trust del TPM mediante EKPub o EKCert.  
  
3.  Un utente dimostra all'autorità di certificazione che la chiave RSA per cui viene richiesto il certificato di crittografia è correlata al ekpub e che l'utente proprietario di EKpriv.  
  
4.  L'autorità di certificazione emette un certificato con un criterio di rilascio speciale OID per indicare che la chiave viene attestata ora possono essere protetti da un TPM.  
  
## <a name="BKMK_DeploymentOverview"></a>Cenni preliminari sulla distribuzione  
In questa distribuzione, si presuppone che una CA dell'organizzazione di Windows Server 2012 R2 viene impostata. Inoltre, i client (Windows 8.1) sono configurati per registrare in tale CA dell'organizzazione utilizzando i modelli di certificato. 

Esistono tre passaggi per la distribuzione di attestazione chiave TPM:  
  
1.  **Pianificare il modello di trust TPM:** il primo passaggio consiste nel decidere quale modello di protezione TPM da utilizzare. Esistono 3 modi per eseguire questa operazione:  
  
    -   **Attendibilità in base alle credenziali dell'utente:** la CA dell'organizzazione considera attendibile il EKPub fornito dall'utente come parte della richiesta di certificato e viene eseguita alcuna convalida diverso da credenziali di dominio dell'utente.  
  
    -   **Attendibilità in base a EKCert:** la CA dell'organizzazione convalida la catena di EKCert che viene fornita come parte della richiesta di certificato con un elenco gestite dall'amministratore di *accettabile catene di certificati di verifica dell'autenticità*. Le catene accettabili sono definite per produttore e sono espressi tramite due archivi di certificati personalizzati nella CA emittente (un solo store per l'intermedio) e uno per i certificati CA radice. Questa modalità di attendibilità significa che **tutti** TPM di un determinato produttore sono attendibili. Si noti che in questa modalità, TPM in uso nell'ambiente deve contenere EKCerts.
  
    -   **Attendibilità in base a EKPub:** convalida di CA dell'organizzazione che di EKPub fornito come parte della richiesta di certificato viene visualizzato un elenco gestite dall'amministratore di consentito EKPubs. Questo elenco viene espresso in una directory di file in cui il nome di ogni file in questa directory è l'hash SHA-2 ekpub consentiti. Questa opzione offre il massimo livello di garanzia, ma richiede ulteriori interventi amministrativi, perché ogni dispositivo viene identificato singolarmente. In questo modello di trust, sono consentiti solo i dispositivi che sono stati EKPub del loro TPM aggiunto all'elenco consentito di EKPubs per registrare un certificato attestata TPM.  
  
    A seconda di quale metodo viene utilizzato, l'autorità di certificazione verrà applicare un criterio di rilascio diversi OID per il certificato emesso. Per ulteriori informazioni sui criteri di rilascio OID, vedere la tabella OID dei criteri di rilascio nel [configurare un modello di certificato](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_ConfigCertTemplate) in questo argomento.  
  
    Si noti che è possibile scegliere una combinazione di modelli di protezione TPM. In questo caso, l'autorità di certificazione accetterà qualsiasi dei metodi attestazione e il criterio di rilascio che OID rifletterà tutti i metodi di attestazione di esito positivo.  
  
2.  **Configurare il modello di certificato:** la configurazione del modello di certificato è descritto nel [dettagli di distribuzione](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_DeploymentDetails) in questo argomento. Questo articolo illustra la modalità di assegnazione di questo modello di certificato per la CA dell'organizzazione o come registrare non ha accesso a un gruppo di utenti. Per ulteriori informazioni, vedere [elenco di controllo: configurare le CA di rilascio e gestione certificati](https://technet.microsoft.com/library/cc771533.aspx).  
  
3.  **Configurare la CA per il modello di trust TPM**  
  
    1.  **Attendibilità in base alle credenziali dell'utente:** è richiesta alcuna configurazione specifica.  
  
    2.  **Attendibilità in base a EKCert:** l'amministratore deve ottenere i certificati della catena EKCert dai produttori TPM e importarle in due nuovi archivi certificati, creati dall'amministratore, la CA che eseguono attestazione chiave TPM. Per ulteriori informazioni, vedere il [configurazione CA](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig) in questo argomento.  
  
    3.  **Attendibilità in base a EKPub:** l'amministratore deve ottenere EKPub per ogni dispositivo che richiedono certificati attestata TPM e aggiungerli all'elenco di EKPubs consentiti. Per ulteriori informazioni, vedere il [configurazione CA](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig) in questo argomento.  
  
    > [!NOTE]  
    > -   Questa funzionalità richiede Windows 8.1 / Windows Server 2012 R2.  
    > -   Attestazione chiave TPM per KSP di terze parti smart card non è supportato. È necessario utilizzare Microsoft piattaforma Crypto Provider KSP.  
    > -   Attestazione chiave TPM funziona solo per le chiavi RSA.  
    > -   Attestazione chiave TPM non è supportato per una CA autonoma.  
    > -   Attestazione chiave TPM non supporta [l'elaborazione di certificati non permanenti](https://technet.microsoft.com/library/ff934598).  
  
## <a name="BKMK_DeploymentDetails"></a>Dettagli di distribuzione  
  
### <a name="BKMK_ConfigCertTemplate"></a>Configurare un modello di certificato  
Per configurare il modello di certificato per l'attestazione chiave TPM, eseguire i passaggi di configurazione seguenti:  
  
1.  **Compatibilità** scheda  
  
    Nel **le impostazioni di compatibilità** sezione:  
  
    -   Verificare **Windows Server 2012 R2** è selezionata per il **autorità di certificazione**.  
  
    -   Verificare **Windows 8.1 / Windows Server 2012 R2** è selezionata per il **destinatario certificato**.  
  
    ![Attestazione chiave TPM](media/TPM-Key-Attestation/GTR_ADDS_CompatibilityTab.gif)  
  
2.  **Crittografia** scheda  
  
    Verificare **Provider di archiviazione chiavi** è selezionata per il **categoria Provider** e **RSA** è selezionata per il **nome dell'algoritmo**. Verificare **le richieste devono utilizzare uno dei seguenti provider** sia selezionata e **Microsoft piattaforma Crypto Provider** opzione selezionata in **provider**.  
  
    ![Attestazione chiave TPM](media/TPM-Key-Attestation/GTR_ADDS_CryptoTab.gif)  
  
3.  **Attestazione chiave** scheda  
  
    Si tratta di una nuova scheda per Windows Server 2012 R2:  
  
    ![Attestazione chiave TPM](media/TPM-Key-Attestation/GTR_ADDS_ConfigCertTemplate.gif)  
  
    Scegliere una modalità di attestazione da tre possibili opzioni.  
  
    ![Attestazione chiave TPM](media/TPM-Key-Attestation/GTR_ADDS_KeyModes.gif)  
  
    -   **None:** implica che non deve essere utilizzata attestazione chiave  
  
    -   **Obbligatorio, se il client è in grado di supportare:** consente agli utenti in un dispositivo che non supportano il TPM tasto attestazione per continuare la registrazione del certificato. Gli utenti che possono eseguire un'attestazione verranno distinto con un criterio di rilascio speciale OID. Alcuni dispositivi potrebbero non essere in grado di eseguire attestazione a causa di un TPM precedente che non supporta l'attestazione chiave o il dispositivo non hanno affatto un TPM.
  
    -   **È necessario:** Client *deve* eseguire attestazione chiave TPM, in caso contrario la richiesta di certificato avrà esito negativo.  
  
    Quindi scegli il modello di trust TPM. Nuovo sono disponibili tre opzioni:  
  
    ![Attestazione chiave TPM](media/TPM-Key-Attestation/GTR_ADDS_KeyTypeToEnforce.gif)  
  
    -   **Le credenziali dell'utente:** consentire a un utente esegue l'autenticazione garantire un TPM valido specificando le credenziali di dominio.  
  
    -   **Certificato di verifica dell'autenticità:** il EKCert del dispositivo deve convalidare mediante certificati CA intermedi a TPM gestite dall'amministratore per un certificato CA radice gestite dall'amministratore. Se si sceglie questa opzione, è necessario impostare EKCA ed EKRoot archivi nella CA emittente del certificato come descritto nel [configurazione CA](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig) in questo argomento.  
  
    -   **Chiave di verifica dell'autenticità:** EKPub del dispositivo deve essere visualizzato nell'elenco PKI gestite dall'amministratore. Questa opzione offre il massimo livello di garanzia, ma richiede uno sforzo amministrativo. Se si sceglie questa opzione, è necessario impostare un elenco di EKPub nella CA emittente come descritto nel [configurazione CA](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig) in questo argomento.  
  
    Infine, decidere quali criteri di rilascio per visualizzare il certificato emesso. Per impostazione predefinita, ogni tipo di applicazione ha un identificatore di oggetto associato (OID) che verrà inserito nel certificato se passa tale tipo di imposizione, come descritto nella tabella seguente. Si noti che è possibile scegliere una combinazione di metodi di imposizione. In questo caso, l'autorità di certificazione accetterà qualsiasi dei metodi attestazione e il criterio di rilascio OID rifletterà tutti i metodi di attestazione che ha avuto esito positivo.  
  
    **OID dei criteri di rilascio**  
  
    |OID|Tipo di attestazione chiave|Descrizione|Livello di garanzia|  
    |-------|------------------------|---------------|-------------------|  
    |1.3.6.1.4.1.311.21.30|VERIFICA DELL'AUTENTICITÀ|"Verifica dell'autenticità Verified": per elenco gestite dall'amministratore di verifica dell'autenticità|Elevata|  
    |1.3.6.1.4.1.311.21.31|Certificato di verifica dell'autenticità|"Verifica dell'autenticità certificato Verified": quando viene convalidata la catena di certificati verifica dell'autenticità|Media|  
    |1.3.6.1.4.1.311.21.32|Credenziali dell'utente|"Verifica dell'autenticità Trusted in uso": per utente attestata verifica dell'autenticità|Basso|  
  
    L'OID verrà inserito nel certificato emesso se **includere criteri di rilascio** è selezionata (configurazione predefinita).  
  
    ![Attestazione chiave TPM](media/TPM-Key-Attestation/GTR_ADDS_IssuancePolicies.gif)  
  
    > [!TIP]  
    > Un uso potenziale di con l'identificatore di oggetto presente nel certificato consiste nel limitare l'accesso a una VPN o wireless per alcuni dispositivi di rete. Ad esempio, il criterio di accesso potrebbe consentire connessione (o l'accesso a una VLAN diversa) se OID 1.3.6.1.4.1.311.21.30 è presente nel certificato. Ciò consente di limitare l'accesso ai dispositivi cui EK TPM è presente nell'elenco EKPUB.  
  
### <a name="BKMK_CAConfig"></a>Configurazione della CA  
  
1.  **Programma di installazione EKCA ed EKROOT archivi certificati in una CA emittente**  
  
    Se si sceglie **certificato di verifica dell'autenticità** per le impostazioni del modello, eseguire i passaggi di configurazione seguenti:  
  
    1.  Utilizzare Windows PowerShell per creare due nuovi archivi certificati nel server di autorità di certificazione che eseguirà l'attestazione chiave TPM.  
  
    2.  Ottenere intermedie e certificati CA da fabbricanti che si desidera consentire nel proprio ambiente enterprise principali. Questi certificati devono essere importati in archivi certificati creati in precedenza (EKCA ed EKROOT) come appropriate.  
  
    Il seguente script Windows PowerShell esegue entrambe queste operazioni. Nell'esempio seguente, il produttore del TPM Fabrikam ha fornito un certificato radice *FabrikamRoot.cer* e un certificato della CA emittente *Fabrikamca.cer*.  
  
    ```powershell  
    PS C:>\cd cert:  
    PS Cert:\>cd .\\LocalMachine  
    PS Cert:\LocalMachine> new-item EKROOT  
    PS Cert:\ LocalMachine> new-item EKCA  
    PS Cert:\EKCA\copy FabrikamCa.cer .\EKCA  
    PS Cert:\EKROOT\copy FabrikamRoot.cer .\EKROOT  
    ```  
  
2.  **Elenco EKPUB di installazione se si usa il tipo di attestazione di verifica dell'autenticità**  
  
    Se si sceglie **chiave di verifica dell'autenticità** nelle impostazioni del modello, i successivi passaggi di configurazione sono per creare e configurare una cartella nella CA emittente, che contiene i file di 0 byte, ognuna denominata per l'hash SHA-2 di una verifica dell'autenticità consentita. In questa cartella viene utilizzato come un "elenco Consenti" dei dispositivi che sono autorizzati a ottenere i certificati di attestata chiave TPM. Poiché è necessario aggiungere manualmente il EKPUB per ogni dispositivo che richiede un certificato attested, fornisce dell'azienda con una garanzia dei dispositivi autorizzati per ottenere i certificati di attestata chiave TPM. La configurazione di un'autorità di certificazione per questa modalità richiede due passaggi:  
  
    1.  **Creare la voce del Registro di sistema EndorsementKeyListDirectories:** utilizzare lo strumento da riga di comando Certutil per configurare i percorsi di cartella in cui sono definiti EKpubs attendibile come descritto nella tabella seguente.  
  
        |Operazione|Sintassi del comando|  
        |-------------|------------------|  
        |Aggiungere percorsi di cartella|certutil.exe - setreg CA\EndorsementKeyListDirectories + "<folder>"|  
        |Rimuovere i percorsi delle cartelle|certutil.exe - setreg CA\EndorsementKeyListDirectories-"<folder>"|  
  
        EndorsementKeyListDirectories nel comando certutil è un'impostazione del Registro di sistema come descritto nella tabella seguente.  
  
        |Nome del valore|Tipo|Dati|  
        |--------------|--------|--------|  
        |EndorsementKeyListDirectories|REG_MULTI_SZ|< percorso locale o UNC EKPUB consentire gli elenchi ><br /><br />Esempio:<br /><br />*\\\blueCA.contoso.com\ekpub*<br /><br />*\\\bluecluster1.contoso.com\ekpub*<br /><br />D:\ekpub|  
  
        HKLM\SYSTEM\CurrentControlSet\Services\CertSvc\Configuration\\<CA Sanitized Name>  
  
        *EndorsementKeyListDirectories* conterrà un elenco di percorsi UNC o locale di file system, ciascuno dei quali punta a una cartella in cui l'autorità di certificazione ha accesso in lettura. Ogni cartella può contenere zero o più consentire le voci dell'elenco, in cui ogni voce è un file con un nome di SHA-2 hash di un EKpub attendibile, con nessuna estensione di file. 
        Creazione o la modifica di questa configurazione della chiave del Registro di sistema richiede un riavvio dell'autorità di certificazione, come impostazioni di configurazione del Registro di sistema CA esistenti. Tuttavia, le modifiche alle impostazioni della configurazione avranno effetto immediato e non saranno necessario riavviare l'autorità di certificazione.  
  
        > [!IMPORTANT]  
        > Proteggere le cartelle nell'elenco da eventuali manomissioni e accessi non autorizzati tramite la configurazione delle autorizzazioni in modo che solo gli amministratori autorizzati lettura e scrittura di accesso. L'account computer della CA richiede accesso in sola lettura.  
  
    2.  **Popolare l'elenco EKPUB:** utilizzare il seguente cmdlet Windows PowerShell per ottenere l'hash della chiave pubblica di EK il TPM utilizzando Windows PowerShell in ogni dispositivo e quindi inviare questa pubblico chiave hash per l'autorità di certificazione e archiviarlo nella cartella EKPubList.  
  
        ```powershell  
        PS C:>\$a=Get-TpmEndorsementKeyInfo -hashalgorithm sha256  
        PS C:>$b=new-item $a.PublickKeyHash -ItemType file  
        ```  
  
## <a name="troubleshooting"></a>Risoluzione dei problemi  
  
### <a name="key-attestation-fields-are-unavailable-on-a-certificate-template"></a>Attestazione chiave campi non sono disponibili in un modello di certificato  
I campi chiave attestazione non sono disponibili se le impostazioni del modello non soddisfano i requisiti per l'attestazione. Cause comuni sono:  
  
1.  Le impostazioni di compatibilità non sono configurate correttamente. Assicurarsi che siano configurate come indicato di seguito:  
  
    1.  **Autorità di certificazione**: **Windows Server 2012 R2**  
  
    2.  **Destinatario del certificato**: **Windows 8.1 / Windows Server 2012 R2**  
  
2.  Le impostazioni di crittografia non sono configurate correttamente. Assicurarsi che siano configurate come indicato di seguito:  
  
    1.  **Categoria provider**: **Provider di archiviazione chiavi**  
  
    2.  **Nome dell'algoritmo**: **RSA**  
  
    3.  **Provider**: **Microsoft piattaforma Crypto Provider**  
  
3.  Le impostazioni di gestione delle richieste non sono configurate correttamente. Assicurarsi che siano configurate come indicato di seguito:  
  
    1.  Il **Rendi la chiave privata esportabile** opzione non deve essere selezionato.  
  
    2.  Il **Archivia chiave privata di crittografia del soggetto** opzione non deve essere selezionato.  
  
### <a name="verification-of-tpm-device-for-attestation"></a>Verifica del dispositivo TPM per l'attestazione  
Utilizzare il cmdlet di Windows PowerShell, **Confirm-CAEndorsementKeyInfo**, per verificare che un determinato dispositivo TPM sia attendibile per l'attestazione dalle autorità di certificazione. Sono disponibili due opzioni: uno per la verifica di EKCert e l'altra per la verifica di un EKPub. Il cmdlet sia eseguito localmente su una CA o in remoto CAs usando la comunicazione remota di Windows PowerShell.  
  
1.  Per la verifica dell'attendibilità in un EKPub, eseguire i due passaggi seguenti:  
  
    1.  **Estrarre il EKPub dal computer client:** EKPub il può essere estratto da un computer client tramite **Get-TpmEndorsementKeyInfo**. Da un prompt dei comandi con privilegi elevati, eseguire le operazioni seguenti:  
  
        ```  
        PS C:>\$a=Get-TpmEndorsementKeyInfo -hashalgorithm sha256  
        ```  
  
    2.  **Verificare i trust in un EKCert in un computer della CA:** copiare la stringa estratta (l'hash SHA-2 di EKPub) al server (ad esempio, tramite posta elettronica) e passarla al cmdlet Confirm-CAEndorsementKeyInfo. Si noti che questo parametro deve essere di 64 caratteri.  
  
        ```  
        Confirm-CAEndorsementKeyInfo [-PublicKeyHash] <string>  
        ```  
  
2.  Per la verifica dell'attendibilità in un EKCert, eseguire i due passaggi seguenti:  
  
    1.  **Estrarre il EKCert dal computer client:** EKCert il può essere estratto da un computer client tramite **Get-TpmEndorsementKeyInfo**. Da un prompt dei comandi con privilegi elevati, eseguire le operazioni seguenti:  
  
        ```  
        PS C:>\$a= Get- TpmEndorsementKeyInfo  
        PS C:>\$a.manufacturerCertificates|Export-Certificate c:\myEkcert.cer  
        ```  
  
    2.  **Verificare i trust in un KCert in un computer della CA:** copiare l'estratto EKCert (EkCert.cer) all'autorità di certificazione (ad esempio, tramite posta elettronica o xcopy). Ad esempio, se si copia il file del certificato nella cartella "c:\diagnose" nel server CA, eseguire il comando seguente per completare la verifica:  
  
        ```  
        PS C:>new-object System.Security.Cryptography.X509Certificates.X509Certificate2 "c:\diagnose\myEKcert.cer" | Confirm-CAEndorsementKeyInfo  
        ```  
  
## <a name="see-also"></a>Vedere anche  
[Panoramica della tecnologia Trusted Platform Module](https://technet.microsoft.com/library/jj131725.aspx)  
[Risorsa esterna: Trusted Platform Module](http://www.cs.unh.edu/~it666/reading_list/Hardware/tpm_fundamentals.pdf)  
  


