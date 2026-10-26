# Cronograma de Culto: configuração da sincronização

O site continua no GitHub Pages, no mesmo link. A sincronização em tempo real usa o **Firebase Firestore** (Google), no plano gratuito *Spark*. Não é preciso cartão de crédito. O plano gratuito permite 50 mil leituras e 20 mil gravações por dia, muito acima do que uma equipe de culto usa.

Enquanto o Firebase não estiver configurado, o app funciona em **modo local**, como antes: salva só no aparelho. Ele já aproveita o cronograma salvo pela versão antiga.

A configuração leva uns 10 minutos e é feita uma única vez, de preferência no computador.

---

## 1. Criar o projeto no Firebase

1. Acesse **https://console.firebase.google.com** e entre com uma conta Google.
2. Clique em **Criar um projeto** e dê um nome, por exemplo `cronograma-culto`.
3. Pode **desativar o Google Analytics**, pois não é necessário. Depois clique em **Criar projeto**.

## 2. Criar o banco de dados (Firestore)

1. No menu da esquerda, vá em **Criação → Firestore Database** e clique em **Criar banco de dados**.
2. Em local, escolha **`southamerica-east1 (São Paulo)`**.
3. Escolha **Iniciar no modo de produção** e conclua.
4. Abra a aba **Regras**, apague o que estiver lá e cole todo o conteúdo do arquivo **`firestore.rules`**.
5. Clique em **Publicar**.

## 3. Registrar o app da Web e copiar a configuração

1. Clique na engrenagem ⚙️ ao lado de "Visão geral do projeto" e depois em **Configurações do projeto**.
2. Em **Seus apps**, clique no ícone **`</>`** (Web).
3. Dê um apelido, como `site`. **Não** marque Firebase Hosting e clique em **Registrar app**.
4. Aparecerá um trecho com `const firebaseConfig = { ... }`. Copie só o objeto entre chaves:

```js
{
  apiKey: "AIza...",
  authDomain: "cronograma-culto.firebaseapp.com",
  projectId: "cronograma-culto",
  storageBucket: "cronograma-culto.appspot.com",
  messagingSenderId: "1234567890",
  appId: "1:1234567890:web:abc123"
}
```

> A `apiKey` do Firebase **não é secreta**: ela sempre fica visível em sites. Quem protege os dados são as regras do passo 2 e o código da equipe.

## 4. Colar a configuração no `index.html`

Abra o `index.html` e procure esta linha, perto do início do `<script>`:

```js
const FIREBASE_CONFIG = null;
```

Troque o `null` pelo objeto copiado:

```js
const FIREBASE_CONFIG = {
  apiKey: "AIza...",
  authDomain: "cronograma-culto.firebaseapp.com",
  projectId: "cronograma-culto",
  storageBucket: "cronograma-culto.appspot.com",
  messagingSenderId: "1234567890",
  appId: "1:1234567890:web:abc123"
};
```

## 5. Publicar no GitHub

Substitua o `index.html` do repositório `Cronograma-Culto` pelo novo e faça o commit. Pode fazer isso pelo próprio site do GitHub: abra o arquivo, clique no lápis, cole o conteúdo e clique em *Commit changes*. Em 1 ou 2 minutos o link https://andrecosta-math.github.io/Cronograma-Culto/ já mostra a versão nova.

## 6. Primeiro acesso da equipe

1. Abra o link e escolha o **código da equipe**. Use algo difícil de adivinhar, com pelo menos 8 caracteres, por exemplo `gerencia-culto-k7p2x9`.
2. Na primeira vez, o app avisa que o código ainda não existe e pergunta se deve criar. Confirme.
3. Envie o código **só para a equipe de gerência**, que deve digitá-lo uma vez em cada celular. O aparelho guarda o código.

> **Dica:** no celular, use "Adicionar à tela inicial" no menu do navegador. O cronograma passa a abrir como um aplicativo.

---

## Como funciona

| Recurso | Detalhes |
|---|---|
| **Tempo real** | Qualquer alteração aparece nos celulares da equipe em cerca de 1 segundo. O indicador no topo mostra *Sincronizado*, *Salvando* ou *Offline*. |
| **Sem internet** | Dá para editar mesmo sem conexão. As alterações são enviadas quando a internet voltar. |
| **5 cronogramas** | Ao criar o 6º, o mais antigo é apagado automaticamente. Toque no nome do culto, no topo, para alternar entre eles. |
| **Modelos** | Em ⋮ → *Salvar como modelo*, a estrutura atual fica salva. Ao criar um cronograma novo, você escolhe o modelo. |
| **Reordenar** | Arraste pelos pontinhos à esquerda do item, ou use ⋮ do item → *Mover para cima/baixo*. |
| **Ao vivo** | Durante o culto, mostra o momento atual com contagem regressiva, o próximo item e se o culto está adiantado ou atrasado. Fica sincronizado: se um gerente avança o item, todos veem. |
| **Enviar imagem** | Gera o PNG e abre o compartilhamento do celular (Telegram etc.), como antes. |

**Edição simultânea:** duas pessoas podem editar ao mesmo tempo, inclusive itens diferentes, sem perder nada. Se as duas editarem *o mesmo campo* ao mesmo tempo, vale a última alteração.

**Segurança:** o código da equipe funciona como uma senha compartilhada. Se ele vazar, passe a usar um código novo (o app cria um espaço vazio; salve antes os seus modelos como referência) e apague a pasta do código antigo em *Firestore Database → Dados* no console do Firebase.
