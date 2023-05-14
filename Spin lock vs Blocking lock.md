# Spin lock vs Blocking lock

**Spin lock**
Também chamado de busy-wait (ou espera ocupada em português), é uma implementação de semáforos, que fazem com que uma thread fique esperando até poder ser sua vez de avançar.

**Blocking lock**
É uma estratégia de implementação de semáforos, que muda o estado da thread para bloqueado, até que seja sua hora. Então ela passa do estado de pronto para rodar e rodando.

![[Pasted image 20230514144939.png]]

| | Spin lock | Blocking lock |
|--| ---------  | ------------- |
| Prós | A mudança para entrar na região crítica é mais rápida  | Não há desperdício de processamento e pode ser usado com uma única CPU |
| Contras | Desperdício de tempo de processamento. Demanda mais de uma CPU. | Mudança para entrar na região crítica lenta, por causa da troca de contexto |
| Uso | Regiões críticas curtas  | Regiões críticas longas |]
| Exemplo | LinkedLists  | Chamadas de API |


