# TAS

```java

AtomicBoolean ab = new AtomicBoolean(false);

void lock() {
	while (ab.getAndSet(true));
}

void unlock() {
	ab.set(false);
}

```

**Atomic Boolean**
Esse é um objeto que possui dois métodos: getAndSet e set.
- getAndSet(v: bool)
  Define um novo valor booleano v e retorna o antigo valor. Essa operação é atômica, ou seja, uma thread não pode perder a CPU enquanto a executa.
- set(v: bool)
  Define um novo valor booleano v.

**Estratégia**
Um boleano atômico é criado com valor inicial falso. Quando uma thread chama o lock ela define o valor do booleano para true e retorna o valor antigo, que inicialmente era falso. Então a primeira thread sai do while, entrando na região crítica. Ao chegar uma segunda thread, ela ira definir **novamente** o seu valor para true e retornar o valor antigo, que no caso é true. Dessa forma a segunda thread fica presa no while. Por fim, quando a primeira thread sai da região crítica, ela define seu valor para falso. Liberando a entrada da segunda thread.

**Garantia**
- Exclusão mútua para N threads

**Problema**
	Lento para muitas threads. A latência aumenta exponencialmente com o aumento da quantidade de threads.
	![[Pasted image 20230513211951.png]]
	Mas porquê isso acontece?
	A resposta está em um problema mais embaixo: no barramento do processador.
	O processador se comunica com a memória por meio de um barramento.
	Esse barramento é compartilhado entre as CPUs.
	Quando um CPU está usando o barramento ele fica ocupado e não pode ser usado por outro.
	Para evitar muitas consultas a memória que são lentas e entopem o barramento, cada CPU possui uma memória cache pequena. Ao fazer uma leitura de uma variável a CPU guarda no cache. E assim também fazem os outros CPUs que lerem essa variável. Contudo, se um CPU faz um operação de escrita todos os outros caches são invalidádos para garantir a consistência do dado. Trazendo esse contexto para a nossa implementação do TAS, nós temos uma variável que será compartilhada entre as CPUs: ab. E há muitas operações de escritas desnecessárias, que atualizamos o valor de ab de true para true, ou seja, não alteramos o valor. Como resolver isso? TTAS.

