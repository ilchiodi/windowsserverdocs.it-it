---
ms.assetid: 2a5d5271-6ac6-4c1b-b4ef-9b568932a55a
title: Aggiornamenti dello schema di livello di dominio di Active Directory
description: Aggiornamenti dello schema a livello di dominio eseguiti da adprep /domainprep durante la promozione di un Controller di dominio
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 10/29/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 20598431b9c2376051247fd7ba14e33af00968cc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812322"
---
# <a name="domain-wide-schema-updates"></a>Aggiornamenti dello schema a livello di dominio

>Si applica a: Windows Server

È possibile esaminare il seguente set di modifiche per comprendere e preparare gli aggiornamenti dello schema che vengono eseguiti da adprep /domainprep in Windows Server.

A partire da Windows Server 2012, i comandi Adprep eseguiti automaticamente in base alle necessità durante l'installazione di Active Directory Domain Services. Possono inoltre essere eseguiti separatamente prima dell'installazione di Active Directory Domain Services. Per ulteriori informazioni, vedere [Esecuzione di Adprep.exe](https://technet.microsoft.com/library/dd464018(v=ws.10).aspx).

Per altre informazioni su come interpretare le stringhe di voce ACE di controllo di accesso, vedere [stringhe di voce ACE](https://msdn.microsoft.com/library/aa374928(VS.85).aspx). Per altre informazioni su come interpretare le stringhe di ID (SID) di sicurezza, vedere [stringhe di SID](https://msdn.microsoft.com/library/aa379602(VS.85).aspx).

## <a name="windows-server-semi-annual-channel-domain-wide-updates"></a>Windows Server (canale semestrale): Aggiornamenti a livello di dominio

Dopo le operazioni eseguite dalle **domainprep** in Windows Server 2016 (operazione 89) completo, il **revisione** attributo CN = ActiveDirectoryUpdate, CN = DomainUpdates, CN = System, DC = Oggetto DominioRadiceForesta è impostata su **16**.

|Numero di operazioni e GUID|Descrizione|Permissions|
|------------------------------|---------------|--------------|---------------|
|**Operation 89**: {A0C238BA-9E30-4EE6-80A6-43F731E9A5CD}|Eliminare la voce ACE concessione del controllo completo alla chiave di Enterprise Admins e aggiungere una voce ACE concede Enterprise Key Admins il controllo completo solo l'attributo msdsKeyCredentialLink.|Elimina (A; CI; RPWPCRLCLOCCDCRCWDWOSDDTSW;; Enterprise Admins chiave) <br /> <br />Aggiungere (OA; CI; RPWP; 5b47d60f-6090-40b2-9f37-2a4de88f3063; Enterprise Admins chiave)|

## <a name="windows-server-2016-domain-wide-updates"></a>Windows Server 2016: Aggiornamenti a livello di dominio

Dopo le operazioni eseguite dalle **domainprep** in Windows Server 2016 (operazioni 82 88) completo, il **revisione** attributo CN = ActiveDirectoryUpdate, CN = DomainUpdates, CN = System, DC = oggetto è impostato su DominioRadiceForesta **15**.

|Numero di operazioni e GUID|Descrizione|Attributi|Permissions|
|------------------------------|---------------|--------------|---------------|
|**Operation 82**: {83C53DA7-427E-47A4-A07A-A324598B88F7}|Creare CN = contenitore delle chiavi a livello radice del dominio|-objectClass: contenitore<br />-Descrizione: Contenitore predefinito per gli oggetti credenziale della chiave<br />-ShowInAdvancedViewOnly: TRUE|(A;CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;;EA)<br />(A;CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;;DA)<br />(A;CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;;SY)<br />(A;CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;;DD)<br />(A;CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;;ED)|
|**Operazione 83**: {C81FC9CC-0130-4FD1-B272-634D74818133}|Aggiungere il controllo completo consentire voci ACE in CN = contenitore di chiavi per "domain\Key Admins" e "rootdomain\Enterprise Key Admins".|N/D|(A; CI; RPWPCRLCLOCCDCRCWDWOSDDTSW;; Chiave amministratore)<br />(A; CI; RPWPCRLCLOCCDCRCWDWOSDDTSW;; Enterprise Admins chiave)|
|**Operation 84**: {E5F9E791-D96D-4FC9-93C9-D53E1DC439BA}|Modifica attributo otherWellKnownObjects in modo che punti a CN = contenitore delle chiavi.|-otherWellKnownObjects: B:32:683A24E2E8164BD3AF86AC3C2CF3F981:CN=Keys,%ws|N/D|
|**Operation 85**: {e6d5fd00-385d-4e65-b02d-9da3493ed850}|Modificare il dominio controller di rete per consentire "domain\Key Admins" e "rootdomain\Enterprise Key Admins" per modificare l'attributo msds-KeyCredentialLink. |N/D|(OA;CI;RPWP;5b47d60f-6090-40b2-9f37-2a4de88f3063;;Key Admins)<br />(OA; CI; RPWP; 5b47d60f-6090-40b2-9f37-2a4de88f3063; Enterprise Key Admins nel dominio radice, ma in domini non radice ha comportato un finto ACE relativo al dominio con un SID-527 non risolvibile)|
|**Operazione 86**: {3a6b3fbf-3168-4312-a10d-dd5b3393952d}|Concedere l'auto convalidato-Write-Computer Servizi di directory a proprietario e self|N/D|(OA;CIIO;SW;9b026da6-0d3c-465c-8bee-5199d7165cba;bf967a86-0de6-11d0-a285-00aa003049e2;PS)<br />(OA;CIIO;SW;9b026da6-0d3c-465c-8bee-5199d7165cba;bf967a86-0de6-11d0-a285-00aa003049e2;CO)|
|**Operation 87**: {7F950403-0AB3-47F9-9730-5D7B0269F9BD}|Eliminare la voce ACE concessione del controllo completo al gruppo Enterprise Admins chiave relativo al dominio corretto e aggiungere una voce ACE concessione del controllo completo al gruppo Enterprise Admins di chiave. |N/D|Elimina (A; CI; RPWPCRLCLOCCDCRCWDWOSDDTSW;; Enterprise Admins chiave)<br /> <br />Aggiungere (A; CI; RPWPCRLCLOCCDCRCWDWOSDDTSW;; Enterprise Admins chiave)|
|**Operation 88**: {434bb40d-dbc9-4fe7-81d4-d57229f7b080}|Aggiungere "msDS-ExpirePasswordsOnSmartCardOnlyAccounts" dell'oggetto di contesto dei nomi di dominio e il valore predefinito impostato su FALSE|N/D|N/D|

Dopo che un Controller di dominio di Windows Server 2016 viene promossa e assume il ruolo FSMO dell'emulatore PDC, vengono creati solo i gruppi Enterprise Admins di chiave e Key Admins.

## <a name="windows-server-2012-r2-domain-wide-updates"></a>Windows Server 2012 R2: Aggiornamenti a livello di dominio

Anche se non vengono eseguite operazioni dal **domainprep** in Windows Server 2012 R2, al termine dell'esecuzione del comando, il **revisione** attributo CN = ActiveDirectoryUpdate, CN = DomainUpdates, CN = System, DC = oggetto è impostato su DominioRadiceForesta **10**.

## <a name="windows-server-2012-domain-wide-updates"></a>Windows Server 2012: Aggiornamenti a livello di dominio

Dopo le operazioni eseguite dalle **domainprep** in Windows Server 2012 (operazioni 78, 79, 80 e 81) completo, il **revisione** attributo CN = ActiveDirectoryUpdate, CN = DomainUpdates, CN = System, DC = oggetto è impostato su DominioRadiceForesta **9**.

|Numero di operazioni e GUID|Descrizione|Attributi|Permissions|
|------------------------------|---------------|--------------|---------------|
|**Operazione 78**: {c3c927a6-cc1d-47c0-966b-be8f9b63d991}|Creare un nuovo oggetto CN = i dispositivi TPM nella partizione di dominio.|Classe di oggetti: msTPM InformationObjectsContainer|N/D|
|**Operation 79**: {54afcfb9-637a-4251-9f47-4d50e7021211}|Creare una voce di controllo di accesso per il servizio TPM.|N/D|(OA;CIIO;WP;ea1b7b93-5e48-46d5-bc6c-4df4fda78a35;bf967a86-0de6-11d0-a285-00aa003049e2;PS)|
|**Operation 80**: {f4728883-84dd-483c-9897-274f2ebcf11e}|Concedere "Clone DC" estesi zprava **controller di dominio clonabili** gruppo|N/D|(OA;;CR;3e0f7e18-2c7a-4c10-ba82-4d926db99a3e;;*domain SID*-522)|
|**Operation 81**: {ff4f9d27-7157-4cb0-80a9-5d6f2b14c8ff}|Concedere ms-DS-Allowed-To-Act-On-Behalf-Of-Other-Identity a identità di protezione in tutti gli oggetti.|N/D|(OA;CIOI;RPWP;3f78c3e5-f79a-46bd-a0b8-9d18116ddc79;;PS)|
