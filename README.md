# JDKMAN - Java Development Kit Manager

**Gerencie múltiplas versões do JDK no Debian/AlmaLinux**  
Uma ferramenta intuitiva para instalar, alternar e remover versões do Java Development Kit sem conflitos.

## 🎯 Objetivo
Facilitar a instalação e gerenciamento de múltiplas versões do JDK em sistemas baseados em Debian/AlmaLinux, permitindo que desenvolvedores alternem entre versões conforme necessário para diferentes projetos Java.

## ⚙️ Requisitos do sistema
- **Sistema Operacional**: Debian 12 (Bookworm) ou AlmaLinux 9.6
- **Dependências**:
  - `wget` - Para download de arquivos
  - `tar` - Para extração de pacotes
  - `gzip` - Para descompressão de arquivos
  - `bash` - Shell para execução dos scripts
- **Permissões**: Execução como usuário comum (não requer root)

---

## 📦 Como instalar o jdkman?

**No terminal com sua conta de usuário comum execute:**

```bash
wget -qO- https://raw.githubusercontent.com/souza-lb/jdkman/main/install | bash
```

Após a instalação, **reinicie seu terminal** ou execute:
```bash
source ~/.bashrc
```

--- 

## 📋 Comandos disponíveis

### ℹ️ Exibir ajuda
```bash
jdkman help
```

### 🔍 Listar versões instaladas
```bash
jdkman list
```

Exemplo de saída:
```
Versões JDK disponíveis:
    11.0.22
--> 17.0.10
    24.0.1
```

### ⬇️ Instalar uma versão
```bash
jdkman install [URL/arquivo]
```

**Exemplos:**
```bash
# Instalar via URL oficial (OpenJDK)
jdkman install https://download.java.net/java/GA/jdk24.0.1/24a58e0e276943138bf3e963e6291ac2/9/GPL/openjdk-24.0.1_linux-x64_bin.tar.gz

# Instalar arquivo local
jdkman install ~/Downloads/openjdk-24.0.1_linux-x64_bin.tar.gz
```

### 🔄 Gerenciar repositório de versões
```bash
jdkman repo update     # Atualiza lista de links de versões disponíveis
jdkman repo list       # Lista versões disponíveis no repositório
jdkman repo [versão]   # Instala versão específica do repositório
```

**Exemplos:**
```bash
# Atualizar lista de links
jdkman repo update

# Listar versões disponíveis no repositório
jdkman repo list

# Instalar a versão 24.0.1 do repositório
jdkman repo 24.0.1
```

Exemplo de saída do `repo list`:
```
Versões disponíveis no repositório:
----------------------------------
Versão       | Link
----------------------------------
24.0.1       | https://download.java.net/java/GA/jdk24.0.1/...tar.gz
17.0.10      | https://download.java.net/java/GA/jdk17.0.10/...tar.gz
...
```

### ⚡ Ativar uma versão
```bash
jdkman use [versão]
```

**Exemplo:**
```bash
jdkman use 24.0.1
```

Saída:
```
Versão 24.0.1 definida como padrão!
Execute 'source ~/.bashrc' para aplicar as mudanças!
```

### 🔌 Desativar versão atual
```bash
jdkman disable
```

Saída:
```
JDK desativado com sucesso!
Execute 'source ~/.bashrc' para aplicar as alterações!
```

### 🗑️ Remover uma versão
```bash
jdkman remove [versão]
```

**Exemplo:**
```bash
jdkman remove 11.0.22
```

Saída com confirmações:
```
Deseja realmente remover a versão 11.0.22? [s/N]: s
Digite o número da versão para confirmar a remoção: 11.0.22
Removendo versao: 11.0.22...
Versão 11.0.22 removida com sucesso!
Execute 'source ~/.bashrc' para aplicar as mudanças!
```

---

## ⚙️ Estrutura de arquivos
O jdkman organiza os arquivos em:
```
~/.jdkman/
├── jdk_versions/      # Versões instaladas
│   ├── 17.0.10/
│   ├── 24.0.1/
│   └── current -> 17.0.10  # Link simbólico
├── jdk_env            # Configuração de ambiente
└── link-jdk.txt       # Lista de links do repositório (atualizado via 'repo update')
```

## 🔄 Pós-instalação
Após usar `use`, `disable` ou `repo`, sempre execute:
```bash
source ~/.bashrc
```
Para aplicar as mudanças no ambiente atual.

---

## ❌ Mensagens de erro comuns
1. **Versão já instalada:**
   ```
   Erro: A versão '17.0.10' já está instalada!
   ```

2. **Remover versão em uso:**
   ```
   Erro: A versão '17.0.10' está em uso!
   Execute: 'jdkman disable' antes de tentar remover esta versão!
   ```

3. **Versão não encontrada:**
   ```
   Erro: Versão 11.0.22 não encontrada!
   ```

4. **Formato de versão inválido:**
   ```
   Erro: Formato inválido! Use X.Y.Z
   ```

5. **Operação já em progresso:**
   ```
   Erro: Operação já em progresso (PID ...).
   ```

6. **Repositório não encontrado:**
   ```
   Erro: Repositório não encontrado!
   Execute 'jdkman repo update' primeiro.
   ```

---

## 🔄 Fluxo de trabalho típico

```mermaid
graph TD
    A[Atualizar repositório] --> B{Instalar versão}
    B -->|Via URL| C[ jdkman install URL ]
    B -->|Via Repositório| D[ jdkman repo X.Y.Z ]
    C & D --> E{Usar versão?}
    E -->|Sim| F[ jdkman use X.Y.Z ]
    E -->|Não| G[Listar versões]
    F --> H[source ~/.bashrc]
    G --> I[Escolher versão]
    I --> F
```

Passos detalhados:
1. **Atualizar repositório (opcional):** `jdkman repo update`
2. **Instalar nova versão do JDK:**
   - Via URL: `jdkman install [URL]`
   - Via repositório: `jdkman repo [versão]`
3. **Decidir se deseja usar a versão imediatamente**:
   - Se sim: ativar com `jdkman use X.Y.Z`
   - Se não: listar versões com `jdkman list`
4. **Para listagem de versões**:
   - Escolher versão específica
   - Ativar com `jdkman use X.Y.Z`
5. **Sempre após operações:** `source ~/.bashrc`
6. **Compilar e executar projetos Java**
7. **Quando necessário**:
   - Desativar versão atual: `jdkman disable`
   - Remover versões antigas: `jdkman remove [versão]`

---

## 🗑️ Como desinstalar?

**No terminal com sua conta de usuário comum execute:**

```bash
wget -qO- https://raw.githubusercontent.com/souza-lb/jdkman/main/uninstall | bash
```

---

## ❤️ Apoie o Projeto

**Dúvidas, sugestões e contribuições?**  
Leonardo Bruno  
souzalb@proton.me  

**Gostou do projeto e quer realizar uma contribuição voluntária?**  
*(Pode ser o valor de uma xícara de café ou chá...) ☕ 🍵*  

Chave PIX:  
`8dcc7e3c-0c6a-4c6f-a4c0-26a5e62686db`  

Ou utilize o QR Code abaixo:  

<p align="center">
  <img src="images/qrcode-pix.png" alt="QR Code PIX" width="200">
</p>

**Você também pode utilizar o PayPal:**  

[![PayPal](https://img.shields.io/badge/Donate-PayPal-00457C?style=for-the-badge&logo=paypal)](https://www.paypal.com/donate/?hosted_button_id=EQVW5QQ7GBGSY)

<p align="center">
  <img src="images/qrcode-paypal.png" alt="QR Code PayPal" width="200">
</p>

**A utilização deste projeto é livre para alterações e adaptações**  
*Desde que feita a devida referência ao repositório original e seu criador.*
