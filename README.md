# Hotel Terabithia: Plataforma Interna de Operações Hoteleiras

## 1) Visão Geral

Neste projeto, você desenvolverá uma **plataforma interna de gestão** para uso de colaboradores do hotel (não voltada a hóspedes), aplicando arquitetura **modular** com subprogramas independentes.

Linguagens permitidas:

- Java
- Python
- JavaScript
- Kotlin

> Recomendação: execute o código-base antes das alterações para entender o fluxo inicial.

---

## 2) Objetivos de Aprendizagem

- Decompor um sistema em módulos coesos.
- Projetar fluxo de navegação com menu e retorno ao contexto principal.
- Implementar validações robustas de entrada e tratamento de erro.
- Trabalhar com estruturas de dados em memória.
- Produzir saída organizada e consistente para auditoria operacional.

---

## 3) Requisitos Globais do Sistema

## 3.1 Identidade, autenticação e sessão

1. Defina o nome do hotel e use-o no lugar de `{Hotel}` em todas as mensagens.
2. Ao abrir o sistema, exiba:
   - `Bem-vindo ao {Hotel}`
3. Solicite nome do usuário e senha.
4. Senha obrigatória: `2678`.
5. Permita no máximo **3 tentativas de senha**.
   - Ao exceder: encerrar sistema com mensagem de bloqueio.
6. Com login válido, exiba:
   - `Bem-vindo ao Hotel {Hotel}, {Nome}. É um imenso prazer ter você por aqui!`

## 3.2 Menu principal e organização

- Todas as opções inválidas devem ser tratadas por função dedicada de erro.
- Ao finalizar qualquer subprograma, retornar automaticamente ao menu principal.
- Ao sair:
  - `Muito obrigado e até logo, {Nome}.`

Menu mínimo:

1. Reservas de Quartos
2. Cadastro de Hóspedes
3. Eventos
4. Ar-Condicionado
5. Abastecimento
6. Relatórios Operacionais
7. Sair

## 3.3 Regras de implementação

- O hotel possui **20 quartos**, inicialmente livres.
- Utilize funções/métodos separados por responsabilidade.
- Não usar variáveis globais sem justificativa.
- Padronizar mensagens e formatação monetária.
- Estruturas em memória são suficientes (sem banco).

---

## 4) Subprograma 1 — Reservas de Quartos (nível avançado)

## 4.1 Requisitos

1. Solicitar valor da diária e quantidade de diárias.
2. Validar:
   - diária > 0
   - diárias entre 1 e 30
3. Em caso inválido:
   - `Valor inválido, {Nome}`
   - retornar ao menu principal.
4. Solicitar nome completo do hóspede.
5. Solicitar tipo de quarto:
   - `S` (standard): fator 1.00
   - `E` (executivo): fator 1.35
   - `L` (luxo): fator 1.65
6. Solicitar quarto de 1 a 20 e validar ocupação.
   - Se ocupado, listar quartos livres e solicitar novo número.
7. Calcular:
   - subtotal = diária × diárias × fator do tipo
   - taxa de serviço = 10% do subtotal
   - total final = subtotal + taxa
8. Confirmar reserva (`S/N`).
9. Se confirmada:
   - ocupar quarto
   - registrar reserva em memória (hóspede, quarto, diárias, total)
10. Exibir mapa de quartos em grade 4x5 com status:
    - `L` = livre
    - `O` = ocupado

## 4.2 Exemplo de execução (formatado)

```text
[Reservas]
Informe o valor da diária: 200
Informe a quantidade de diárias (1-30): 3
Informe o nome do hóspede: Carla Souza
Tipo de quarto (S/E/L): E
Escolha um quarto (1-20): 7

Resumo:
Hóspede: Carla Souza
Quarto: 7 (Executivo)
Subtotal: R$ 810,00
Taxa de serviço (10%): R$ 81,00
Total: R$ 891,00

{Nome}, confirma a reserva? (S/N): S
Reserva efetuada com sucesso.
```

Segundo Exemplo

```text
[Reservas]
Informe o valor da diária: 200
Informe a quantidade de diárias (1-30): 3
Informe o nome do hóspede: Carla Souza
Tipo de quarto (S/E/L): E
Escolha um quarto (1-20): 7

Resumo:
Hóspede: Carla Souza
Quarto: 7 (Executivo)
Subtotal: R$ 810,00
Taxa de serviço (10%): R$ 81,00
Total: R$ 891,00

{Nome}, confirma a reserva? (S/N): N
Reserva não efetuada.
```

---

## 5) Subprograma 2 — Cadastro de Hóspedes (com índices)

## 5.1 Requisitos

Menu interno:

1. Cadastrar
2. Pesquisar por nome exato
3. Pesquisar por prefixo
4. Listar ordenado (A-Z)
5. Atualizar cadastro
6. Remover cadastro
7. Voltar

Regras:

- Capacidade máxima: **15 hóspedes ativos**.
- Não permitir cadastro duplicado por nome exato.
- Pesquisa por prefixo deve retornar todos os nomes iniciados pelo texto informado.
- Listagem deve exibir índice, nome e data/hora de cadastro.
- Atualização e remoção devem ser feitas por índice da listagem.

Mensagens mínimas:

- `Máximo de cadastros atingido`
- `Hóspede já cadastrado`
- `Hóspede não encontrado`
- `Operação realizada com sucesso`

## 5.2 Exemplo de execução (formatado)

```text
[Cadastro de Hóspedes]
1-Cadastrar 2-Pesquisar exato 3-Pesquisar prefixo 4-Listar 5-Atualizar 6-Remover 7-Voltar
Opção: 1
Nome do hóspede: Rosani Albuquerque
Hóspede cadastrado com sucesso.

Opção: 3
Prefixo: Ro
Resultados:
[1] Rosani Albuquerque
```

---

## 6) Subprograma 3 — Eventos (pipeline completo)

> Este subprograma deve executar as etapas abaixo em sequência, com validação por etapa.

## 6.1 Parte A — Capacidade e seleção de auditório

Auditórios:

- **Laranja**: 150 lugares + 70 cadeiras extras (máx. 220)
- **Colorado**: 350 lugares

Validação:

- convidados < 0 → inválido
- convidados > 350 → capacidade excedida

Regra de escolha:

1. Priorizar Laranja quando possível.
2. Exibir número de cadeiras adicionais quando necessário.

## 6.2 Parte B — Agenda e disponibilidade

Janela de reserva:

- Segunda a sexta: 07h–23h
- Sábado e domingo: 07h–15h

Solicitar dia, hora inicial e duração (horas).

Validações adicionais (mais difícil):

- `hora inicial` deve ser inteiro.
- `duração` entre 1 e 12 horas.
- evento deve terminar dentro da janela do dia.

Se disponível, solicitar nome da empresa e registrar agenda.

## 6.3 Parte C — Equipe de garçons

Regras:

- Base: `ceil(convidados / 12)`
- Reforço por duração: `floor(duração / 2)`
- Total garçons = base + reforço
- Custo garçom/hora = R$ 10,50

Cálculo:

- `custo_garcons = total_garcons × duração × 10,50`

## 6.4 Parte D — Buffet

Consumo por convidado:

- Café: 0,2 L
- Água: 0,5 L
- Salgados: 7 un

Custos unitários:

- Café: R$ 0,80/L
- Água: R$ 0,40/L
- Salgados: R$ 34,00 por cento

Calcular quantidades e custo de buffet.

## 6.5 Parte E — Relatório e decisão

Exibir relatório técnico:

- auditório, empresa, data, hora início/fim
- convidados, garçons, duração
- custo garçons, custo buffet, total geral

Perguntar confirmação (`S/N`).

- `S`: `Reserva efetuada com sucesso.`
- `N`: `Reserva não efetuada.`

## 6.6 Exemplo de execução (formatado)

```text
[Eventos]
Convidados: 192
Auditório selecionado: Laranja (42 cadeiras adicionais)

Dia: segunda
Hora inicial: 13
Duração: 8
Empresa: Lojas Transilvânia
Status: Auditório reservado.

Garçons necessários: 20
Custo com garçons: R$ 1.680,00

Buffet:
Café: 38,4 L
Água: 96,0 L
Salgados: 1344 un
Custo buffet: R$ 540,96

Total do evento: R$ 2.220,96
Confirmar reserva? (S/N): S
Reserva efetuada com sucesso.
```

---

## 7) Subprograma 4 — Ar-Condicionado (comparativo técnico)

## 7.1 Requisitos

Permitir entrada de múltiplas empresas. Para cada uma, solicitar:

1. Nome da empresa
2. Valor por aparelho
3. Quantidade de aparelhos
4. Percentual de desconto
5. Quantidade mínima para desconto
6. Valor fixo de deslocamento (nova exigência)

## 7.2 Regras de cálculo

- `bruto = valor_aparelho × quantidade`
- aplicar desconto somente se `quantidade >= minimo`
- `total = bruto - desconto + deslocamento`

Exibir:

- `O serviço de {empresa} custará R$ {total}`

Ao final, apresentar:

- menor orçamento
- maior orçamento
- diferença percentual entre melhor e pior proposta

## 7.3 Exemplo de execução (formatado)

```text
[Ar-Condicionado]
Empresa: Clima Forte
Valor por aparelho: 120
Quantidade: 10
Desconto (%): 8
Mínimo para desconto: 8
Deslocamento: 150
Total: R$ 1.254,00

Deseja informar novos dados, {Nome}? (S/N): N
Melhor orçamento: Clima Forte — R$ 1.254,00
```

---

## 8) Subprograma 5 — Abastecimento (análise econômica)

Postos conveniados:

- Wayne Oil
- Stark Petrol

Entradas por posto:

- preço do álcool
- preço da gasolina

## 8.1 Regras

- Tanque fixo: **42 litros**.
- Etanol é vantajoso se `preço_etanol <= 0,70 × preço_gasolina`.
- Determinar combustível ideal para cada posto.
- Calcular custo final de 42 L em cada cenário.
- Exibir ranking de menor para maior custo.

## 8.2 Exemplo de execução (formatado)

```text
[Abastecimento]
Wayne Oil -> Álcool: 4,20 | Gasolina: 5,82
Stark Petrol -> Álcool: 4,35 | Gasolina: 6,17

Wayne Oil: melhor opção = Gasolina | Total (42L) = R$ 244,44
Stark Petrol: melhor opção = Gasolina | Total (42L) = R$ 259,14

{Nome}, é mais barato abastecer com gasolina no posto Wayne Oil.
```

---

## 9) Subprograma 6 — Relatórios Operacionais (novo)

Este módulo deve consolidar dados registrados em memória durante a execução:

1. Total de reservas de quartos confirmadas
2. Taxa de ocupação atual (`quartos_ocupados / 20`)
3. Quantidade de hóspedes cadastrados
4. Quantidade de eventos confirmados
5. Receita acumulada:
   - hospedagem
   - eventos
   - total geral

Formato recomendado: tabela textual alinhada.

---

## 10) Requisitos de Qualidade e Organização

## 10.1 Estrutura sugerida

- `main` / `inicio`
- `auth`
- `menu`
- `reservas`
- `hospedes`
- `eventos`
- `ar_condicionado`
- `abastecimento`
- `relatorios`
- `utils` (entrada, validação, formatação)

## 10.2 Padrões obrigatórios

- Nomes de funções/métodos sem ambiguidade.
- Separação entre entrada/saída e regras de negócio.
- Reaproveitamento de validações comuns.
- Controle de erros sem encerrar abruptamente o programa.

---

## 11) Critérios de Avaliação

- Cobertura integral dos requisitos funcionais
- Qualidade da modularização
- Robustez de validações e tratamento de exceções
- Clareza dos relatórios e das mensagens
- Organização, legibilidade e consistência do código
- Fidelidade da implementação em relação ao enunciado

---

## 12) Entregáveis

1. Código-fonte completo
2. Instruções de execução
3. Evidências de teste (capturas ou logs)
4. Breve documentação da arquitetura modular adotada

