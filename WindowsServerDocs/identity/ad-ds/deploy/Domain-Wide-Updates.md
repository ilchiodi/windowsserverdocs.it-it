---
ms.assetid: 2a5d5271-6ac6-4c1b-b4ef-9b568932a55a
title: Aggiornamenti dello schema a livello di dominio di Active Directory
description: Gli aggiornamenti dello schema di livello di dominio eseguiti da adprep /domainprep l'innalzamento di livello un Controller di dominio
author: MicrosoftGuyJFlo
ms.author: joflore
manager: femila
ms.date: 07/14/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 59efb7c854b7f3693776bf9a1a371f98c2206d60
ms.sourcegitcommit: 79c1359232bece2e5c3ee5507f1e4bf19b44f487
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2018
---
# <a name="domain-wide-schema-updates"></a>Aggiornamenti dello schema a livello di dominio

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È possibile esaminare le seguenti modifiche per comprendere e preparazione per gli aggiornamenti dello schema che vengono eseguiti da adprep /domainprep in Windows Server. 

A partire da Windows Server 2012, comandi Adprep vengono eseguiti automaticamente in base alle necessità durante l'installazione di Active Directory. Si può anche essere eseguiti separatamente prima installazione di Active Directory. For more information, see [Running Adprep.exe](https://technet.microsoft.com/library/dd464018(v=ws.10).aspx).

For more information about how to interpret the access control entry (ACE) strings, see [ACE strings](https://msdn.microsoft.com/library/aa374928(VS.85).aspx). For more information about how to interpret the security ID (SID) strings, see [SID strings](https://msdn.microsoft.com/library/aa379602(VS.85).aspx).

## <a name="windows-server-semi-annual-channel-domain-wide-updates"></a>Windows Server (canale annuale e virgola): Gli aggiornamenti a livello di dominio

Dopo le operazioni eseguite da **domainprep** in Windows Server 2016 (operazione 89) completo, il **revisione** attributo per CN = ActiveDirectoryUpdate, CN = DomainUpdates, CN = System, DC = DominioRadiceForesta oggetto è impostato su **16**.

|Operations number and GUID|Descrizione|Autorizzazioni|
|------------------------------|---------------|--------------|---------------|
|**Operazione 89**: {A0C238BA-9E30-4EE6-80A6-43F731E9A5CD}|Eliminare la voce concessione del controllo completo per Enterprise Admins chiave e aggiungere una voce ACE concessione del controllo completo dell'organizzazione chiave Admins tramite l'attributo di msdsKeyCredentialLink.|Delete (A;CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;;Enterprise Key Admins) <br /> <br />Aggiungere (OA; CI; RPWP; 5b47d60f-6090-40b2-9f37-2a4de88f3063; Enterprise Admins chiave)|

## <a name="windows-server-2016-domain-wide-updates"></a>Windows Server 2016: Domain-wide updates

After the operations that are performed by **domainprep** in Windows Server 2016 (operations 82-88) complete, the **revision** attribute for the CN=ActiveDirectoryUpdate,CN=DomainUpdates,CN=System,DC=ForestRootDomain object is set to **15**.

|Operations number and GUID|Descrizione|Attributi|Autorizzazioni|
|------------------------------|---------------|--------------|---------------|
|**Operation 82**: {83C53DA7-427E-47A4-A07A-A324598B88F7}|Create CN=Keys container at root of domain|- objectClass: container<br />- description: Default container for key credential objects<br />- ShowInAdvancedViewOnly: TRUE|(A;CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;;EA)<br />(A;CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;;DA)<br />(A;CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;;SY)<br />(A;CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;;DD)<br />(A;CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;;ED)|
|**Operation 83**: {C81FC9CC-0130-4FD1-B272-634D74818133}|Add Full Control allow aces to CN=Keys container for "domain\Key Admins" and "rootdomain\Enterprise Key Admins".|N/D|(A;CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;;Key Admins)<br />(A;CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;;Enterprise Key Admins)|
|**Operation 84**: {E5F9E791-D96D-4FC9-93C9-D53E1DC439BA}|Modify otherWellKnownObjects attribute to point to the CN=Keys container.|- otherWellKnownObjects: B:32:683A24E2E8164BD3AF86AC3C2CF3F981:CN=Keys,%ws|N/D|
|**Operation 85**: {e6d5fd00-385d-4e65-b02d-9da3493ed850}|Modify the domain NC to permit "domain\Key Admins" and "rootdomain\Enterprise Key Admins" to modify the msds-KeyCredentialLink attribute. |N/D|(OA;CI;RPWP;5b47d60f-6090-40b2-9f37-2a4de88f3063;;Key Admins)<br />(OA;CI;RPWP;5b47d60f-6090-40b2-9f37-2a4de88f3063;;Enterprise Key Admins in root domain, but in non-root domains resulted in a bogus domain-relative ACE with a non-resolvable -527 SID)|
|**Operation 86**: {3a6b3fbf-3168-4312-a10d-dd5b3393952d}|Grant the DS-Validated-Write-Computer CAR to creator owner and self|N/D|(OA;CIIO;SW;9b026da6-0d3c-465c-8bee-5199d7165cba;bf967a86-0de6-11d0-a285-00aa003049e2;PS)<br />(OA;CIIO;SW;9b026da6-0d3c-465c-8bee-5199d7165cba;bf967a86-0de6-11d0-a285-00aa003049e2;CO)|
|**Operation 87**: {7F950403-0AB3-47F9-9730-5D7B0269F9BD}|Delete the ACE granting Full Control to the incorrect domain-relative Enterprise Key Admins group, and add an ACE granting Full Control to Enterprise Key Admins group. |N/D|Delete (A;CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;;Enterprise Key Admins)<br /> <br />Add (A;CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;;Enterprise Key Admins)|
|**Operation 88**: {434bb40d-dbc9-4fe7-81d4-d57229f7b080}|Add "msDS-ExpirePasswordsOnSmartCardOnlyAccounts" on the domain NC object and set default value to FALSE|N/D|N/D|

The Enterprise Key Admins and Key Admins groups are only created after a Windows Server 2016 Domain Controller is promoted and takes over the PDC Emulator FSMO role.

## <a name="windows-server-2012-r2-domain-wide-updates"></a>Windows Server 2012 R2: Domain-wide updates

Although no operations are performed by **domainprep** in Windows Server 2012 R2, after the command completes, the **revision** attribute for the CN=ActiveDirectoryUpdate,CN=DomainUpdates,CN=System,DC=ForestRootDomain object is set to **10**.

## <a name="windows-server-2012-domain-wide-updates"></a>Windows Server 2012: Domain-wide updates

After the operations that are performed by **domainprep** in Windows Server 2012 (operations 78, 79, 80, and 81) complete, the **revision** attribute for the CN=ActiveDirectoryUpdate,CN=DomainUpdates,CN=System,DC=ForestRootDomain object is set to **9**.

|Operations number and GUID|Descrizione|Attributi|Autorizzazioni|
|------------------------------|---------------|--------------|---------------|
|**Operation 78**: {c3c927a6-cc1d-47c0-966b-be8f9b63d991}|Create a new object CN=TPM Devices in the Domain partition.|Object class: msTPM-InformationObjectsContainer|N/D|
|**Operation 79**: {54afcfb9-637a-4251-9f47-4d50e7021211}|Created an access control entry for the TPM service.|N/D|(OA; CIIO; WP;ea1b7b93-5e48-46d5-bc6c-4df4fda78a35;bf967a86-0de6-11D0-a285-00aa003049e2;PS)|
|**Operation 80**: {f4728883-84dd-483c-9897-274f2ebcf11e}|Grant "Clone DC" extended right to **Cloneable Domain Controllers** group|N/D|(OA;;CR;3e0f7e18-2c7a-4c10-ba82-4d926db99a3e;;*domain SID*-522)|
|**Operation 81**: {ff4f9d27-7157-4cb0-80a9-5d6f2b14c8ff}|Grant ms-DS-Allowed-To-Act-On-Behalf-Of-Other-Identity to Principal Self on all objects.|N/D|(OA; CIOI; RPWP; 3f78c3e5-f79a-46bd-a0b8-9d18116ddc79; PS)|
