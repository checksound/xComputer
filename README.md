# xComputer

Vedi: https://iisponti.gitbook.io/sistemi_terza/xcomputer

Da: https://math.hws.edu/eck/js/xComputer/xComputer.html e https://math.hws.edu/eck/js/xComputer/info.html


## PROGRAMMA SOMMA I PRIMI CENTO NUMERI in ASSEMBLY

Somma i numeri da 1 a 100. Per fare questo, il programma dovrebbe fare 100 addizioni. L'idea sarebbe di partire da 0 e sommare ogni numero uno alla volta. Sebbene potrebbe essere fatto con 100 istruzioni di ADD è più sensato utilizzare un ciclo (loop). Ogni volta che si attraversa il ciclo, aggiungiamo un numero. Il cuore del ciclo, quindi, consiste nell'aggiungere il prossimo numero alla somma calcolata fino a quel momento. Questa parte è essenzialmente la stessa programma di tre istruzioni visto all'inizio di questa sezione. Nel complesso sembra semplice ma ci sono alcuni particolari da tener in conto. Dobbiamo utilizzare una locazione di memoria per immagazzinare la somma che stiamo calcolando, un'altra per tenere traccia di quale numero abbiamo aggiunto alla somma finora e dobbiamo terminare il ciclo quando tutti i numeri sono stati aggiunti. E' leggermente più semplice aggiungere i numeri nell'ordine inverso, partendo da 100. In questo modo, possiamo utilizzare l'istruzione JMZ per uscire dal ciclo quando il numero che deve essere aggiunto arriva a zero.

```
Location        Instruction                      Label      Instruction
--------        -----------                      ------     -------------
0                LOC-C 100                                  LOC-C 100
1                STO   13                                   STO   Num           
2                LOD-C 0                                    LOD-C 0
3                STO   14                                   STO   Ans
4                LOD   14                         Loop:     LOD   Ans                        
5                ADD   13                                   ADD   Num
6                STO   14                                   STO   Ans
7                LOD   13                                   LOD   Num
8                DEC                                        DEC
9                JMZ   12                                   JMZ   Done  
10               STO   13                                   STO   Num
11               JMP   4                                    JUMP  Loop
12               HLT                               Done:    HLT
                                                   Num:     data
                                                   Ans:     data
```

[Prima versione](./SOMMA_PRIMI_CENTO_NUMERI.txt) e la versione
con le etichette [Versione con etichette](./SOMMA_PRIMI_CENTO_NUMERI_CON_ETICHETTE.txt)

## Programma lista inversa

Ora prova a scrivere un programma che dato una lista di numeri memorizzati da una certa cella di memoria e terminanti con la cella con valore 0 per indicare la fine della lista, li metta in ordine inverso di inserimento. Quindi se per esempio la lista1 ha i numeri 8 9 3 21 17 6 2 91 0 allora la lista ottenuta chiamiamola lista2 deve essere 91 2 6 17 21 3 9 8 0 (sempre lo 0 per indicare l'ultimo elemento). Riesci a produrre un programma per xComputer?

[Soluzione di Gemini](https://gemini.google.com/share/dba16be891bc)

La soluzione proposta da Gemini ha avuto bisogno di alcune correzioni per far funzionare correttamente 
il programma.

```

; **********************************************************
; FASE 1: Trova la Fine della LISTA1 (Punta a dove va lo 0)
; **********************************************************

    lod-c LISTA1 ; Carica l'indirizzo di inizio lista 
    sto P_ORIG_R ; P_ORIG_R (Puntatore Originale, R come Read/Lettura)
    
trova_fine:
    lod-i P_ORIG_R ; Carica il valore della cella puntata
    jmz prepara_copia ; Se il valore è 0, abbiamo trovato la fine (il puntatore è già posizionato sullo 0)
    
    lod P_ORIG_R ; Altrimenti, incrementa il puntatore
    inc
    sto P_ORIG_R
    jmp trova_fine

; A questo punto, P_ORIG_R punta alla cella che contiene il valore 0.

; **********************************************************
; FASE 2: Prepara i Puntatori per la Copia Inversa
; **********************************************************
prepara_copia:
    
    ; 1. P_ORIG_W (Puntatore Originale, W come Write/Scrittura)
    ; Deve puntare all'ULTIMO ELEMENTO VALIDO della LISTA1
    lod P_ORIG_R ; Parte da dove c'è lo 0
    dec          ; Decrementa: ora punta all'ultimo elemento valido (es. 91)
    sto P_ORIG_W ;
    
    ; 2. P_NUOVA_R (Puntatore Nuova Lista, R come Read/Lettura)
    ; Inizializza il puntatore della lista di destinazione
    lod-c START_LISTA2
    sto P_NUOVA_R
    
; **********************************************************
; FASE 3: Copia gli Elementi in Ordine Inverso
; **********************************************************
copia_inversa:

    ; 1. Verifica se abbiamo superato l'inizio della LISTA1
    ; L'indirizzo di P_ORIG_W è minore dell'indirizzo di LISTA1?
    lod-c LISTA1
    sub P_ORIG_W ; AC = LISTA1 - P_ORIG_W
    jmn copia_elemento ; Se il risultato è negativo (P_ORIG_W > LISTA1), copia l'elemento
    
    jmz copia_elemento ; Se il risultato è zezo (P_ORIG_W == LISTA1), copia l'elemento
    
    ; Altrimenti, P_ORIG_W ha raggiunto o superato l'inizio della lista.
    jmp fine_ciclo ; Termina il ciclo

copia_elemento:
    ; 2. Copia il valore
    lod-i P_ORIG_W ; Carica l'elemento puntato dall'originale (es. 91)
    sto-i P_NUOVA_R ; Scrivilo nella nuova lista
    
    ; 3. Aggiorna i puntatori
    
    ; Decrementa P_ORIG_W (passa all'elemento precedente)
    lod P_ORIG_W
    dec
    sto P_ORIG_W
    
    ; Incrementa P_NUOVA_R (passa alla cella successiva nella lista di destinazione)
    lod P_NUOVA_R
    inc
    sto P_NUOVA_R
    
    jmp copia_inversa ; Ripeti il ciclo

; **********************************************************
; FASE 4: Termina il Programma e Aggiunge lo 0 Finale
; **********************************************************
fine_ciclo:

    ; Aggiunge lo 0 finale alla LISTA2 (dove punta P_NUOVA_R)
    lod-c 0
    sto-i P_NUOVA_R

    hlt ; Fine del programma

; **********************************************************
; Dati e Variabili
; **********************************************************
P_ORIG_R: data ; Indirizzo: Puntatore alla LISTA1 (usato per trovare lo 0)
P_ORIG_W: data ; Indirizzo: Puntatore alla LISTA1 (usato per leggere all'indietro)
P_NUOVA_R: data ; Indirizzo: Puntatore alla LISTA2 (usato per scrivere)

; Inizio della lista originale
LISTA1: 8
        9
        3
        21
        17
        6
        2
        91
        0 ; Sentinella (fine lista)

; Inizio della lista di destinazione
START_LISTA2: data ; Qui verrà scritta la lista invertita

```

Il programma corretto [CREA_LISTA_INVERSA](./CREA_LISTA_INVERSA.txt) e versione Gemini 
[CREA_LISTA_INVERSA_VERSIONE_GEMINI](./CREA_LISTA_INVERSA_GEMINI.txt) (ERRATA!).

