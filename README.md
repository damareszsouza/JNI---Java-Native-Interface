# JNI---Java-Native-Interface


Lançar um processo do sistema operacional di-
retamente do código Java já é possível através das 
chamadas Java Native Interface (JNI), mas com isso é 
bem provável se deparar com resultados inesperados 
ou exceções sérias mais para a frente. Ainda assim, é 
um mal necessário. 
Além disso, os processos têm mais um lado desa-
gradável – o fato de tenderem a ficar suspensos. O 
problema ao iniciar um processo a partir do código 
Java até a versão 7 tem sido difícil o controle sobre um 
processo uma vez iniciado.
Para ajudar nessa tarefa o Java 8 traz três novos 
métodos na classe “Process”:

1. destroyForcibly() termina um processo com 
um grau de sucesso bem maior do que antes;
2. isAlive() informa se um processo iniciado ainda 
está vivo;
3. Uma nova sobrecarga de waitFor() especifica a 
quantidade de tempo para que o processo termine. 

Isso retorna ou o sucesso da execução ou o time-out, 
neste último caso é possível terminar o processo.
Duas boas utilizações para estes métodos são:
• Caso um processo não termine no tempo deter-
minado, forçar seu término e seguir em diante:


if (process.wait(MY_TIMEOUT, TimeUnit.MILLISECONDS)){
 //success! 
} else {
 process.destroyForcibly();
}


• Garantir que, antes que um código termine, não 
seja deixado nenhum processo para trás. Proces-
sos suspensos certamente esgotarão seu sistema 
operacional, mesmo que lentamente.


for (Process p : processes) {
 if (p.isAlive()) {
 p.destroyForcibly();
 }
}


recursos que funcionam corretamente em pré-produ-
ção, podem começar a se comportarem de maneira 
completamente estranha de repente, quando as ope-
rações sofrem o overflow e produzem valores ines-
perados.
Para auxiliar nesse problema, o Java 8 adicionou 
diversos métodos “exact” à classe Math, protegendo 
um código suscetível a overflows, através do lança-
mento de “uncheckedArithmeticException” quando 
o valor de uma operação causa um overflow.

int safeC = Math.multiplyExact(bigA, bigB); 
// Será lançada uma ArithmeticException caso o 
resultado exceda +-2^31

O único aspecto negativo disso é que a responsa-
bilidade em identificar como o código pode ter pro-
blemas com overflow fica por conta de quem está de-
senvolvendo. Não há solução mágica, porém isso já é 
melhor que nada.
