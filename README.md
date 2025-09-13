[READ.ME JogodaVelhaJava.md](https://github.com/user-attachments/files/22308009/READ.ME.JogodaVelhaJava.md)
# TicTacToeNetworkJava (Servidor/Cliente)

Jogo da Velha (Tic‑Tac‑Toe) em **Java**, com **rede TCP** (máx. 2 jogadores), interface **Swing** simples, placar de vitórias e mensagens de **vitória/derrota/empate**.

> Projeto pensado para uso educacional em **Sistemas Distribuídos**: sockets, protocolo de mensagens em texto e concorrência com `synchronized`.

---

## 📁 Estrutura do projeto

```
TicTacToeNetworkJava/
├─ .project
├─ .classpath
├─ README.txt (ou este README.md)
└─ src/
   ├─ Server.java   # Servidor TCP: estado, regras, placar e orquestração
   └─ Client.java   # Cliente Swing: UI, conexão, envio de jogadas e exibição de estado
```

---

## ⚙️ Requisitos

- **JDK 11+** (recomendado)
- **Eclipse IDE** (ou outra IDE que suporte projetos Java)
- Duas instâncias do cliente (podem ser no mesmo PC ou em PCs diferentes na mesma rede local)

---

## 🚀 Como importar no Eclipse

1. `File → Import…`
2. **General → Existing Projects into Workspace → Next**
3. Em **Select root directory**, selecione a pasta `TicTacToeNetworkJava` (do ZIP).
4. Marque o projeto e clique **Finish**.

> Se preferir criar do zero: crie um *Java Project*, adicione `Server.java` e `Client.java` ao `src`, sem `module-info.java`.

---

## ▶️ Como executar

1. **Servidor**
   - `src/Server.java` → botão direito → **Run As → Java Application**  
   - O console mostrará algo como:
     ```
     === Servidor Jogo da Velha ===
     IP local: 192.168.x.x | Porta: 5000
     Aguarde conexões (máximo 2 jogadores)...
     ```

2. **Cliente**
   - `src/Client.java` → botão direito → **Run As → Java Application**
   - Na janela: informe **IP** e **porta** (padrão `5000`) e clique em **Conectar**.
   - Se cliente e servidor estiverem no **mesmo PC**, use `127.0.0.1`.
   - Em PCs diferentes, use o **IP mostrado no console do servidor**.

3. **Jogando**
   - Primeiro cliente conectado joga com **X**, o segundo com **O**.
   - O servidor controla turnos, valida jogadas e contabiliza o **placar**.
   - Após **vitória** ou **empate**, o tabuleiro reinicia e **alterna quem começa**.

---

## 🧠 Arquitetura e protocolo

- **Servidor** mantém:
  - `board[3][3]` (`' '`, `'X'`, `'O'`), `currentPlayer`, `xWins`, `oWins`, `draws`.
  - Sincronização via `synchronized (lock)` para atomicidade nas jogadas.
  - Até **2 clientes**; o 3º recebe `FULL` e é recusado.

- **Protocolo (texto, UTF‑8):**
  - **Servidor → Cliente**
    - `ASSIGNED X|O`
    - `BOARD <9-chars>` — estado linearizado do tabuleiro (linha a linha)
    - `TURN X|O`
    - `SCORE <xWins> <oWins> <draws>`
    - `WIN X|O`
    - `DRAW`
    - `MSG <texto>`
    - `FULL`
  - **Cliente → Servidor**
    - `MOVE <row> <col>` (valores `0..2`)

---

## 🌐 Notas de rede (LAN/Wi‑Fi)

- O servidor tenta descobrir um **IPv4 não‑loopback** para facilitar a conexão.
- Se o firewall bloquear a porta **5000**, crie uma exceção ou altere a porta no `Server.java`:
  ```java
  private static final int PORT = 5000;
  ```
- Em redes com múltiplas interfaces, confira o IP exibido no console do servidor.

---

## 🛠️ Personalizações rápidas

- **Porta:** altere `PORT` em `Server.java` (cliente deve usar a mesma porta).
- **Tema/UI:** ajuste fontes, tamanhos e espaçamentos em `Client.java` (Swing).
- **Mensagens:** modifique textos enviados via `broadcast("MSG ...")` no servidor.

---

## ❓ Troubleshooting

- **Cliente não conecta:** verifique IP e porta; teste local com `127.0.0.1`.
- **Trava turno/jogada inválida:** garanta que somente uma janela por jogador esteja conectada; o servidor valida e envia mensagens de erro.
- **Terceiro cliente:** será recusado com `FULL` por design.
- **Erros de módulo (`module-info.java`):** este projeto **não usa módulos**. Se houver, exclua `module-info.java`.

---

## 📜 Licença

Uso educacional/didático. Sem garantias. Adapte livremente para seus estudos e práticas.

---

## ✍️ Autor & Créditos

Implementação preparada para demonstração de **sistemas distribuídos** com sockets Java, concorrência básica e UI Swing.
