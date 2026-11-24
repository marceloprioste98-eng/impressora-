# 🖨️ Integração Java com Impressora Elgin i9
### Projeto Final – Impressora Elgin

---

## 📘 Sobre o Projeto

Este projeto foi desenvolvido com o objetivo de criar uma aplicação Java capaz de se comunicar com a impressora **Elgin i9**, usando a biblioteca oficial da Elgin (**E1_Impressora01.dll**) acessada pelo Java através do **JNA (Java Native Access)**.

O programa funciona por um **menu no console**: o usuário abre a conexão com a impressora (via **USB**) e escolhe a função desejada, como impressão de texto, QR Code, código de barras, cupom SAT, abertura de gaveta e sinal sonoro.

O trabalho faz parte da disciplina de **Programação Orientada a Objetos**.

---

## 🧰 Tecnologias Utilizadas

- **Java 27**
- **IntelliJ IDEA**
- **JNA – Java Native Access** (`com.sun.jna.Library` e `com.sun.jna.Native`)
- **E1_Impressora01.dll** – biblioteca da Elgin
- **Git e GitHub** – versionamento do código

---

## 📁 Estrutura do Projeto

```
Projeto Final/
├── src/
│   └── Main.java              → Código principal (menu + funções da impressora)
├── libs Java/                 → Bibliotecas externas (JNA)
├── E1_Impressora01.dll        → Biblioteca da Elgin (precisa ficar na raiz do projeto)
├── XMLSAT.xml                 → XML de exemplo para impressão de venda SAT
├── CANC_SAT.xml               → XML de exemplo para impressão de cancelamento SAT
├── .gitignore                 → Arquivos ignorados pelo Git
└── README.md                  → Este arquivo
```

> 📌 O programa procura a DLL e os XMLs na **pasta raiz do projeto** (`System.getProperty("user.dir")`).
> Ao iniciar, ele mostra no console o caminho onde está procurando a DLL:
> `Procurando a DLL em: ...`

---

## 🧱 Como o Código Está Organizado

### `interface ImpressoraDLL extends Library`
Interface que liga o Java à DLL da Elgin. Ela é carregada com:

```java
ImpressoraDLL INSTANCE = Native.load(CAMINHO_DLL, ImpressoraDLL.class);
```

Funções da DLL declaradas na interface:

| Função da DLL | Para que serve |
|---|---|
| `AbreConexaoImpressora(tipo, modelo, conexao, param)` | Abre a conexão com a impressora |
| `FechaConexaoImpressora()` | Fecha a conexão |
| `ImpressaoTexto(dados, posicao, estilo, tamanho)` | Imprime texto |
| `ImpressaoQRCode(dados, tamanho, nivelCorrecao)` | Imprime QR Code |
| `ImpressaoCodigoBarras(tipo, dados, altura, largura, HRI)` | Imprime código de barras |
| `Corte(avanco)` | Avança o papel e corta |
| `AvancaPapel(linhas)` | Avança linhas de papel |
| `StatusImpressora(param)` | Consulta o status da impressora |
| `ImprimeXMLSAT(dados, param)` | Imprime o cupom de venda SAT a partir do XML |
| `ImprimeXMLCancelamentoSAT(dados, assQRCode, param)` | Imprime o cupom de cancelamento SAT |
| `AbreGavetaElgin()` | Abre a gaveta Elgin |
| `AbreGaveta(pino, ti, tf)` | Abre gaveta genérica |
| `SinalSonoro(qtd, tempoInicio, tempoFim)` | Emite bipes na impressora |

### Métodos da classe `Main`

| Método | O que faz |
|---|---|
| `abrirConexao()` | Conecta na impressora modelo **i9** via **USB** |
| `fecharConexao()` | Encerra a conexão |
| `impressaoTexto()` | Imprime "Ola" e corta o papel (se der certo) |
| `impressaoQRCode()` | Imprime um QR Code e corta o papel |
| `impressaoCodigoBarras()` | Imprime um código de barras (Code 128) e corta |
| `imprimeXMLSAT()` | Imprime o cupom SAT usando o `XMLSAT.xml` |
| `imprimeXMLCancelamentoSAT()` | Imprime o cancelamento SAT usando o `CANC_SAT.xml` |
| `abreGavetaElgin()` | Abre a gaveta Elgin |
| `abreGaveta()` | Abre a gaveta com parâmetros personalizados |
| `sinalSonoro()` | Faz a impressora apitar |
| `exibirMenu()` | Mostra o menu no console |
| `main()` | Loop principal que lê a opção e chama o método certo |

Todos os métodos de impressão verificam a variável `conexaoAberta` antes de executar. Se a conexão não estiver aberta, aparece:
**"Erro: Conexão não está aberta."**

---

## 📋 Menu do Programa

```
************** MENU IMPRESSORA **************
1  - Abrir Conexão
2  - Fechar Conexão
3  - Impressão Texto
4  - Impressão QRCode
5  - Impressão Código Barras
6  - Impressão XML SAT
7  - Impressão XML Cancelamento SAT
8  - Abrir Gaveta Elgin
9  - Abrir Gaveta
10 - Sinal Sonoro
0  - Sair
```

> Sempre escolha a opção **1 – Abrir Conexão** antes das outras.
> Ao escolher **0 – Sair**, o programa fecha a conexão automaticamente se ela estiver aberta.

---

## ⚙️ Instalação e Execução

### 🔽 1. Baixar o Projeto do GitHub

**Opção A – ZIP**
1. Acesse o repositório.
2. Clique no botão verde **Code**.
3. Selecione **Download ZIP**.
4. Clique com o botão direito no arquivo → **Extrair tudo**.

**Opção B – Clonar pelo IntelliJ**
1. Abra o **IntelliJ IDEA**.
2. Clique em **Clone Repository** (ou **Get from VCS**).
3. Cole o link do repositório e clique em **Clone**.

### 🧭 2. Abrir no IntelliJ IDEA
1. Clique em **Open** e escolha a pasta do projeto.
2. Aguarde o carregamento.
3. Confira o SDK em **File → Project Structure → Project → SDK** (**Java 27**).
4. Confira se o **JNA** está adicionado em **File → Project Structure → Libraries** (pasta `libs Java`).

### ▶️ 3. Executar
1. Conecte a impressora **Elgin i9** ao computador pela **USB** e ligue-a.
2. Confirme que o arquivo **E1_Impressora01.dll** está na **raiz do projeto** (mesma pasta onde fica a pasta `src`).
3. Abra `src/Main.java` e clique em ▶️ **Run**.
4. No console, digite **1** para abrir a conexão e depois escolha a função desejada.

> ⚠️ A DLL da Elgin funciona apenas no **Windows**. Use um Java **64 bits** se a DLL for 64 bits (ou 32 bits se a DLL for 32 bits).

---

## 🧪 Testes Realizados

- ✅ Abertura e fechamento da conexão com a impressora
- ✅ Impressão de texto
- ✅ Impressão de QR Code
- ✅ Impressão de código de barras
- ✅ Impressão de cupom SAT (venda)
- ✅ Impressão de cupom SAT (cancelamento)
- ✅ Abertura de gaveta (Elgin e genérica)
- ✅ Sinal sonoro
- ✅ Mensagem de erro quando a conexão não está aberta
- ✅ Tratamento de opção inválida no menu

---

## 📚 Documentação Adicional

- Manual da impressora **Elgin i9**
- Documentação da biblioteca **E1_Impressora** (Elgin Developers)
- Documentação do **JNA** (Java Native Access)

---

## 👥 Autores
*(em ordem alfabética)*

| Nome | RA |
|---|---|
| Alan de Santana | 251293 |
| Caio Santos Ferreira | 209281 |
| Luiz Eduardo da Silva Nascimento | 250802 |
| Marcelo Prioste | 246848 |
