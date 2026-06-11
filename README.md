Relatório: Central de Sensores IoT (Estufa)

1. Arquitetura e Paradigma

O sistema foi mudado de procedural para a Orientação a Objetos (POO), utilizando:

Abstração: Classe mãe Sensor para definir a base dos dispositivos.

Interfaces: Classe ConectavelIoT como contrato para garantir o método enviar_dados_rede().

Herança e Polimorfismo: As classes filhas (Temperatura, Umidade e pH) herdam de Sensor e são tratadas de forma genérica pela CentralDeControle.

2. Princípio de Substituição de Liskov (LSP)

O código respeita o LSP ao garantir que as classes filhas possam substituir a classe mãe sem quebrar o sistema.

Prova: Cada sensor possui Invariantes (limites físicos). Se um valor inválido é enviado (ex: pH 20), o sensor bloqueia a alteração e mantém o estado íntegro. Isso prova que as subclasses estendem o comportamento da mãe sem violar suas regras de consistência.
