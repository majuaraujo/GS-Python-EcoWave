# Python-EcoWave

<div align="center">
  <h1>EcoWave</h1>
</div>
<hr/>

<p align="center">
  <a href="#pushpin-Descrição">Descrição</a>&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;
  <a href="#pushpin-Descrição">Funcionalidades</a>&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;
  <a href="#pushpin-Descrição">Requisitos</a>&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;
  <a href="#pushpin-Descrição">Instruções de Uso</a>&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp; 
  <a href="#pushpin-Descrição">Código</a>&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp; 
  <a href="#pushpin-Descrição">Exemplo Output</a>&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp; 
  <a href="#smile-Integrantes">Integrante</a>&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;

</p>
<hr/>

## :pushpin: Descrição

Projeto realizado na [FIAP](https://www.fiap.com.br/).

O EcoWave é um protótipo de sistema de monitoramento da saúde dos oceanos. Ele gera dados aleatórios para parâmetros como pH, salinidade e temperatura dos oceanos e os avalia para informar se estão dentro da normalidade ou se medidas devem ser adotadas. O sistema permite ao usuário visualizar dados atuais, histórico de dados e avaliações, além de calcular a média dos históricos de dados e avaliar se os valores médios estão normais.

## :pushpin: Funcionalidades

- Geração de dados aleatórios para pH, salinidade e temperatura dos oceanos.
- Avaliação dos dados gerados para informar se estão dentro da normalidade ou se medidas devem ser adotadas.
- Visualização dos dados atuais.
- Histórico de dados e avaliações.
- Cálculo e avaliação da média dos históricos de dados.
- Interação com o usuário por meio de um menu de opções

## :pushpin:Requisitos

- Python 3.x

## :pushpin: Instruções de Uso

### 1. Clone o repositório

Clone o repositório ou baixe os arquivos do projeto.

```bash
git clone https://github.com/seu-usuario/seu-repositorio.git
cd seu-repositorio
```

- Navegue até o diretório do projeto.
- Execute o script Python eco_wave.py para iniciar o sistema.

## :pushpin: Código

```bash
import random

# Função para gerar dados aleatórios
def gerar_dados():
    dados = {
        "ph": round(random.uniform(6.5, 8.5), 2),
        "salinidade": round(random.uniform(30, 40), 2),
        "temperatura": round(random.uniform(15, 35), 2)
    }
    return dados

# Função para avaliar os dados
def avaliar_dados(dados):
    avaliacao = {}

    # Avaliação do pH
    if dados["ph"] < 7.0:
        avaliacao["ph"] = f"Ácido (valor: {dados['ph']})"
    elif dados["ph"] > 7.5:
        avaliacao["ph"] = f"Alcalino (valor: {dados['ph']})"
    else:
        avaliacao["ph"] = f"Neutro (valor: {dados['ph']})"

    # Avaliação da salinidade
    if dados["salinidade"] < 32:
        avaliacao["salinidade"] = f"Baixa salinidade (valor: {dados['salinidade']})"
    elif dados["salinidade"] > 37:
        avaliacao["salinidade"] = f"Alta salinidade (valor: {dados['salinidade']})"
    else:
        avaliacao["salinidade"] = f"Salinidade normal (valor: {dados['salinidade']})"

    # Avaliação da temperatura
    if dados["temperatura"] < 20:
        avaliacao["temperatura"] = f"Temperatura baixa (valor: {dados['temperatura']})"
    elif dados["temperatura"] > 30:
        avaliacao["temperatura"] = f"Temperatura alta (valor: {dados['temperatura']})"
    else:
        avaliacao["temperatura"] = f"Temperatura normal (valor: {dados['temperatura']})"

    return avaliacao

# Função para exibir o menu e capturar a escolha do usuário
def exibir_menu(primeira_vez):
    if primeira_vez:
        print("Bem-vindo ao EcoWave!")

    print("Selecione uma opção:")
    print("1. Ver dados atuais")
    print("2. Ver avaliação dos dados")
    print("3. Ver histórico de dados")
    print("4. Ver histórico de avaliações")
    print("5. Ver média dos históricos de dados")
    print("6. Sair")
    escolha = input("Digite o número da sua escolha: ")

    # Verificar se a escolha é um número válido
    while not escolha.isdigit() or int(escolha) not in [1, 2, 3, 4, 5, 6]:
        print("Escolha inválida. Tente novamente.\n")
        escolha = input("Digite o número da sua escolha: ")

    return int(escolha)

# Função para perguntar ao usuário se deseja ver o menu novamente
def perguntar_ver_menu():
    ver_menu_novamente = input("Deseja ver o menu novamente? (s/n): ").lower()
    while ver_menu_novamente not in ['s', 'n']:
        print("Entrada inválida. Tente novamente.\n")
        ver_menu_novamente = input("Deseja ver o menu novamente? (s/n): ").lower()
    return ver_menu_novamente == 's'

# Função para calcular a média dos históricos de dados
def calcular_media_historico(dados_historico):
    if not dados_historico:
        return None

    soma_dados = {"ph": 0, "salinidade": 0, "temperatura": 0}
    for dados in dados_historico:
        soma_dados["ph"] += dados["ph"]
        soma_dados["salinidade"] += dados["salinidade"]
        soma_dados["temperatura"] += dados["temperatura"]

    media_dados = {chave: valor / len(dados_historico) for chave, valor in soma_dados.items()}
    return media_dados

# Função principal do sistema EcoWave
def eco_wave():
    dados_historico = []  # Lista para armazenar dicionários de dados
    avaliacoes_historico = []  # Lista para armazenar dicionários de avaliações
    primeira_vez = True

    while True:
        escolha = exibir_menu(primeira_vez)
        primeira_vez = False

        if escolha == 1:
            dados = gerar_dados()
            dados_historico.append(dados)
            print("\nDados atuais:")
            for chave, valor in dados.items():
                print(f"{chave.capitalize()}: {valor}")
            print("\n")

        elif escolha == 2:
            if not dados_historico:
                print("\nNenhum dado disponível para avaliação.\n")
            else:
                avaliacao = avaliar_dados(dados_historico[-1])
                avaliacoes_historico.append(avaliacao)
                print("\nAvaliação dos dados:")
                for chave, valor in avaliacao.items():
                    print(f"{chave.capitalize()}: {valor}")
                print("\n")

        elif escolha == 3:
            if not dados_historico:
                print("\nNenhum dado disponível no histórico.\n")
            else:
                print("\nHistórico de dados:")
                for index, dados in enumerate(dados_historico):
                    print(f"\nLeitura {index + 1}:")
                    for chave, valor in dados.items():
                        print(f"{chave.capitalize()}: {valor}")
                print("\n")

        elif escolha == 4:
            if not avaliacoes_historico:
                print("\nNenhuma avaliação disponível no histórico.\n")
            else:
                print("\nHistórico de avaliações:")
                for index, avaliacao in enumerate(avaliacoes_historico):
                    print(f"\nAvaliação {index + 1}:")
                    for chave, valor in avaliacao.items():
                        print(f"{chave.capitalize()}: {valor}")
                print("\n")

        elif escolha == 5:
            media_dados = calcular_media_historico(dados_historico)
            if media_dados is None:
                print("\nNenhum dado disponível para calcular a média.\n")
            else:
                print("\nMédia dos históricos de dados:")
                for chave, valor in media_dados.items():
                    print(f"{chave.capitalize()}: {valor:.2f}")
                print("\n")

        elif escolha == 6:
            print("Saindo do EcoWave. Até logo!")
            break

        if not perguntar_ver_menu():
            print("Saindo do EcoWave. Até logo!")
            break

# Executar o sistema EcoWave
eco_wave()

```

## :pushpin: Exemplo Output

```bash
Bem-vindo ao EcoWave!
Selecione uma opção:
1. Ver dados atuais
2. Ver avaliação dos dados
3. Ver histórico de dados
4. Ver histórico de avaliações
5. Ver média dos históricos de dados
6. Sair
Digite o número da sua escolha: 1

Dados atuais:
Ph: 7.45
Salinidade: 34.67
Temperatura: 26.45

```

## :smile: Integrante

- [Maria Julia Araujo Rodrigues - RM553384](https://github.com/majuaraujo)

###### Clique no nome para visitar o GitHub
