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

2)A aceleração foi grande, quase duplicando o desempenho e reduzindo o tempo pela metade.

3 e 4) 
A aceleração se sustenta em todos os casos, tanto para poucas repeticoes com tamanho reduzido e ainda mais para muitas repetiçóes.
| threads | tamanho | repetições  | usec    | 
|---------|---------|-------------|---------| 
| 1       | 1000000 | 2000        | 5577281 | 
| 2       | 500000  | 2000        | 2819439 | 
| 4       | 250000  | 2000        | 1482222 | 
| 6       | 166667  | 2000        | 1026981 | 
| 1       | 6000000 | 2000        | 33640613| 
| 2       | 3000000 | 2000        | 16912392| 
| 4       | 1500000 | 2000        | 8784468 | 
| 6       | 1000000 | 2000        | 6378443 |
| 1       | 600000  | 1000        | 1694027 | 
| 2       | 300000  | 1000        | 849157  | 
| 4       | 150000  | 1000        | 430100  | 
| 6       | 100000  | 1000        | 312729  | 
| 1       | 6000000 | 4000        | 67352580 | 
| 2       | 3000000 | 4000        | 33648448 | 
| 4       | 1500000 | 4000        | 17397380 | 
| 6       | 1000000 | 4000        | 12884853 | 
| 1       | 6000    | 1000000     | 16737025 | 
| 2       | 3000    | 1000000     | 8360447 | 
| 4       | 1500    | 1000000     | 4258185 | 
| 6       | 1000    | 1000000     | 3050845 | 

E o grafico de speedup, onde cada série de teste mostrou um padrão, onde o desempenho quase duplicava com a adição de novas threads.
 <img src="speedup+.jpg">

5)A diferença principal entre os programas são essas linhas "pthread_mutex_lock (&mutexsum);" e "pthread_mutex_unlock (&mutexsum);". Elas fazem o controle da sincronia das threads, evitando o caso de condição de corrida, onde duas thread podem pegar o valor e sobreescrever o resultado da outra. Se for retirado essas linhas, a condição de corrida vai acontecer.

Parte 2: OpenMP
