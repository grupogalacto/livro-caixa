# Livro-Caixa — publicar no GitHub com sincronização em tempo real

Este pacote tem 2 arquivos:

- `index.html` — o site inteiro (visual, formulário, lista). É o único arquivo que vai para o GitHub.
- `firestore.rules` — as regras de acesso do banco de dados. Você cola o conteúdo dele no console do Firebase (não vai para o GitHub).

O site sozinho não guarda nada — quem guarda os lançamentos e avisa todo mundo em tempo real é o **Firestore**, um banco de dados gratuito do Google (Firebase). Por isso são duas contas/dois passos: primeiro o Firebase (o banco), depois o GitHub (o endereço público do site).

---

## Parte 1 — Criar o banco de dados (Firebase)

1. Acesse **console.firebase.google.com** e entre com uma conta Google.
2. Clique em **"Adicionar projeto"**. Dê um nome (ex.: `livro-caixa`). Pode desligar o Google Analytics — não é necessário. Clique em **"Criar projeto"**.
3. No menu à esquerda, vá em **Build → Firestore Database**. Clique em **"Criar banco de dados"**.
   - Escolha uma região perto de vocês (ex.: `southamerica-east1 (São Paulo)`).
   - Pode iniciar em **modo de produção** — vamos colar as regras certas no passo 5.
4. Ainda no console, clique na **engrenagem ⚙ → Configurações do projeto**. Role até **"Seus aplicativos"** e clique no ícone **`</>`** (Web).
   - Dê um apelido ao app (ex.: `site`). **Não** marque a opção de Firebase Hosting — vamos hospedar pelo GitHub.
   - Clique em **"Registrar app"**. Vai aparecer um bloco de código com `const firebaseConfig = { ... }`.
   - **Copie esse bloco inteiro** — você vai usá-lo no passo 7.
5. Vá em **Firestore Database → Regras** (aba "Rules"). Apague o conteúdo e cole o conteúdo do arquivo `firestore.rules` que veio junto. Clique em **"Publicar"**.
   - Essas regras deixam a coleção `expenses` aberta para qualquer pessoa que tenha o link do site ler e lançar — sem precisar de login, igual a uma planilha compartilhada "qualquer um com o link edita". Se um dia vocês quiserem exigir login, me chamem que eu adiciono autenticação.
6. Abra o arquivo `index.html` (no computador, com o Bloco de Notas, VS Code, ou qualquer editor de texto).
7. Procure por `firebaseConfig` perto do final do arquivo e **substitua os valores de exemplo pelos que você copiou no passo 4**. Fica assim (com os SEUS valores):

   ```js
   const firebaseConfig = {
     apiKey: "AIzaSy...",
     authDomain: "livro-caixa-xxxxx.firebaseapp.com",
     projectId: "livro-caixa-xxxxx",
     storageBucket: "livro-caixa-xxxxx.appspot.com",
     messagingSenderId: "123456789",
     appId: "1:123456789:web:abcabc"
   };
   ```

   Salve o arquivo.

> Esses valores **não são secretos** — é normal e seguro eles aparecerem no código do site. Quem protege os dados de verdade são as regras do passo 5, não essa chave.

---

## Parte 2 — Publicar o site (GitHub Pages)

1. Acesse **github.com** e entre (ou crie uma conta gratuita).
2. Clique no **+** no canto superior direito → **"New repository"**.
   - Nome: `livro-caixa` (ou o que preferir).
   - Marque **"Public"**.
   - Clique em **"Create repository"**.
3. Na página do repositório recém-criado, clique em **"uploading an existing file"** (ou **Add file → Upload files**).
4. Arraste o arquivo `index.html` (**já editado com sua firebaseConfig**) para a área de upload. Clique em **"Commit changes"**.
5. Vá em **Settings** (aba do repositório) **→ Pages** (menu à esquerda).
   - Em **"Build and deployment" → Source**, escolha **"Deploy from a branch"**.
   - Em **Branch**, escolha `main` e a pasta `/ (root)`. Clique em **Save**.
6. Espere cerca de 1 minuto e recarregue a página de Settings → Pages. Vai aparecer um link assim:

   ```
   https://SEU-USUARIO.github.io/livro-caixa/
   ```

   Esse é o endereço público e definitivo do Livro-Caixa. Compartilhe esse link com quem for lançar despesas.

---

## Testando

Abra o link em dois aparelhos diferentes (por exemplo, seu computador e o celular). Lance uma despesa em um — ela deve aparecer no outro em poucos segundos, sem recarregar a página. O ponto no rodapé do site fica verde piscando quando está sincronizado.

## Para atualizar o site depois

Sempre que precisar mudar algo no `index.html`, edite o arquivo e faça upload de novo pelo GitHub (mesma tela do passo 3-4) — o link continua o mesmo.
