# BarberFlow --- Plano de Agendamento

## Sobre o projeto

O BarberFlow é um site para barbearia onde o próprio cliente poderá
fazer o agendamento, sem precisar marcar o horário por fora do site.

A ideia é deixar o processo simples: o cliente escolhe o serviço, o
barbeiro, a data e um horário que esteja disponível. Depois de validar
as informações, o agendamento será confirmado automaticamente se o
horário estiver livre.

## Como pretendo que o agendamento funcione

1.  O cliente entra no site e faz o cadastro ou acessa sua conta.
2.  Escolhe o serviço que deseja fazer.
3.  Escolhe o barbeiro, a data e um horário disponível.
4.  Preenche os dados pedidos no formulário e, se precisar, coloca uma
    observação.
5.  O sistema verifica se o barbeiro está aprovado, se trabalha naquele
    horário e se não tem folga, férias, intervalo ou outro bloqueio.
6.  O sistema também verifica se o serviço cabe no horário escolhido,
    considerando a duração dele.
7.  Se estiver tudo certo, o agendamento é confirmado automaticamente.
8.  O cliente poderá consultar os detalhes e o status do agendamento e,
    conforme as regras do sistema, cancelar o agendamento.

## Disponibilidade dos horários

Os horários não devem ficar fixos no HTML. Eles deverão ser carregados
de acordo com a disponibilidade real do barbeiro e as informações salvas
no banco de dados.

Preciso considerar o expediente do barbeiro, os intervalos, o horário de
almoço, as folgas, as férias e outros períodos em que ele não poderá
atender. Também preciso considerar a duração de cada serviço, para não
marcar um atendimento em um horário que entre em conflito com outro.

Um barbeiro poderá oferecer vários serviços, e um mesmo serviço poderá
ser oferecido por mais de um barbeiro. Por isso, a relação entre
barbeiros e serviços será de muitos para muitos (N:N).

## Evitar dois agendamentos no mesmo horário

Uma parte importante do projeto é impedir que dois clientes consigam
reservar horários incompatíveis ao mesmo tempo.

Por exemplo: dois clientes podem consultar o site quase no mesmo
instante e ver o horário das 14h como livre. Se os dois tentarem
confirmar, não basta o sistema ter consultado a disponibilidade antes. O
horário precisa ser validado novamente de uma forma segura na hora de
salvar o agendamento.

Quero usar transações e as regras de integridade do PostgreSQL para
evitar conflitos e impedir que sejam confirmados dois agendamentos que
se sobreponham para o mesmo barbeiro. Também preciso tratar o caso em
que o banco rejeita uma tentativa de reserva porque outro cliente
conseguiu confirmar primeiro.

Os intervalos dos agendamentos devem ser tratados de forma que um
atendimento possa terminar exatamente quando o próximo começar, sem
considerar isso um conflito. Por exemplo, um atendimento das 14h às
14h30 pode ser seguido por outro que comece às 14h30, desde que as
demais regras de disponibilidade sejam respeitadas.

## Cadastro de clientes e barbeiros

O site terá uma página de cadastro para clientes e outra para barbeiros.

O cadastro de barbeiro não ficará ativo automaticamente: ele precisará
ser aprovado pelo administrador antes de poder receber agendamentos.

O barbeiro deverá poder informar os serviços que oferece e sua
disponibilidade, incluindo os horários de trabalho e os períodos em que
não estará atendendo.

## Regras e cuidados do sistema

-   O cliente escolhe o barbeiro e o serviço que deseja.
-   O agendamento só é confirmado quando o horário estiver disponível e
    as validações forem aprovadas.
-   O sistema não deve permitir agendamentos em horários de almoço,
    folga, férias ou outros bloqueios do barbeiro.
-   O preço oficial do serviço deverá ser consultado no banco de dados
    pelo backend. Não posso confiar em um preço enviado pelo navegador,
    porque os dados do HTML podem ser alterados pelo usuário.
-   O cliente deverá conseguir consultar os detalhes e o status do
    agendamento.
-   O sistema deverá respeitar as permissões de cada tipo de usuário,
    como administrador, recepcionista, barbeiro e cliente.
-   Cancelamentos e alterações precisam respeitar as regras que forem
    definidas para o projeto.
-   A confirmação automática não pode causar dois agendamentos
    incompatíveis para o mesmo barbeiro.

## Tecnologias

-   HTML para a estrutura das páginas.
-   CSS para a aparência e a responsividade.
-   JavaScript para as interações no navegador.
-   Node.js e Express para o backend.
-   PostgreSQL para armazenar os dados e proteger a integridade dos
    agendamentos.

## Estado atual

Neste momento, estou desenvolvendo a estrutura das páginas com HTML e
CSS. O formulário já está sendo preparado para receber os dados do
agendamento. A parte de JavaScript, o backend e a integração com o
PostgreSQL ainda serão desenvolvidos para que a disponibilidade seja
dinâmica e as regras de agendamento sejam validadas de verdade.

## Objetivo

Meu objetivo é construir um sistema de agendamento que seja simples para
o cliente, organizado para a barbearia e seguro para evitar erros de
disponibilidade e conflitos entre reservas.
