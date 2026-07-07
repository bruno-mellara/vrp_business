# vrp_business

Script para servidores **FiveM** rodando o framework **vRP**, que adiciona um sistema de **compra, venda e gestão de estabelecimentos comerciais** usados como fachada para **lavagem de dinheiro** (roleplay).

Baseado no `vrp_business` original da **Zirix**, com ajustes e expansão de conteúdo.

## ✨ Funcionalidades

- **206 estabelecimentos** já configurados com coordenadas no mapa (lojas de departamento, lojas de roupas, armarias, salões de beleza, tatuagem, postos de gasolina, restaurantes, oficinas, farmácias, e diversos pontos únicos como *Bean Machine Cafe*, *MEGA MALL*, *Koi Spa*, etc.).
- Cada estabelecimento tem configuração própria de:
  - Preço de compra e valor de revenda (70% do valor pago);
  - Capital máximo de investimento;
  - Faixa percentual de retorno na lavagem (mínimo/máximo);
  - Nome de exibição;
  - Número máximo de sócios.
- **Lavagem de dinheiro**: jogadores trocam `dinheiro-sujo` do inventário por dinheiro limpo, com taxa de retorno aleatória dentro da faixa configurada por estabelecimento.
- Um ponto especial (`shopid 999`) funciona como **lavanderia geral**, sem vínculo a um estabelecimento específico.
- **Investimento de capital**: sócios podem investir dinheiro sujo para aumentar o capital disponível para lavagem, respeitando o teto de cada estabelecimento.
- **Reset automático**: o valor já lavado (`launded`) é zerado a cada 3 dias, liberando novamente a capacidade de lavagem.
- **Sociedade**: o dono pode adicionar ou remover sócios, respeitando o limite configurado por loja.
- **Transferência de propriedade** para outro jogador, com confirmação via caixa de diálogo.
- **Consulta de informações**: preço (se não tiver dono), proprietário atual, capital geral/lavado, limite de investimento, tempo até o próximo reset e lista de sócios com nome e CPF.
- Tudo é persistido em banco de dados (MySQL), sem uso de arquivos locais para o estado dos negócios.
- Resource leve: **0.00ms** no Resource Monitor quando ocioso.

## 🎮 Comandos

Todos os comandos são usados próximos (até 2.5m) do ponto de um estabelecimento, através do comando `/shop`:

| Comando | Uso | Descrição |
|---|---|---|
| `/shop comprar` | `/shop comprar` | Compra o estabelecimento, se ainda não tiver dono. |
| `/shop vender` | `/shop vender` | Vende o estabelecimento de volta (70% do valor pago). |
| `/shop lavar` | `/shop lavar <valor>` | Troca dinheiro sujo por dinheiro limpo. |
| `/shop investir` | `/shop investir <valor>` | Investe dinheiro sujo para aumentar o capital do estabelecimento. |
| `/shop check` | `/shop check` | Mostra capital, capital lavado, teto de investimento, tempo até o reset e lista de sócios. |
| `/shop info` | `/shop info` | Mostra o preço (se disponível) ou o proprietário atual. |
| `/shop add` | `/shop add <id_jogador>` | Adiciona um sócio ao estabelecimento. |
| `/shop rem` | `/shop rem <id_jogador>` | Remove um sócio do estabelecimento. |
| `/shop trans` | `/shop trans <id_jogador>` | Transfere a propriedade do estabelecimento para outro jogador. |

Digitando `/shop` sem argumentos próximo a um ponto, o jogo exibe a lista de comandos disponíveis.

## 📦 Dependências

- [vRP](https://github.com/vRP-framework/vRP) (framework base — `Tunnel` e `Proxy`)
- Banco de dados MySQL/MariaDB (via `mysql-async` ou equivalente usado pelo vRP)

## 🛠️ Instalação

1. Importe o arquivo `nav_business.sql` no banco de dados do seu servidor.
2. Copie a pasta `vrp_business` para a pasta `resources` do seu servidor FiveM.
3. No `server.cfg`, adicione:
   ```
   ensure vrp_business
   ```
4. Reinicie o servidor ou dê `restart vrp_business` no console.

## 🗺️ Adicionando novos estabelecimentos

Atualmente, os dados de cada loja ficam definidos diretamente no código:

- Em `server.lua`, na tabela `shoplist`: preço, capital máximo, % mínimo/máximo de lavagem, nome e número de sócios.
- Em `client.lua`, na tabela `shoplist`: coordenadas (`x, y, z`) de cada ponto no mapa.

As duas listas são vinculadas pelo mesmo índice (ID do estabelecimento), então adicionar uma nova loja exige inserir uma entrada em ambos os arquivos, na mesma posição.

> ⚠️ Ainda não existe um arquivo de configuração separado. A ideia é futuramente migrar essas tabelas para um `config.lua`, facilitando a organização e a adição de novos estabelecimentos sem editar a lógica do script.

## 📄 Créditos

Código construído em cima do `vrp_business` original da **Zirix**.
