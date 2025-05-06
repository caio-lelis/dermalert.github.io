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

---

## 🧭 Sobre a landing page

A landing page do projeto foi construída com **Vite + TailwindCSS** e está localizada em `docs/land`.  
Ela é compilada com `npm run build` e integrada diretamente ao MkDocs, sendo servida como parte do site final.

> 📌 Para instruções completas de desenvolvimento e manutenção da landing, consulte o [README da pasta `docs/land`](https://github.com/DermAlert/dermalert.github.io/blob/main/docs/land/README.md).

---

## 📝 Sobre o blog do projeto

O blog foi construído com o plugin `mkdocs-blog` e permite registrar publicações, diários técnicos e tutoriais.

### 🧱 Estrutura dos posts

Os posts ficam organizados em `docs/blog/posts`, onde cada pasta representa um post.

Exemplo de estrutura:
```
docs/
└── blog/
    └── posts/
        └── 2025-05-06-exemplo-de-colina/
            ├── 2025-05-06-exemplo-de-colina.md
            └── image.png
```

### ✏️ Formato dos arquivos `.md`

Cada post deve conter um **front-matter** no início do arquivo, como este:

```yaml
---
title: Exemplo de Colina
date: 2025-05-06
image: /assets/posts/bg/montanha_bg.png
description: Um breve olhar sobre uma trilha bucólica.
icon: /assets/posts/icons/montanha_icon.png
---
```

- Os caminhos de `image` e `icon` são relativos à pasta `docs/assets/posts/`.
- As mídias usadas **dentro do post** devem estar na mesma pasta do `.md` e referenciadas com caminho relativo, como `./image.png`.

### 🧪 Visualizar localmente

Para testar o blog em tempo real:

```bash
mkdocs serve
```

E acesse:

```
http://127.0.0.1:8000/blog/
```

Você verá os cards de cada publicação com imagem, título, descrição e data.