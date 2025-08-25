# Playbook installazione ifix
## note di utilizzo

Il playbook si appoggia alla collection ibm.power_aix 

Il playbook si aspetta di poter scaricare l'ifix da un repository tramite una chiamata HTTP; tale repository va configurato all'inizio del playbook nella variabile "repository_url" 

il playbook richiede in input:
 - ifix :   nome del file con la ifix presente sul repository 

e accetta due opzioni: 
 - reinstall : se valorizzata permette di reinstallare una ifix
 - reboot :  se valorizzata permette al playbook di fare il riavvio del server in autonomia.


es:
 ansible-playbook -i inventory aix-patching-ifix.yaml -e ifix=IJ22714s1a.200212.AIX72TL04SP00-01.epkg.Z


## Flusso logico seguito:

task:
 1 Verifico che sia stata passata la variabile "ifix" e che non sia vuota
 2 mi assicuro che eista la directory di staging dove copiare la ifix
 3 scarico la ifix dal repository nella directory di staging
 4-6 faccio la lista delle ifix installate e verifico se ne è già presente una con la stessa label 
 7-8 faccio una installazione in preview per verificare eventuali ifix da eliminare prima dell'installazione
 9 rimuovo la ifix in conflitto con la nuova 
 10-11 se il remove richiede il riavvio del server procedo a farlo oppure fallisco indicando che deve essere fatto il riavvio manuale 
 12-13 installo la nuova ifix
 14-15 se l'installazione richiede faccio il riavvio del server procedo a farlo oppure fallisco indicando che deve essere fatto il riavvio manuale  
 16 se ho fatto il reboot in automatico mi assicuro che la ifix sia installata


