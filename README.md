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


