---
ms.assetid: 99a68050-8d19-4c58-ad86-e08a3dcdb4f7
title: Appendice L - eventi da monitorare
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9d153a16b31070f2bbfac4a47a814792ef66b472
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="appendix-l-events-to-monitor"></a>Appendice l: eventi da monitorare

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

  
## <a name="appendix-l-events-to-monitor"></a>Appendice l: eventi da monitorare  
Nella tabella seguente elenca gli eventi da monitorare nel proprio ambiente, in base ai consigli forniti in [monitoraggio di Active Directory per i segni di compromissione](../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md). Nella tabella seguente, la colonna "ID evento Windows corrente" Elenca l'ID evento quando viene implementato nelle versioni di Windows e Windows Server che sono attualmente coperte dal supporto mainstream.  
  
La colonna "ID evento Windows Legacy" Elenca l'ID evento corrispondente nelle versioni precedenti di Windows, ad esempio i computer client che eseguono Windows XP o versioni precedenti e i server che esegue Windows Server 2003 o versioni precedenti. Il "criticità potenziale livello" colonna identifica se l'evento deve essere considerato di basso, medio o alta criticità di rilevamento di attacchi e la colonna "Riepilogo evento" fornisce una breve descrizione dell'evento.  
  
Una criticità potenziale di alto livello significa che uno occorrenza dell'evento dovrebbe essere esaminata. Criticità potenziale di medio o basso indica che questi eventi devono essere esaminati solo se si verificano in modo imprevisto o numeri che superano significativamente la base prevista in un periodo di tempo. Tutte le organizzazioni devono testare queste raccomandazioni nei propri ambienti prima di creare avvisi che richiedono risposte indagine obbligatorie. Ogni ambiente è diverso e alcuni degli eventi classificati con una criticità potenziale di alto livello potrebbe verificarsi a causa di altri eventi innocui.  
  
|||||  
|-|-|-|-|  
|**ID evento Windows corrente**|**ID evento Windows legacy**|**Criticità potenziale**|**Riepilogo evento**|  
|4618|N/D|Elevata|Si è verificato un modello di eventi di sicurezza monitorati.|  
|4649|N/D|Elevata|È stato rilevato un attacco di tipo replay. Potrebbe essere un numero positivo false innocuo a causa di errore di configurazione errata.|  
|4719|612|Elevata|Criteri di controllo di sistema è stato modificato.|  
|4765|N/D|Elevata|History SID è stato aggiunto a un account.|  
|4766|N/D|Elevata|Impossibile aggiungere SID History a un account.|  
|4794|N/D|Elevata|È stato effettuato un tentativo di impostare la modalità ripristino servizi Directory.|  
|4897|801|Elevata|Separazione ruoli abilitata:|  
|4964|N/D|Elevata|Gruppi speciali sono stati assegnati a un nuovo accesso.|  
|5124|N/D|Elevata|Un'impostazione di sicurezza sono stata aggiornata il servizio Risponditore OCSP|  
|N/D|550|Medio-alto|Attacco possibili attacchi denial of service (DoS)|  
|1102|517|Medio-alto|Il Registro di controllo è stato cancellato|  
|4621|N/D|Media|Amministratore ha ripristinato il sistema da CrashOnAuditFail. Gli utenti che non sono amministratori ora possono effettuare l'accesso. Alcune attività controllabili potrebbe non essere state registrate.|  
|4675|N/D|Media|Sono stati filtrati SID.|  
|4692|N/D|Media|Backup della chiave master di protezione dati è stato tentato.|  
|4693|N/D|Media|Tentativo di ripristino della chiave master di protezione dati.|  
|4706|610|Media|A un dominio è stato creato un nuovo trust.|  
|4713|617|Media|Criteri Kerberos è stato modificato.|  
|4714|618|Media|È stato modificato il criterio di recupero dati crittografati.|  
|4715|N/D|Media|I criteri di controllo (SACL) su un oggetto è stato modificato.|  
|4716|620|Media|Modifica delle informazioni sul dominio trusted.|  
|4724|628|Media|È stato effettuato un tentativo di reimpostare la password dell'account.|  
|4727|631|Media|È stato creato un gruppo globale protetto.|  
|4735|639|Media|Un gruppo locale protetto è stato modificato.|  
|4737|641|Media|Un gruppo globale protetto è stato modificato.|  
|4739|643|Media|Criteri di dominio è stato modificato.|  
|4754|658|Media|È stato creato un gruppo universale protetto.|  
|4755|659|Media|Un gruppo universale protetto è stato modificato.|  
|4764|667|Media|Eliminazione di un gruppo di protezione disabilitata|  
|4764|668|Media|Tipo di un gruppo è stato modificato.|  
|4780|684|Media|L'ACL è stato impostato per gli account che sono membri del gruppo administrators.|  
|4816|N/D|Media|RPC ha rilevato una violazione di integrità durante la decrittografia di un messaggio in arrivo.|  
|4865|N/D|Media|È stata aggiunta una voce di informazioni foresta trusted.|  
|4866|N/D|Media|È stata rimossa una voce di informazioni foresta trusted.|  
|4867|N/D|Media|Una voce di informazioni foresta trusted è stata modificata.|  
|4868|772|Media|Il gestore di certificati ha negato una richiesta di certificato in sospeso.|  
|4870|774|Media|Servizi certificati ha revocato un certificato.|  
|4882|786|Media|Le autorizzazioni di sicurezza per i servizi certificati è stato modificato.|  
|4885|789|Media|Il filtro di controllo per Servizi certificati è stato modificato.|  
|4890|794|Media|Le impostazioni di gestione di certificati per Servizi certificati è stato modificato.|  
|4892|796|Media|Una proprietà di servizi certificati è cambiata.|  
|4896|800|Media|Uno o più righe sono state eliminate dal database dei certificati.|  
|4906|N/D|Media|È stato modificato il valore CrashOnAuditFail.|  
|4907|N/D|Media|Le impostazioni di controllo sull'oggetto sono state modificate.|  
|4908|N/D|Media|Tabella accesso gruppi speciale modificata.|  
|4912|807|Media|Per ogni criterio di controllo utente è stato modificato.|  
|4960|N/D|Media|IPsec rilasciato un pacchetto in ingresso che non hanno un controllo di integrità. Se il problema persiste, potrebbe indicare che un problema di rete o che i pacchetti vengono modificate in transito verso questo computer. Verificare che i pacchetti inviati dal computer remoto siano gli stessi di quelli ricevuti da questo computer. Questo errore potrebbe inoltre indicare problemi di interoperabilità con altre implementazioni IPsec.|  
|4961|N/D|Media|IPsec rilasciato un pacchetto in ingresso che non hanno un controllo di riproduzione. Se il problema persiste, potrebbe indicare un attacco di riproduzione in questo computer.|  
|4962|N/D|Media|IPsec rilasciato un pacchetto in ingresso che non hanno un controllo di riproduzione. Il pacchetto in ingresso era troppo basso un numero di sequenza per assicurarsi che non è una riproduzione.|  
|4963|N/D|Media|IPsec rilasciato un pacchetto in entrata come testo non crittografato che avrebbero dovuto essere protetti. Si tratta in genere a causa del computer remoto, la modifica dei criteri IPsec senza informare questo computer. Potrebbe anche trattarsi di un tentativo di attacco di spoofing.|  
|4965|N/D|Media|IPsec ha ricevuto un pacchetto da un computer remoto con indice di un parametro di protezione errato (SPI). Questo è in genere causato da problemi di hardware che danneggia i pacchetti. Se questi errori persistono, verificare che i pacchetti inviati dal computer remoto siano gli stessi di quelli ricevuti da questo computer. Questo errore può anche indicare problemi di interoperabilità con altre implementazioni IPsec. In tal caso, se non è ostacolata connettività, quindi questi eventi possono essere ignorati.|  
|4976|N/D|Media|Durante la negoziazione in modalità principale, IPsec ha ricevuto un pacchetto di negoziazione non valido. Se il problema persiste, potrebbe indicare un problema di rete o un tentativo di modificare o la negoziazione di tipo replay.|  
|4977|N/D|Media|Durante la negoziazione in modalità rapida IPsec ha ricevuto un pacchetto di negoziazione non valido. Se il problema persiste, potrebbe indicare un problema di rete o un tentativo di modificare o la negoziazione di tipo replay.|  
|4978|N/D|Media|Durante la negoziazione in modalità estesa IPsec ha ricevuto un pacchetto di negoziazione non valido. Se il problema persiste, potrebbe indicare un problema di rete o un tentativo di modificare o la negoziazione di tipo replay.|  
|4983|N/D|Media|Negoziazione in modalità estesa IPsec non riuscita. L'associazione di sicurezza in modalità principale corrispondente è stato eliminato.|  
|4984|N/D|Media|Negoziazione in modalità estesa IPsec non riuscita. L'associazione di sicurezza in modalità principale corrispondente è stato eliminato.|  
|5027|N/D|Media|Il servizio Windows Firewall non è riuscito a recuperare i criteri di sicurezza dalla risorsa di archiviazione locale. Il servizio continuerà a imporre i criteri correnti.|  
|5028|N/D|Media|Il servizio Windows Firewall è stato in grado di analizzare il nuovo criterio di sicurezza. Il servizio continuerà con attualmente applicati i criteri.|  
|5029|N/D|Media|Il servizio Windows Firewall non è riuscito a inizializzare il driver. Il servizio continuerà ad applicare i criteri correnti.|  
|5030|N/D|Media|Impossibile avviare il servizio Windows Firewall.|  
|5035|N/D|Media|Impossibile avviare il Driver di Windows Firewall.|  
|5037|N/D|Media|Il Driver di Windows Firewall rilevato errore critico durante l'esecuzione. In fase di arresto.|  
|5038|N/D|Media|Integrità del codice determinato che l'immagine hash di un file non è valido. Il file potrebbe essere danneggiato a causa di modifiche non autorizzate o l'hash non valido potrebbe indicare un errore del dispositivo disco potenziali.|  
|5120|N/D|Media|Servizio Risponditore OCSP avviato|  
|5121|N/D|Media|Servizio Risponditore OCSP arrestato|  
|5122|N/D|Media|Una voce di configurazione modificata nel servizio Risponditore OCSP|  
|5123|N/D|Media|Una voce di configurazione modificata nel servizio Risponditore OCSP|  
|5376|N/D|Media|Le credenziali di gestione credenziali è sono eseguito il backup.|  
|5377|N/D|Media|Gestione credenziali le credenziali sono state ripristinate da un backup.|  
|5453|N/D|Media|Negoziazione IPsec con un computer remoto non è riuscito perché non è stato avviato il servizio IKE e AuthIP IPsec trasparenza moduli (IKEEXT).|  
|5480|N/D|Media|Servizi IPsec non è riuscito a ottenere l'elenco completo delle interfacce di rete nel computer. Questo implica un potenziale rischio per la sicurezza perché alcune delle interfacce di rete potrebbero non fornire la protezione fornita dai filtri IPsec applicati. Utilizzare lo snap-in Monitor di sicurezza IP per isolare il problema.|  
|5483|N/D|Media|Servizi IPsec non è riuscito a inizializzare il server RPC. Impossibile avviare servizi IPsec.|  
|5484|N/D|Media|IPsec servizi ha rilevato un errore critico ed è stato arrestato. L'arresto dei servizi IPsec può esporre il computer a maggiore rischio di attacchi di rete o esporre il computer a potenziali rischi di sicurezza.|  
|5485|N/D|Media|Servizi IPsec non è riuscito a elaborare alcuni filtri IPsec in un evento plug and play per interfacce di rete. Questo implica un potenziale rischio per la sicurezza perché alcune delle interfacce di rete potrebbero non fornire la protezione fornita dai filtri IPsec applicati. Utilizzare lo snap-in Monitor di sicurezza IP per isolare il problema.|  
|6145|N/D|Media|Si è verificato uno o più errori durante l'elaborazione di criteri di sicurezza in oggetti Criteri di gruppo.|  
|6273|N/D|Media|Server dei criteri di rete negato l'accesso a un utente.|  
|6274|N/D|Media|Server dei criteri di rete scartati la richiesta per un utente.|  
|6275|N/D|Media|Server dei criteri di rete scartati la richiesta di accounting per un utente.|  
|6276|N/D|Media|Server dei criteri di rete in quarantena un utente.|  
|6277|N/D|Media|Server dei criteri di rete concesso l'accesso a un utente ma metterlo in prova, perché l'host non soddisfa i criteri di integrità definiti.|  
|6278|N/D|Media|Server dei criteri di rete concesso l'accesso completo a un utente poiché l'host soddisfatti i criteri di integrità definiti.|  
|6279|N/D|Media|Server dei criteri di rete bloccato l'account utente a causa di ripetuti tentativi di autenticazione falliti.|  
|6280|N/D|Media|Server dei criteri di rete sbloccare l'account utente.|  
|-|640|Media|Database di account generale modificata|  
|-|619|Media|Qualità del servizio criterio modificato|  
|24586|N/D|Media|Errore durante la conversione di volume|  
|24592|N/D|Media|Un tentativo di riavvio automatico nel volume %2 conversione non riuscita.|  
|24593|N/D|Media|Scrittura di metadati: errori di ritorno Volume %2 durante il tentativo di modificare i metadati. Se l'errore persiste, decrittografare volume|  
|24594|N/D|Media|Ricostruzione dei metadati: tentativo di scrivere una copia dei metadati nel volume %2 non è riuscita e potrebbe essere visualizzato come disco rigido è danneggiato. Se l'errore persiste, decrittografare il volume.|  
|4608|512|Basso|Dell'avvio di Windows.|  
|4609|513|Basso|Windows è in corso l'arresto.|  
|4610|514|Basso|Un pacchetto di autenticazione è stato caricato dall'autorità di sicurezza locale.|  
|4611|515|Basso|Un processo di accesso attendibile è stato registrato con l'autorità di sicurezza locale.|  
|4612|516|Basso|Le risorse interne allocate per l'accodamento dei messaggi di controllo sono state esaurite, causando la perdita di alcuni controlli.|  
|4614|518|Basso|Un pacchetto di notifica caricato da Gestione Account di protezione.|  
|4615|519|Basso|Utilizzo non valido della porta LPC.|  
|4616|520|Basso|L'ora di sistema è stato modificato.|  
|4622|N/D|Basso|Un pacchetto di protezione è stato caricato dall'autorità di sicurezza locale.|  
|4624|528,540|Basso|Un account è stato eseguito l'accesso.|  
|4625|529-537,539|Basso|Un account di accesso non riuscito.|  
|4634|538|Basso|Un account è stato disconnesso.|  
|4646|N/D|Basso|Modalità di prevenzione DoS IKE avviata.|  
|4647|551|Basso|Disconnessione avviata dall'utente.|  
|4648|552|Basso|Si è tentato un accesso utilizzando credenziali esplicite.|  
|4650|N/D|Basso|È stata stabilita un'associazione di sicurezza in modalità principale IPsec. Modalità estesa non è stata abilitata. Non è stata utilizzata l'autenticazione del certificato.|  
|4651|N/D|Basso|È stata stabilita un'associazione di sicurezza in modalità principale IPsec. Modalità estesa non è stata abilitata. È stato utilizzato un certificato per l'autenticazione.|  
|4652|N/D|Basso|Negoziazione in modalità principale IPsec non riuscita.|  
|4653|N/D|Basso|Negoziazione in modalità principale IPsec non riuscita.|  
|4654|N/D|Basso|Negoziazione in modalità rapida IPsec non riuscita.|  
|4655|N/D|Basso|Un'associazione di sicurezza in modalità principale IPsec è terminato.|  
|4656|560|Basso|È stato richiesto un handle per un oggetto.|  
|4657|567|Basso|Un valore del Registro di sistema è stato modificato.|  
|4658|562|Basso|L'handle per un oggetto è stata chiusa.|  
|4659|N/D|Basso|È stato richiesto un handle per un oggetto con l'intento di eliminare.|  
|4660|564|Basso|Un oggetto è stato eliminato.|  
|4661|565|Basso|È stato richiesto un handle per un oggetto.|  
|4662|566|Basso|È stata eseguita un'operazione su un oggetto.|  
|4663|567|Basso|È stato effettuato un tentativo di accedere a un oggetto.|  
|4664|N/D|Basso|È stato effettuato un tentativo di creare un collegamento fisso.|  
|4665|N/D|Basso|È stato effettuato un tentativo di creare un contesto client dell'applicazione.|  
|4666|N/D|Basso|Un'applicazione tentato di eseguire un'operazione:|  
|4667|N/D|Basso|È stato eliminato un contesto client dell'applicazione.|  
|4668|N/D|Basso|Un'applicazione è stata inizializzata.|  
|4670|N/D|Basso|Sono state modificate le autorizzazioni per un oggetto.|  
|4671|N/D|Basso|Un'applicazione ha tentata di accedere a un numero ordinale bloccato tramite TBS.|  
|4672|576|Basso|Privilegi speciali assegnati al nuovo accesso.|  
|4673|577|Basso|Un servizio privilegiato chiamato.|  
|4674|578|Basso|È stata tentata un'operazione su un oggetto con privilegiato.|  
|4688|592|Basso|È stato creato un nuovo processo.|  
|4689|593|Basso|Un processo è terminato.|  
|4690|594|Basso|È stato effettuato un tentativo di duplicare un handle per un oggetto.|  
|4691|595|Basso|È stato richiesto l'accesso indiretto a un oggetto.|  
|4694|N/D|Basso|È stata tentata la protezione dei dati protetti controllabili.|  
|4695|N/D|Basso|È stata tentata unprotection dei dati protetti controllabili.|  
|4696|600|Basso|Un token principale è stato assegnato a un processo.|  
|4697|601|Basso|Tentativo di installare un servizio|  
|4698|602|Basso|È stata creata un'attività pianificata.|  
|4699|602|Basso|Un'attività pianificata è stata eliminata.|  
|4700|602|Basso|Un'attività pianificata è stata abilitata.|  
|4701|602|Basso|Un'attività pianificata è stata disabilitata.|  
|4702|602|Basso|Un'attività pianificata è stata aggiornata.|  
|4704|608|Basso|È stato assegnato un diritto utente.|  
|4705|609|Basso|È stato rimosso un diritto utente.|  
|4707|611|Basso|Una relazione di trust a un dominio è stato rimosso.|  
|4709|N/D|Basso|Servizi IPsec è stato avviato.|  
|4710|N/D|Basso|Servizi IPsec è stato disabilitato.|  
|4711|N/D|Basso|Può contenere uno o più delle operazioni seguenti: PAStore Engine ha applicato copia memorizzata nella cache locale del criterio IPsec di archiviazione di Active Directory nel computer. Motore PAStore applicato il criterio IPsec di archiviazione di Active Directory nel computer. Motore PAStore applicato il criterio IPsec di archiviazione del Registro di sistema locale nel computer. PAStore Engine non è riuscito ad applicare una copia memorizzata nella cache locale del criterio IPsec di archiviazione di Active Directory nel computer. PAStore Engine non riesce ad applicare il criterio IPsec di archiviazione di Active Directory nel computer. Motore PAStore Impossibile applicare il criterio IPsec di archiviazione del Registro di sistema locale nel computer. PAStore Engine non è riuscito ad applicare alcune regole del criterio IPsec attivo nel computer. PAStore Engine non è riuscito a caricare l'archivio directory criteri IPsec nel computer. Motore PAStore caricato archiviazione directory criteri IPsec nel computer. PAStore Engine non è riuscito a caricare l'archiviazione locale dei criteri IPsec nel computer. PAStore Engine ha caricato il criterio IPsec nel computer di archiviazione locale. Motore PAStore eseguito il polling delle modifiche per il criterio IPsec e non rilevata nessuna modifica.|  
|4712|N/D|Basso|IPsec si è verificato un errore potenzialmente grave.|  
|4717|621|Basso|È stato concesso l'accesso di protezione sistema a un account.|  
|4718|622|Basso|Accesso di protezione sistema è stato rimosso da un account.|  
|4720|624|Basso|È stato creato un account utente.|  
|4722|626|Basso|Un account utente è stato abilitato.|  
|4723|627|Basso|È stato effettuato un tentativo di modificare la password dell'account.|  
|4725|629|Basso|Un account utente è stato disabilitato.|  
|4726|630|Basso|Un account utente è stato eliminato.|  
|4728|632|Basso|Aggiunta di un membro a un gruppo globale protetto.|  
|4729|633|Basso|È stato rimosso un membro da un gruppo globale protetto.|  
|4730|634|Basso|Un gruppo globale protetto è stato eliminato.|  
|4731|635|Basso|È stato creato un gruppo locale protetto.|  
|4732|636|Basso|Aggiunta di un membro a un gruppo locale protetto.|  
|4733|637|Basso|È stato rimosso un membro da un gruppo locale protetto.|  
|4734|638|Basso|Un gruppo locale protetto è stato eliminato.|  
|4738|642|Basso|Un account utente è stato modificato.|  
|4740|644|Basso|Un account utente è stato bloccato.|  
|4741|645|Basso|È stato modificato un account computer.|  
|4742|646|Basso|È stato modificato un account computer.|  
|4743|647|Basso|Un account computer è stato eliminato.|  
|4744|648|Basso|È stato creato un gruppo locale disabilitata.|  
|4745|649|Basso|Modifica di un gruppo locale disabilitata.|  
|4746|650|Basso|Aggiunta di un membro a un gruppo locale disabilitata.|  
|4747|651|Basso|È stato rimosso un membro da un gruppo locale disabilitata.|  
|4748|652|Basso|È stato eliminato un gruppo locale disabilitata.|  
|4749|653|Basso|È stato creato un gruppo globale con sicurezza disattivata.|  
|4750|654|Basso|Un gruppo globale con sicurezza disattivata è stato modificato.|  
|4751|655|Basso|Aggiunta di un membro a un gruppo globale con sicurezza disattivata.|  
|4752|656|Basso|È stato rimosso un membro da un gruppo globale con sicurezza disattivata.|  
|4753|657|Basso|Un gruppo globale con sicurezza disattivata è stato eliminato.|  
|4756|660|Basso|Aggiunta di un membro a un gruppo universale protetto.|  
|4757|661|Basso|È stato rimosso un membro da un gruppo universale protetto.|  
|4758|662|Basso|Un gruppo universale protetto è stato eliminato.|  
|4759|663|Basso|È stato creato un gruppo universale protetto.|  
|4760|664|Basso|Un gruppo universale protetto è stato modificato.|  
|4761|665|Basso|Aggiunta di un membro a un gruppo universale protetto.|  
|4762|666|Basso|È stato rimosso un membro da un gruppo universale protetto.|  
|4767|671|Basso|Un account utente è stato sbloccato.|  
|4768|672,676|Basso|È stato richiesto un ticket di autenticazione Kerberos (TGT).|  
|4769|673|Basso|È stato richiesto un ticket di servizio Kerberos.|  
|4770|674|Basso|È stato rinnovato un ticket di servizio Kerberos.|  
|4771|675|Basso|Preautenticazione Kerberos non riuscita.|  
|4772|672|Basso|Una richiesta di ticket di autenticazione Kerberos non riuscita.|  
|4774|678|Basso|È stato eseguito il mapping di un account per l'accesso.|  
|4775|679|Basso|Un account potrebbe non essere mappato per l'accesso.|  
|4776|680,681|Basso|Il controller di dominio ha tentato di convalidare le credenziali per un account.|  
|4777|N/D|Basso|Il controller di dominio non è riuscito a convalidare le credenziali per un account.|  
|4778|682|Basso|Una sessione è stata riconnessa a una stazione di finestra.|  
|4779|683|Basso|Una sessione disconnessa da una postazione.|  
|4781|685|Basso|È stato modificato il nome di un account:|  
|4782|N/D|Basso|L'hash della password un account è stato eseguito.|  
|4783|667|Basso|È stato creato un gruppo di applicazioni di base.|  
|4784|N/D|Basso|Modifica di un gruppo di applicazioni di base.|  
|4785|689|Basso|Aggiunta di un membro a un gruppo di applicazioni di base.|  
|4786|690|Basso|È stato rimosso un membro da un gruppo di applicazioni di base.|  
|4787|691|Basso|È stato aggiunto non di un membro a un gruppo di applicazioni di base.|  
|4788|692|Basso|Non una membro è stato rimosso da un gruppo di applicazioni di base.|  
|4789|693|Basso|Eliminazione di un gruppo di applicazioni di base.|  
|4790|694|Basso|È stato creato un gruppo di query LDAP.|  
|4793|N/D|Basso|È stato chiamato l'API di verifica dei criteri Password.|  
|4800|N/D|Basso|La workstation è stata bloccata.|  
|4801|N/D|Basso|La workstation non è sbloccata.|  
|4802|N/D|Basso|È stato richiamato lo screen saver.|  
|4803|N/D|Basso|È stata chiusa lo screen saver.|  
|4864|N/D|Basso|È stato rilevato un conflitto di nomi.|  
|4869|773|Basso|Servizi certificati ha ricevuto una richiesta di certificato inviata di nuovo.|  
|4871|775|Basso|Servizi certificati ha ricevuto una richiesta di pubblicare l'elenco di revoche di certificati (CRL).|  
|4872|776|Basso|Servizi certificati ha pubblicato l'elenco di revoche di certificati (CRL).|  
|4873|777|Basso|Estensione di una richiesta di certificato è stato modificato.|  
|4874|778|Basso|Uno o più attributi della richiesta di certificato è stato modificato.|  
|4875|779|Basso|Servizi certificati ha ricevuto una richiesta di chiusura.|  
|4876|780|Basso|Backup di servizi certificati è stato avviato.|  
|4877|781|Basso|Completato il backup di servizi certificati.|  
|4878|782|Basso|Ripristino di servizi certificati è stato avviato.|  
|4879|783|Basso|Certificato completato il ripristino di servizi.|  
|4880|784|Basso|Servizi certificati è stato avviato.|  
|4881|785|Basso|Servizi certificati è stato arrestato.|  
|4883|787|Basso|Servizi certificati ha recuperato una chiave archiviata.|  
|4884|788|Basso|Servizi certificati ha importato un certificato nel database.|  
|4886|790|Basso|Servizi certificati ha ricevuto una richiesta di certificato.|  
|4887|791|Basso|Servizi certificati approvato una richiesta di certificato e un certificato emesso.|  
|4888|792|Basso|Servizi certificati ha negato una richiesta di certificato.|  
|4889|793|Basso|Servizi certificati ha impostato lo stato di una richiesta di certificato in sospeso.|  
|4891|795|Basso|Una voce di configurazione modificata in Servizi certificati.|  
|4893|797|Basso|Servizi certificati ha archiviato una chiave.|  
|4894|798|Basso|Servizi certificati importato e archiviato una chiave.|  
|4895|799|Basso|Servizi certificati ha pubblicato il certificato della CA di servizi di dominio Active Directory.|  
|4898|802|Basso|Modello caricato da Servizi certificati.|  
|4902|N/D|Basso|La tabella dei criteri di controllo Per utente è stata creata.|  
|4904|N/D|Basso|È stato effettuato un tentativo di registrare un'origine di eventi di sicurezza.|  
|4905|N/D|Basso|È stato effettuato un tentativo di annullare la registrazione di un'origine di eventi di sicurezza.|  
|4909|N/D|Basso|Impostazioni dei criteri locali per il servizio sono state modificate.|  
|4910|N/D|Basso|Sono state modificate le impostazioni di criteri di gruppo per il servizio.|  
|4928|N/D|Basso|È stato stabilito un contesto di denominazione origine di replica Active Directory.|  
|4929|N/D|Basso|È stato rimosso un contesto di denominazione origine di replica Active Directory.|  
|4930|N/D|Basso|Un contesto di denominazione origine di replica Active Directory è stato modificato.|  
|4931|N/D|Basso|Un contesto di denominazione destinazione di replica Active Directory è stato modificato.|  
|4932|N/D|Basso|Sincronizzazione di una replica di Active Directory ha iniziato a un contesto dei nomi.|  
|4933|N/D|Basso|Sincronizzazione di una replica di un contesto dei nomi di Active Directory è terminata.|  
|4934|N/D|Basso|Gli attributi di un oggetto di Active Directory sono stati replicati.|  
|4935|N/D|Basso|Errore di replica inizia.|  
|4936|N/D|Basso|Errore di replica termina.|  
|4937|N/D|Basso|Un oggetto residuo è stato rimosso da una replica.|  
|4944|N/D|Basso|Il seguente criterio era attivo all'avvio di Windows Firewall.|  
|4945|N/D|Basso|Una regola è stata elencata all'avvio di Windows Firewall.|  
|4946|N/D|Basso|È stata apportata una modifica all'elenco delle eccezioni di Windows Firewall. È stata aggiunta una regola.|  
|4947|N/D|Basso|È stata apportata una modifica all'elenco delle eccezioni di Windows Firewall. Una regola è stata modificata.|  
|4948|N/D|Basso|È stata apportata una modifica all'elenco delle eccezioni di Windows Firewall. Una regola è stata eliminata.|  
|4949|N/D|Basso|Impostazioni di Windows Firewall sono state ripristinate i valori predefiniti.|  
|4950|N/D|Basso|Un'impostazione di Windows Firewall è stato modificato.|  
|4951|N/D|Basso|Una regola è stata ignorata perché il numero di versione principale non è stato riconosciuto da Windows Firewall.|  
|4952|N/D|Basso|Parti di una regola sono state ignorate perché il numero di versione secondaria non è stato riconosciuto da Windows Firewall. Verranno applicate le altre parti della regola.|  
|4953|N/D|Basso|Una regola è stata ignorata da Windows Firewall, perché non è riuscito ad analizzare la regola.|  
|4954|N/D|Basso|Impostazioni di criteri di gruppo di Windows Firewall sono state modificate. Sono state applicate le nuove impostazioni.|  
|4956|N/D|Basso|Windows Firewall è stato modificato il profilo attivo.|  
|4957|N/D|Basso|Windows Firewall è stato non applica la regola seguente:|  
|4958|N/D|Basso|Windows Firewall è stato non applica la regola seguente perché la regola di cui elementi non è configurato su questo computer:|  
|4979|N/D|Basso|Sono state stabilite associazioni di sicurezza IPsec in modalità principale o Extended.|  
|4980|N/D|Basso|Sono state stabilite associazioni di sicurezza IPsec in modalità principale o Extended.|  
|4981|N/D|Basso|Sono state stabilite associazioni di sicurezza IPsec in modalità principale o Extended.|  
|4982|N/D|Basso|Sono state stabilite associazioni di sicurezza IPsec in modalità principale o Extended.|  
|4985|N/D|Basso|Lo stato di una transazione è cambiato.|  
|5024|N/D|Basso|Il servizio Windows Firewall avviato correttamente.|  
|5025|N/D|Basso|Il servizio Windows Firewall è stato arrestato.|  
|5031|N/D|Basso|Il servizio Windows Firewall è bloccato da un'applicazione di accettare le connessioni in ingresso nella rete.|  
|5032|N/D|Basso|Windows Firewall non è riuscito a notificare all'utente che bloccato l'accettazione di connessioni in ingresso nella rete di un'applicazione.|  
|5033|N/D|Basso|Il Driver di Windows Firewall è stato avviato correttamente.|  
|5034|N/D|Basso|Il Driver di Windows Firewall è stato arrestato.|  
|5039|N/D|Basso|Una chiave del Registro di sistema è stata virtualizzata.|  
|5040|N/D|Basso|È stata apportata una modifica alle impostazioni di IPsec. È stato aggiunto un Set di autenticazione.|  
|5041|N/D|Basso|È stata apportata una modifica alle impostazioni di IPsec. Un Set di autenticazione è stato modificato.|  
|5042|N/D|Basso|È stata apportata una modifica alle impostazioni di IPsec. Un Set di autenticazione è stato eliminato.|  
|5043|N/D|Basso|È stata apportata una modifica alle impostazioni di IPsec. È stata aggiunta una regola di sicurezza della connessione.|  
|5044|N/D|Basso|È stata apportata una modifica alle impostazioni di IPsec. Una regola di sicurezza di connessione è stata modificata.|  
|5045|N/D|Basso|È stata apportata una modifica alle impostazioni di IPsec. È stata eliminata una regola di sicurezza della connessione.|  
|5046|N/D|Basso|È stata apportata una modifica alle impostazioni di IPsec. È stato aggiunto un Set di crittografia.|  
|5047|N/D|Basso|È stata apportata una modifica alle impostazioni di IPsec. Un Set di crittografia è stato modificato.|  
|5048|N/D|Basso|È stata apportata una modifica alle impostazioni di IPsec. Un Set di crittografia è stato eliminato.|  
|5050|N/D|Basso|Un tentativo di disabilitare a livello di programmazione di Windows Firewall con una chiamata a InetFwProfile.FirewallEnabled(False)|  
|5051|N/D|Basso|Un file è stato virtualizzato.|  
|5056|N/D|Basso|È stata eseguita una verifica automatica di crittografia.|  
|5057|N/D|Basso|Un'operazione di primitiva di crittografia non è riuscito.|  
|5058|N/D|Basso|Operazione di file di chiave.|  
|5059|N/D|Basso|Operazione di migrazione delle chiavi.|  
|5060|N/D|Basso|Verifica operazione non riuscita.|  
|5061|N/D|Basso|Operazione di crittografia.|  
|5062|N/D|Basso|Crittografia che test automatico in modalità di kernel è stata eseguita.|  
|5063|N/D|Basso|Un'operazione di provider di crittografia è stata tentata.|  
|5064|N/D|Basso|Un'operazione di contesto di crittografia è stata tentata.|  
|5065|N/D|Basso|È stata tentata una modifica del contesto di crittografia.|  
|5066|N/D|Basso|Un'operazione di funzione di crittografia è stata tentata.|  
|5067|N/D|Basso|È stata tentata una modifica della funzione di crittografia.|  
|5068|N/D|Basso|Un'operazione di provider di funzione di crittografia è stata tentata.|  
|5069|N/D|Basso|Un'operazione di proprietà della funzione di crittografia è stata tentata.|  
|5070|N/D|Basso|È stata tentata una modifica di proprietà della funzione di crittografia.|  
|5125|N/D|Basso|È stata inviata una richiesta per il servizio Risponditore OCSP|  
|5126|N/D|Basso|Certificato di firma è stato aggiornato automaticamente tramite il servizio Risponditore OCSP|  
|5127|N/D|Basso|Il Provider di revoca OCSP ha aggiornato correttamente le informazioni di revoca|  
|5136|566|Basso|Un oggetto servizio directory è stato modificato.|  
|5137|566|Basso|È stato creato un oggetto servizio directory.|  
|5138|N/D|Basso|Un oggetto servizio directory è stato annullato.|  
|5139|N/D|Basso|Un oggetto servizio directory è stato spostato.|  
|5140|N/D|Basso|Un oggetto condivisione di rete è stato eseguito.|  
|5141|N/D|Basso|Un oggetto servizio directory è stato eliminato.|  
|5152|N/D|Basso|Piattaforma filtro Windows è bloccato da un pacchetto.|  
|5153|N/D|Basso|Un filtro più restrittivo piattaforma filtro Windows è bloccato un pacchetto.|  
|5154|N/D|Basso|Piattaforma filtro Windows ha consentito a un'applicazione o un servizio per l'ascolto su una porta per le connessioni in ingresso.|  
|5155|N/D|Basso|Piattaforma filtro Windows è bloccato a un'applicazione o servizio in ascolto su una porta per le connessioni in ingresso.|  
|5156|N/D|Basso|Piattaforma filtro Windows è consentita la connessione.|  
|5157|N/D|Basso|Piattaforma filtro Windows è bloccata una connessione.|  
|5158|N/D|Basso|Piattaforma filtro Windows è consentito un binding a una porta locale.|  
|5159|N/D|Basso|Piattaforma filtro Windows è bloccato un binding a una porta locale.|  
|5378|N/D|Basso|È stata annullata la delega delle credenziali richieste dai criteri.|  
|5440|N/D|Basso|Il callout seguenti era presente all'avvio del filtro piattaforma Base filtro motore di Windows.|  
|5441|N/D|Basso|Il filtro seguente era presente all'avvio del filtro piattaforma Base filtro motore di Windows.|  
|5442|N/D|Basso|Provider di servizi di era presente all'avvio del filtro piattaforma Base filtro motore di Windows.|  
|5443|N/D|Basso|Il contesto del provider seguito era presente all'avvio del filtro piattaforma Base filtro motore di Windows.|  
|5444|N/D|Basso|Il seguente sottolivello era presente all'avvio del filtro piattaforma Base filtro motore di Windows.|  
|5446|N/D|Basso|Callout piattaforma filtro Windows è stato modificato.|  
|5447|N/D|Basso|Un filtro piattaforma filtro Windows è stato modificato.|  
|5448|N/D|Basso|Un provider di piattaforma filtro Windows è stato modificato.|  
|5449|N/D|Basso|Il contesto di un provider di piattaforma filtro Windows è stato modificato.|  
|5450|N/D|Basso|Un sottolivello piattaforma filtro Windows è stato modificato.|  
|5451|N/D|Basso|È stata stabilita un'associazione di sicurezza IPsec in modalità rapida.|  
|5452|N/D|Basso|Un'associazione di sicurezza IPsec in modalità rapida è terminato.|  
|5456|N/D|Basso|Motore PAStore applicato il criterio IPsec di archiviazione di Active Directory nel computer.|  
|5457|N/D|Basso|PAStore Engine non riesce ad applicare il criterio IPsec di archiviazione di Active Directory nel computer.|  
|5458|N/D|Basso|Motore PAStore applicato copia memorizzata nella cache locale del criterio IPsec di archiviazione di Active Directory nel computer.|  
|5459|N/D|Basso|PAStore Engine non è riuscito ad applicare una copia memorizzata nella cache locale del criterio IPsec di archiviazione di Active Directory nel computer.|  
|5460|N/D|Basso|Motore PAStore applicato il criterio IPsec di archiviazione del Registro di sistema locale nel computer.|  
|5461|N/D|Basso|Motore PAStore Impossibile applicare il criterio IPsec di archiviazione del Registro di sistema locale nel computer.|  
|5462|N/D|Basso|PAStore Engine non è riuscito ad applicare alcune regole del criterio IPsec attivo nel computer. Utilizzare lo snap-in Monitor di sicurezza IP per isolare il problema.|  
|5463|N/D|Basso|Motore PAStore eseguito il polling delle modifiche per il criterio IPsec e non rilevata nessuna modifica.|  
|5464|N/D|Basso|Motore PAStore eseguito il polling delle modifiche per il criterio IPsec rilevato modifiche e applicate ai servizi IPsec.|  
|5465|N/D|Basso|Motore PAStore ricevuto un controllo per forzare il caricamento del criterio IPsec e il controllo ha elaborato senza errori.|  
|5466|N/D|Basso|Motore PAStore eseguito il polling delle modifiche per il criterio IPsec di Active Directory, definito Active Directory non è raggiungibile che verrà utilizzato invece la copia memorizzata nella cache del criterio IPsec di Active Directory. Tutte le modifiche apportate al criterio IPsec di Active Directory, poiché non è possibile applicare l'ultimo polling.|  
|5467|N/D|Basso|Motore PAStore eseguito il polling delle modifiche per il criterio IPsec di Active Directory, definito Active Directory può essere raggiunto che ha rilevato alcuna modifica ai criteri. La copia memorizzata nella cache del criterio IPsec di Active Directory non è più utilizzata.|  
|5468|N/D|Basso|Motore PAStore eseguito il polling delle modifiche per il criterio IPsec di Active Directory, definito Active Directory può essere raggiunto, trovare le modifiche ai criteri e applicate tali modifiche. La copia memorizzata nella cache del criterio IPsec di Active Directory non è più utilizzata.|  
|5471|N/D|Basso|PAStore Engine ha caricato il criterio IPsec nel computer di archiviazione locale.|  
|5472|N/D|Basso|PAStore Engine non è riuscito a caricare l'archiviazione locale dei criteri IPsec nel computer.|  
|5473|N/D|Basso|Motore PAStore caricato archiviazione directory criteri IPsec nel computer.|  
|5474|N/D|Basso|PAStore Engine non è riuscito a caricare l'archivio directory criteri IPsec nel computer.|  
|5477|N/D|Basso|Impossibile aggiungere il filtro di modalità rapida PAStore Engine.|  
|5479|N/D|Basso|Servizi IPsec è stato arrestato correttamente. L'arresto dei servizi IPsec può esporre il computer a maggiore rischio di attacchi di rete o esporre il computer a potenziali rischi di sicurezza.|  
|5632|N/D|Basso|È stata effettuata una richiesta per l'autenticazione a una rete wireless.|  
|5633|N/D|Basso|È stata effettuata una richiesta per l'autenticazione a una rete cablata.|  
|5712|N/D|Basso|È stata tentata una chiamata RPC (Remote Procedure).|  
|5888|N/D|Basso|È stato modificato un oggetto nel catalogo COM+.|  
|5889|N/D|Basso|Un oggetto è stato eliminato dal catalogo COM+.|  
|5890|N/D|Basso|È stato aggiunto un oggetto nel catalogo COM+.|  
|6008|N/D|Basso|L'arresto del sistema precedenti non era previsto|  
|6144|N/D|Basso|Criteri di sicurezza in oggetti Criteri di gruppo sono stato applicato correttamente.|  
|6272|N/D|Basso|Server dei criteri di rete concesso l'accesso a un utente.|  
|N/D|561|Basso|È stato richiesto un handle per un oggetto.|  
|N/D|563|Basso|Apertura per l'eliminazione oggetto|  
|N/D|625|Basso|Tipo di Account utente modificato|  
|N/D|613|Basso|Avviata servizio Agente criteri IPsec|  
|N/D|614|Basso|Agente criteri IPsec disabilitato|  
|N/D|615|Basso|Agente criteri IPsec|  
|N/D|616|Basso|Agente criteri IPsec ha rilevato un errore grave potenziale|  
|24577|N/D|Basso|Crittografia del volume di avvio|  
|24578|N/D|Basso|Crittografia del volume arrestato|  
|24579|N/D|Basso|Crittografia del volume completato|  
|24580|N/D|Basso|Decrittografia del volume di avvio|  
|24581|N/D|Basso|Decrittografia del volume arrestato|  
|24582|N/D|Basso|Decrittografia del volume completato|  
|24583|N/D|Basso|Thread di lavoro di conversione per il volume di avvio|  
|24584|N/D|Basso|Thread di lavoro di conversione per il volume è temporaneamente interrotta|  
|24588|N/D|Basso|Errore di settori danneggiati nel volume %2 l'operazione di conversione. Convalidare i dati sul volume|  
|24595|N/D|Basso|Volume %2 contiene cluster danneggiati. Questi cluster verranno ignorati durante la conversione.|  
|24621|N/D|Basso|Controllo stato iniziale: rollback delle transazioni di conversione volume su %2.|  
|5049|N/D|Basso|Un'associazione di sicurezza IPsec è stata eliminata.|  
|5478|N/D|Basso|Servizi IPsec è stato avviato correttamente.|  
  
> [!NOTE]  
> Fare riferimento a [articolo di supporto Microsoft 947226](https://support.microsoft.com/kb/947226) per un elenco di molti ID degli eventi di sicurezza e i relativi significati.  
>   
> Eseguire **wevtutil criteri di gruppo Microsoft-Windows-Security-Auditing /ge /gm:true** per ottenere un elenco dettagliato di sicurezza tutti gli ID evento  
  
Per ulteriori informazioni sull'ID evento di sicurezza di Windows e il relativo significato, Vedi gli articoli di supporto Microsoft [descrizione degli eventi di sicurezza in Windows Vista e Windows Server 2008](https://support.microsoft.com/kb/947226) e [descrizione degli eventi di sicurezza in Windows 7 e Windows Server 2008 R2](https://support.microsoft.com/kb/977519). È inoltre possibile scaricare [gli eventi di controllo della protezione per Windows 7 e Windows Server 2008 R2](https://www.microsoft.com/download/details.aspx?id=21561) e [Windows 8 e dettagli sull'evento di sicurezza di Windows Server 2012](https://www.microsoft.com/download/details.aspx?id=35753), che forniscono evento informazioni dettagliate per i sistemi operativi a cui fa riferimento nel formato foglio di calcolo.  
  


