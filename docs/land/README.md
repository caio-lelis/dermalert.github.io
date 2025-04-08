# ✨ Como atualizar a landing page do projeto DermAlert

A landing page está localizada na pasta `docs/land`, e é construída com **Vite + TailwindCSS**.  
Para que alterações entrem em produção (GitHub Pages), é necessário **rebuildar a landing** antes de executar o deploy do MkDocs.

## 📁 Estrutura da pasta `docs/land`

| Caminho | Descrição |
|--------|-----------|
| `dist/` | Arquivos finais gerados após o build com Vite (HTML, CSS, JS, imagens otimizadas). É o que o MkDocs realmente serve. |
| `public/` | Imagens e arquivos estáticos utilizados pela landing. São copiados automaticamente durante o build. |
| `src/` | Arquivos fonte: CSS, JS e o código principal da landing. |
| `index.html` | HTML principal usado como base para o build da landing. |
| `vite.config.js` | Arquivo de configuração do Vite. Define caminho base (`base: './'`) e pasta de saída (`dist`). |
| `tailwind.config.js` | Configuração do TailwindCSS. Define estilos, temas e paths usados na geração de classes utilitárias. |
| `postcss.config.cjs` | Configuração do PostCSS para processar o TailwindCSS corretamente. |
| `package.json` | Scripts e dependências da landing. Permite rodar build, preview e deploy. |
| `package-lock.json` | Travamento de versões das dependências instaladas via npm. |

## 💻 Comandos úteis

Todos os comandos abaixo devem ser executados dentro da pasta `docs/land`.

| Comando | O que faz |
|--------|-----------|
| `npm run dev` | Abre o servidor de desenvolvimento com hot reload. Ideal para editar e visualizar ao vivo. |
| `npm run build` | Gera a versão final da landing em `dist/` (HTML, CSS, JS otimizados). |
| `npm run preview` | Abre um servidor local que simula o comportamento pós-build. |
| `npm run build:preview` | Build + preview em sequência. Útil para revisar rapidamente. |
| `npm run deploy` | Publica a landing de forma isolada usando `gh-pages`. *(não usar se o deploy for feito via MkDocs)* |

## 🚀 Fluxo completo para atualizar a landing integrada ao MkDocs

> ⚠️ Como o **MkDocs serve os arquivos gerados pelo Vite**, é essencial gerar um novo build da landing sempre que fizer alterações!

1. **Entre na pasta da landing:**

   ```bash
   cd docs/land
   ```

2. **Rode o build do Vite:**

   ```bash
   npm run build
   ```

   Isso irá atualizar os arquivos da pasta `dist/`.

3. **Volte para a raiz do projeto e faça o build do MkDocs:**

   ```bash
   mkdocs build
   ```

4. **Suba o site para o GitHub Pages:**

   ```bash
   mkdocs gh-deploy
   ```

5. **Acesse a versão publicada da landing:**

   ```
   https://dermalert.github.io/land/dist/index.html
   ```