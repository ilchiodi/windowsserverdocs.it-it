**Rimuovere i dispositivi dal gruppo di sicurezza di DirectAccess.** Come eseguire la migrazione degli utenti correttamente, si rimuovono i propri dispositivi dal gruppo di sicurezza di DirectAccess. Prima di rimuovere DirectAccess dall'ambiente in uso, controllare se il gruppo di sicurezza di DirectAccess è vuoto. **No** rimuovere il gruppo di sicurezza se contiene ancora i membri. Se si rimuove il gruppo di sicurezza con i membri in esso, si rischia di lasciare i dipendenti senza accesso remoto dai propri dispositivi. Usare Microsoft System Center Configuration Manager o Microsoft Intune per ricavare informazioni sull'assegnazione di dispositivi e individuare il dispositivo a cui appartiene ogni utente. 