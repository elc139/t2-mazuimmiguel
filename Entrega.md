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

2)

3)

4)
|                        | 
|------------------------| 
| size,repetitions,usec  | 
| 1,1000000,2000,5227948 | 
| 2,500000,2000,2580534  | 


5)A diferença principal entre os programas são essas linhas "pthread_mutex_lock (&mutexsum);" e "pthread_mutex_unlock (&mutexsum);". Elas fazem o controle da sincronia das threads, evitando o caso de condição de corrida, onde duas thread podem pegar o valor e sobreescrever o resultado da outra. Se for retirado essas linhas, a condição de corrida vai acontecer.
