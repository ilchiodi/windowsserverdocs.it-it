---
ms.assetid: 99a68050-8d19-4c58-ad86-e08a3dcdb4f7
title: 'Appendice L: eventi da monitorare'
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 07/30/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0b937debf2d9156c50f3c0ae51fdab8bd2a2bf2d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863482"
---
# <a name="appendix-l-events-to-monitor"></a>Appendice l: Eventi da monitorare

>Si applica a: Windows Server

Nella tabella seguente elenca gli eventi da monitorare nell'ambiente in uso, secondo le indicazioni disponibili in [monitoraggio di Active Directory per i segni di compromissione](../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md). Nella tabella seguente, la colonna "ID evento Windows corrente" Elenca l'ID evento implementato nelle versioni di Windows e Windows Server che sono attualmente in supporto "Mainstream".  
  
La colonna "ID evento Windows Legacy" Elenca l'ID evento corrispondenti nelle versioni legacy di Windows, ad esempio i computer client che eseguono Windows XP o versioni precedenti e i server che eseguono Windows Server 2003 o versione precedente. Il "criticità potenziale livello" colonna indica se l'evento deve essere considerato di bassa, Media o alta criticità il rilevamento di attacchi e la colonna "Riepilogo evento" fornisce una breve descrizione dell'evento.  
  
Una criticità potenziale di alto livello significa che una occorrenza dell'evento deve essere analizzato. Criticità potenziale di Media o bassa significa che questi eventi devono essere esaminati solo se si verificano in modo imprevisto o numeri che superano significativamente la base prevista in un periodo di tempo. Tutte le organizzazioni devono testare queste raccomandazioni nei propri ambienti prima di creare avvisi che richiedono risposte obbligatorie in seguito a indagini. Ogni ambiente è diverso e alcuni degli eventi classificati con una criticità potenziale di alto livello possono verificarsi a causa di altri eventi innocui.  
  
|||||  
|-|-|-|-|  
|**ID evento Windows corrente**|**ID evento Windows legacy**|**Criticità potenziale**|**Riepilogo evento**|  
|4618|N/D|Alto|Si è verificato un modello di eventi di sicurezza monitorato.|  
|4649|N/D|Alto|È stato rilevato un attacco di tipo replay. Potrebbe essere un innocuo falso positivo a causa dell'errore di configurazione errata.|  
|4719|612|Alto|Criteri di controllo di sistema è stato modificato.|  
|4765|N/D|Alto|History SID è stato aggiunto a un account.|  
|4766|N/D|Alto|Tentativo di aggiungere History SID a un account non è riuscito.|  
|4794|N/D|Alto|È stato effettuato un tentativo di impostare la modalità ripristino servizi Directory.|  
|4897|801|Alto|Separazione ruoli abilitata:|  
|4964|N/D|Alto|Gruppi speciali sono stati assegnati a un nuovo account di accesso.|  
|5124|N/D|Alto|È stata aggiornata un'impostazione di sicurezza nel servizio Risponditore OCSP|  
|N/D|550|Medio-alta|Attacco possibili denial of service (DoS)|  
|1102|517|Medio-alta|Il log di controllo è stato cancellato|  
|4621|N/D|Medio|Amministratore recuperata CrashOnAuditFail di sistema. Agli utenti non amministratori a questo punto saranno possibile effettuare l'accesso. Potrebbe essere controllabili o più attività non siano stati registrati.|  
|4675|N/D|Medio|I SID sono stati filtrati.|  
|4692|N/D|Medio|Backup della chiave master di protezione dei dati è stato tentato.|  
|4693|N/D|Medio|Tentativo di recupero della chiave master di protezione dei dati.|  
|4706|610|Medio|È stato creato un nuovo trust a un dominio.|  
|4713|617|Medio|Criteri Kerberos è stato modificato.|  
|4714|618|Medio|Criteri di recupero di dati crittografati è stato modificato.|  
|4715|N/D|Medio|I criteri di controllo (SACL) su un oggetto è stato modificato.|  
|4716|620|Medio|Informazioni sui domini trusted è state modificate.|  
|4724|628|Medio|È stato effettuato il tentativo di reimpostare la password di un account.|  
|4727|631|Medio|È stato creato un gruppo globale con sicurezza abilitata.|  
|4735|639|Medio|Un gruppo locale abilitato per la sicurezza è stato modificato.|  
|4737|641|Medio|È stato modificato un gruppo globale con sicurezza abilitata.|  
|4739|643|Medio|Criterio del dominio è stato modificato.|  
|4754|658|Medio|È stato creato un gruppo universale con sicurezza abilitata.|  
|4755|659|Medio|Un gruppo universale abilitato per la sicurezza è stato modificato.|  
|4764|667|Medio|Eliminazione di un gruppo di protezione disabilitata|  
|4764|668|Medio|Tipo di un gruppo è stato modificato.|  
|4780|684|Medio|L'ACL è stata impostata per gli account che sono membri del gruppo administrators.|  
|4816|N/D|Medio|RPC ha rilevato una violazione dell'integrità durante la decrittografia di un messaggio in arrivo.|  
|4865|N/D|Medio|È stata aggiunta una voce di informazioni foresta trusted.|  
|4866|N/D|Medio|È stata rimossa una voce di informazioni foresta trusted.|  
|4867|N/D|Medio|È stata modificata una voce di informazioni foresta trusted.|  
|4868|772|Medio|Il gestore dei certificati ha negato una richiesta di certificati in sospeso.|  
|4870|774|Medio|Servizi certificati ha revocato un certificato.|  
|4882|786|Medio|Le autorizzazioni di sicurezza di Servizi certificati sono cambiate.|  
|4885|789|Medio|Il filtro di controllo di Servizi certificati è cambiato.|  
|4890|794|Medio|Le impostazioni del gestore di certificati per Servizi certificati sono cambiate.|  
|4892|796|Medio|Una proprietà di Servizi certificati è cambiata.|  
|4896|800|Medio|Una o più righe sono state eliminate dal database dei certificati.|  
|4906|N/D|Medio|Il valore CrashOnAuditFail è stato modificato.|  
|4907|N/D|Medio|Sono state modificate le impostazioni di controllo nell'oggetto.|  
|4908|N/D|Medio|Tabella di accesso di gruppi speciale modificata.|  
|4912|807|Medio|Per ogni criterio di controllo utente è stato modificato.|  
|4960|N/D|Medio|IPsec eliminato un pacchetto in ingresso non riuscite di un controllo di integrità. Se il problema persiste, ciò potrebbe indicare che un problema di rete o che i pacchetti vengono modificati in transito per questo computer. Verificare che i pacchetti inviati dal computer remoto siano gli stessi di quelli ricevuti da questo computer. Questo errore potrebbe indicare anche i problemi di interoperabilità con altre implementazioni di IPsec.|  
|4961|N/D|Medio|IPsec eliminato un pacchetto in ingresso che non superavano una verifica di riproduzione. Se il problema persiste, potrebbe indicare un attacco di tipo replay contro il computer.|  
|4962|N/D|Medio|IPsec eliminato un pacchetto in ingresso che non superavano una verifica di riproduzione. Il pacchetto in ingresso ha un valore troppo basso un numero di sequenza per assicurarsi che non era una riproduzione.|  
|4963|N/D|Medio|IPsec eliminato un pacchetto in ingresso come testo non crittografato che avrebbero dovuto essere protetti. Ciò può dipendere da computer remoto, la modifica dei criteri IPsec senza informare questo computer. Potrebbe anche trattarsi di un tentativo di attacco di spoofing.|  
|4965|N/D|Medio|IPsec ha ricevuto un pacchetto da un computer remoto con indice un parametro di sicurezza non corretto (SPI). Ciò è causato generalmente da problemi di hardware che è del danneggiamento dei pacchetti. Se questi errori persistono, verificare che i pacchetti inviati dal computer remoto siano gli stessi di quelli ricevuti da questo computer. Questo errore potrebbe indicare anche i problemi di interoperabilità con altre implementazioni di IPsec. In tal caso, se non viene impedita la connettività, quindi questi eventi possono essere ignorati.|  
|4976|N/D|Medio|Durante la negoziazione in modalità principale, IPsec ha ricevuto un pacchetto di negoziazione non è valido. Se il problema persiste, ciò potrebbe indicare un problema di rete o un tentativo di modificare o riprodurre questa negoziazione.|  
|4977|N/D|Medio|Durante la negoziazione in modalità rapida, IPsec ha ricevuto un pacchetto di negoziazione non è valido. Se il problema persiste, ciò potrebbe indicare un problema di rete o un tentativo di modificare o riprodurre questa negoziazione.|  
|4978|N/D|Medio|Durante la negoziazione in modalità estesa, IPsec ha ricevuto un pacchetto di negoziazione non è valido. Se il problema persiste, ciò potrebbe indicare un problema di rete o un tentativo di modificare o riprodurre questa negoziazione.|  
|4983|N/D|Medio|Una negoziazione in modalità estesa IPsec non riuscita. L'associazione di sicurezza in modalità principale corrispondente è stata eliminata.|  
|4984|N/D|Medio|Una negoziazione in modalità estesa IPsec non riuscita. L'associazione di sicurezza in modalità principale corrispondente è stata eliminata.|  
|5027|N/D|Medio|Il servizio di Windows Firewall non è riuscito a recuperare i criteri di sicurezza dalla risorsa di archiviazione locale. Il servizio continuerà ad applicare i criteri correnti.|  
|5028|N/D|Medio|Il servizio di Windows Firewall non è in grado di analizzare i nuovi criteri di sicurezza. Il servizio continuerà con i criteri applicati correnti.|  
|5029|N/D|Medio|Il servizio di Windows Firewall non è stato possibile inizializzare il driver. Il servizio continuerà ad applicare i criteri correnti.|  
|5030|N/D|Medio|Impossibile avviare il servizio di Windows Firewall.|  
|5035|N/D|Medio|Impossibile avviare il Driver di Windows Firewall.|  
|5037|N/D|Medio|Il Driver di Windows Firewall ha rilevato errore di run-time critici. Arresto in corso.|  
|5038|N/D|Medio|L'integrità del codice determinato che l'hash di un file di immagine non è valido. Il file potrebbe essere danneggiato a causa di modifiche non autorizzate o l'hash non valido potrebbe indicare un potenziale errore del dispositivo disco.|  
|5120|N/D|Medio|Servizio Risponditore OCSP avviato|  
|5121|N/D|Medio|Servizio Risponditore OCSP arrestato|  
|5122|N/D|Medio|Una voce di configurazione modificata nel servizio Risponditore OCSP|  
|5123|N/D|Medio|Una voce di configurazione modificata nel servizio Risponditore OCSP|  
|5376|N/D|Medio|Le credenziali di gestione credenziali è sono eseguito il.|  
|5377|N/D|Medio|Gestione credenziali le credenziali sono state ripristinate da un backup.|  
|5453|N/D|Medio|Una negoziazione IPsec con un computer remoto non è riuscita perché non è stato avviato il servizio IKE e AuthIP IPsec trasparenza moduli (IKEEXT).|  
|5480|N/D|Medio|I servizi di IPsec non è stato possibile ottenere l'elenco completo di interfacce di rete nel computer. Ciò costituisce un potenziale rischio di sicurezza perché alcune delle interfacce di rete potrebbero non fornire la protezione fornita dai filtri IPsec applicati. Utilizzare lo snap-in Monitor di sicurezza IP per la diagnosi del problema.|  
|5483|N/D|Medio|I servizi di IPsec non è stato possibile inizializzare il server RPC. Non è stato possibile avviare i servizi di IPsec.|  
|5484|N/D|Medio|Servizi di IPsec ha riscontrato un errore critico ed è stato arrestato. L'arresto dei servizi di IPsec può mettere il computer a maggiore rischio di attacco di rete o esporre il computer da potenziali rischi di sicurezza.|  
|5485|N/D|Medio|I servizi di IPsec non riuscita per l'elaborazione di alcuni filtri IPsec su un evento plug and play per interfacce di rete. Ciò costituisce un potenziale rischio di sicurezza perché alcune delle interfacce di rete potrebbero non fornire la protezione fornita dai filtri IPsec applicati. Utilizzare lo snap-in Monitor di sicurezza IP per la diagnosi del problema.|  
|6145|N/D|Medio|Uno o più errori durante l'elaborazione di criteri di sicurezza in oggetti Criteri di gruppo.|  
|6273|N/D|Medio|Server dei criteri di rete negato l'accesso a un utente.|  
|6274|N/D|Medio|Server dei criteri di rete rimosse la richiesta per un utente.|  
|6275|N/D|Medio|Server dei criteri di rete rimosse la richiesta di contabilità per un utente.|  
|6276|N/D|Medio|Server dei criteri di rete in quarantena un utente.|  
|6277|N/D|Medio|Server dei criteri di rete concedere l'accesso a un utente, ma inserirlo prova perché l'host non soddisfa i criteri di integrità definiti.|  
|6278|N/D|Medio|Server dei criteri di rete concesso l'accesso completo a un utente perché l'host soddisfatto i criteri di integrità definiti.|  
|6279|N/D|Medio|Server dei criteri di rete bloccato l'account utente a causa dell'autenticazione non riusciti ripetuti tentativi.|  
|6280|N/D|Medio|Server dei criteri di rete sbloccare l'account utente.|  
|-|640|Medio|Database di account generale modificata|  
|-|619|Medio|Criterio qualità del servizio modificato|  
|24586|N/D|Medio|Errore durante la conversione di volume|  
|24592|N/D|Medio|Tentativo di riavviare automaticamente la conversione nel volume %2 non riuscito.|  
|24593|N/D|Medio|Scrittura dei metadati: Restituzione degli errori volume %2 durante il tentativo di modificare i metadati. Se l'errore persiste, decrittografare i volumi|  
|24594|N/D|Medio|Ricompilazione dei metadati: Un tentativo di scrivere una copia dei metadati nel volume %2 non riuscita e potrebbe essere visualizzato come il danneggiamento del disco. Se l'errore persiste, è possibile decrittografare il volume.|  
|4608|512|Bassa|Avvio di Windows in corso.|  
|4609|513|Bassa|Windows è in corso l'arresto.|  
|4610|514|Bassa|Un pacchetto di autenticazione è stato caricato dall'autorità di sicurezza locale.|  
|4611|515|Bassa|Un processo di accesso attendibile è stato registrato con l'autorità di sicurezza locale.|  
|4612|516|Bassa|Le risorse interne allocate per l'accodamento dei messaggi di controllo sono state esaurite, causando la perdita di alcuni controlli.|  
|4614|518|Bassa|Un pacchetto di notifica è stato caricato da Security Account Manager.|  
|4615|519|Bassa|Utilizzo della porta LPC non valido.|  
|4616|520|Bassa|L'ora di sistema è stato modificato.|  
|4622|N/D|Bassa|Un pacchetto di sicurezza è stato caricato dall'autorità di sicurezza locale.|  
|4624|528,540|Bassa|Un account è stato connesso.|  
|4625|529-537,539|Bassa|Un account di accesso non riuscito.|  
|4634|538|Bassa|Un account è stato disconnesso.|  
|4646|N/D|Bassa|Modalità di prevenzione DoS IKE avviata.|  
|4647|551|Bassa|Disconnessione avviata dall'utente.|  
|4648|552|Bassa|Un account di accesso è stata tentata utilizzando credenziali esplicite.|  
|4650|N/D|Bassa|È stata stabilita un'associazione di sicurezza IPsec Main Mode. Modalità estesa non è stata abilitata. Non è stata utilizzata l'autenticazione del certificato.|  
|4651|N/D|Bassa|È stata stabilita un'associazione di sicurezza IPsec Main Mode. Modalità estesa non è stata abilitata. È stato usato un certificato per l'autenticazione.|  
|4652|N/D|Bassa|Una negoziazione in modalità principale IPsec non riuscita.|  
|4653|N/D|Bassa|Una negoziazione in modalità principale IPsec non riuscita.|  
|4654|N/D|Bassa|Una negoziazione in modalità rapida IPsec non riuscita.|  
|4655|N/D|Bassa|Un'associazione di sicurezza IPsec Main Mode è terminata.|  
|4656|560|Bassa|È stato richiesto un handle a un oggetto.|  
|4657|567|Bassa|Un valore del Registro di sistema è stato modificato.|  
|4658|562|Bassa|L'handle a un oggetto è stato chiuso.|  
|4659|N/D|Bassa|È stato richiesto un handle a un oggetto con l'intenzione di eliminare.|  
|4660|564|Bassa|Un oggetto è stato eliminato.|  
|4661|565|Bassa|È stato richiesto un handle a un oggetto.|  
|4662|566|Bassa|È stata eseguita un'operazione su un oggetto.|  
|4663|567|Bassa|È stato effettuato un tentativo di accedere a un oggetto.|  
|4664|N/D|Bassa|È stato effettuato un tentativo per creare un collegamento reale.|  
|4665|N/D|Bassa|È stato effettuato un tentativo per creare un contesto di client dell'applicazione.|  
|4666|N/D|Bassa|Un'applicazione tentata un'operazione:|  
|4667|N/D|Bassa|Un contesto di client dell'applicazione è stato eliminato.|  
|4668|N/D|Bassa|Un'applicazione è stata inizializzata.|  
|4670|N/D|Bassa|Sono state modificate le autorizzazioni per un oggetto.|  
|4671|N/D|Bassa|Un'applicazione ha tentata di accedere a un numero ordinale di bloccati tramite TBS.|  
|4672|576|Bassa|Assegnato al nuovo account di accesso privilegi speciali.|  
|4673|577|Bassa|È stato chiamato un servizio con privilegi.|  
|4674|578|Bassa|È stata tentata un'operazione su un oggetto con privilegi.|  
|4688|592|Bassa|È stato creato un nuovo processo.|  
|4689|593|Bassa|Un processo è terminato.|  
|4690|594|Bassa|È stato effettuato un tentativo di duplicare un handle a un oggetto.|  
|4691|595|Bassa|È stato richiesto l'accesso indiretto a un oggetto.|  
|4694|N/D|Bassa|È stata tentata la protezione dei dati protetti controllabili.|  
|4695|N/D|Bassa|Tentativo di rimuovere la protezione dei dati protetti controllabili.|  
|4696|600|Bassa|È stato assegnato un token primario per l'elaborazione.|  
|4697|601|Bassa|Tenta di installare un servizio|  
|4698|602|Bassa|È stata creata un'attività pianificata.|  
|4699|602|Bassa|Un'attività pianificata è stata eliminata.|  
|4700|602|Bassa|È stata abilitata un'attività pianificata.|  
|4701|602|Bassa|Un'attività pianificata è stata disabilitata.|  
|4702|602|Bassa|Un'attività pianificata è stata aggiornata.|  
|4704|608|Bassa|È stato assegnato un utente a destra.|  
|4705|609|Bassa|È stato rimosso un diritto utente.|  
|4707|611|Bassa|Una relazione di trust a un dominio è stato rimosso.|  
|4709|N/D|Bassa|IPsec è stato avviato.|  
|4710|N/D|Bassa|I servizi di IPsec è stato disabilitato.|  
|4711|N/D|Bassa|Può contenere uno dei seguenti: Motore PAStore applicati copia memorizzato nella cache locale del criterio IPsec di archiviazione di Active Directory nel computer. Motore PAStore applicati criteri IPsec di archiviazione di Active Directory nel computer. Motore PAStore applicati i criteri IPsec di archiviazione del Registro di sistema locale nel computer. Motore PAStore non è riuscito ad applicare una copia della cache locale del criterio IPsec di archiviazione di Active Directory nel computer. Motore PAStore non è riuscito ad applicare i criteri IPsec di archiviazione di Active Directory nel computer. Motore PAStore non è stato possibile applicare i criteri IPsec di archiviazione del Registro di sistema locale nel computer. Motore PAStore non è riuscito ad applicare alcune regole dei criteri IPsec attivo nel computer. Motore PAStore non è stato possibile caricare i criteri IPsec nel computer di archiviazione di directory. Motore PAStore caricato archiviazione directory criteri IPsec nel computer. Motore PAStore non è riuscito a caricare una risorsa di archiviazione locale dei criteri IPsec nel computer. Motore PAStore caricato archiviazione locale dei criteri IPsec nel computer. Motore PAStore eseguito il polling delle modifiche per il criterio IPsec e non rilevata nessuna modifica.|  
|4712|N/D|Bassa|IPsec si è verificato un errore potenzialmente grave.|  
|4717|621|Bassa|Accesso di sicurezza di sistema è stato concesso a un account.|  
|4718|622|Bassa|Accesso di sicurezza di sistema è stato rimosso da un account.|  
|4720|624|Bassa|È stato creato un account utente.|  
|4722|626|Bassa|Un account utente è stato abilitato.|  
|4723|Procedura di risoluzione per l'ID evento 627|Bassa|È stato effettuato il tentativo di modificare la password di un account.|  
|4725|629|Bassa|Un account utente è stato disabilitato.|  
|4726|630|Bassa|Un account utente è stato eliminato.|  
|4728|632|Bassa|Un membro è stato aggiunto a un gruppo globale con sicurezza abilitata.|  
|4729|633|Bassa|È stato rimosso un membro da un gruppo globale con sicurezza abilitata.|  
|4730|634|Bassa|È stato eliminato un gruppo globale con sicurezza abilitata.|  
|4731|635|Bassa|È stato creato un gruppo locale abilitato per la sicurezza.|  
|4732|636|Bassa|È stato aggiunto un membro a un gruppo locale abilitato per la sicurezza.|  
|4733|637|Bassa|Un membro è stato rimosso da un gruppo locale abilitato per la sicurezza.|  
|4734|638|Bassa|Un gruppo locale abilitato per la sicurezza è stato eliminato.|  
|4738|642|Bassa|Un account utente è stato modificato.|  
|4740|644|Bassa|Un account utente è stato bloccato.|  
|4741|645|Bassa|Un account del computer è stato modificato.|  
|4742|646|Bassa|Un account del computer è stato modificato.|  
|4743|647|Bassa|Un account del computer è stato eliminato.|  
|4744|648|Bassa|È stato creato un gruppo locale con sicurezza disabilitata.|  
|4745|649|Bassa|È stato modificato un gruppo locale con sicurezza disabilitata.|  
|4746|650|Bassa|È stato aggiunto un membro a un gruppo locale disabilitata.|  
|4747|651|Bassa|Un membro è stato rimosso da un gruppo locale disabilitata.|  
|4748|652|Bassa|È stato eliminato un gruppo locale con sicurezza disabilitata.|  
|4749|653|Bassa|È stato creato un gruppo globale con sicurezza disabilitata.|  
|4750|654|Bassa|È stato modificato un gruppo globale con sicurezza disabilitata.|  
|4751|655|Bassa|Un membro è stato aggiunto a un gruppo globale con sicurezza disabilitata.|  
|4752|656|Bassa|È stato rimosso un membro da un gruppo globale con sicurezza disabilitata.|  
|4753|657|Bassa|È stato eliminato un gruppo globale con sicurezza disabilitata.|  
|4756|660|Bassa|Un membro è stato aggiunto a un gruppo universale con sicurezza abilitata.|  
|4757|661|Bassa|È stato rimosso un membro da un gruppo universale con sicurezza abilitata.|  
|4758|662|Bassa|È stato eliminato un gruppo universale con sicurezza abilitata.|  
|4759|663|Bassa|È stato creato un gruppo universale con sicurezza disabilitata.|  
|4760|664|Bassa|È stato modificato un gruppo universale con sicurezza disabilitata.|  
|4761|665|Bassa|Un membro è stato aggiunto a un gruppo universale con sicurezza disabilitata.|  
|4762|666|Bassa|È stato rimosso un membro da un gruppo universale con sicurezza disabilitata.|  
|4767|671|Bassa|Un account utente è stato sbloccato.|  
|4768|672,676|Bassa|È stato richiesto un ticket di autenticazione Kerberos (TGT).|  
|4769|673|Bassa|È stato richiesto un ticket di servizio Kerberos.|  
|4770|674|Bassa|È stato rinnovato un ticket di servizio Kerberos.|  
|4771|675|Bassa|Preautenticazione Kerberos non è riuscita.|  
|4772|672|Bassa|Una richiesta di ticket di autenticazione Kerberos non è riuscita.|  
|4774|678|Bassa|È stato eseguito il mapping di un account per l'accesso.|  
|4775|679|Bassa|Un account non è stato possibile eseguire il mapping per account di accesso.|  
|4776|680,681|Bassa|Il controller di dominio ha tentato di convalidare le credenziali per un account.|  
|4777|N/D|Bassa|Il controller di dominio non è stato possibile convalidare le credenziali per un account.|  
|4778|682|Bassa|Una sessione è stata riconnessa a una stazione di finestra.|  
|4779|683|Bassa|Una sessione è stata disconnessa da una postazione.|  
|4781|685|Bassa|È stato modificato il nome di un account:|  
|4782|N/D|Bassa|È stato eseguito l'hash della password un account.|  
|4783|667|Bassa|È stato creato un gruppo di applicazioni di base.|  
|4784|N/D|Bassa|Un gruppo di applicazioni di base è stato modificato.|  
|4785|689|Bassa|È stato aggiunto un membro a un gruppo di applicazioni di base.|  
|4786|690|Bassa|Un membro è stato rimosso da un gruppo di applicazioni di base.|  
|4787|691|Bassa|Una funzione non membro è stato aggiunto a un gruppo di applicazioni di base.|  
|4788|692|Bassa|Una funzione non membro è stato rimosso da un gruppo di applicazioni di base.|  
|4789|693|Bassa|Un gruppo di applicazioni di base è stato eliminato.|  
|4790|694|Bassa|È stato creato un gruppo di query LDAP.|  
|4793|N/D|Bassa|È stato chiamato l'API di controllo dei criteri Password.|  
|4800|N/D|Bassa|La workstation è stata bloccata.|  
|4801|N/D|Bassa|La workstation è stata sbloccata.|  
|4802|N/D|Bassa|È stato richiamato lo screen saver.|  
|4803|N/D|Bassa|Lo screen saver è stata chiusa.|  
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
|4880|784|Bassa|Servizi certificati è stato avviato.|  
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
|4895|799|Bassa|Servizi certificati ha pubblicato il certificato della CA in servizi di dominio Active Directory.|  
|4898|802|Bassa|Modello caricato da Servizi certificati.|  
|4902|N/D|Bassa|La tabella dei criteri di controllo Per utente è stata creata.|  
|4904|N/D|Bassa|È stato effettuato un tentativo per registrare un'origine evento di sicurezza.|  
|4905|N/D|Bassa|È stato effettuato un tentativo per annullare la registrazione di un'origine evento di sicurezza.|  
|4909|N/D|Bassa|Sono state modificate le impostazioni dei criteri locali per i TB.|  
|4910|N/D|Bassa|Sono state modificate le impostazioni di criteri di gruppo per i TB.|  
|4928|N/D|Bassa|È stato stabilito un contesto di denominazione origine di replica Active Directory.|  
|4929|N/D|Bassa|È stato rimosso un contesto di denominazione origine di replica Active Directory.|  
|4930|N/D|Bassa|Un contesto di denominazione origine di replica Active Directory è stato modificato.|  
|4931|N/D|Bassa|Un contesto dei nomi di destinazione Active Directory di replica è stato modificato.|  
|4932|N/D|Bassa|Sincronizzazione di una replica di un contesto di denominazione di Active Directory è stata avviata.|  
|4933|N/D|Bassa|Sincronizzazione di una replica di un contesto di denominazione di Active Directory è stata terminata.|  
|4934|N/D|Bassa|Attributi di un oggetto di Active Directory sono stati replicati.|  
|4935|N/D|Bassa|Errore di replica viene avviata.|  
|4936|N/D|Bassa|Errore di replica termina.|  
|4937|N/D|Bassa|Un oggetto residuo è stato rimosso da una replica.|  
|4944|N/D|Bassa|Il seguente criterio era attivo all'avvio di Windows Firewall.|  
|4945|N/D|Bassa|Una regola è stata elencata quando Windows Firewall avviato.|  
|4946|N/D|Bassa|È stata apportata una modifica all'elenco di eccezioni di Windows Firewall. È stata aggiunta una regola.|  
|4947|N/D|Bassa|È stata apportata una modifica all'elenco di eccezioni di Windows Firewall. È stata modificata una regola.|  
|4948|N/D|Bassa|È stata apportata una modifica all'elenco di eccezioni di Windows Firewall. È stata eliminata una regola.|  
|4949|N/D|Bassa|Le impostazioni del Firewall di Windows sono state ripristinate ai valori predefiniti.|  
|4950|N/D|Bassa|È stata modificata un'impostazione del Firewall di Windows.|  
|4951|N/D|Bassa|Una regola è stata ignorata perché il numero di versione principale non è stato riconosciuto da Windows Firewall.|  
|4952|N/D|Bassa|Parti di una regola sono state ignorate perché il numero di versione secondaria non è stato riconosciuto da Windows Firewall. Le altre parti della regola verranno applicate.|  
|4953|N/D|Bassa|Una regola è stata ignorata da Windows Firewall in quanto l'impossibilità di analizzare la regola.|  
|4954|N/D|Bassa|Impostazioni di criteri di gruppo di Windows Firewall sono state modificate. Sono state applicate le nuove impostazioni.|  
|4956|N/D|Bassa|Windows Firewall è stato modificato il profilo attivo.|  
|4957|N/D|Bassa|Windows Firewall è stata non applica la regola seguente:|  
|4958|N/D|Bassa|Windows Firewall è stata non applica la regola seguente poiché la regola di riferimento agli elementi non è configurati nel computer:|  
|4979|N/D|Bassa|Sono state fissate le associazioni di sicurezza IPsec in modalità principale o esteso.|  
|4980|N/D|Bassa|Sono state fissate le associazioni di sicurezza IPsec in modalità principale o esteso.|  
|4981|N/D|Bassa|Sono state fissate le associazioni di sicurezza IPsec in modalità principale o esteso.|  
|4982|N/D|Bassa|Sono state fissate le associazioni di sicurezza IPsec in modalità principale o esteso.|  
|4985|N/D|Bassa|È stato modificato lo stato di una transazione.|  
|5024|N/D|Bassa|Il servizio di Windows Firewall avviato correttamente.|  
|5025|N/D|Bassa|Il servizio Firewall di Windows è stato arrestato.|  
|5031|N/D|Bassa|Il servizio di Windows Firewall è bloccato da un'applicazione di accettare connessioni in ingresso nella rete.|  
|5032|N/D|Bassa|Windows Firewall è riuscito a notificare all'utente che bloccata un'applicazione di accettare connessioni in ingresso nella rete.|  
|5033|N/D|Bassa|Il Driver di Windows Firewall è stato avviato.|  
|5034|N/D|Bassa|Il Driver di Windows Firewall è stato arrestato.|  
|5039|N/D|Bassa|Una chiave del Registro di sistema è virtualizzata.|  
|5040|N/D|Bassa|È stata apportata una modifica alle impostazioni di IPsec. È stato aggiunto un Set di autenticazione.|  
|5041|N/D|Bassa|È stata apportata una modifica alle impostazioni di IPsec. Un Set di autenticazione è stato modificato.|  
|5042|N/D|Bassa|È stata apportata una modifica alle impostazioni di IPsec. Un Set di autenticazione è stato eliminato.|  
|5043|N/D|Bassa|È stata apportata una modifica alle impostazioni di IPsec. È stata aggiunta una regola di sicurezza della connessione.|  
|5044|N/D|Bassa|È stata apportata una modifica alle impostazioni di IPsec. Una regola di sicurezza di connessione è stata modificata.|  
|5045|N/D|Bassa|È stata apportata una modifica alle impostazioni di IPsec. Una regola di sicurezza di connessione è stata eliminata.|  
|5046|N/D|Bassa|È stata apportata una modifica alle impostazioni di IPsec. È stato aggiunto un Set di crittografia.|  
|5047|N/D|Bassa|È stata apportata una modifica alle impostazioni di IPsec. Un Set di crittografia è stato modificato.|  
|5048|N/D|Bassa|È stata apportata una modifica alle impostazioni di IPsec. Un Set di crittografia è stato eliminato.|  
|5050|N/D|Bassa|Un tentativo di disabilitare a livello di codice il Firewall di Windows tramite una chiamata a InetFwProfile.FirewallEnabled(False)|  
|5051|N/D|Bassa|Un file è stato virtualizzato.|  
|5056|N/D|Bassa|È stata eseguita una verifica automatica di crittografia.|  
|5057|N/D|Bassa|Non è riuscita un'operazione di primitiva di crittografia.|  
|5058|N/D|Bassa|Operazione di file di chiave.|  
|5059|N/D|Bassa|Operazione di migrazione da una chiave.|  
|5060|N/D|Bassa|Operazione di verifica non riuscita.|  
|5061|N/D|Bassa|Operazione di crittografia.|  
|5062|N/D|Bassa|Crittografia che automatica di test in modalità di kernel è stata eseguita.|  
|5063|N/D|Bassa|È stata tentata un'operazione di provider di crittografia.|  
|5064|N/D|Bassa|È stata tentata un'operazione di contesto di crittografia.|  
|5065|N/D|Bassa|È stata tentata una modifica del contesto di crittografia.|  
|5066|N/D|Bassa|È stata tentata un'operazione di funzione di crittografia.|  
|5067|N/D|Bassa|È stata tentata una modifica della funzione di crittografia.|  
|5068|N/D|Bassa|È stata tentata un'operazione di provider di funzionalità di crittografia.|  
|5069|N/D|Bassa|È stata tentata un'operazione di proprietà di funzione di crittografia.|  
|5070|N/D|Bassa|È stata tentata una modifica di proprietà di funzione di crittografia.|  
|5125|N/D|Bassa|È stata inviata una richiesta al servizio Risponditore OCSP|  
|5126|N/D|Bassa|Certificato di firma è stato aggiornato automaticamente dal servizio Risponditore OCSP|  
|5127|N/D|Bassa|Il Provider di revoca OCSP aggiornato correttamente le informazioni di revoca|  
|5136|566|Bassa|Un oggetto servizio directory è stato modificato.|  
|5137|566|Bassa|È stato creato un oggetto servizio di directory.|  
|5138|N/D|Bassa|Un oggetto servizio directory è stato annullato.|  
|5139|N/D|Bassa|Un oggetto servizio directory è stato spostato.|  
|5140|N/D|Bassa|È stato eseguito un oggetto condivisione di rete.|  
|5141|N/D|Bassa|Un oggetto servizio directory è stato eliminato.|  
|5152|N/D|Bassa|La piattaforma filtro Windows è bloccata da un pacchetto.|  
|5153|N/D|Bassa|Un filtro piattaforma filtro Windows più restrittivo ha bloccato un pacchetto.|  
|5154|N/D|Bassa|La piattaforma filtro Windows ha dato l'autorizzazione di un'applicazione o servizio in ascolto su una porta per le connessioni in ingresso.|  
|5155|N/D|Bassa|La piattaforma filtro Windows è bloccata un'applicazione o un servizio all'ascolto su una porta per le connessioni in ingresso.|  
|5156|N/D|Bassa|La piattaforma filtro Windows è consentita la connessione.|  
|5157|N/D|Bassa|La piattaforma filtro Windows ha bloccato una connessione.|  
|5158|N/D|Bassa|La piattaforma filtro Windows è consentito un binding a una porta locale.|  
|5159|N/D|Bassa|La piattaforma filtro Windows ha bloccato un binding a una porta locale.|  
|5378|N/D|Bassa|La delega di credenziali richiesto non è consentita dai criteri.|  
|5440|N/D|Bassa|Il callout seguente è presente all'avvio di Windows filtro piattaforma Base Filtering Engine.|  
|5441|N/D|Bassa|Il filtro seguente è presente all'avvio di Windows filtro piattaforma Base Filtering Engine.|  
|5442|N/D|Bassa|Il seguente provider non è presente all'avvio di Windows filtro piattaforma Base Filtering Engine.|  
|5443|N/D|Bassa|Il contesto del provider seguente è presente all'avvio di Windows filtro piattaforma Base Filtering Engine.|  
|5444|N/D|Bassa|Il seguente sottolivello era presente all'avvio di Windows filtro piattaforma Base Filtering Engine.|  
|5446|N/D|Bassa|Un callout piattaforma filtro Windows è stato modificato.|  
|5447|N/D|Bassa|Un filtro piattaforma filtro Windows è stato modificato.|  
|5448|N/D|Bassa|Un provider di piattaforma filtro Windows è stato modificato.|  
|5449|N/D|Bassa|Il contesto di un provider di piattaforma filtro Windows è stato modificato.|  
|5450|N/D|Bassa|Un sottolivello piattaforma filtro Windows è stato modificato.|  
|5451|N/D|Bassa|È stata stabilita un'associazione di sicurezza IPsec in modalità rapida.|  
|5452|N/D|Bassa|Un'associazione di sicurezza di modalità rapida IPsec è terminata.|  
|5456|N/D|Bassa|Motore PAStore applicati criteri IPsec di archiviazione di Active Directory nel computer.|  
|5457|N/D|Bassa|Motore PAStore non è riuscito ad applicare i criteri IPsec di archiviazione di Active Directory nel computer.|  
|5458|N/D|Bassa|Motore PAStore applicati copia memorizzato nella cache locale del criterio IPsec di archiviazione di Active Directory nel computer.|  
|5459|N/D|Bassa|Motore PAStore non è riuscito ad applicare una copia della cache locale del criterio IPsec di archiviazione di Active Directory nel computer.|  
|5460|N/D|Bassa|Motore PAStore applicati i criteri IPsec di archiviazione del Registro di sistema locale nel computer.|  
|5461|N/D|Bassa|Motore PAStore non è stato possibile applicare i criteri IPsec di archiviazione del Registro di sistema locale nel computer.|  
|5462|N/D|Bassa|Motore PAStore non è riuscito ad applicare alcune regole dei criteri IPsec attivo nel computer. Utilizzare lo snap-in Monitor di sicurezza IP per la diagnosi del problema.|  
|5463|N/D|Bassa|Motore PAStore eseguito il polling delle modifiche per il criterio IPsec e non rilevata nessuna modifica.|  
|5464|N/D|Bassa|Motore PAStore eseguito il polling delle modifiche per il criterio IPsec, ha rilevato alcune modifiche e sono state applicate per i servizi di IPsec.|  
|5465|N/D|Bassa|Motore PAStore ha ricevuto un controllo per forzare il caricamento dei criteri IPsec ed elaborato correttamente il controllo.|  
|5466|N/D|Bassa|Motore PAStore eseguito il polling delle modifiche per il criterio IPsec Active Directory, determinato che non può essere raggiunto Active Directory e userà invece la copia memorizzata nella cache del criterio IPsec di Active Directory. Modifiche apportate ai criteri IPsec di Active Directory perché non è possibile applicare l'ultimo polling.|  
|5467|N/D|Bassa|Motore PAStore eseguito il polling delle modifiche per il criterio IPsec Active Directory, determinato che Active Directory può essere raggiunto e non trovare alcuna modifica ai criteri. La copia memorizzata nella cache del criterio IPsec di Active Directory non è più utilizzata.|  
|5468|N/D|Bassa|Motore PAStore eseguito il polling delle modifiche per il criterio IPsec Active Directory, determinato che Active Directory può essere raggiunto, trovare le modifiche ai criteri e applicare tali modifiche. La copia memorizzata nella cache del criterio IPsec di Active Directory non è più utilizzata.|  
|5471|N/D|Bassa|Motore PAStore caricato archiviazione locale dei criteri IPsec nel computer.|  
|5472|N/D|Bassa|Motore PAStore non è riuscito a caricare una risorsa di archiviazione locale dei criteri IPsec nel computer.|  
|5473|N/D|Bassa|Motore PAStore caricato archiviazione directory criteri IPsec nel computer.|  
|5474|N/D|Bassa|Motore PAStore non è stato possibile caricare i criteri IPsec nel computer di archiviazione di directory.|  
|5477|N/D|Bassa|Motore PAStore non è stato possibile aggiungere il filtro di modalità rapida.|  
|5479|N/D|Bassa|I servizi di IPsec è stato arrestato correttamente. L'arresto dei servizi di IPsec può mettere il computer a maggiore rischio di attacco di rete o esporre il computer da potenziali rischi di sicurezza.|  
|5632|N/D|Bassa|È stata effettuata una richiesta per l'autenticazione a una rete wireless.|  
|5633|N/D|Bassa|È stata effettuata una richiesta per l'autenticazione a una rete cablata.|  
|5712|N/D|Bassa|È stata tentata una chiamata RPC (Remote Procedure).|  
|5888|N/D|Bassa|È stato modificato un oggetto nel catalogo COM+.|  
|5889|N/D|Bassa|Un oggetto è stato eliminato dal catalogo COM+.|  
|5890|N/D|Bassa|Un oggetto è stato aggiunto al catalogo COM+.|  
|6008|N/D|Bassa|Arresto del sistema precedente non era previsto|  
|6144|N/D|Bassa|Criteri di sicurezza in oggetti Criteri di gruppo siano stati applicati correttamente.|  
|6272|N/D|Bassa|Server dei criteri di rete concedere l'accesso a un utente.|  
|N/D|561|Bassa|È stato richiesto un handle a un oggetto.|  
|N/D|563|Bassa|Oggetto aperto per l'eliminazione|  
|N/D|625|Bassa|Tipo di Account utente modificata|  
|N/D|613|Bassa|Agente criteri IPsec avviato|  
|N/D|614|Bassa|Agente criteri IPsec disabilitato|  
|N/D|615|Bassa|Agente criteri IPsec|  
|N/D|616|Bassa|Agente criteri IPsec ha rilevato un errore grave potenziali|  
|24577|N/D|Bassa|Crittografia del volume di avvio|  
|24578|N/D|Bassa|Crittografia del volume arrestato|  
|24579|N/D|Bassa|Crittografia del volume completata|  
|24580|N/D|Bassa|Decrittografia del volume di avvio|  
|24581|N/D|Bassa|Decrittografia del volume arrestato|  
|24582|N/D|Bassa|Decrittografia del volume completata|  
|24583|N/D|Bassa|Thread di lavoro di conversione per volume di avvio|  
|24584|N/D|Bassa|Thread di lavoro di conversione per volume temporaneamente interrotta|  
|24588|N/D|Bassa|L'operazione di conversione nel volume %2 ha rilevato un errore di settore danneggiato. Verificare che i dati in questo volume|  
|24595|N/D|Bassa|Volume %2 contiene cluster danneggiati. Questi cluster verranno ignorati durante la conversione.|  
|24621|N/D|Bassa|Controllo dello stato iniziale: Rollback delle transazioni di conversione di volume in %2.|  
|5049|N/D|Bassa|È stata eliminata un'associazione di sicurezza IPsec.|  
|5478|N/D|Bassa|I servizi di IPsec è stato avviato.|  
  
> [!NOTE]  
> Fare riferimento a [eventi di controllo di sicurezza di Windows](https://www.microsoft.com/download/details.aspx?id=50034) per un elenco di molti ID evento di sicurezza e i relativi significati.  
>
> Eseguire **wevtutil criteri di gruppo Microsoft-Windows-Security-Auditing /ge /gm:true** per ottenere un elenco molto dettagliato della sicurezza di tutti gli ID evento  
  
Per altre informazioni sull'ID evento di sicurezza di Windows e i relativi significati, vedere l'articolo di supporto tecnico Microsoft [descrizione degli eventi di sicurezza in Windows 7 e Windows Server 2008 R2](https://support.microsoft.com/kb/977519). È anche possibile scaricare [sicurezza controllo eventi per Windows 7 e Windows Server 2008 R2](https://www.microsoft.com/download/details.aspx?id=21561) e [dettagli sull'evento di sicurezza di Windows Server 2012 e Windows 8](https://www.microsoft.com/download/details.aspx?id=35753), che forniscono eventi informazioni dettagliate per la sistemi operativi cui viene fatto riferimento in formato di foglio di calcolo.  
