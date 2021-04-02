# FTP
- [Definizione](#Definizione)
- [Modalità di lavoro](#Modalità-di-lavoro)
  - [Active mode](#Active-mode)
  - [Passive mode](#Passive-mode)
- [Modalità di accesso](#Modalità-di-accesso)
- [Vulnerabilità](#Vulnerabilità)
- [Funzioni](#Funzioni)


## *Definizione*

*Il File Transfer Protocol* è un protocollo a livello applicativo che serve per condividere dei file tra un client è un server.

> L'FTP venne standardizzato negli anni Ottanta e fa uso del protocollo TCP a livello di trasporto, qualche anno dopo la sua standardizzazione uscì una sua versione più leggera denominata TFTP (Trivial FTP) che fa uso del protocollo UDP a livello di trasporto.

Anche se al giorno d'oggi ci sono molti modi per trasferire file attraverso la rete (email, chat, web server) che vengono usati largamente per via della familiarità degli utenti con le loro interfacce, questi metodi non sono stati specificatamente pensati per questo e l'FTP risulta dunque più robusto di essi. 

---
## *Modalità di lavoro*

FTP utilizza due canali per la comunicazione tra client server:
1.	Un canale viene utilizzato per l’invio di comandi tra client e server, e relative risposte, questo canale viene sempre aperto in direzione client => server e utilizza la porta a 21 (detta anche porta di controllo) 
2.	L’altro canale utilizzato per l’invio dei dati, viene quindi aperto in direzione server => client utilizza la porta 20 (porta dati)

La connessione tra client e server può avvenire secondo due modalità: FTP active mode e FTP passive mode
-	### *Active mode:*
  1.	Il client si connette con una porta N (> 1023) alla porta di controllo del server e si mette in ascolto nella porta N+1
  2.	Il client invia il comando Port N+1 al server
  3.	Il server si connette alla porta specificata dal client con la propria porta dati
  4.	Il client invia un riscontro (ACK) di conferma ricezione, da questo punto inizia il trasferimento dei dati dalla porta dati del server. 
  
![active mode](active_mode.jpeg)
 
 ### Problematiche:
 Non essendo il client FTP a connettersi alla porta dati del server, ma il contrario (esso infatti indica al server a quale porta connettersi), il tentativo di connessione del server potrebbe essere percepito da un possibile firewall attivo nel lato client come un tentativo di intrusione e venir dunque bloccato.
È sconsigliato l'uso della active mode per poter garantire la sicurezza della intranet, evitando di dover aggiungere o rimuovere delle regole al firewall per ovviare alla problematica precedentemente descritta.

-	### *Passive mode:*

Il client FTP inizia entrambe le connessioni con il server, sia comandi che dati, risolvendo dunque il problema di filtraggio della connessione da parte del firewall lato client.
1.	Il client apre localmente due porte N (> 1023) e N+1; con la porta N contatta il server sulla sua porta comandi inviando il comando PASV
2.	Il server apre una porta casuale P (> 1023) ed invia il comando Port P al client sulla sua porta comandi N
3.	Il client si connette alla porta P dalla sua porta N+1
4.	Il server invia un riscontro (ACK) di conferma ricezione, da questo punto inizia il trasferimento dei dati dalla porta P del server 

 ![Passive mode](.FTP/Img/passive_mode.jpeg "passive mode") 
 
 ---
 ## *Modalità di accesso*
 FTP HA DUE MODALITÀ PREDEFINITE DI ACCESSO: UTENTE E ANONIMA.

-	La modalità utente prevede un accesso al server FTP con username e password, mentre in modalità anonima sia accesso come utente Anonymous. 
Quest’ultima è molto utilizzata per scambi di dati pubblici e presenta due limitazioni:
1.	È necessario limitare l’accesso alle sole informazioni che si vogliono diffondere.

2.	Non bisogna consentire l’uso di effe tipi server per la distribuzione di materiale di terzi (per esempio si può rendere write-only la cartella di upload).

Per la sicurezza del sistema è meglio evitare di usare l’accesso anonimo. Nel caso in cui sia proprio necessario, esso deve essere configurato correttamente e amministrato con attenzione, soprattutto se si vuole rendere accessibili file in upload quindi scrivibili dalle directory (o cartella) nelle aree FTP Anonymous. 

---
## *Vulnerabilità*

I maggiori problemi di sicurezza sono riconducibili al fatto che le specifiche non prevedono la cifratura delle informazioni scambiate tra client e server:
-	Password in chiaro: le password viaggiano in chiaro attraverso la rete sono facilmente intercettabili con strumenti (sniffer) che consentono di controllare il traffico tra client e server.
-	Dati in chiaro: anche i dati vengono trasferiti senza essere crittografati, anche se sono intercettabili.

La soluzione a questi problemi è stata una nuova specifica di FTP denominata FTPS che aggiunge un livello fra il Transport(TCP) e l’application (FTP) per la gestione della crittografia, che usa il protocollo TLS =Transport layer security.

Altri problemi di sicurezza sono legati a:
-	Sessione in due processi: la necessità di avere due processi per ogni connessione rende più semplici manovre malevole da parte di malintenzionati.
-	Permessi utente: i permessi di accesso FTP vanno incrociati con i permessi utente del server in modo da limitare lo spazio su disco e le operazioni sui file


 
 
 
