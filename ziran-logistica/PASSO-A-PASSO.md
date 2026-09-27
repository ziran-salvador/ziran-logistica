# Ziran Logística: como colocar o sistema no ar

Guia para quem nunca usou Firebase nem GitHub. Leva entre 40 e 60 minutos. Siga na ordem, sem pular etapas.

## Como o sistema funciona

- **GitHub Pages** hospeda as telas do sistema (o site). É gratuito.
- **Firebase Authentication** cuida do login: e-mail, senha e recuperação de senha.
- **Firebase Firestore** é o banco de dados onde ficam bookings, contêineres, transportes, lotes, ovações e o cadastro de usuários.

As telas ficam públicas no GitHub, mas os **dados não**: só quem tem login aprovado consegue ler ou gravar. Quem garante isso são as regras de segurança do Firestore (Parte 6).

## O que você vai precisar

- Uma conta Google (Gmail). De preferência uma conta da empresa, e não pessoal, porque ela será a dona do banco de dados.
- Os 5 arquivos da pasta `ziran-logistica` (que vieram no arquivo .zip):
  - `index.html`: o sistema
  - `firebase-config.js`: onde você cola a "chave" do seu Firebase
  - `firestore.rules`: as regras de segurança do banco
  - `PASSO-A-PASSO.md`: este guia
  - `README.md`: resumo do projeto
- Um computador com navegador (Chrome ou Edge).

---

## Parte 1: Criar o projeto no Firebase

1. Acesse **https://console.firebase.google.com** e entre com a conta Google.
2. Clique em **Criar um projeto** (ou "Adicionar projeto" / "Get started with a Firebase project").
3. Nome do projeto: `ziran-logistica`. O Firebase pode acrescentar letras no final do nome, e isso é normal. Clique em **Continuar**.
4. Na tela do Google Analytics, **desative** a opção. O sistema não precisa dela. Clique em **Criar projeto**.
5. Aguarde e clique em **Continuar**. Você está no painel do projeto.

## Parte 2: Ativar o login por e-mail e senha

1. No menu da esquerda, abra **Criação** (em inglês: *Build*) e clique em **Authentication**.
2. Clique em **Vamos começar** (*Get started*).
3. Na aba **Método de login** (*Sign-in method*), clique em **E-mail/senha**.
4. Ative **somente a primeira chave** ("E-mail/senha"). Deixe "Link do e-mail (login sem senha)" desligado.
5. Clique em **Salvar**.

## Parte 3: Criar o banco de dados (Firestore)

1. No menu da esquerda: **Criação** e depois **Firestore Database**.
2. Clique em **Criar banco de dados**.
3. **Local:** escolha **`southamerica-east1 (São Paulo)`**. Atenção: o local não pode ser trocado depois.
4. Escolha **Iniciar no modo de produção** e clique em **Criar**.
5. Aguarde até aparecer a tela do banco vazio, com as abas **Dados**, **Regras**, **Índices**...

## Parte 4: Registrar o site e copiar a configuração

1. No topo do menu da esquerda, clique na **engrenagem ⚙** e depois em **Configurações do projeto**.
2. Role até **Seus apps** e clique no ícone **`</>`** (Web).
3. Apelido do app: `ziran-web`. **Não** marque "Configure também o Firebase Hosting".
4. Clique em **Registrar app**.
5. Vai aparecer um trecho de código com `const firebaseConfig = { ... }`. **Copie só o que está entre as chaves `{ }`** e guarde num bloco de notas. É parecido com isto:

```
apiKey: "AIzaSyB...........",
authDomain: "ziran-logistica-xxxxx.firebaseapp.com",
projectId: "ziran-logistica-xxxxx",
storageBucket: "ziran-logistica-xxxxx.appspot.com",
messagingSenderId: "123456789012",
appId: "1:123456789012:web:abc123..."
```

6. Clique em **Continuar no console**.

> Esses dados **não são senha**. É normal que fiquem visíveis no site. A proteção vem das regras de segurança (Parte 6) e do login.

---

## Parte 5: Criar o repositório no GitHub e enviar os arquivos

### 5.1 Criar a conta (se ainda não tiver)
1. Acesse **https://github.com** e clique em **Sign up**.
2. Informe e-mail, senha e um nome de usuário, por exemplo `ziran-salvador`. **Esse nome vai aparecer no endereço do site**, então escolha algo simples e profissional.
3. Confirme o código enviado por e-mail.

### 5.2 Criar o repositório
1. Já dentro do GitHub, clique no **+** (canto superior direito) e depois em **New repository**.
2. **Repository name:** `ziran-logistica`
3. Marque **Public**. O GitHub Pages gratuito exige repositório público. Só o código das telas fica visível; os dados não.
4. Não marque nenhuma outra opção. Clique em **Create repository**.

### 5.3 Enviar os arquivos
1. No seu computador, **extraia o .zip** (botão direito e depois "Extrair tudo").
2. Na página do repositório recém-criado, clique no link **uploading an existing file**.
3. Arraste **todos os arquivos** da pasta `ziran-logistica` para a área indicada. Arraste os arquivos, não a pasta.
4. Role até embaixo e clique em **Commit changes**.
5. Confira: a lista do repositório deve mostrar `index.html`, `firebase-config.js`, `firestore.rules`, `PASSO-A-PASSO.md` e `README.md`.

### 5.4 Colar a configuração do Firebase
1. No repositório, clique no arquivo **`firebase-config.js`**.
2. Clique no **lápis ✏️** (Edit this file), no canto direito.
3. Substitua os valores de exemplo pelos que você copiou na Parte 4. O arquivo deve ficar assim (com os **seus** valores):

```js
window.FIREBASE_CONFIG = {
  apiKey: "AIzaSyB...........",
  authDomain: "ziran-logistica-xxxxx.firebaseapp.com",
  projectId: "ziran-logistica-xxxxx",
  storageBucket: "ziran-logistica-xxxxx.appspot.com",
  messagingSenderId: "123456789012",
  appId: "1:123456789012:web:abc123..."
};
```

4. Cuidado com as **aspas** e as **vírgulas** no fim de cada linha: elas precisam continuar lá.
5. Clique em **Commit changes...** e depois em **Commit changes** de novo.

---

## Parte 6: Publicar as regras de segurança do banco

Esta é a etapa mais importante para a segurança.

1. No GitHub, abra o arquivo **`firestore.rules`** e clique no ícone **Copy raw file** (dois quadradinhos, no canto direito acima do código).
2. No Firebase, vá em **Firestore Database** e depois na aba **Regras**.
3. **Apague todo o texto** que está lá e **cole** o que você copiou.
4. Clique em **Publicar**. Deve aparecer a mensagem de que as regras foram publicadas.

O que essas regras garantem:
- Quem não fez login não lê nem grava nada.
- Quem cria conta entra como **operador pendente** e não vê nenhum dado até ser aprovado.
- Só o **administrador** aprova usuários e muda perfis.
- O histórico de movimentações não pode ser alterado, apenas acrescentado.

## Parte 7: Ligar o site no GitHub Pages

1. No repositório, clique em **Settings** (engrenagem, no menu de cima).
2. No menu da esquerda, clique em **Pages**.
3. Em **Build and deployment** e depois **Source**, escolha **Deploy from a branch**.
4. Em **Branch**, escolha **`main`** e a pasta **`/ (root)`**. Clique em **Save**.
5. Aguarde de 1 a 3 minutos e recarregue a página. Vai aparecer:
   **"Your site is live at https://SEU-USUARIO.github.io/ziran-logistica/"**
6. Esse é o **endereço do sistema**. Guarde e compartilhe com a equipe.

## Parte 8: Autorizar o endereço do site no Firebase

Sem esta etapa, o login dá erro de "domínio não autorizado".

1. No Firebase, vá em **Authentication** e depois na aba **Configurações** (*Settings*).
2. Clique em **Domínios autorizados** e depois em **Adicionar domínio**.
3. Digite **só** `SEU-USUARIO.github.io`, sem `https://` e sem `/ziran-logistica`. Exemplo: `ziran-salvador.github.io`.
4. Clique em **Adicionar**.

---

## Parte 9: Criar o primeiro administrador (você)

O primeiro administrador é liberado direto no banco, só esta vez.

1. Abra o endereço do sistema (Parte 7). Deve aparecer a tela **Entrar**, com o logo da Ziran.
2. Clique em **Criar conta**, preencha nome, e-mail e senha e clique em **Criar conta**.
3. Aparece **"Aguardando aprovação"**. Deixe essa aba aberta.
4. No Firebase, vá em **Firestore Database**, aba **Dados**. Agora existe a coleção **`users`**.
5. Clique em **`users`** e depois no documento que aparece (tem um código estranho como nome, e isso é normal).
6. No campo **`status`**, passe o mouse sobre o valor, clique no **lápis**, troque `pendente` por **`ativo`** e clique em **Atualizar**.
7. No campo **`perfil`**, troque `operador` por **`admin`** e clique em **Atualizar**.
8. Volte à aba do sistema: ele abre sozinho. Se não abrir, recarregue a página (F5).

Pronto: você é administrador. Daqui em diante **todas as aprovações são feitas dentro do sistema**, no menu **Usuários**.

## Parte 10: Primeiros passos dentro do sistema

1. **Treinar a equipe (opcional):** em **Usuários**, clique em **Carregar dados de exemplo**. Quando terminar o treinamento, clique em **Apagar todos os dados operacionais** (é preciso clicar duas vezes para confirmar) para começar a operação real do zero.
2. **Cadastros:** cadastre armadores, clientes/exportadores, navios, motoristas e veículos.
3. **Colocar a equipe no sistema.** Há duas formas:
   - **A pessoa se cadastra:** envie o endereço do sistema; ela clica em **Criar conta** e você aprova em **Usuários** (mude a Situação para **Ativo** e escolha o Perfil).
   - **Você cria:** em **Usuários**, clique em **+ Criar usuário** e informe nome, e-mail e uma senha provisória. O usuário já nasce ativo. Depois ele pode trocar a senha em "Esqueci a senha".
4. **Perfis:**

| Perfil | O que pode fazer |
|---|---|
| Operador | Lança e avança operações: bookings, contêineres, transportes, ovação/desova, pré-stacking, armazenagem e relatórios |
| Supervisor | Tudo do operador, mais importar planilhas (SISTER e bookings) e editar cadastros |
| Administrador | Tudo, mais aprovar e bloquear usuários, mudar perfis e gerenciar os dados do sistema |

5. **Tirar alguém do sistema:** em **Usuários**, mude a Situação para **Inativo**. O acesso é cortado na hora, mesmo que a pessoa esteja com o sistema aberto.

## Parte 11: Como atualizar o sistema no futuro

Quando eu (ou outra pessoa) enviar uma nova versão do `index.html`:

1. No repositório do GitHub, clique em **Add file** e depois em **Upload files**.
2. Arraste o novo `index.html`. Ele substitui o antigo.
3. Clique em **Commit changes**.
4. Aguarde de 1 a 2 minutos e, no sistema, aperte **Ctrl + F5** para forçar a nova versão.

**Não reenvie o `firebase-config.js`** numa atualização, senão sua configuração volta a ter os valores de exemplo. Os dados não se perdem em atualizações, porque ficam no Firebase.

---

## Custos e limites do plano gratuito

O Firebase começa no plano gratuito **Spark**. Os limites por dia são:

| Recurso | Grátis por dia |
|---|---|
| Leituras no banco | 50.000 |
| Gravações no banco | 20.000 |
| Exclusões | 20.000 |
| Armazenamento | 1 GB no total |

**Atenção:** nesta versão, cada vez que alguém abre o sistema, ele lê todos os registros do banco. Com a operação crescendo, o limite de leituras pode ser atingido. Por exemplo, 3.000 registros × 15 pessoas × 3 aberturas por dia = 135 mil leituras.
Quando o limite estoura, o sistema **para de carregar até o dia seguinte**. Para evitar isso:

1. No Firebase, clique em **Fazer upgrade** e mude para o plano **Blaze**, que cobra só pelo uso acima da cota gratuita. Pelos preços que eu conheço, cada 100 mil leituras extras custam alguns centavos de dólar. Confira os valores atuais na página de preços do Firebase antes de decidir.
2. Crie um **alerta de orçamento** (por exemplo, US$ 10 por mês) em **Uso e faturamento** e depois **Detalhes e configurações**, para receber um e-mail se o custo subir.

Importar um relatório grande do SISTER também consome gravações: cada contêiner conta como 2 gravações. Evite importar mais de 5.000 linhas no mesmo dia no plano gratuito.

## Segurança: boas práticas

- Ative a **verificação em duas etapas** na conta Google dona do Firebase e na conta do GitHub.
- Tenha **pelo menos 2 administradores**, para não depender de uma pessoa só.
- Não compartilhe o login do **console do Firebase**: ele dá acesso a tudo. A equipe usa apenas o endereço do sistema.
- Quando alguém sair da empresa, mude o usuário para **Inativo** no mesmo dia.

---

## Problemas comuns

| O que aparece | Causa | Como resolver |
|---|---|---|
| "Falta configurar o Firebase" | `firebase-config.js` ainda tem os valores de exemplo, ou tem erro de aspas/vírgula | Refaça a Parte 5.4 com cuidado |
| "Este endereço não está autorizado no Firebase" | Domínio do GitHub não autorizado | Refaça a Parte 8 |
| "Login por e-mail e senha não está ativado" | Método de login desligado | Refaça a Parte 2 |
| "O banco recusou o cadastro" ou "Sem permissão" | Regras não publicadas ou coladas pela metade | Refaça a Parte 6 |
| Fica em "Aguardando aprovação" | Usuário ainda pendente | Admin libera em **Usuários** (ou, só para o 1º admin, a Parte 9) |
| Página 404 no endereço do site | GitHub Pages ainda publicando, ou o arquivo não se chama exatamente `index.html` | Aguarde 3 min; confira o nome do arquivo e a Parte 7 |
| Mudança nova não aparece | O navegador guardou a versão antiga | Aperte **Ctrl + F5** |
| "Muitas tentativas" no login | Proteção contra senha errada repetida | Aguarde alguns minutos ou use "Esqueci a senha" |
| O e-mail de "Esqueci a senha" não chega | Filtro de spam | Procure na caixa de spam por "noreply@...firebaseapp.com" |
| O sistema parou de carregar dados no meio do dia | Limite diário do plano gratuito | Veja "Custos e limites" e mude para o plano Blaze |

## O que ainda é demonstração

- **Relatórios:** os gráficos históricos ainda usam dados simulados. O estoque atual e o free time já usam os dados reais. O sistema já grava os horários de envio de draft/VGM, entrega no terminal, início e fim de ovação/desova e das viagens. O próximo passo é ligar os relatórios a esses dados reais.
- **Exportar para Excel:** use o botão **Copiar tabela para Excel** e cole na planilha.
