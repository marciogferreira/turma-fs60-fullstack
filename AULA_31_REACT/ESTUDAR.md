# Projeto Prático: Autenticação com JWT, React JS e DummyJSON

## 1. Objetivo

Desenvolver uma aplicação web utilizando React JS para implementar um fluxo de autenticação de usuários com JWT (JSON Web Token), consumindo a API [DummyJSON](https://dummyjson.com/).

Ao final da atividade, o aluno deverá compreender como funciona o processo de autenticação, como armazenar um token, como controlar o acesso às páginas e como consumir uma API externa.

## 2. Tecnologias utilizadas

- React JS
- JavaScript
- HTML5 e CSS3
- Bootstrap
- React Router DOM
- Fetch API ou Axios
- API DummyJSON
- JWT (JSON Web Token)

## 3. O que é JWT?

JWT significa **JSON Web Token**. É um padrão utilizado para transmitir informações entre sistemas por meio de um token.

Em aplicações web, ele é frequentemente utilizado para representar uma sessão autenticada e permitir que o cliente envie credenciais de acesso em requisições posteriores.

Um JWT possui três partes:

1. **Header:** informa o tipo do token e o algoritmo utilizado.
2. **Payload:** contém informações e declarações, como o identificador do usuário e a data de expiração.
3. **Signature:** permite verificar a integridade e a autenticidade do token.

O token costuma ser representado neste formato:

```text
header.payload.signature
```

Importante: o conteúdo de um JWT não é necessariamente criptografado. Portanto, não se deve armazenar senhas ou informações secretas em seu payload.

## 4. O que é autenticação?

Autenticação é o processo de verificar a identidade de um usuário.

Em uma aplicação web, o fluxo básico funciona da seguinte forma:

1. O usuário informa seu nome de usuário e sua senha.
2. O React envia as credenciais para a API.
3. A API verifica as credenciais.
4. Se forem válidas, a API retorna um token de autenticação.
5. A aplicação armazena o token conforme a estratégia adotada.
6. O usuário é direcionado para o dashboard.
7. A aplicação pode utilizar o token em novas requisições.
8. Ao sair da aplicação, o token é removido do armazenamento utilizado.

### Fluxo de autenticação

```text
Usuário
   |
   v
Tela de Login (React)
   |
   v
POST /auth/login
   |
   v
API DummyJSON
   |
   v
Validação das credenciais
   |
   v
Retorno do accessToken
   |
   v
Dashboard protegido
```

## 5. Conhecendo a API DummyJSON

A DummyJSON é uma API de demonstração que permite testar requisições HTTP sem precisar desenvolver um backend próprio.

Documentação oficial:

https://dummyjson.com/docs/auth

### Endpoint de autenticação

```http
POST https://dummyjson.com/auth/login
```

Exemplo de corpo da requisição:

```json
{
  "username": "emilys",
  "password": "emilyspass",
  "expiresInMins": 30
}
```

A API retorna informações do usuário e tokens, incluindo o `accessToken` e o `refreshToken`.

Observação: a duração do token depende do comportamento e da configuração da API. O parâmetro `expiresInMins` pode ser utilizado para solicitar uma duração específica, conforme o suporte do serviço.

### Exemplo de resposta

A resposta contém informações semelhantes a estas:

```json
{
  "id": 1,
  "username": "emilys",
  "email": "emily@example.com",
  "accessToken": "token_de_acesso",
  "refreshToken": "token_de_atualizacao"
}
```

Os valores apresentados são ilustrativos. A resposta real pode conter outros campos e valores.

### Credenciais para testes

Acesse a documentação de autenticação da DummyJSON para consultar as credenciais de demonstração disponíveis.

Essas credenciais são destinadas ao ambiente de testes, não a aplicações reais.

## 6. Preparando o ambiente React

Crie um projeto React utilizando Vite:

```bash
npm create vite@latest jwt-react-app -- --template react
```

Entre na pasta do projeto:

```bash
cd jwt-react-app
```

Instale as dependências:

```bash
npm install
npm install react-router-dom bootstrap
```

Execute a aplicação:

```bash
npm run dev
```

## 7. Estrutura sugerida do projeto

Organize os arquivos da seguinte maneira:

```text
src/
├── components/
│   └── PrivateRoute.jsx
├── contexts/
│   └── AuthContext.jsx
├── pages/
│   ├── Login.jsx
│   └── Dashboard.jsx
├── services/
│   └── api.js
├── App.jsx
├── main.jsx
└── index.css
```

Responsabilidade dos arquivos:

- `Login.jsx`: apresenta o formulário de login.
- `Dashboard.jsx`: apresenta as informações do usuário autenticado.
- `AuthContext.jsx`: gerencia o estado de autenticação.
- `PrivateRoute.jsx`: controla o acesso às rotas protegidas.
- `api.js`: concentra as chamadas para a API.
- `App.jsx`: define as rotas da aplicação.
- `main.jsx`: inicializa o React e importa os estilos.

## 8. Implementando o login

Crie uma tela contendo os seguintes elementos:

- Campo de nome de usuário.
- Campo de senha.
- Botão Entrar.
- Mensagem de erro para credenciais inválidas.
- Indicador de carregamento durante a requisição.

Ao clicar no botão Entrar, a aplicação deverá realizar uma requisição HTTP para a DummyJSON.

Exemplo de chamada utilizando Fetch API:

```javascript
async function login(username, password) {
  const response = await fetch(
    "https://dummyjson.com/auth/login",
    {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        username,
        password,
        expiresInMins: 30,
      }),
    }
  );

  const data = await response.json();

  if (!response.ok) {
    throw new Error(
      data.message || "Falha na autenticação."
    );
  }

  return data;
}
```

Esse código deve ser integrado ao formulário React.

Quando a autenticação for bem-sucedida, a aplicação deverá receber o token e atualizar o estado de autenticação.

**Atenção:** não basta verificar se existe um token no navegador para garantir que ele seja válido. A validade precisa ser verificada pelo servidor quando o token for utilizado.

## 9. Armazenamento do token

Para fins didáticos, o aluno poderá utilizar `sessionStorage` para manter o token durante a sessão da aba do navegador.

Exemplo:

```javascript
sessionStorage.setItem(
  "accessToken",
  data.accessToken
);
```

Para recuperar o token:

```javascript
const token = sessionStorage.getItem(
  "accessToken"
);
```

Para remover o token ao sair:

```javascript
sessionStorage.removeItem("accessToken");
```

O `sessionStorage` reduz a persistência em comparação com o `localStorage`, mas não impede o acesso ao token caso exista uma vulnerabilidade de Cross-Site Scripting (XSS).

Em aplicações de produção, considere uma arquitetura com cookies `HttpOnly`, `Secure` e `SameSite`, além das proteções adequadas contra CSRF. A estratégia ideal depende da arquitetura do sistema.

## 10. Protegendo as rotas

Utilize o React Router DOM para criar rotas públicas e privadas.

Rotas esperadas:

| Rota | Acesso |
|---|---|
| `/login` | Público |
| `/dashboard` | Somente usuários autenticados |

O componente `PrivateRoute` deverá verificar o estado de autenticação.

Comportamento esperado:

- Se o usuário estiver autenticado, poderá acessar o dashboard.
- Se não estiver autenticado, deverá ser redirecionado para o login.
- Após sair da aplicação, o usuário não deverá continuar acessando o dashboard por meio da navegação interna.

Lembre-se: uma rota privada no React controla apenas a navegação da interface. Em uma aplicação real, o backend também deve proteger os dados e validar o token.

## 11. Consumindo dados protegidos

Depois do login, utilize o token retornado pela API para consultar os dados do usuário autenticado.

Exemplo de endpoint:

```http
GET https://dummyjson.com/auth/me
```

Envie o token no cabeçalho `Authorization`:

```javascript
const response = await fetch(
  "https://dummyjson.com/auth/me",
  {
    headers: {
      Authorization: `Bearer ${token}`,
    },
  }
);

if (!response.ok) {
  throw new Error("Não foi possível carregar o usuário.");
}

const user = await response.json();
```

O prefixo `Bearer` indica que a credencial enviada no cabeçalho é um token do tipo Bearer.

A API DummyJSON disponibiliza esse endpoint para demonstração. Em uma aplicação real, o servidor deverá validar a assinatura, a expiração e as permissões aplicáveis ao token.

## 12. Criando o Dashboard

A tela de dashboard deverá apresentar:

- Nome do usuário autenticado.
- E-mail do usuário.
- Nome de usuário.
- Imagem de perfil, se disponível.
- Botão Sair.
- Mensagem de boas-vindas.

Exemplo de apresentação:

```text
+--------------------------------------+
| Dashboard                   [Sair]   |
+--------------------------------------+
|                                      |
| Bem-vindo, Emily!                    |
|                                      |
| Usuário: emilys                      |
| E-mail: emily@example.com            |
|                                      |
| Você está autenticado na aplicação. |
|                                      |
+--------------------------------------+
```

Os dados exibidos devem vir da resposta da API, e não de valores fixos no código.

## 13. Implementando o logout

Ao clicar no botão Sair, a aplicação deverá:

1. Remover o token do armazenamento utilizado.
2. Limpar o estado do usuário autenticado.
3. Redirecionar o usuário para `/login`.
4. Impedir o acesso ao dashboard pela navegação da interface.

Se a aplicação implementar renovação de tokens, o fluxo de logout também deverá considerar a invalidação ou revogação de credenciais quando suportada pelo backend.

## 14. Tratamento de erros

Implemente mensagens apropriadas para as seguintes situações:

- Usuário ou senha incorretos.
- Campos obrigatórios não preenchidos.
- Falha de conexão com a API.
- Token inválido ou expirado.
- Falha ao carregar os dados do usuário.
- Tentativa de acessar o dashboard sem autenticação.

Não apresente detalhes técnicos desnecessários ao usuário final. Registre informações de depuração sem expor senhas ou tokens.

## 15. Desafio prático

Desenvolva a aplicação completa com os seguintes requisitos:

- [ ] Criar a tela de login utilizando React e Bootstrap.
- [ ] Implementar validação dos campos.
- [ ] Consumir o endpoint `POST /auth/login`.
- [ ] Exibir mensagens de erro e carregamento.
- [ ] Armazenar o token de autenticação.
- [ ] Criar um contexto de autenticação com React Context API.
- [ ] Implementar rotas com React Router DOM.
- [ ] Criar um componente `PrivateRoute`.
- [ ] Criar o dashboard com dados reais da API.
- [ ] Consumir o endpoint `GET /auth/me` com o token Bearer.
- [ ] Implementar o logout.
- [ ] Testar o acesso ao dashboard sem autenticação.
- [ ] Testar o comportamento após remover ou invalidar o token.
- [ ] Publicar o código-fonte no GitHub.

## 16. Critérios de avaliação

| Critério | Pontuação |
|---|---:|
| Interface de login e dashboard | 2,0 |
| Integração com a API DummyJSON | 2,0 |
| Gerenciamento do estado de autenticação | 2,0 |
| Proteção das rotas e