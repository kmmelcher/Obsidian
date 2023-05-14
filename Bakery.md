[[Concorrente]]

```java

boolean flags[N];
int tickets[N]; // Todos os tickets são inicializados com zero

void lock(){
	int i = thread.getId();
	
	// Sorteia seu ticket
	flag[i] = true;
	for (int j=0; j<N; j++){
		if (tickets[j] > tickets[i]) {
			tickets[i] = tickets[j];
		}
	}
	tickets[i]++;
	flag[i] = false;

	// Espera sua vez
	for (int j=0; j<N; j++){
		while (flags[j]); // Espere enquanto ele estiver na fase de sorteio
		while (
			(tickets[j] != 0 AND tickets[j] < tickets[i]) OR
			(tickets[i] == tickets[j] AND j < i)
		);	
	}
}

void unlock(){
	int i = thread.getId();
	tickets[i] = 0;
}
```

**Estratégia**
	José chega na padaria para comprar um pão e pega um ticket. De início, seu ticket é um papel em branco, então José precisa descobrir seu número. Para isso, ele vai levantar a mão e perguntar a cada cliente da padaria o número de seu ticket. Depois de perguntar a todos, ele vai escrever no seu ticket o maior número mais um, então ele irá abaixar a mão. Agora José precisa esperar a sua vez. Para isso, ele novamente olha o ticket de cada cliente que está na padaria um a um. Quando ele vê que um cliente que está com a mão levantada, ou que tem um número menor que o dele ele aguarda ao seu lado, porque não é sua vez ainda. Se ele ver alguém com o mesmo ticket que o dele, ele educadamente cede a vez. Depois que José viu os tickets de todo mundo ele tem certeza que é a vez dele. Então josé pega seu pão. Depois de pegar seu pão, josé vai embora e joga seu ticket no lixo com raiva da história toda.
f 
**Garantia**
- Exclusão mútua para N threads
- Livre de deadlocks
- Justiça (O primeiro a chegar, será o primeiro a ser atendido)
- Livre de starvation
