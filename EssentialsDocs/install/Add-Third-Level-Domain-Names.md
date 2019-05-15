---
title: Aggiunta dei nomi di dominio di terzo livello
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e5b4a362-1881-4024-ae4e-cc3b05e50103
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 64bf24e45155fdd981e2061b3de7ebce1c53b36c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833322"
---
# <a name="add-third-level-domain-names"></a>Aggiunta dei nomi di dominio di terzo livello

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

È possibile aggiungere la possibilità per gli utenti di richiedere nomi di dominio di terzo livello nella procedura guidata per l'impostazione dei nomi di dominio. A tale scopo, creare e installare un assembly di codice utilizzato da Gestione dominio nel sistema operativo.  
  
## <a name="create-a-provider-of-third-level-domain-names"></a>Creazione di un provider dei nomi di dominio di terzo livello  
 È possibile rendere disponibili nomi di dominio di terzo livello attraverso la creazione e l'installazione di un assembly di codice che fornisce i nomi di dominio alla procedura guidata. A tale scopo, completare le seguenti attività:  
  
-   [Aggiungere un'implementazione dell'interfaccia IDomainSignupProvider all'assembly](Add-Third-Level-Domain-Names.md#BKMK_DomainSignup)  
  
-   [Aggiungere un'implementazione dell'interfaccia IDomainMaintenanceProvider all'assembly](Add-Third-Level-Domain-Names.md#BKMK_DomainMaintenance)  
  
-   [Firmare l'assembly con una firma Authenticode](Add-Third-Level-Domain-Names.md#BKMK_SignAssembly)  
  
-   [Installare l'assembly nel computer di riferimento](Add-Third-Level-Domain-Names.md#BKMK_InstallAssembly)  
  
-   [Riavviare il servizio di gestione nome dominio di Windows Server](Add-Third-Level-Domain-Names.md#BKMK_RestartService)  
  
###  <a name="BKMK_DomainSignup"></a> Aggiungere un'implementazione dell'interfaccia IDomainSignupProvider all'assembly  
 L'interfaccia IDomainSignupProvider viene utilizzata per aggiungere offerte di domini alla procedura guidata.  
  
##### <a name="to-add-the-idomainsignupprovider-code-to-the-assembly"></a>Per aggiungere il codice IDomainSignupProvider all'assembly  
  
1.  Accedere a Visual Studio 2008 come amministratore facendo clic con il pulsante destro del mouse sul programma nel menu **Start**, quindi selezionando **Esegui come amministratore**.  
  
2.  Fare clic su **File**, quindi su **Nuovo**e infine su **Progetto**.  
  
3.  Nella finestra di dialogo **Nuovo progetto** , fare clic su **Visual C#**, quindi su **Libreria di classi**, infine immettere un nome per la soluzione e fare clic su **OK**.  
  
4.  Rinominare il file Class1.cs. Ad esempio, MyDomainNameProvider.cs.  
  
5.  Aggiungere i riferimenti ai file Wssg.Web.DomainManagerObjectModel.dll, CertManaged.dll, WssgCertMgmt.dll e WssgCommon.dll.  
  
6.  Aggiungere i seguenti elementi utilizzando le istruzioni.  
  
    ```c#  
  
    using System.Collections.ObjectModel;  
    using System.Net;  
    using System.Net.Sockets;  
    using Microsoft.WindowsServerSolutions.Certificates;  
    using Microsoft.WindowsServerSolutions.CertificateManagement;  
    using Microsoft.WindowsServerSolutions.Common;  
    using Microsoft.Win32;  
    ```  
  
7.  Modificare lo spazio dei nomi e l'intestazione della classe in modo che corrispondano a quelli del seguente esempio.  
  
    ```c#  
    namespace Microsoft.WindowsServerSolutions.RemoteAccess.Domains  
    {  
        public class MyDomainNameProvider : IDomainSignupProvider  
        {  
        }  
    }  
    ```  
  
8.  Aggiungere il metodo Initialize e le variabili richieste alla classe per definire le offerte presentate nella procedura guidata.  
  
    > [!NOTE]
    >  Il metodo Initialize definisce per il provider del dominio un identificatore che deve essere univoco. Solitamente, a tale scopo occorre specificare un GUID come identificatore. Per altre informazioni sulla creazione di un GUID, vedere [Crea GUID (guidgen.exe)](https://go.microsoft.com/fwlink/?LinkId=116098).  
  
     Il seguente esempio di codice mostra il metodo Initialize.  
  
    ```c#  
  
    static readonly Guid MyID = new Guid("8C999DF5-696A-47af-822D-94F1673D3397");  
    public Guid ID { get { return MyID; } }  
    public string Name { get { return "My Provider"; } }  
    List<Offering> offerings = new List<Offering>();  
  
    public void Initialize(DomainProviderSettings config)  
    {  
       var offer1 = new Offering()  
       {  
          Description = "My Domain Provider",  
          Name = "Offering 1",  
          ProviderID = ID,  
          MoreInfoUrl = new Uri("http://www.contoso.com"),  
          MembershipServiceName = "My Membership",  
          EulaUrl = new Uri("http://www.contoso.com"),  
       };  
  
       this.offerings.Add(offer1);  
       RegistryKey key =   
          Registry.LocalMachine.CreateSubKey(@"Software\Microsoft\Windows Server\Domain Manager\Settings");  
       key.Close();  
    }  
    ```  
  
9. Aggiungere il metodo GetOfferings, che restituisce l'elenco di offerte inizializzato al passaggio precedente. Il seguente esempio di codice mostra il metodo GetOfferings.  
  
    ```c#  
  
    public ReadOnlyCollection<Offering> GetOfferings()  
    {  
       return this.offerings.AsReadOnly();  
    }  
    ```  
  
10. Aggiungere il metodo FindOfferingForDomain, che restituisce l'offerta dall'elenco. Il seguente esempio di codice mostra il metodo FindOfferingForDomain.  
  
    ```  
  
    public Offering FindOfferingForDomain(string domain)  
    {  
       // Return the offering that has the domain name.  
       return offerings[0];  
    }  
  
    ```  
  
11. Aggiungere il metodo SetCredentials, che definisce le credenziali richieste per accedere alle offerte. Il seguente esempio di codice mostra il metodo SetCredentials.  
  
    ```c#  
  
    private string currentUser { get; set; }  
    private string currentPassword { get; set; }  
  
    public bool SetCredentials(DomainNameRequest request,   
       DomainProviderCredentials credentials, bool validate)  
    {  
       currentUser = credentials.UserName;  
       currentPassword = credentials.Password;  
       if (validate)  
       {  
          return ValidateCredentials();  
       }  
  
       return true;  
    }  
    ```  
  
12. Aggiungere il metodo ValidateCredentials, che convalida le credenziali definite da SetCredentials. Il seguente esempio di codice mostra il metodo ValidateCredentials.  
  
    ```c#  
  
    public static readonly string offerUser = "user1@contoso.com";  
    public static readonly string offerPassword = "password1";  
  
    public bool ValidateCredentials()  
    {  
       if (IsUser())  
          return string.Equals(currentPassword, offerPassword);  
       else  
          return false;  
    }   
  
    private bool IsUser()  
    {  
       return string.Equals(currentUser, offerUser, StringComparison.OrdinalIgnoreCase);  
    }  
    ```  
  
13. Aggiungere il metodo GetAvailableDomainRoots, che restituisce l'elenco dei nomi di dominio radice supportati dall'offerta specificata nella richiesta. Questo elenco dei nomi di dominio radice non deve essere vuoto. Il seguente codice di esempio mostra il metodo GetAvailableDomainRoots.  
  
    ```c#  
  
    public ReadOnlyCollection<string> GetAvailableDomainRoots(DomainNameRequest request)  
    {  
       List<string> list = new List<string>();  
       list.Add("domain1.com");  
       list.Add("domain1.org");  
  
       return list.AsReadOnly();  
    }  
    ```  
  
14. Aggiungere il metodo GetUserDomainNames, che restituisce un elenco dei nomi di dominio già di proprietà dell'utente corrente, in relazione all'offerta corrente. Questo elenco può essere vuoto. Il seguente codice di esempio mostra il metodo GetUserDomainNames.  
  
    ```c#  
  
    public static readonly string AvailableDomain1 = "available.domain1.com",  
       AvailableDomain2 = "available.domain2.com";  
    public static readonly string OccupiedDomain1 = "occupied.domain1.com",  
       OccupiedDomain2 = "occupied.domain2.com";  
  
    public ReadOnlyCollection<string> GetUserDomainNames(DomainNameRequest request)  
    {  
       var userDomains = new List<string>();  
       userDomains.Add(OccupiedDomain1);  
       userDomains.Add(AvailableDomain1);  
  
       return userDomains.AsReadOnly();  
    }  
    ```  
  
15. Aggiungere il metodo GetUserDomainQuota, che restituisce il numero massimo di domini consentito dall'offerta specificata. Se non è applicabile un massimo,il metodo restituisce 0. Il seguente codice di esempio mostra il metodo GetUserDomainQuota.  
  
    ```c#  
  
    public int GetUserDomainQuota(DomainNameRequest request)  
    {  
       return 0;  
    }  
    ```  
  
16. Aggiungere il metodo CheckDomainAvailability, che verifica la disponibilità del nome di dominio richiesto e può restituire un elenco di suggerimenti. Il seguente codice di esempio mostra il metodo CheckDomainAvailability.  
  
    ```c#  
  
    public bool CheckDomainAvailability(DomainNameRequest request,   
       out ReadOnlyCollection<string> suggestions)  
    {  
       suggestions = null;  
  
       return true;  
    }  
    ```  
  
17. Aggiungere il metodo CommitDomain, che conferma il nome di dominio richiesto. Il corretto completamento di questo metodo implica l'associazione del nome di dominio all'account utente, la chiamata immediata al provider dei servizi di manutenzione per recuperare il certificato se lo stato è completamente operativo, nonché l'attivazione del provider e dell'offerta. Il seguente codice di esempio mostra il metodo CommitDomain.  
  
    ```c#  
  
    public DomainStatus CommitDomain(DomainNameRequest request)  
    {              
       ReadOnlyCollection<string> suggestions;  
       if (!CheckDomainAvailability(request, out suggestions))  
       {  
          throw new DomainException(FailureReason.InvalidDomainName, null, null);  
       }  
  
       return DomainStatus.Ready;  
    }  
    ```  
  
18. Aggiungere il metodo ReleaseDomain, che notifica al provider l'intenzione da parte dell'utente di rilasciare il nome di dominio. Il seguente codice di esempio mostra il metodo ReleaseDomain.  
  
    ```c#  
  
    public bool ReleaseDomain(DomainNameRequest request)  
    {  
       return true;  
    }  
    ```  
  
19. Aggiungere il metodo GetProviderLandingUrl, che restituisce l'URL per la pagina di destinazione nel flusso di lavoro di accesso al dominio. Il seguente codice di esempio mostra il metodo GetProviderLandingUrl.  
  
    ```c#  
  
    public Url GetProviderLandingUrl(DomainNameRequest request)  
    {  
       return new Url("www.contoso.com");  
    }  
    ```  
  
20. Aggiungere il metodo GetDomainMaintenanceProvider, che restituisce un'istanza di IDomainMaintenanceProvider utilizzata per le attività di manutenzione del dominio. Questo metodo viene richiamato dopo il corretto completamento del metodo CommitDomain e all'avvio di Gestione dominio. Il seguente codice di esempio mostra il metodo GetDomainMaintenanceProvider.  
  
    ```c#  
  
    public IDomainMaintenanceProvider GetDomainMaintenanceProvider()  
    {  
       return new MyDomainMaintenanceProvider();  
    }  
    ```  
  
21. Salvare il progetto senza chiuderlo, poiché la procedura successiva prevede un'aggiunta ad esso. Fino a che la procedura successiva non sarà stata completata, non sarà possibile generare il progetto.  
  
###  <a name="BKMK_DomainMaintenance"></a> Aggiungere un'implementazione dell'interfaccia IDomainMaintenanceProvider all'assembly  
 IDomainMaintenanceProvider viene utilizzato per gestire il dominio dopo averlo creato.  
  
##### <a name="to-add-the-idomainmaintenanceprovider-code-to-the-assembly"></a>Per aggiungere il codice IDomainMaintenanceProvider all'assembly  
  
1.  Aggiungere l'intestazione della classe per il provider dei servizi di manutenzione del dominio. Verificare che il nome definito dall'utente per il provider corrisponda al nome nel metodo GetDomainMaintenanceProvider precedentemente definito.  
  
    ```c#  
  
    public class MyDomainMaintenanceProvider : IDomainMaintenanceProvider  
    {  
    }  
    ```  
  
2.  Aggiungere il metodo Activate, che imposta il provider attivo. Il seguente codice di esempio mostra il metodo Activate.  
  
    ```c#  
  
    string DomainName { get; set; }  
    protected DomainProviderSettings Settings { get; set; }  
  
    public void Activate(DomainProviderSettings settings,   
       DomainNameConfiguration config, DomainProviderCredentials credentials)  
    {  
       Settings = settings;  
       SetCredentials(credentials);  
       DomainName = config.AutoConfiguredAnywhereAccessFullName.Punycode;  
    }  
    ```  
  
3.  Aggiungere il metodo Deactivate, che consente di disattivare tutte le azioni. Il seguente codice di esempio mostra il metodo Deactivate.  
  
    ```  
  
    public void Deactivate()  
    {  
       //Deactivate all actions  
    }  
  
    ```  
  
4.  Aggiungere il metodo SetCredentials, che aggiorna le credenziali dell'utente. Ad esempio, questo metodo può essere richiamato per aggiornare credenziali non più valide. Il seguente esempio di codice mostra il metodo SetCredentials.  
  
    ```c#  
  
    protected DomainProviderCredentials Credentials { get; set; }  
  
    public bool SetCredentials(DomainProviderCredentials credentials)  
    {  
       this.Credentials = credentials;  
  
       return true;  
    }  
    ```  
  
5.  Aggiungere il metodo ValidateCredentials, che convalida le credenziali specificate. Il seguente esempio di codice mostra il metodo ValidateCredentials.  
  
    ```c#  
  
    public static readonly string offerUser = "user1@contoso.com";  
    public static readonly string offerPassword = "password1";  
  
    public bool ValidateCredentials()  
    {  
       if (string.Equals(this.Credentials.UserName,   
          offerUser,   
          StringComparison.OrdinalIgnoreCase) &&   
             string.Equals(this.Credentials.Password, offerPassword))  
          return true;  
  
       return false;  
    }  
    ```  
  
6.  Aggiungere il metodo GetPublicAddress, che restituisce l'indirizzo IP esterno del server. Il seguente codice di esempio mostra il metodo GetPublicAddress.  
  
    ```c#  
  
    public IPAddress GetPublicIPAddress()  
    {  
       string PublicIP = "0.0.0.0";  
       using (RegistryKey key = ProductInfo.RegKey.OpenSubKey("Domain Manager\\Settings", true))  
       {  
          PublicIP = (key == null) ? "0.0.0.0" : key.GetValue("PublicIP", "0.0.0.0").ToString();  
       }  
       IPAddress ip = IPAddress.Parse(PublicIP);  
  
       if (PublicIP == "0.0.0.0")  
       {  
          string strHostName = Dns.GetHostName();  
          IPHostEntry ipEntry = Dns.GetHostEntry(strHostName);  
  
          IPAddress[] addr = ipEntry.AddressList;  
          foreach (IPAddress add in addr)  
          {  
             if (add.AddressFamily == AddressFamily.InterNetwork)  
             {  
                return add;  
             }  
          }  
       }  
       else  
       {  
          return IPAddress.Parse(PublicIP);  
       }  
  
       return null;    
    }  
    ```  
  
7.  Aggiungere il metodo SubmitCertificateRequest, che invia la richiesta del certificato per il nome di dominio attualmente configurato.  
  
    ```c#  
  
    string cert=null;  
  
    public void SubmitCertificateRequest(string certificateRequest)  
    {  
       cert = CertManaged.SubmitRequest(certificateRequest, CertCommon.CAServerFQDN + "\\" +      
          CertCommon.CAName,   
          Microsoft.WindowsServerSolutions.CertificateManagement.CRFlags.Base64Header,   
          CertCommon.CATemplate,   
          EncodingFlags.Base64);  
    }  
    ```  
  
8.  Aggiungere il metodo GetCertificateResponse, che restituisce la risposta di certificato se lo stato del dominio è completamente operativo. Questo metodo viene richiamato sia per le richieste di nuovi certificati che per le richieste di rinnovo dei certificati. Il seguente codice di esempio mostra il metodo GetCertificateResponse.  
  
    ```c#  
  
    public string GetCertificateResponse(bool renew)  
    {  
       return cert;  
    }  
    ```  
  
9. Aggiungere il metodo SubmitRenewCertificateRequest, che elabora il rinnovo del certificato. Il seguente codice di esempio mostra il metodo SubmitRenewCertificateRequest.  
  
    ```c#  
  
    public void SubmitRenewCertificateRequest()  
    {  
       // Add certificate renewal code   
    }  
    ```  
  
10. Aggiungere il metodo UpdateDNSRecords, che aggiorna i record DNS archiviati dal provider. Il seguente codice di esempio mostra il metodo UpdateDNS.  
  
    ```c#  
  
    public bool UpdateDnsRecords(IList<DnsRecord> records)  
    {  
       string UpdateDNS = "true";  
       using (RegistryKey key = ProductInfo.RegKey.OpenSubKey("Domain Manager\\Settings", true))  
       {  
          UpdateDNS = (key == null) ? "true" : key.GetValue("UpdateDNS", "true").ToString();  
       }  
  
       return UpdateDNS == "true";  
    }  
  
    ```  
  
11. Aggiungere il metodo TestConnection, che tenta di stabilire una connessione al servizio back-end. Se questo metodo richiede l'autenticazione, occorre generare un'eccezione appropriata. Il seguente codice di esempio mostra il metodo TestConnection.  
  
    ```c#  
  
    public bool TestConnection()  
    {  
       // Add code to test connection  
  
       return true;  
    }  
    ```  
  
12. Aggiungere il metodo GetDomainState, che restituisce lo stato corrente del dominio. Il seguente codice di esempio mostra il metodo GetDomainState.  
  
    ```c#  
  
    public DomainState GetDomainState()  
    {  
       string domainstatus = "FullyOperational";  
       long expirationDate = 365;  
       using (RegistryKey key = ProductInfo.RegKey.OpenSubKey("Domain Manager\\Settings", true))  
       {  
          domainstatus = (key == null) ? "Ready" : key.GetValue("DomainStatus", "Ready").ToString();  
          expirationDate = Int64.Parse(key.GetValue("ExpirationDate", "365").ToString());  
       }  
  
       switch (domainstatus)  
       {  
          case "Failed":  
             return new DomainState(DomainStatus.Failed,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          case "Ready":  
             return new DomainState(DomainStatus.Ready,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          case "InRenewal":  
             return new DomainState(DomainStatus.InRenewal,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          case "InRenewalCustomerInterventionRequired":  
             return new DomainState(DomainStatus.InRenewalCustomerInterventionRequired,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          case "Pending":  
             return new DomainState(DomainStatus.Pending,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          case "PendingCustomerInterventionRequired":  
             return new DomainState(DomainStatus.PendingCustomerInterventionRequired,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          case "RenewalFailed":  
             return new DomainState(DomainStatus.RenewalFailed,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          default:  
             return new DomainState(DomainStatus.Unknown,   
                null,   
                DateTime.Now.AddDays(expirationDate));                   
          }  
    }  
    ```  
  
13. Aggiungere il metodo GetCertificateState, che restituisce lo stato corrente del certificato. Il seguente codice di esempio mostra il metodo GetCertificateState.  
  
    ```c#  
  
    public CertificateState GetCertificateState(bool renew)  
    {  
       return new CertificateState(CertificateStatus.Ready, string.Empty);  
    }  
    ```  
  
14. Salvare e generare la soluzione.  
  
###  <a name="BKMK_SignAssembly"></a> Firmare l'assembly con una firma Authenticode  
 Per poter utilizzare l'assembly nel sistema operativo, è necessario applicarvi una firma Authenticode. Per altre informazioni sulla firma dell'assembly, vedere [Firma e verifica del codice con Authenticode](https://msdn.microsoft.com/library/ms537364\(VS.85\).aspx#SignCode).  
  
###  <a name="BKMK_InstallAssembly"></a> Installare l'assembly nel computer di riferimento  
 Collocare l'assembly in una cartella nel computer di riferimento. Annotare il percorso della cartella, dal momento che dovrà essere immesso nel Registro di sistema al passaggio successivo.  
  
### <a name="add-a-key-to-the-registry"></a>Aggiunta di una chiave al Registro di sistema  
 L'aggiunta di una voce al Registro di sistema consente di definire le caratteristiche e la posizione dell'assembly.  
  
##### <a name="to-add-a-key-to-the-registry"></a>Per aggiungere una chiave al Registro di sistema  
  
1.  Nel computer di riferimento fare clic su **Start**, immettere **regedit**e quindi premere **INVIO**.  
  
2.  Nel riquadro sinistro, espandere **HKEY_LOCAL_MACHINE**, **SOFTWARE**, **Microsoft**, quindi **Windows Server**, **Gestione dominio** e infine **Provider**.  
  
3.  Fare clic con il pulsante destro del mouse su **Provider**, scegliere **Nuovo**, quindi fare clic su **Chiave**.  
  
4.  Digitare l'identificatore per il provider come nome della chiave. L'identificatore è il GUID definito per il provider al passaggio 8 della procedura [Aggiunta di un'implementazione dell'interfaccia IDomainSignupProvider all'assembly](Add-Third-Level-Domain-Names.md#BKMK_DomainSignup).  
  
5.  Fare clic con il pulsante destro del mouse sulla chiave appena creata, quindi fare clic su **Valore stringa**.  
  
6.  Digitare **Assembly** per il nome della stringa, quindi premere **Invio**.  
  
7.  Fare clic con il pulsante destro del mouse sulla nuova stringa **Assembly** nel riquadro destro, quindi fare clic su **Modifica**.  
  
8.  Digitare il percorso completo del file di assembly precedentemente creato, quindi fare clic su **OK**.  
  
9. Fare di nuovo clic con il pulsante destro del mouse sulla chiave, quindi fare clic su **Valore stringa**.  
  
10. Digitare **Abilitata** per il nome della stringa, quindi premere **Invio**.  
  
11. Fare clic con il pulsante destro del mouse sulla nuova stringa **Enabled** nel riquadro destro, quindi fare clic su **Modifica**.  
  
12. Digitare **True**e quindi fare clic su **OK**.  
  
13. Fare di nuovo clic con il pulsante destro del mouse sulla chiave, quindi fare clic su **Valore stringa**.  
  
14. Digitare **Tipo** per il nome della stringa, quindi premere **Invio**.  
  
15. Fare clic con il pulsante destro del mouse sulla nuova stringa **Type** nel riquadro destro, quindi fare clic su **Modifica**.  
  
16. Digitare il nome completo della classe del provider definito nell'assembly, quindi fare clic su **OK**.  
  
###  <a name="BKMK_RestartService"></a> Riavviare il servizio di gestione nome dominio di Windows Server  
 È necessario riavviare il servizio di gestione dei domini di Windows Server affinché il provider diventi disponibile per il sistema operativo.  
  
##### <a name="restart-the-service"></a>Riavvio del servizio  
  
1.  Fare clic su **Start**, digitare **mmc**e quindi premere **INVIO**.  
  
2.  Se lo snap-in Servizi non è elencato nella console, aggiungerlo completando i seguenti passaggi:  
  
    1.  Fare clic su **Aggiungi/Rimuovi snap-in** nel menu **File**.  
  
    2.  Nell'elenco **Snap-in disponibili** , fare clic su **Servizi**, quindi su **Aggiungi**.  
  
    3.  Nella finestra di dialogo **Servizi**, verificare che il **computer locale** sia selezionato, quindi fare clic su **Fine**.  
  
    4.  Fare clic su **OK** per chiudere la finestra di dialogo **Aggiungi/Rimuovi snap-in**.  
  
3.  Fare doppio clic su **Servizi**, scorrere verso il basso, quindi selezionare **Windows Server Domain Management**, infine fare clic su **Riavvia il servizio**.  
  
## <a name="see-also"></a>Vedere anche  
 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Personalizzazioni aggiuntive](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Testare l'esperienza dei clienti](Testing-the-Customer-Experience.md)