**Projeto de Reconhecimento Facial com Python e PostgreSQL**

## 📖 Resumo do Projeto

Este projeto nasce da necessidade de controlar o acesso de usuários em uma academia de forma automática e segura, utilizando reconhecimento facial. Nele, desenvolvemos dois scripts em Python:

1. **Cadastro de Rosto**: Captura a imagem do rosto via webcam e armazena os dados faciais em um banco de dados PostgreSQL.
2. **Validação de Acesso**: Captura uma nova imagem pela webcam, busca a face cadastrada no banco de dados e valida se pertence ao usuário correto antes de liberar a catraca.

O sistema grava informações simples sobre cada conta (nome, sexo, idade) e relaciona-as ao `id` do rosto na tabela de rostos. As imagens faciais são armazenadas em bytes no banco para maior eficiência e segurança.

> "Este projeto demonstra uma solução prática de reconhecimento facial, integrando visão computacional em Python 3.7 com um banco de dados robusto, garantindo agilidade e confiabilidade no gerenciamento de acessos."

---

## 📋 Pré-requisitos

Antes de começar, certifique-se de ter instalado em sua máquina:

* **Python 3.7** ou superior
* **PostgreSQL** (versão 10 ou superior)
* **Git** (para clonar este repositório)

Além disso, vamos utilizar as seguintes bibliotecas Python:

```txt
face_recognition
opencv-python
psycopg2
numpy
```

---

## ⚙️ Instalação e Configuração

1. **Clone o repositório**:

   ```bash
   git clone https://github.com/LucasLeitePereira/
   cd seu-projeto
   ```

2. **Crie e ative um ambiente virtual** (opcional, mas recomendado):

   ```bash
   python3.7 -m venv venv
   source venv/bin/activate   # Linux/macOS
   venv\Scripts\activate    # Windows
   ```

3. **Instale as dependências**:

   ```bash
   pip install -r requirements.txt
   ```

4. **Configure o PostgreSQL**:

   * Crie um banco de dados, por exemplo `face_access_db`.
   * Em um cliente SQL (psql, PgAdmin, etc.), execute as tabelas abaixo:

     ```sql
     -- Tabela de contas
     CREATE TABLE conta (
       id_conta SERIAL PRIMARY KEY,
       nome_conta VARCHAR(50) NOT NULL,
       sexo VARCHAR(9) NOT NULL,
       idade INT NOT NULL,
       id_rosto SERIAL REFERENCES rostos(id_rosto)
     );

     -- Tabela de rostos
     CREATE TABLE rostos (
        id_rosto SERIAL PRIMARY KEY,
        shape TEXT    NOT NULL,
        dtype TEXT    NOT NULL,
        data  BYTEA   NOT NULL
        );
     ```

5. **Atualize o arquivo de configuração** (`config.py` ou variáveis de ambiente) com suas credenciais do PostgreSQL.

---

## ▶️ Uso

### 1. Cadastro de Rosto

Este script solicita que o usuário posicione o rosto à frente da webcam e, quando detectado, salva a imagem no banco:

```bash
python cadastrarConta.py
```

#### Fluxo:

1. Abre a câmera e detecta o rosto.
2. Converte a imagem para vetores faciais.
3. Insere os bytes da imagem na tabela `rosto`.
4. Captura dados da conta (nome, sexo, idade) e insere na tabela `conta` com o `rosto_id` gerado.

### 2. Validação de Acesso

Este script abre a webcam, captura a face e compara com as cadastradas:

```bash
python validarRosto.py
```

#### Fluxo:

1. Abre a câmera e detecta o rosto ao vivo.
2. Gera o vetor facial da captura.
3. Busca todos os vetores do banco e compara similaridade.
4. Se encontrar correspondência acima do limiar, exibe "Acesso Liberado" com nome do usuário; caso contrário, "Acesso Negado".

---

## 🤝 Contribuição

Sinta-se à vontade para abrir issues e pull requests! Para sugestões ou melhorias no código, siga as normas de estilo Python (PEP8) e escreva testes quando possível.

---

## 📄 Licença

Este projeto está licenciado sob a [MIT License](LICENSE).
