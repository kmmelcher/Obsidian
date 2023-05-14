[[Concorrente]]

```java

AtomicBoolean ab = new AtomicBoolean(false);

void lock(){
	while(true){
		while(ab.get()); // Espere enquanto alguém está na região crítica
		if (!ab.getAndSet(true)){ // Pegue o valor e defina como true atômicamente
			return; // Sai do loop e entra na região crítica
		}
	}
}

void unlock(){
	ab.set(false);
}

```

**Garantia**
- Exclusão mútua para N threads

**Vantagens**

O algoritmo TTAS tem uma perfomance muito melhor que o TAS, porque ele faz muito mais operações de leitura, do que de escrita. A latência com o aumento do número de threads pode ser visto no gráfico:

![[Pasted image 20230513213151.png]]

Esse ganho acontece, porque enquanto uma thread está bloqueada esperando ela vai repetidamente realizar a operaçõa de leitura (*get*), que é uma simples consulta na cache do processador. Isso, além de ser rápido, não vai requisitar de um acesso ao barramento. Deixando-o livre para que outras threads, que realmente precisem um um recurso computacional significante usem. 

**Tudo resolvido, certo? Quase lá...**

Infelizmente o TTAS ainda tem um problema de performance. Quando uma thread sai da região crítica, todas as outras que estavam esperando iniciam uma corrida para chamar a função *getAndSet*. A primeira que conseguir irá entrar na região crítica e todas as outras terão seus caches inválidados até iniciar uma nova fase de espera. 

**Como melhorar?**

```java

void lock(){
	while(true){
		while(ab.get());
		if (!ab.getAndSet(true)){
			return;
		}
		int t = random();
		sleep(t);
	}
}

```

As threads que conseguiram sair do primeiro while, mas não foram a primeira a chamar a função de getAndSet dormem por um tempo *t* randômico antes de atualizar seu cache, para evitar que ela cause um congestionamento no sistema. O estudo sobre qual deve ser o tempo *t* será estudado mais na frente na disciplina.