# controle-pdv (site)

Este repositório contém uma versão estática do frontend do Controle PDV preparada para publicação via GitHub Pages.

Conteúdo commitado na branch `site-deploy`:

- `index.html` — arquivo principal com o bloco `FIREBASE_CONFIG` já inserido (compat com firebase-app-compat / firestore-compat).
- `README.md` — este arquivo, com instruções de uso e deploy.
- `package.json` — scripts úteis para rodar localmente.
- `.gitignore` — arquivos comuns a ignorar.

## Configurar o Firebase

O arquivo `index.html` contém o objeto `FIREBASE_CONFIG` para o projeto Firebase usado pelo frontend. Essa configuração é segura para o cliente (chave de API pública) mas não concede acesso administrativo ao projeto.

Recomenda-se configurar regras do Firestore para proteger os dados. Exemplo mínimo (recomendado):

```rules
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read: if true;                       // leitura pública (ajuste conforme necessidade)
      allow write: if request.auth != null;     // escrita somente para usuários autenticados
    }
  }
}
```

Se preferir apenas usuários autenticados ou um conjunto restrito de e-mails façam escrita, substitua `allow write` por uma condição que valide `request.auth.token.email` ou `request.auth.uid`.

## Rodando localmente

Você não precisa instalar dependências para visualizar o site localmente — ele é uma aplicação estática.

Opções rápidas (requer Node.js para `npx`):

- Servir com `serve` (recomendado):

```bash
npm run start
# abre em http://localhost:8080
```

- Servir com `http-server`:

```bash
npm run dev
# abre em http://localhost:8080
```

## Publicando com GitHub Pages

1. Vá em `Settings` → `Pages` no repositório.
2. Em `Source` selecione a branch `site-deploy` e a pasta `/ (root)`.
3. Salve. O GitHub irá construir e publicar o site em alguns minutos.

A URL esperada será: `https://cazumgammer-cmd.github.io/controle-pdv` (ou a URL exibida pela interface do GitHub).

Observação: se preferir, posso commitar numa branch `gh-pages` já pronta para Pages — diga se prefere essa opção.

## Pull Request / Merges

Você solicitou que eu abra um Pull Request `site-deploy` → `main` após o commit — eu commitei os arquivos nesta branch. A criação automática do PR não está disponível via esta sessão, portanto abaixo seguem os passos para abrir o PR manualmente:

1. No GitHub abra a página do repositório: `https://github.com/cazumgammer-cmd/controle-pdv`.
2. Clique em "Compare & pull request" para a branch `site-deploy` (ou vá em "Pull requests" → "New Pull Request" e escolha `site-deploy` como branch de comparação).
3. Revise as mudanças e crie o PR.

Se desejar que eu abra o PR automaticamente, autorize-me a fazê-lo e me confirme como devo criar o PR (título e descrição). Caso tenha uma integração ou token que permita criar PRs, posso usar essa rota.

## Segurança

- Não commitei chaves administrativas nem credenciais sensíveis.
- Proteja o Firestore com regras adequadas antes de permitir escrita pública.

Se quiser que eu ajuste o README, inclua regras mais restritas ou crie uma branch `gh-pages`, me diga e eu atualizo.
