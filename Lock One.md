[[Concorrente]]


```java

boolean flag = new boolean[2];

void lock() {
	int id = thread.getId();
	int otherId = 1 - id;
	flag[id] = True;
	while (flag[otherId])
}

void unlock() {
	int id = thread.getId();
	flag[id] = false;	
}

```

**Estratégia**
	 Uma thread define sua flag como True e espera até a outra thread ficar com sua flag false. A thread libera a região crítica definindo sua flag como false.

**Garantia**
	Garante exclusão mútua contando que uma thread chegue depois da outra

**Restições**
	Só funciona para duas threads

**Problema**
	Gera deadlock se ambas as threads escreverem True na sua flag antes de executarem o while. Dessa forma, as duas threads ficaram eternamente no loop presas.
