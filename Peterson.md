[[Concorrente]]

```java

int victim = -1;
boolean flag = new boolean[2];

void lock() {
	int id = thread.getId();
	int otherId = 1 - id;

	flag[id] = true; // Eu estou interessado
	victim = id;     // Se você também estiver, pode ir primeiro
	while (flag[otherId] AND victim == id); // Espero minha vez
}

void unlock() {
	int id = thread.getId();
	flag[id] = false; // Não estou interessado
}

```

**Estratégia**
	Combina as estratégias de LockOne e LockTwo.
	Quando uma thread chama o Lock ela levanta uma bandeira dizendo que está interessada. Depois,  ela se torna a vítima, de forma que ela educadamente cede a vez para a outra thread se a outra também estiver interessada. Caso a outra não tenha interesse ela entra na região crítica.
	Quando uma thread chama o unlock ela desce sua bandeira dizendo que ela não está mais interessada em entrar na região crítica.

**Garantia**
- Exclusão mútua
- Starvation free
- Deadlock-free

**Restições**
- Só funciona para duas threads