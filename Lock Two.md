[[Concorrente]]

```java

int victim = -1

void lock() {
	int id = thread.getId();
	victim = id;
	while (victim == id);
}

void unlock() { }

```

**Estratégia**
	Usa uma única vítima para ficar em espera. Quando uma thread chama o lock, ela se torna a vítima e então espera até que outra thread chegue e se torne a próxima vítima.

**Garantia**
	Garante exclusão mútua para duas threads

**Restições**
	Só funciona para duas threads e uma thread sempre fica travada, por isso só funciona de forma concorrente, porque se for sequencial a segunda thread ficaria eternamente esperando pela primeira.
