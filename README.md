# DermAlert

## 🚀 Como rodar a documentação localmente e publicar no GitHub Pages

### 1. Pré-requisitos

| Requisito | Descrição |
|-----------|-----------|
| [Python 3.x](https://www.python.org/downloads/) | Interpretador da linguagem Python |
| [Git](https://git-scm.com/downloads) | Sistema de versionamento |
| Chave SSH configurada no GitHub | Para clonar e fazer push com segurança |

### 2. Configurar chave SSH (caso necessário)

Se ainda não possui uma chave SSH configurada no GitHub, siga os guias oficiais:

| Etapa | Link |
|-------|------|
| 🔑 Gerar uma nova chave SSH | [Gerar chave e adicionar ao agente](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) |
| ➕ Adicionar a chave ao GitHub | [Adicionar chave ao GitHub](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account) |

### 3. Instalação

Clone o repositório e instale as dependências do projeto:

```bash
git clone git@github.com:DermAlert/dermalert.github.io.git
cd dermalert.github.io
pip install -r requirements.txt
```

> O arquivo `requirements.txt` já inclui o MkDocs, o tema Material e extensões necessárias.

### 4. Executar servidor local

Para visualizar a documentação com a landing page integrada:

```bash
mkdocs serve
```

Acesse no navegador:

```
http://127.0.0.1:8000/land/dist/index.html
```

### 5. Deploy para o GitHub Pages

Para publicar o site no GitHub Pages:

```bash
mkdocs gh-deploy
```

Isso irá:

- Gerar os arquivos estáticos com `mkdocs build`
- Atualizar automaticamente a branch `gh-pages`
- Publicar a documentação + landing no GitHub Pages

O site ficará acessível em:

```
https://dermalert.github.io/land/dist/index.html
```