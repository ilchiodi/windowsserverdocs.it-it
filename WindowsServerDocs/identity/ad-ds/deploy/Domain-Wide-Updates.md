---
ms.assetid: 2a5d5271-6ac6-4c1b-b4ef-9b568932a55a
title: Aggiornamenti dello schema Active Directory a livello di dominio
description: Aggiornamenti dello schema a livello di dominio eseguiti da adprep/domainprep quando si innalza di livello un controller di dominio
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 10/29/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 3b82958471e5292f202aa338aee7f4f5863459af
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80825214"
---
# <a name="domain-wide-schema-updates"></a>Aggiornamenti dello schema a livello di dominio

>Si applica a: Windows Server

È possibile esaminare il set di modifiche seguente per semplificare la comprensione e la preparazione degli aggiornamenti dello schema eseguiti da adprep/domainprep in Windows Server.

A partire da Windows Server 2012, i comandi adprep vengono eseguiti automaticamente quando necessario durante l'installazione di servizi di dominio Active Directory. Possono anche essere eseguiti separatamente prima dell'installazione di servizi di dominio Active Directory. Per ulteriori informazioni, vedere [Esecuzione di Adprep.exe](https://technet.microsoft.com/library/dd464018(v=ws.10).aspx).

Per ulteriori informazioni su come interpretare le stringhe ACE (Access Control Entry), vedere [stringhe ACE](https://msdn.microsoft.com/library/aa374928(VS.85).aspx). Per ulteriori informazioni su come interpretare le stringhe dell'ID di sicurezza (SID), vedere [stringhe SID](https://msdn.microsoft.com/library/aa379602(VS.85).aspx).

## <a name="windows-server-semi-annual-channel-domain-wide-updates"></a>Windows Server (canale semestrale): aggiornamenti a livello di dominio

Una volta completate le operazioni eseguite da **domainprep** in Windows Server 2016 (operazione 89), l'attributo **Revision** per l'oggetto CN = ActiveDirectoryUpdate, CN = DomainUpdates, CN = System, DC = DominioRadiceForesta è impostato su **16**.

|Numero di operazioni e GUID|Descrizione|Autorizzazioni|
|------------------------------|---------------|--------------|---------------|
|**Operazione 89**: {A0C238BA-9E30-4EE6-80A6-43F731E9A5CD}|Eliminare la voce ACE che concede il controllo completo agli amministratori della chiave Enterprise e aggiungere una voce ACE che concede a Enterprise Key Admins il controllo completo sull'attributo msdsKeyCredentialLink.|Delete (A; Ci RPWPCRLCLOCCDCRCWDWOSDDTSW;;; Amministratori chiave aziendali <br /> <br />Aggiungi (OA; Ci RPWP;5b47d60f-6090-40b2-9f37-2a4de88f3063;; Amministratori chiave aziendali|

## <a name="windows-server-2016-domain-wide-updates"></a>Windows Server 2016: aggiornamenti a livello di dominio

Una volta completate le operazioni eseguite da **domainprep** in Windows Server 2016 (Operations 82-88), l'attributo **Revision** per l'oggetto CN = ActiveDirectoryUpdate, CN = DomainUpdates, CN = System, DC = DominioRadiceForesta è impostato su **15**.

|Numero di operazioni e GUID|Descrizione|Attributi|Autorizzazioni|
|------------------------------|---------------|--------------|---------------|
|**Operazione 82**: {83C53DA7-427E-47a4-a07a-A324598B88F7}|Crea il contenitore CN = Keys alla radice del dominio|-objectClass: contenitore<br />-Description: contenitore predefinito per gli oggetti credenziale chiave<br />-ShowInAdvancedViewOnly: TRUE|Un Ci RPWPCRLCLOCCDCRCWDWOSDDTSW;;; EA<br />Un Ci RPWPCRLCLOCCDCRCWDWOSDDTSW;;;D Un<br />Un Ci RPWPCRLCLOCCDCRCWDWOSDDTSW;;; SY<br />Un Ci RPWPCRLCLOCCDCRCWDWOSDDTSW;;;D D<br />Un Ci RPWPCRLCLOCCDCRCWDWOSDDTSW;;; ED|
|**Operazione 83**: {C81FC9CC-0130-4fd1-b272-634D74818133}|Aggiungere il controllo completo Consenti ACE al contenitore CN = Keys per "domain\Key Admins" e "rootdomain\Enterprise Key Admins".|N/D|Un Ci RPWPCRLCLOCCDCRCWDWOSDDTSW;;; Amministratori chiave)<br />Un Ci RPWPCRLCLOCCDCRCWDWOSDDTSW;;; Amministratori chiave aziendali|
|**Operazione 84**: {E5F9E791-D96D-4FC9-93C9-D53E1DC439BA}|Modificare l'attributo otherWellKnownObjects archiviato nel in modo che punti al contenitore CN = Keys.|-otherWellKnownObjects archiviato nel: B:32:683A24E2E8164BD3AF86AC3C2CF3F981: CN = Keys,% WS|N/D|
|**Operazione 85**: {e6d5fd00-385d-4E65-B02D-9da3493ed850}|Modificare il controller di dominio per consentire a "domain\Key Admins" e "rootdomain\Enterprise Key Admins" di modificare l'attributo msDS-KeyCredentialLink. |N/D|OA Ci RPWP;5b47d60f-6090-40b2-9f37-2a4de88f3063;; Amministratori chiave)<br />OA Ci RPWP;5b47d60f-6090-40b2-9f37-2a4de88f3063;; Enterprise Key Admins nel dominio radice, ma nei domini non radice ha generato una voce ACE relativa al dominio fasullo con un SID non risolvibile-527)|
|**Operazione 86**: {3a6b3fbf-3168-4312-a10d-dd5b3393952d}|Concedere a DS-Validated-write-computer CAR il proprietario e l'utente del creatore|N/D|OA CIIO; SW; 9b026da6-0d3c-465c-8bee-5199d7165cba; bf967a86-0de6-11D0-A285-00aa003049e2; PS)<br />OA CIIO; SW; 9b026da6-0d3c-465c-8bee-5199d7165cba; bf967a86-0de6-11D0-A285-00aa003049e2; CO)|
|**Operazione 87**: {7F950403-0AB3-47F9-9730-5D7B0269F9BD}|Eliminare l'ACE che concede il controllo completo al gruppo di amministratori di chiavi Enterprise relativo al dominio errato e aggiungere una voce ACE che concede il controllo completo al gruppo Enterprise Key Admins. |N/D|Delete (A; Ci RPWPCRLCLOCCDCRCWDWOSDDTSW;;; Amministratori chiave aziendali<br /> <br />Aggiungi (A; Ci RPWPCRLCLOCCDCRCWDWOSDDTSW;;; Amministratori chiave aziendali|
|**Operazione 88**: {434bb40d-dbc9-4fe7-81D4-d57229f7b080}|Aggiungere "msDS-ExpirePasswordsOnSmartCardOnlyAccounts" nell'oggetto NC del dominio e impostare il valore predefinito su FALSE|N/D|N/D|

I gruppi Enterprise Key Admins e Key Admins vengono creati solo dopo l'innalzamento di livello di un controller di dominio Windows Server 2016 e acquisisce il ruolo FSMO dell'emulatore PDC.

## <a name="windows-server-2012-r2-domain-wide-updates"></a>Windows Server 2012 R2: aggiornamenti a livello di dominio

Sebbene nessuna operazione venga eseguita da **domainprep** in Windows Server 2012 R2, dopo il completamento del comando, l'attributo **Revision** per l'oggetto CN = ActiveDirectoryUpdate, CN = DomainUpdates, CN = System, DC = DominioRadiceForesta è impostato su **10**.

## <a name="windows-server-2012-domain-wide-updates"></a>Windows Server 2012: aggiornamenti a livello di dominio

Una volta completate le operazioni eseguite da **domainprep** in Windows Server 2012 (Operations 78, 79, 80 e 81), l'attributo **Revision** per l'oggetto CN = ActiveDirectoryUpdate, CN = DomainUpdates, CN = System, DC = DominioRadiceForesta è impostato su **9**.

|Numero di operazioni e GUID|Descrizione|Attributi|Autorizzazioni|
|------------------------------|---------------|--------------|---------------|
|**Operazione 78**: {c3c927a6-cc1d-47C0-966B-be8f9b63d991}|Creare un nuovo oggetto CN = TPM dispositivi nella partizione di dominio.|Classe di oggetti: msTPM-InformationObjectsContainer|N/D|
|**Operazione 79**: {54afcfb9-637a-4251-9f47-4d50e7021211}|Creazione di una voce di controllo di accesso per il servizio TPM.|N/D|OA CIIO; WP; ea1b7b93-5e48-46d5-bc6c-4df4fda78a35; bf967a86-0de6-11D0-A285-00aa003049e2; PS)|
|**Operazione 80**: {f4728883-84dd-483C-9897-274f2ebcf11e}|Concedere il diritto esteso "clone DC" al gruppo **controller di dominio clonabili**|N/D|(OA;; CR; 3e0f7e18-2C7A-4c10-ba82-4d926db99a3e;; *SID dominio*-522)|
|**Operazione 81**: {ff4f9d27-7157-4CB0-80a9-5d6f2b14c8ff}|Concedere ms-DS-allowed-to-Act-on-conto-of-other-Identity all'entità autonoma su tutti gli oggetti.|N/D|OA CIOI; RPWP;3f78c3e5-f79a-46bd-a0b8-9d18116ddc79;; PS|
