Nome: Miguel Mazuim da Silva / Disciplina: Programação Paralela

Parte I: Pthreads

1)Particionamento: É representado por esta parte do código:
" typedef struct 
 {
   double *a;
   double *b;
   double c; 
   int wsize;
   int repeat; 
 } dotdata_t;", onde é definido toda estrutura de como vai ser separado os dados.
 
 Comunicação:Toda comunicação entre as threads é definida na função dotprod_threads, onde é feito o controle delas, desde da sua criação até a união de todas e deletar.
 
 Aglomeração:É representado por este código:
 "  pthread_mutex_lock (&mutexsum);
   dotdata.c += mysum;
   pthread_mutex_unlock (&mutexsum);
"  e

" for (i = 0; i < nthreads; i++) {
      pthread_join(threads[i], NULL);
   }
", Serve principalmente para impedir a condição de corrida e unir os resultados.

 Mapeamento: É feito em dois trechos do código, toda a função dotprod_worker, onde é realizado os calculos e este trecho:
 "for (i = 0; i < nthreads; i++) {
      pthread_create(&threads[i], &attr, dotprod_worker, (void *) i);
   }
", onde é criado e destribuido as threads.

2)Na teoria, deveria ser acelerado, pois com as threads é feito o processo em paralelo. Mas quando eu executei, com cada thread que se adicionava, o processo demorava cerca de meio segundo a mais (para o caso de 1000000 de tamanho com 2000 repetições).

3 e 4) 
O tempo diminui se caso reduzir o tamanho ou as repetições, porém tem perda de desempenho se caso aplicar mais threads.
| threads | size    | repetitions | usec    | 
|---------|---------|-------------|---------| 
| 1       | 1000000 | 2000        | 5577281 | 
| 2       | 1000000 | 2000        | 5610864 | 
| 3       | 1000000 | 2000        | 5711905 | 
| 4       | 1000000 | 2000        | 5677085 | 
| 5       | 1000000 | 2000        | 6029346 | 
| 6       | 1000000 | 2000        | 6265394 | 
| 1       | 500000  | 2000        | 2799390 | 
| 2       | 500000  | 2000        | 2820662 | 
| 3       | 500000  | 2000        | 2811438 | 
| 4       | 500000  | 2000        | 2853940 | 
| 5       | 500000  | 2000        | 2885253 | 
| 6       | 500000  | 2000        | 3052700 |
| 1       | 500000  | 1000        | 1417319 | 
| 2       | 500000  | 1000        | 1417995 | 
| 3       | 500000  | 1000        | 1420447 | 
| 4       | 500000  | 1000        | 1427474 | 
| 5       | 500000  | 1000        | 1528584 | 
| 6       | 500000  | 1000        | 1617952 | 



5)A diferença principal entre os programas são essas linhas "pthread_mutex_lock (&mutexsum);" e "pthread_mutex_unlock (&mutexsum);". Elas fazem o controle da sincronia das threads, evitando o caso de condição de corrida, onde duas thread podem pegar o valor e sobreescrever o resultado da outra. Se for retirado essas linhas, a condição de corrida vai acontecer.
