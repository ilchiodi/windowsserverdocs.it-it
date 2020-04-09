---
ms.assetid: 99a68050-8d19-4c58-ad86-e08a3dcdb4f7
title: Appendice L-eventi da monitorare
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 07/30/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: e47b1fbe2df16aca9514e8a29c82d56d8dc96cba
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822854"
---
# <a name="appendix-l-events-to-monitor"></a>Appendice L: Eventi da monitorare

>Si applica a: Windows Server

La tabella seguente elenca gli eventi che è necessario monitorare nell'ambiente in uso, in base alle indicazioni fornite nel [monitoraggio Active Directory per i segnali di compromissione](../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md). Nella tabella seguente la colonna "ID evento Windows corrente" elenca l'ID evento in quanto è implementato nelle versioni di Windows e Windows Server attualmente supportate dal supporto mainstream.  
  
La colonna "ID evento legacy Windows" elenca l'ID evento corrispondente nelle versioni legacy di Windows, ad esempio i computer client che eseguono Windows XP o versioni precedenti e i server che eseguono Windows Server 2003 o versioni precedenti. La colonna "criticità potenziale" indica se l'evento deve essere considerato critico basso, medio o alto nel rilevamento degli attacchi e la colonna "Riepilogo evento" fornisce una breve descrizione dell'evento.  
  
Una criticità potenziale di livello elevato indica che deve essere analizzata un'occorrenza dell'evento. Una criticità potenziale di media o bassa indica che questi eventi devono essere esaminati solo se si verificano in modo imprevisto o in numeri che superano significativamente la linea di base prevista in un periodo di tempo misurato. Tutte le organizzazioni devono testare queste raccomandazioni nei propri ambienti prima di creare avvisi che richiedono risposte obbligatorie in seguito a indagini. Ogni ambiente è diverso e alcuni degli eventi classificati con una criticità potenziale di livello elevato possono verificarsi a causa di altri eventi innocui.  
  
|||||  
|-|-|-|-|  
|**ID evento Windows corrente**|**ID evento Windows legacy**|**Criticità potenziale**|**Riepilogo eventi**|  
|4618|N/D|Alta|Si è verificato un modello di eventi di sicurezza monitorato.|  
|4649|N/D|Alta|È stato rilevato un attacco di riproduzione. Può essere un falso positivo non dannoso a causa di un errore di configurazione.|  
|4719|612|Alta|Il criterio di controllo del sistema è stato modificato.|  
|4765|N/D|Alta|La cronologia SID è stata aggiunta a un account.|  
|4766|N/D|Alta|Il tentativo di aggiungere la cronologia SID a un account non è riuscito.|  
|4794|N/D|Alta|È stato effettuato un tentativo di impostare la modalità di ripristino dei servizi directory.|  
|4897|801|Alta|Separazione ruoli abilitata:|  
|4964|N/D|Alta|Sono stati assegnati gruppi speciali a un nuovo account di accesso.|  
|5124|N/D|Alta|Un'impostazione di sicurezza è stata aggiornata nel servizio risponditore OCSP|  
|N/D|550|Da medio a alto|Possibile attacco Denial of Service (DoS)|  
|1102|517|Da medio a alto|Il log di controllo è stato cancellato|  
|4621|N/D|Media|Sistema ripristinato dall'amministratore da CrashOnAuditFail. Agli utenti che non sono amministratori verrà ora consentito l'accesso. Alcune attività controllabili potrebbero non essere state registrate.|  
|4675|N/D|Media|I SID sono stati filtrati.|  
|4692|N/D|Media|Tentativo di backup della chiave master di protezione dati.|  
|4693|N/D|Media|È stato eseguito un tentativo di ripristino della chiave master di protezione dati.|  
|4706|610|Media|Un nuovo trust è stato creato in un dominio.|  
|4713|617|Media|Il criterio Kerberos è stato modificato.|  
|4714|618|Media|Il criterio di recupero dati crittografato è stato modificato.|  
|4715|N/D|Media|Il criterio di controllo (SACL) per un oggetto è stato modificato.|  
|4716|620|Media|Le informazioni sul dominio trusted sono state modificate.|  
|4724|628|Media|È stato effettuato un tentativo di reimpostare la password di un account.|  
|4727|631|Media|È stato creato un gruppo globale abilitato per la sicurezza.|  
|4735|639|Media|Un gruppo locale abilitato per la sicurezza è stato modificato.|  
|4737|641|Media|Un gruppo globale abilitato per la sicurezza è stato modificato.|  
|4739|643|Media|Il criterio di dominio è stato modificato.|  
|4754|658|Media|È stato creato un gruppo universale abilitato per la sicurezza.|  
|4755|659|Media|Un gruppo universale abilitato per la sicurezza è stato modificato.|  
|4764|667|Media|Un gruppo disabilitato per la sicurezza è stato eliminato|  
|4764|668|Media|Il tipo di un gruppo è stato modificato.|  
|4780|684|Media|L'ACL è stato impostato per gli account che sono membri dei gruppi Administrators.|  
|4816|N/D|Media|RPC ha rilevato una violazione di integrità durante la decrittografia di un messaggio in arrivo.|  
|4865|N/D|Media|È stata aggiunta una voce di informazioni foresta attendibile.|  
|4866|N/D|Media|È stata rimossa una voce di informazioni foresta attendibile.|  
|4867|N/D|Media|È stata modificata una voce di informazioni foresta attendibile.|  
|4868|772|Media|Il gestore dei certificati ha negato una richiesta di certificati in sospeso.|  
|4870|774|Media|Servizi certificati ha revocato un certificato.|  
|4882|786|Media|Le autorizzazioni di sicurezza di Servizi certificati sono cambiate.|  
|4885|789|Media|Il filtro di controllo di Servizi certificati è cambiato.|  
|4890|794|Media|Le impostazioni del gestore di certificati per Servizi certificati sono cambiate.|  
|4892|796|Media|Una proprietà di Servizi certificati è cambiata.|  
|4896|800|Media|Una o più righe sono state eliminate dal database dei certificati.|  
|4906|N/D|Media|Il valore di CrashOnAuditFail è stato modificato.|  
|4907|N/D|Media|Le impostazioni di controllo per l'oggetto sono state modificate.|  
|4908|N/D|Media|Tabella di accesso gruppi speciali modificata.|  
|4912|807|Media|Il criterio di controllo per utente è stato modificato.|  
|4960|N/D|Media|IPsec ha eliminato un pacchetto in ingresso che non ha superato un controllo di integrità. Se il problema persiste, è possibile che si verifichi un problema di rete o che i pacchetti vengano modificati in transito in questo computer. Verificare che i pacchetti inviati dal computer remoto siano identici a quelli ricevuti dal computer. Questo errore potrebbe inoltre indicare problemi di interoperabilità con altre implementazioni di IPsec.|  
|4961|N/D|Media|IPsec ha eliminato un pacchetto in ingresso che non ha superato un controllo di riproduzione. Se il problema persiste, potrebbe indicare un attacco di riproduzione contro questo computer.|  
|4962|N/D|Media|IPsec ha eliminato un pacchetto in ingresso che non ha superato un controllo di riproduzione. Il pacchetto in ingresso aveva un numero di sequenza troppo basso per assicurarsi che non si trattasse di una riproduzione.|  
|4963|N/D|Media|IPsec ha eliminato un pacchetto di testo non crittografato in ingresso che dovrebbe essere stato protetto. Questa operazione è in genere dovuta al computer remoto che modifica i criteri IPsec senza informare il computer. Potrebbe anche trattarsi di un tentativo di attacco di spoofing.|  
|4965|N/D|Media|IPsec ha ricevuto un pacchetto da un computer remoto con un indice del parametro di sicurezza (SPI) non corretto. Questo problema si verifica in genere quando si verifica un malfunzionamento dell'hardware che danneggia i pacchetti. Se questi errori permangono, verificare che i pacchetti inviati dal computer remoto corrispondano a quelli ricevuti dal computer. Questo errore può inoltre indicare problemi di interoperabilità con altre implementazioni di IPsec. In tal caso, se la connettività non è ostacolata, questi eventi possono essere ignorati.|  
|4976|N/D|Media|Durante la negoziazione in modalità principale, IPsec ha ricevuto un pacchetto di negoziazione non valido. Se il problema persiste, potrebbe indicare un problema di rete o un tentativo di modificare o riprodurre la negoziazione.|  
|4977|N/D|Media|Durante la negoziazione in modalità rapida, IPsec ha ricevuto un pacchetto di negoziazione non valido. Se il problema persiste, potrebbe indicare un problema di rete o un tentativo di modificare o riprodurre la negoziazione.|  
|4978|N/D|Media|Durante la negoziazione in modalità estesa, IPsec ha ricevuto un pacchetto di negoziazione non valido. Se il problema persiste, potrebbe indicare un problema di rete o un tentativo di modificare o riprodurre la negoziazione.|  
|4983|N/D|Media|Negoziazione in modalità estesa IPsec non riuscita. L'associazione di sicurezza in modalità principale corrispondente è stata eliminata.|  
|4984|N/D|Media|Negoziazione in modalità estesa IPsec non riuscita. L'associazione di sicurezza in modalità principale corrispondente è stata eliminata.|  
|5027|N/D|Media|Il servizio Windows Firewall non è stato in grado di recuperare i criteri di sicurezza dalla risorsa di archiviazione locale. Il servizio continuerà ad applicare i criteri correnti.|  
|5028|N/D|Media|Il servizio Windows Firewall non è stato in grado di analizzare i nuovi criteri di sicurezza. Il servizio continuerà con i criteri applicati correnti.|  
|5029|N/D|Media|Il servizio Windows Firewall non è riuscito a inizializzare il driver. Il servizio continuerà ad applicare i criteri correnti.|  
|5030|N/D|Media|Impossibile avviare il servizio Windows Firewall.|  
|5035|N/D|Media|Non è stato possibile avviare il driver Windows Firewall.|  
|5037|N/D|Media|Errore di runtime critico rilevato dal driver Windows Firewall. Arresto in corso.|  
|5038|N/D|Media|L'integrità del codice ha determinato che l'hash dell'immagine di un file non è valido. Il file potrebbe essere danneggiato a causa di una modifica non autorizzata o l'hash non valido potrebbe indicare un potenziale errore del dispositivo disco.|  
|5120|N/D|Media|Il servizio risponditore OCSP è stato avviato|  
|5121|N/D|Media|Il servizio risponditore OCSP è stato arrestato|  
|5122|N/D|Media|Una voce di configurazione è cambiata nel servizio risponditore OCSP|  
|5123|N/D|Media|Una voce di configurazione è cambiata nel servizio risponditore OCSP|  
|5376|N/D|Media|È stato eseguito il backup delle credenziali di gestione credenziali.|  
|5377|N/D|Media|Le credenziali di gestione credenziali sono state ripristinate da un backup.|  
|5453|N/D|Media|Una negoziazione IPsec con un computer remoto non è riuscita perché il servizio AuthIP (IKEEXT) IPsec IKE e non è stato avviato.|  
|5480|N/D|Media|Impossibile per i servizi IPsec ottenere l'elenco completo delle interfacce di rete nel computer. Questo costituisce un potenziale rischio per la sicurezza perché alcune interfacce di rete potrebbero non ottenere la protezione fornita dai filtri IPsec applicati. Utilizzare lo snap-in Monitor di sicurezza IP per diagnosticare il problema.|  
|5483|N/D|Media|Impossibile inizializzare il server RPC da servizi IPsec. Impossibile avviare i servizi IPsec.|  
|5484|N/D|Media|Si è verificato un errore critico dei servizi IPsec ed è stato arrestato. L'arresto dei servizi IPsec può comportare un maggiore rischio di attacchi alla rete o esporre il computer a potenziali rischi per la sicurezza.|  
|5485|N/D|Media|I servizi IPsec non sono riusciti a elaborare alcuni filtri IPsec in un evento plug and Play per le interfacce di rete. Questo costituisce un potenziale rischio per la sicurezza perché alcune interfacce di rete potrebbero non ottenere la protezione fornita dai filtri IPsec applicati. Utilizzare lo snap-in Monitor di sicurezza IP per diagnosticare il problema.|  
|6145|N/D|Media|Si sono verificati uno o più errori durante l'elaborazione dei criteri di sicurezza negli oggetti Criteri di gruppo.|  
|6273|N/D|Media|Il server dei criteri di rete ha negato l'accesso a un utente.|  
|6274|N/D|Media|Il server dei criteri di rete ha ignorato la richiesta di un utente.|  
|6275|N/D|Media|Il server dei criteri di rete ha ignorato la richiesta di accounting per un utente.|  
|6276|N/D|Media|Server dei criteri di rete in quarantena di un utente.|  
|6277|N/D|Media|Il server dei criteri di rete ha concesso l'accesso a un utente, ma lo ha messo in prova perché l'host non soddisfa i criteri di integrità definiti.|  
|6278|N/D|Media|Il server dei criteri di rete ha concesso l'accesso completo a un utente perché l'host soddisfa i criteri di integrità definiti.|  
|6279|N/D|Media|Il server dei criteri di rete ha bloccato l'account utente a causa di tentativi di autenticazione ripetuti non riusciti.|  
|6280|N/D|Media|Il server dei criteri di rete ha sbloccato l'account utente.|  
|-|640|Media|Database account generale modificato|  
|-|619|Media|Criteri di qualità del servizio modificati|  
|24586|N/D|Media|Si è verificato un errore durante la conversione del volume|  
|24592|N/D|Media|Tentativo di riavvio automatico della conversione sul volume %2 non riuscito.|  
|24593|N/D|Media|Scrittura dei metadati: il volume %2 restituisce errori durante il tentativo di modificare i metadati. Se gli errori continuano, decrittografare il volume|  
|24594|N/D|Media|Ricompilazione dei metadati: tentativo di scrittura di una copia dei metadati nel volume %2 non riuscito e potrebbe essere danneggiato. Se gli errori continuano, decrittografare il volume.|  
|4608|512|Bassa|Avvio di Windows in corso.|  
|4609|513|Bassa|È in corso l'arresto di Windows.|  
|4610|514|Bassa|Un pacchetto di autenticazione è stato caricato dall'autorità di sicurezza locale.|  
|4611|515|Bassa|Un processo di accesso attendibile è stato registrato con l'autorità di sicurezza locale.|  
|4612|516|Bassa|Le risorse interne allocate per l'accodamento dei messaggi di controllo sono state esaurite, causando la perdita di alcuni controlli.|  
|4614|518|Bassa|Un pacchetto di notifica è stato caricato da Gestione account di sicurezza.|  
|4615|519|Bassa|Uso non valido della porta LPC.|  
|4616|520|Bassa|L'ora di sistema è stata modificata.|  
|4622|N/D|Bassa|Un pacchetto di sicurezza è stato caricato dall'autorità di sicurezza locale.|  
|4624|528.540|Bassa|Accesso di un account completato.|  
|4625|529-537539|Bassa|Accesso non riuscito per un account.|  
|4634|538|Bassa|Un account è stato disconnesso.|  
|4646|N/D|Bassa|Modalità di prevenzione DoS IKE avviata.|  
|4647|551|Bassa|Disconnessione avviata dall'utente.|  
|4648|552|Bassa|Tentativo di accesso con credenziali esplicite.|  
|4650|N/D|Bassa|È stata stabilita un'associazione di sicurezza in modalità principale IPsec. La modalità estesa non è stata abilitata. Non è stata utilizzata l'autenticazione del certificato.|  
|4651|N/D|Bassa|È stata stabilita un'associazione di sicurezza in modalità principale IPsec. La modalità estesa non è stata abilitata. Per l'autenticazione è stato usato un certificato.|  
|4652|N/D|Bassa|Negoziazione in modalità principale IPsec non riuscita.|  
|4653|N/D|Bassa|Negoziazione in modalità principale IPsec non riuscita.|  
|4654|N/D|Bassa|Negoziazione in modalità rapida IPsec non riuscita.|  
|4655|N/D|Bassa|È stata terminata un'associazione di sicurezza in modalità principale IPsec.|  
|4656|560|Bassa|È stato richiesto un handle a un oggetto.|  
|4657|567|Bassa|È stato modificato un valore del registro di sistema.|  
|4658|562|Bassa|Handle per un oggetto chiuso.|  
|4659|N/D|Bassa|È stato richiesto un handle a un oggetto con finalità da eliminare.|  
|4660|564|Bassa|Un oggetto è stato eliminato.|  
|4661|565|Bassa|È stato richiesto un handle a un oggetto.|  
|4662|566|Bassa|È stata eseguita un'operazione su un oggetto.|  
|4663|567|Bassa|È stato effettuato un tentativo di accesso a un oggetto.|  
|4664|N/D|Bassa|È stato effettuato un tentativo di creare un collegamento fisso.|  
|4665|N/D|Bassa|È stato effettuato un tentativo di creare un contesto client dell'applicazione.|  
|4666|N/D|Bassa|Un'applicazione ha tentato di eseguire un'operazione:|  
|4667|N/D|Bassa|Un contesto client applicazione è stato eliminato.|  
|4668|N/D|Bassa|Un'applicazione è stata inizializzata.|  
|4670|N/D|Bassa|Le autorizzazioni per un oggetto sono state modificate.|  
|4671|N/D|Bassa|Un'applicazione ha tentato di accedere a un ordinale bloccato tramite TBS.|  
|4672|576|Bassa|Privilegi speciali assegnati al nuovo accesso.|  
|4673|577|Bassa|È stato chiamato un servizio con privilegi.|  
|4674|578|Bassa|Si è tentato di eseguire un'operazione su un oggetto con privilegi.|  
|4688|592|Bassa|È stato creato un nuovo processo.|  
|4689|593|Bassa|Un processo è stato terminato.|  
|4690|594|Bassa|È stato effettuato un tentativo di duplicare un handle per un oggetto.|  
|4691|595|Bassa|È stato richiesto l'accesso indiretto a un oggetto.|  
|4694|N/D|Bassa|È stata tentata la protezione dei dati protetti controllabili.|  
|4695|N/D|Bassa|È stato eseguito un tentativo di inProtezione dei dati protetti controllabili.|  
|4696|600|Bassa|Un token primario è stato assegnato al processo.|  
|4697|601|Bassa|Tentativo di installare un servizio|  
|4698|602|Bassa|È stata creata un'attività pianificata.|  
|4699|602|Bassa|Un'attività pianificata è stata eliminata.|  
|4700|602|Bassa|È stata abilitata un'attività pianificata.|  
|4701|602|Bassa|Un'attività pianificata è stata disabilitata.|  
|4702|602|Bassa|È stata aggiornata un'attività pianificata.|  
|4704|608|Bassa|Un diritto utente è stato assegnato.|  
|4705|609|Bassa|Un diritto utente è stato rimosso.|  
|4707|611|Bassa|Una relazione di trust con un dominio è stata rimossa.|  
|4709|N/D|Bassa|Servizi IPsec è stato avviato.|  
|4710|N/D|Bassa|Servizi IPsec è stato disabilitato.|  
|4711|N/D|Bassa|Può contenere uno degli elementi seguenti: il motore PAStore ha applicato la copia memorizzata nella cache locale del criterio IPsec di archiviazione Active Directory nel computer. Il motore PAStore ha applicato Active Directory criterio IPsec di archiviazione nel computer. Il motore PAStore ha applicato i criteri IPsec di archiviazione del registro di sistema locale nel computer. Il motore PAStore non è riuscito a applicare la copia memorizzata nella cache locale del criterio IPsec di archiviazione Active Directory nel computer. Il motore PAStore non è stato in grado di applicare Active Directory criterio IPsec di archiviazione nel computer. Il motore PAStore non è riuscito ad applicare il criterio IPsec di archiviazione del registro di sistema locale nel computer. Il motore PAStore non è riuscito ad applicare alcune regole del criterio IPsec attivo nel computer. Il motore PAStore non è riuscito a caricare i criteri IPsec di archiviazione directory nel computer. Criterio IPsec di archiviazione directory caricato dal motore PAStore nel computer. Il motore PAStore non è riuscito a caricare i criteri IPsec di archiviazione locale nel computer. Il motore PAStore ha caricato i criteri IPsec di archiviazione locale nel computer. Il motore PAStore ha eseguito il polling delle modifiche apportate al criterio IPsec attivo e non ha rilevato alcuna modifica. |  
|4712|N/D|Bassa|Si è verificato un errore potenzialmente grave nei servizi IPsec.|  
|4717|621|Bassa|L'accesso alla sicurezza del sistema è stato concesso a un account.|  
|4718|622|Bassa|L'accesso alla sicurezza del sistema è stato rimosso da un account.|  
|4720|624|Bassa|È stato creato un account utente.|  
|4722|626|Bassa|Un account utente è stato abilitato.|  
|4723|Procedura di risoluzione per l'ID evento 627|Bassa|È stato effettuato un tentativo di modificare la password di un account.|  
|4725|629|Bassa|Un account utente è stato disabilitato.|  
|4726|630|Bassa|Un account utente è stato eliminato.|  
|4728|632|Bassa|Un membro è stato aggiunto a un gruppo globale abilitato per la sicurezza.|  
|4729|633|Bassa|Un membro è stato rimosso da un gruppo globale abilitato per la sicurezza.|  
|4730|634|Bassa|Un gruppo globale abilitato per la sicurezza è stato eliminato.|  
|4731|635|Bassa|È stato creato un gruppo locale abilitato per la sicurezza.|  
|4732|636|Bassa|Un membro è stato aggiunto a un gruppo locale abilitato per la sicurezza.|  
|4733|637|Bassa|Un membro è stato rimosso da un gruppo locale abilitato per la sicurezza.|  
|4734|638|Bassa|Un gruppo locale abilitato per la sicurezza è stato eliminato.|  
|4738|642|Bassa|Un account utente è stato modificato.|  
|4740|644|Bassa|Un account utente è stato bloccato.|  
|4741|645|Bassa|Un account computer è stato modificato.|  
|4742|646|Bassa|Un account computer è stato modificato.|  
|4743|647|Bassa|Un account computer è stato eliminato.|  
|4744|648|Bassa|È stato creato un gruppo locale disabilitato per la sicurezza.|  
|4745|649|Bassa|Un gruppo locale disabilitato per la sicurezza è stato modificato.|  
|4746|650|Bassa|Un membro è stato aggiunto a un gruppo locale disabilitato per la sicurezza.|  
|4747|651|Bassa|Un membro è stato rimosso da un gruppo locale disabilitato per la sicurezza.|  
|4748|652|Bassa|Un gruppo locale disabilitato per la sicurezza è stato eliminato.|  
|4749|653|Bassa|È stato creato un gruppo globale disabilitato per la sicurezza.|  
|4750|654|Bassa|Un gruppo globale disabilitato per la sicurezza è stato modificato.|  
|4751|655|Bassa|Un membro è stato aggiunto a un gruppo globale disabilitato per la sicurezza.|  
|4752|656|Bassa|Un membro è stato rimosso da un gruppo globale disabilitato per la sicurezza.|  
|4753|657|Bassa|Un gruppo globale disabilitato per la sicurezza è stato eliminato.|  
|4756|660|Bassa|Un membro è stato aggiunto a un gruppo universale abilitato per la sicurezza.|  
|4757|661|Bassa|Un membro è stato rimosso da un gruppo universale abilitato per la sicurezza.|  
|4758|662|Bassa|È stato eliminato un gruppo universale abilitato per la sicurezza.|  
|4759|663|Bassa|È stato creato un gruppo universale disabilitato per la sicurezza.|  
|4760|664|Bassa|Un gruppo universale disabilitato per la sicurezza è stato modificato.|  
|4761|665|Bassa|Un membro è stato aggiunto a un gruppo universale disabilitato per la sicurezza.|  
|4762|666|Bassa|Un membro è stato rimosso da un gruppo universale disabilitato per la sicurezza.|  
|4767|671|Bassa|Un account utente è stato sbloccato.|  
|4768|672.676|Bassa|È stato richiesto un ticket di autenticazione Kerberos (TGT).|  
|4769|673|Bassa|È stato richiesto un ticket di servizio Kerberos.|  
|4770|674|Bassa|Un ticket di servizio Kerberos è stato rinnovato.|  
|4771|675|Bassa|Pre-autenticazione Kerberos non riuscita.|  
|4772|672|Bassa|Una richiesta di ticket di autenticazione Kerberos non è riuscita.|  
|4774|678|Bassa|È stato eseguito il mapping di un account per l'accesso.|  
|4775|679|Bassa|Non è stato possibile eseguire il mapping di un account per l'accesso.|  
|4776|680.681|Bassa|Il controller di dominio ha tentato di convalidare le credenziali per un account.|  
|4777|N/D|Bassa|Il controller di dominio non è riuscito a convalidare le credenziali per un account.|  
|4778|682|Bassa|Una sessione è stata riconnessa a una stazione di finestra.|  
|4779|683|Bassa|Una sessione è stata disconnessa da una stazione di finestra.|  
|4781|685|Bassa|Il nome di un account è stato modificato:|  
|4782|N/D|Bassa|Hash della password a cui è stato eseguito l'accesso a un account.|  
|4783|667|Bassa|È stato creato un gruppo di applicazioni di base.|  
|4784|N/D|Bassa|Un gruppo di applicazioni di base è stato modificato.|  
|4785|689|Bassa|Un membro è stato aggiunto a un gruppo di applicazioni di base.|  
|4786|690|Bassa|Un membro è stato rimosso da un gruppo di applicazioni di base.|  
|4787|691|Bassa|Un membro non membro è stato aggiunto a un gruppo di applicazioni di base.|  
|4788|692|Bassa|Un oggetto non membro è stato rimosso da un gruppo di applicazioni di base.|  
|4789|693|Bassa|Un gruppo di applicazioni di base è stato eliminato.|  
|4790|694|Bassa|È stato creato un gruppo di query LDAP.|  
|4793|N/D|Bassa|È stata chiamata l'API di controllo dei criteri password.|  
|4800|N/D|Bassa|La workstation è stata bloccata.|  
|4801|N/D|Bassa|La workstation è stata sbloccata.|  
|4802|N/D|Bassa|Il screen saver è stato richiamato.|  
|4803|N/D|Bassa|Il screen saver è stato scartato.|  
|4864|N/D|Bassa|È stato rilevato un conflitto dello spazio dei nomi.|  
|4869|773|Bassa|Servizi certificati ha ricevuto una richiesta di certificato inviata di nuovo.|  
|4871|775|Bassa|Servizi certificati ha ricevuto una richiesta di pubblicare l'elenco di revoche di certificati (CRL).|  
|4872|776|Bassa|Servizi certificati ha pubblicato l'elenco di revoche di certificati (CRL).|  
|4873|777|Bassa|L'estensione di una richiesta di certificato è cambiata.|  
|4874|778|Bassa|Uno o più attributi della richiesta di certificato sono cambiati.|  
|4875|779|Bassa|Servizi certificati ha ricevuto una richiesta di arresto.|  
|4876|780|Bassa|Il backup di Servizi certificati è stato avviato.|  
|4877|781|Bassa|È stato completato il backup di Servizi certificati.|  
|4878|782|Bassa|È stato avviato il ripristino di Servizi certificati.|  
|4879|783|Bassa|È stato completato il ripristino di Servizi certificati.|  
|4880|Procedura di risoluzione per l'ID evento 784|Bassa|Servizi certificati è stato avviato.|  
|4881|785|Bassa|Servizi certificati è stato arrestato.|  
|4883|787|Bassa|Servizi certificati ha recuperato una chiave archiviata.|  
|4884|788|Bassa|Servizi certificati ha importato un certificato nel database.|  
|4886|790|Bassa|Servizi certificati ha ricevuto una richiesta di certificato.|  
|4887|791|Bassa|Servizi certificati ha approvato una richiesta di certificato e ha emesso un certificato.|  
|4888|792|Bassa|Servizi certificati ha negato una richiesta di certificato.|  
|4889|793|Bassa|Servizi certificati ha impostato in sospeso lo stato di una richiesta di certificato.|  
|4891|795|Bassa|Una voce di configurazione in Servizi certificati è cambiata.|  
|4893|797|Bassa|Servizi certificati ha archiviato una chiave.|  
|4894|798|Bassa|Servizi certificati ha importato e archiviato una chiave.|  
|4895|799|Bassa|Servizi certificati ha pubblicato il certificato CA per Active Directory Domain Services.|  
|4898|802|Bassa|Modello caricato da Servizi certificati.|  
|4902|N/D|Bassa|È stata creata la tabella dei criteri di controllo per utente.|  
|4904|N/D|Bassa|È stato effettuato un tentativo di registrare un'origine evento di sicurezza.|  
|4905|N/D|Bassa|È stato effettuato un tentativo di annullare la registrazione di un'origine evento di sicurezza.|  
|4909|N/D|Bassa|Le impostazioni dei criteri locali per TBS sono state modificate.|  
|4910|N/D|Bassa|Le impostazioni Criteri di gruppo per TBS sono state modificate.|  
|4928|N/D|Bassa|È stato stabilito un contesto dei nomi di origine della replica Active Directory.|  
|4929|N/D|Bassa|Un contesto dei nomi di origine della replica Active Directory è stato rimosso.|  
|4930|N/D|Bassa|Un contesto dei nomi di origine della replica Active Directory è stato modificato.|  
|4931|N/D|Bassa|Un contesto dei nomi di destinazione della replica Active Directory è stato modificato.|  
|4932|N/D|Bassa|È iniziata la sincronizzazione di una replica di un Active Directory contesto dei nomi.|  
|4933|N/D|Bassa|La sincronizzazione di una replica di un contesto dei nomi Active Directory è terminata.|  
|4934|N/D|Bassa|Attributi di un oggetto Active Directory replicati.|  
|4935|N/D|Bassa|Inizio errore di replica.|  
|4936|N/D|Bassa|L'errore di replica termina.|  
|4937|N/D|Bassa|Un oggetto persistente è stato rimosso da una replica.|  
|4944|N/D|Bassa|Il criterio seguente era attivo al momento dell'avvio del Windows Firewall.|  
|4945|N/D|Bassa|Una regola è stata elencata all'avvio del Windows Firewall.|  
|4946|N/D|Bassa|È stata apportata una modifica a Windows Firewall elenco di eccezioni. È stata aggiunta una regola.|  
|4947|N/D|Bassa|È stata apportata una modifica a Windows Firewall elenco di eccezioni. È stata modificata una regola.|  
|4948|N/D|Bassa|È stata apportata una modifica a Windows Firewall elenco di eccezioni. È stata eliminata una regola.|  
|4949|N/D|Bassa|Windows Firewall impostazioni sono state ripristinate ai valori predefiniti.|  
|4950|N/D|Bassa|È stata modificata un'impostazione di Windows Firewall.|  
|4951|N/D|Bassa|Una regola è stata ignorata perché il numero di versione principale non è stato riconosciuto dal Windows Firewall.|  
|4952|N/D|Bassa|Le parti di una regola sono state ignorate perché il numero di versione secondario non è stato riconosciuto dal Windows Firewall. Le altre parti della regola verranno applicate.|  
|4953|N/D|Bassa|Una regola è stata ignorata da Windows Firewall perché non è stata in grado di analizzare la regola.|  
|4954|N/D|Bassa|Windows Firewall impostazioni Criteri di gruppo sono state modificate. Sono state applicate le nuove impostazioni.|  
|4956|N/D|Bassa|Il profilo attivo è stato modificato Windows Firewall.|  
|4957|N/D|Bassa|Windows Firewall non è stata applicata la regola seguente:|  
|4958|N/D|Bassa|Windows Firewall non ha applicato la regola seguente perché la regola si riferisce a elementi non configurati in questo computer:|  
|4979|N/D|Bassa|Sono state stabilite le associazioni di sicurezza IPsec in modalità principale e in modalità estesa.|  
|4980|N/D|Bassa|Sono state stabilite le associazioni di sicurezza IPsec in modalità principale e in modalità estesa.|  
|4981|N/D|Bassa|Sono state stabilite le associazioni di sicurezza IPsec in modalità principale e in modalità estesa.|  
|4982|N/D|Bassa|Sono state stabilite le associazioni di sicurezza IPsec in modalità principale e in modalità estesa.|  
|4985|N/D|Bassa|Lo stato di una transazione è stato modificato.|  
|5024|N/D|Bassa|Il servizio Windows Firewall è stato avviato correttamente.|  
|5025|N/D|Bassa|Il servizio Windows Firewall è stato arrestato.|  
|5031|N/D|Bassa|Il servizio Windows Firewall ha impedito a un'applicazione di accettare le connessioni in ingresso sulla rete.|  
|5032|N/D|Bassa|Windows Firewall non è stato in grado di notificare all'utente che l'applicazione ha bloccato l'accettazione delle connessioni in ingresso sulla rete.|  
|5033|N/D|Bassa|Il driver Windows Firewall è stato avviato correttamente.|  
|5034|N/D|Bassa|Il driver Windows Firewall è stato arrestato.|  
|5039|N/D|Bassa|Una chiave del registro di sistema è stata virtualizzata.|  
|5040|N/D|Bassa|È stata apportata una modifica alle impostazioni IPsec. È stato aggiunto un set di autenticazione.|  
|5041|N/D|Bassa|È stata apportata una modifica alle impostazioni IPsec. Un set di autenticazione è stato modificato.|  
|5042|N/D|Bassa|È stata apportata una modifica alle impostazioni IPsec. Un set di autenticazione è stato eliminato.|  
|5043|N/D|Bassa|È stata apportata una modifica alle impostazioni IPsec. È stata aggiunta una regola di sicurezza della connessione.|  
|5044|N/D|Bassa|È stata apportata una modifica alle impostazioni IPsec. Una regola di sicurezza della connessione è stata modificata.|  
|5045|N/D|Bassa|È stata apportata una modifica alle impostazioni IPsec. Una regola di sicurezza della connessione è stata eliminata.|  
|5046|N/D|Bassa|È stata apportata una modifica alle impostazioni IPsec. È stato aggiunto un set di crittografia.|  
|5047|N/D|Bassa|È stata apportata una modifica alle impostazioni IPsec. Un set di crittografia è stato modificato.|  
|5048|N/D|Bassa|È stata apportata una modifica alle impostazioni IPsec. Un set di crittografia è stato eliminato.|  
|5050|N/D|Bassa|Tentativo di disabilitare a livello di codice la Windows Firewall tramite una chiamata a InetFwProfile. FirewallEnabled (false)|  
|5051|N/D|Bassa|Un file è stato virtualizzato.|  
|5056|N/D|Bassa|È stato eseguito un self test crittografico.|  
|5057|N/D|Bassa|Operazione primitiva di crittografia non riuscita.|  
|5058|N/D|Bassa|Operazione del file di chiave.|  
|5059|N/D|Bassa|Operazione di migrazione della chiave.|  
|5060|N/D|Bassa|Operazione di verifica non riuscita.|  
|5061|N/D|Bassa|Operazione crittografica.|  
|5062|N/D|Bassa|È stato eseguito un self test crittografico in modalità kernel.|  
|5063|N/D|Bassa|Si è tentato di eseguire un'operazione del provider del servizio di crittografia.|  
|5064|N/D|Bassa|Si è tentato di eseguire un'operazione di contesto crittografico.|  
|5065|N/D|Bassa|È stata tentata una modifica del contesto crittografico.|  
|5066|N/D|Bassa|È stata tentata un'operazione della funzione di crittografia.|  
|5067|N/D|Bassa|È stata tentata una modifica della funzione di crittografia.|  
|5068|N/D|Bassa|Si è tentato di eseguire un'operazione del provider della funzione di crittografia.|  
|5069|N/D|Bassa|È stata tentata un'operazione di proprietà della funzione di crittografia.|  
|5070|N/D|Bassa|È stata tentata una modifica della proprietà della funzione di crittografia.|  
|5125|N/D|Bassa|Una richiesta è stata inviata al servizio risponditore OCSP|  
|5126|N/D|Bassa|Il certificato di firma è stato aggiornato automaticamente dal servizio risponditore OCSP|  
|5127|N/D|Bassa|Il provider di revoche OCSP ha aggiornato correttamente le informazioni di revoca|  
|5136|566|Bassa|Un oggetto servizio directory è stato modificato.|  
|5137|566|Bassa|È stato creato un oggetto servizio directory.|  
|5138|N/D|Bassa|Un oggetto servizio directory non è stato eliminato.|  
|5139|N/D|Bassa|Un oggetto servizio directory è stato spostato.|  
|5140|N/D|Bassa|È stato eseguito l'accesso A un oggetto condivisione di rete.|  
|5141|N/D|Bassa|Un oggetto servizio directory è stato eliminato.|  
|5152|N/D|Bassa|Il pacchetto è stato bloccato dalla piattaforma filtro Windows.|  
|5153|N/D|Bassa|Un filtro della piattaforma filtro Windows più restrittivo ha bloccato un pacchetto.|  
|5154|N/D|Bassa|La piattaforma filtro Windows ha consentito a un'applicazione o a un servizio di restare in ascolto su una porta per le connessioni in ingresso.|  
|5155|N/D|Bassa|La piattaforma filtro Windows ha bloccato un'applicazione o un servizio che non è in ascolto su una porta per le connessioni in ingresso.|  
|5156|N/D|Bassa|La piattaforma filtro Windows ha consentito una connessione.|  
|5157|N/D|Bassa|La piattaforma filtro Windows ha bloccato una connessione.|  
|5158|N/D|Bassa|La piattaforma filtro Windows ha consentito l'associazione a una porta locale.|  
|5159|N/D|Bassa|La piattaforma filtro Windows ha bloccato un'associazione a una porta locale.|  
|5378|N/D|Bassa|La delega delle credenziali richieste non è consentita dai criteri.|  
|5440|N/D|Bassa|Il callout seguente era presente durante l'avvio del motore di filtro di base della piattaforma filtro Windows.|  
|5441|N/D|Bassa|Il filtro seguente era presente durante l'avvio del motore di filtro di base della piattaforma filtro Windows.|  
|5442|N/D|Bassa|Il provider seguente era presente durante l'avvio del motore di filtro di base della piattaforma filtro Windows.|  
|5443|N/D|Bassa|Il contesto del provider seguente era presente durante l'avvio del motore di filtro di base della piattaforma filtro Windows.|  
|5444|N/D|Bassa|Il sottolivello seguente era presente durante l'avvio del motore di filtro di base della piattaforma filtro Windows.|  
|5446|N/D|Bassa|Un callout della piattaforma filtro Windows è stato modificato.|  
|5447|N/D|Bassa|È stato modificato un filtro della piattaforma filtro Windows.|  
|5448|N/D|Bassa|Un provider della piattaforma filtro Windows è stato modificato.|  
|5449|N/D|Bassa|Un contesto del provider della piattaforma filtro Windows è stato modificato.|  
|5450|N/D|Bassa|Un sottolivello della piattaforma filtro Windows è stato modificato.|  
|5451|N/D|Bassa|È stata stabilita un'associazione di sicurezza in modalità rapida IPsec.|  
|5452|N/D|Bassa|Un'associazione di sicurezza in modalità rapida IPsec è terminata.|  
|5456|N/D|Bassa|Il motore PAStore ha applicato Active Directory criterio IPsec di archiviazione nel computer.|  
|5457|N/D|Bassa|Il motore PAStore non è stato in grado di applicare Active Directory criterio IPsec di archiviazione nel computer.|  
|5458|N/D|Bassa|Il motore PAStore ha applicato la copia memorizzata nella cache locale del criterio IPsec di archiviazione Active Directory nel computer.|  
|5459|N/D|Bassa|Il motore PAStore non è riuscito a applicare la copia memorizzata nella cache locale del criterio IPsec di archiviazione Active Directory nel computer.|  
|5460|N/D|Bassa|Il motore PAStore ha applicato i criteri IPsec di archiviazione del registro di sistema locale nel computer.|  
|5461|N/D|Bassa|Il motore PAStore non è riuscito ad applicare il criterio IPsec di archiviazione del registro di sistema locale nel computer.|  
|5462|N/D|Bassa|Il motore PAStore non è riuscito ad applicare alcune regole del criterio IPsec attivo nel computer. Utilizzare lo snap-in Monitor di sicurezza IP per diagnosticare il problema.|  
|5463|N/D|Bassa|Il motore PAStore ha eseguito il polling delle modifiche apportate al criterio IPsec attivo e non ha rilevato alcuna modifica.|  
|5464|N/D|Bassa|Il motore PAStore ha eseguito il polling delle modifiche apportate al criterio IPsec attivo, ha rilevato le modifiche e le ha applicate ai servizi IPsec.|  
|5465|N/D|Bassa|Il motore PAStore ha ricevuto un controllo per il ricaricamento forzato del criterio IPsec ed ha elaborato correttamente il controllo.|  
|5466|N/D|Bassa|Il motore PAStore ha eseguito il polling delle modifiche apportate al criterio IPsec Active Directory, ha determinato che non è possibile raggiungere Active Directory e utilizzerà invece la copia memorizzata nella cache del criterio IPsec Active Directory. Non è stato possibile applicare le modifiche apportate al criterio IPsec Active Directory dall'ultimo polling.|  
|5467|N/D|Bassa|Il motore PAStore ha eseguito il polling delle modifiche apportate al criterio IPsec Active Directory, ha determinato che è possibile raggiungere Active Directory e non è stata trovata alcuna modifica ai criteri. La copia memorizzata nella cache del criterio IPsec Active Directory non viene più utilizzata.|  
|5468|N/D|Bassa|Il motore PAStore ha eseguito il polling delle modifiche apportate al Active Directory criterio IPsec, ha determinato che è possibile raggiungere Active Directory, avere trovato modifiche ai criteri e applicato tali modifiche. La copia memorizzata nella cache del criterio IPsec Active Directory non viene più utilizzata.|  
|5471|N/D|Bassa|Il motore PAStore ha caricato i criteri IPsec di archiviazione locale nel computer.|  
|5472|N/D|Bassa|Il motore PAStore non è riuscito a caricare i criteri IPsec di archiviazione locale nel computer.|  
|5473|N/D|Bassa|Criterio IPsec di archiviazione directory caricato dal motore PAStore nel computer.|  
|5474|N/D|Bassa|Il motore PAStore non è riuscito a caricare i criteri IPsec di archiviazione directory nel computer.|  
|5477|N/D|Bassa|Il motore PAStore non è riuscito ad aggiungere un filtro in modalità rapida.|  
|5479|N/D|Bassa|La chiusura dei servizi IPsec è stata completata. L'arresto dei servizi IPsec può comportare un maggiore rischio di attacchi alla rete o esporre il computer a potenziali rischi per la sicurezza.|  
|5632|N/D|Bassa|È stata effettuata una richiesta di autenticazione a una rete wireless.|  
|5633|N/D|Bassa|È stata effettuata una richiesta di autenticazione a una rete cablata.|  
|5712|N/D|Bassa|È stato eseguito un tentativo di chiamata di procedura remota (RPC).|  
|5888|N/D|Bassa|Un oggetto nel catalogo COM+ è stato modificato.|  
|5889|N/D|Bassa|Un oggetto è stato eliminato dal catalogo COM+.|  
|5890|N/D|Bassa|Un oggetto è stato aggiunto al catalogo COM+.|  
|6008|N/D|Bassa|L'arresto del sistema precedente era imprevisto|  
|6144|N/D|Bassa|Il criterio di sicurezza negli oggetti Criteri di gruppo è stato applicato correttamente.|  
|6272|N/D|Bassa|Il server dei criteri di rete ha concesso l'accesso a un utente.|  
|N/D|561|Bassa|È stato richiesto un handle a un oggetto.|  
|N/D|563|Bassa|Oggetto aperto per l'eliminazione|  
|N/D|625|Bassa|Tipo di account utente modificato|  
|N/D|613|Bassa|Agente criteri IPsec avviato|  
|N/D|614|Bassa|Agente criteri IPsec disabilitato|  
|N/D|615|Bassa|Agente criteri IPsec|  
|N/D|616|Bassa|Agente criteri IPsec ha rilevato un potenziale errore grave|  
|24577|N/D|Bassa|Crittografia del volume avviata|  
|24578|N/D|Bassa|Crittografia del volume interrotta|  
|24579|N/D|Bassa|Crittografia del volume completata|  
|24580|N/D|Bassa|Decrittografia del volume avviata|  
|24581|N/D|Bassa|Decrittografia del volume interrotta|  
|24582|N/D|Bassa|Decrittografia del volume completata|  
|24583|N/D|Bassa|Thread di lavoro conversione per volume avviato|  
|24584|N/D|Bassa|Thread di lavoro conversione per volume interrotto temporaneamente|  
|24588|N/D|Bassa|L'operazione di conversione sul volume %2 ha rilevato un errore di settore non valido. Convalidare i dati in questo volume|  
|24595|N/D|Bassa|Il volume %2 contiene cluster non validi. Questi cluster verranno ignorati durante la conversione.|  
|24621|N/D|Bassa|Controllo dello stato iniziale: transazione di conversione del volume in corso su %2.|  
|5049|N/D|Bassa|Un'associazione di sicurezza IPsec è stata eliminata.|  
|5478|N/D|Bassa|Avvio dei servizi IPsec completato.|  
  
> [!NOTE]  
> Per un elenco di molti ID evento di sicurezza e dei relativi significati, fare riferimento agli [eventi di controllo di sicurezza di Windows](https://www.microsoft.com/download/details.aspx?id=50034) .  
>
> Eseguire **wevtutil gp Microsoft-Windows-Security-Auditing/GE/GM: true** per ottenere un elenco dettagliato di tutti gli ID evento di sicurezza  
  
Per ulteriori informazioni sugli ID degli eventi di sicurezza di Windows e sui relativi significati, vedere l'articolo supporto tecnico Microsoft [Descrizione degli eventi di sicurezza in Windows 7 e in Windows Server 2008 R2](https://support.microsoft.com/kb/977519). È anche possibile scaricare [gli eventi di controllo di sicurezza per i dettagli degli eventi di sicurezza di Windows 7 e Windows server 2008 R2](https://www.microsoft.com/download/details.aspx?id=21561) e [Windows 8 e Windows Server 2012](https://www.microsoft.com/download/details.aspx?id=35753), che forniscono informazioni dettagliate sugli eventi per i sistemi operativi a cui si fa riferimento in formato foglio di calcolo.  
