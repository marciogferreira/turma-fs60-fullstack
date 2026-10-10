# React JS: Aprendendo Context API

## 1. Objetivo

Compreender o funcionamento da Context API do React JS e aprender a compartilhar informações entre diferentes componentes sem precisar passar propriedades manualmente por vários níveis da aplicação.

Ao final da atividade, o aluno deverá ser capaz de criar um contexto, disponibilizar dados por meio de um Provider e consumir essas informações em diferentes componentes.

## 2. Tecnologias utilizadas

- React JS
- JavaScript
- Vite
- HTML5 e CSS3
- Bootstrap (opcional)
- React Hooks (`useState`, `useContext` e `createContext`)

## 3. O que é Context API?

A Context API é um recurso nativo do React que permite compartilhar informações entre componentes sem precisar passar props manualmente por todos os componentes intermediários.

Ela é útil quando diferentes partes da aplicação precisam acessar os mesmos dados, como:

- Informações do usuário autenticado.
- Tema da aplicação (claro ou escuro).
- Idioma selecionado.
- Carrinho de compras.
- Preferências globais.
- Estado de autenticação.

### Exemplo de um problema sem Context API

Imagine a seguinte estrutura de componentes:

```text
App
 └── Dashboard
      └── Header
           └── UserMenu
                └── UserName
```

Se o nome do usuário estiver no componente `App`, mas precisar ser apresentado em `UserName`, será necessário passar a informação por meio de propriedades intermediárias.

Esse processo é conhecido como **Prop Drilling**.

### O que é Prop Drilling?

Prop Drilling acontece quando uma propriedade precisa ser repassada por vários componentes até chegar ao componente que realmente precisa utilizá-la.

Exemplo:

```jsx
function App() {
  const user = { name: "Marcio" };

  return <Dashboard user={user} />;
}

function Dashboard({ user }) {
  return <Header user={user} />;
}

function Header({ user }) {
  return <UserMenu user={user} />;
}

function UserMenu({ user }) {
  return <UserName user={user} />;
}

function UserName({ user }) {
  return <h2>Olá, {user.name}!</h2>;
}
```

Observe que os componentes `Dashboard`, `Header` e `UserMenu` apenas repassam a propriedade `user`.

Com a Context API, os componentes interessados podem acessar diretamente o contexto.

## 4. Principais recursos da Context API

A Context API utiliza três recursos fundamentais.

### `createContext`

Cria um contexto que poderá armazenar e disponibilizar um valor aos componentes consumidores.

```jsx
import { createContext } from "react";

export const UserContext = createContext(null);
```

### `Provider`

O Provider é o componente que disponibiliza o valor do contexto para os componentes descendentes.

Nas versões atuais do React, também é possível utilizar diretamente o contexto como Provider. Neste material, utilizaremos a forma `UserContext.Provider`, que continua sendo válida.

```jsx
<UserContext.Provider value={{ name: "Marcio" }}>
  <Dashboard />
</UserContext.Provider>
```

A propriedade `value` contém as informações que serão compartilhadas.

### `useContext`

O hook `useContext` permite que um componente leia o valor do contexto mais próximo na árvore de componentes.

```jsx
import { useContext } from "react";
import { UserContext } from "./UserContext";

function UserName() {
  const user = useContext(UserContext);

  return <h2>Olá, {user.name}!</h2>;
}
```

## 5. Criando o primeiro projeto

Crie uma aplicação React utilizando Vite:

```bash
npm create vite@latest context-api-app -- --template react
```

Acesse o diretório:

```bash
cd context-api-app
```

Instale as dependências:

```bash
npm install
```

Execute a aplicação:

```bash
npm run dev
```

## 6. Estrutura sugerida do projeto

Organize os arquivos da seguinte forma:

```text
src/
├── contexts/
│   └── UserContext.jsx
├── components/
│   ├── Header.jsx
│   └── UserProfile.jsx
├── App.jsx
├── main.jsx
└── index.css
```

Responsabilidades:

- `UserContext.jsx`: cria o contexto e disponibiliza as informações.
- `Header.jsx`: apresenta o nome do usuário.
- `UserProfile.jsx`: apresenta os dados do usuário.
- `App.jsx`: organiza os componentes e define o Provider.
- `main.jsx`: inicializa a aplicação React.

## 7. Criando o contexto

No arquivo `src/contexts/UserContext.jsx`, crie o contexto:

```jsx
import { createContext } from "react";

export const UserContext = createContext(null);
```

Esse contexto será utilizado para compartilhar as informações do usuário com os componentes descendentes.

## 8. Disponibilizando o contexto

No arquivo `App.jsx`, disponibilize os dados por meio do Provider:

```jsx
import { UserContext } from "./contexts/UserContext";
import Header from "./components/Header";
import UserProfile from "./components/UserProfile";

function App() {
  const user = {
    name: "Marcio",
    email: "marcio@example.com",
    role: "Desenvolvedor",
  };

  return (
    <UserContext.Provider value={user}>
      <Header />
      <UserProfile />
    </UserContext.Provider>
  );
}

export default App;
```

Nesse exemplo, os componentes `Header` e `UserProfile` podem acessar o contexto sem receber `user` por meio de props.

## 9. Consumindo o contexto com useContext

Crie o arquivo `src/components/Header.jsx`:

```jsx
import { useContext } from "react";
import { UserContext } from "../contexts/UserContext";

function Header() {
  const user = useContext(UserContext);

  return (
    <header>
      <h1>Minha Aplicação</h1>
      <p>Bem-vindo, {user.name}!</p>
    </header>
  );
}

export default Header;
```

Agora, crie `src/components/UserProfile.jsx`:

```jsx
import { useContext } from "react";
import { UserContext } from "../contexts/UserContext";

function UserProfile() {
  const user = useContext(UserContext);

  return (
    <section>
      <h2>Perfil do Usuário</h2>
      <p>Nome: {user.name}</p>
      <p>E-mail: {user.email}</p>
      <p>Função: {user.role}</p>
    </section>
  );
}

export default UserProfile;
```

Os dois componentes acessam o mesmo valor disponibilizado pelo Provider.

## 10. Compartilhando estados com Context API

Até agora, compartilhamos apenas um objeto fixo. Na prática, é comum compartilhar estados que podem ser modificados durante a execução da aplicação.

Vamos criar um contador global.

No arquivo `App.jsx`:

```jsx
import { createContext, useState } from "react";

export const CounterContext = createContext(null);

function App() {
  const [count, setCount] = useState(0);

  return (
    <CounterContext.Provider value={{ count, setCount }}>
      <h1>Contador Global</h1>
      <CounterDisplay />
      <CounterButtons />
    </CounterContext.Provider>
  );
}
```

Para manter os componentes organizados, importe `useContext` e crie os componentes `CounterDisplay` e `CounterButtons` no mesmo arquivo, ou mova-os para arquivos separados.

Exemplo dos componentes:

```jsx
import { useContext } from "react";

function CounterDisplay() {
  const { count } = useContext(CounterContext);

  return <h2>Valor: {count}</h2>;
}

function CounterButtons() {
  const { setCount } = useContext(CounterContext);

  return (
    <div>
      <button onClick={() => setCount((value) => value + 1)}>
        Incrementar
      </button>

      <button onClick={() => setCount((value) => value - 1)}>
        Decrementar
      </button>
    </div>
  );
}
```

**Importante:** os componentes que utilizam `CounterContext` devem estar dentro do Provider. Para esse exemplo funcionar, mantenha os componentes no mesmo arquivo ou exporte e importe o contexto corretamente.

Quando `setCount` modifica o estado, o React atualiza os componentes que consomem esse contexto.

## 11. Criando um AuthContext

Um dos usos mais comuns da Context API é gerenciar o estado de autenticação de uma aplicação.

Imagine que uma aplicação possui:

- Tela de login.
- Cabeçalho.
- Dashboard.
- Menu do usuário.
- Botão de logout.

Vários componentes podem precisar saber se o usuário está autenticado.

Em vez de passar essa informação por props, podemos criar um `AuthContext`.

Exemplo simplificado de `src/contexts/AuthContext.jsx`:

```jsx
import {
  createContext,
  useContext,
  useState,
} from "react";

const AuthContext = createContext(null);

export function AuthProvider({ children }) {
  const [user, setUser] = useState(null);

  function login(userData) {
    setUser(userData);
  }

  function logout() {
    setUser(null);
  }

  return (
    <AuthContext.Provider
      value={{
        user,
        isAuthenticated: Boolean(user),
        login,
        logout,
      }}
    >
      {children}
    </AuthContext.Provider>
  );
}

export function useAuth() {
  const context = useContext(AuthContext);

  if (!context) {
    throw new Error(
      "useAuth deve ser utilizado dentro de AuthProvider."
    );
  }

  return context;
}
```

Esse exemplo demonstra apenas o gerenciamento do estado. Ele não valida credenciais, não gera JWT e não protege recursos do servidor. Para implementar autenticação real, é necessário integrar o contexto a um serviço de autenticação.

### Utilizando o AuthProvider

No `App.jsx`:

```jsx
import { AuthProvider } from "./contexts/AuthContext";
import Header from "./components/Header";
import Dashboard from "./components/Dashboard";

function App() {
  return (
    <AuthProvider>
      <Header />
      <Dashboard />
    </AuthProvider>
  );
}

export default App;
```

### Consumindo o contexto

Um componente poderá utilizar o hook personalizado `useAuth`:

```jsx
import { useAuth } from "../contexts/AuthContext";

function Header() {
  const { user, isAuthenticated, logout } = useAuth();

  return (
    <header>
      {isAuthenticated ? (
        <>
          <span>Olá, {user.name}</span>
          <button onClick={logout}>Sair</button>
        </>
      ) : (
        <span>Visitante</span>
      )}
    </header>
  );
}

export default Header;
```

O hook personalizado deixa o acesso ao contexto mais simples e centraliza a lógica de consumo.

## 12. Context API e useState: qual é a diferença?

Esses recursos possuem funções diferentes e podem ser utilizados juntos.

| Recurso | Finalidade |
|---|---|
| `useState` | Criar e atualizar um estado local. |
| `createContext` | Criar um contexto compartilhável. |
| `Provider` | Disponibilizar o valor do contexto. |
| `useContext` | Consumir o valor disponibilizado. |

A Context API não substitui o `useState`. É comum utilizar `useState` dentro de um Provider para compartilhar um estado global.

## 13. Context API ou gerenciamento de estado externo?

A Context API costuma ser suficiente para estados globais simples ou moderados, como tema, idioma e usuário autenticado.

Em aplicações maiores, bibliotecas como Redux Toolkit e Zustand podem oferecer recursos adicionais para organização, atualização seletiva e depuração do estado.

A escolha depende da complexidade da aplicação. Não é necessário utilizar uma biblioteca externa apenas para compartilhar um valor simples.

### Atenção ao desempenho

Quando o valor de um contexto muda, os componentes que o consomem podem precisar renderizar novamente.

Para evitar renderizações desnecessárias em aplicações maiores, considere:

- Dividir contextos com responsabilidades diferentes.
- Evitar colocar todo o estado da aplicação em um único contexto.
- Memorizar valores e funções quando houver uma necessidade real de desempenho.
- Utilizar ferramentas de análise antes de realizar otimizações.

## 14. Desafio prático

Desenvolva uma aplicação chamada **Painel do Usuário**, utilizando React JS e Context API.

### Requisitos funcionais

- [ ] Criar um contexto para compartilhar os dados do usuário.
- [ ] Implementar um Provider.
- [ ] Criar um cabeçalho que apresente o nome do usuário.
- [ ] Criar uma tela de perfil com nome, e-mail e função.
- [ ] Criar um componente que permita alterar o nome do usuário.
- [ ] Compartilhar as alterações entre os componentes.
- [ ] Criar um botão para alternar entre tema claro e escuro.
- [ ] Criar um contexto separado para o tema.
- [ ] Implementar um hook personalizado para consumir pelo menos um dos contextos.
- [ ] Organizar o projeto em componentes e pastas.
- [ ] Publicar o projeto no GitHub.

### Requisitos técnicos

O projeto deverá utilizar:

- `createContext`
- `useContext`
- `useState`
- Componentes funcionais
- Props quando forem apropriadas
- Hooks personalizados
- Separação de responsabilidades

## 15. Desafio extra

Implemente um carrinho de compras compartilhado entre componentes.

A aplicação deverá permitir:

1. Listar produtos.
2. Adicionar produtos ao carrinho.
3. Exibir a quantidade de itens no cabeçalho.
4. Apresentar os produtos do carrinho em outra tela ou componente.
5. Remover produtos do carrinho.
6. Calcular o valor total da compra.

O estado do carrinho deverá ser compartilhado por meio de um `CartContext`.

## 16. Perguntas para reflexão

Responda às perguntas no README do seu projeto:

1. O que é a Context API?
2. O que é Prop Drilling?
3. Qual é a finalidade de `createContext`?
4. Qual é a função do Provider?
5. Para que serve o hook `useContext`?
6. Qual é a diferença entre props e Context API?
7. Por que a Context API pode ser útil em um sistema de autenticação?
8. A Context API substitui o `useState`? Explique.
9. Quais problemas de desempenho podem ocorrer com contextos muito grandes?
10. Em quais situações seria interessante utilizar Redux Toolkit ou Zustand?

## 17. Critérios de avaliação

| Critério | Pontuação |
|---|---:|
| Criação e configuração dos contextos | 2,0 |
| Compartilhamento correto de estados | 2,0 |
| Organização dos componentes | 2,0 |
| Funcionalidades de alteração e tema | 2,0 |
| Qualidade do código e respostas teóricas | 2,0 |
| **Total** | **10,0** |

## 18. Entrega

O aluno deverá entregar:

1. Link do repositório GitHub.
2. Código-fonte completo.
3. README com as instruções para instalação e execução.
4. Capturas de tela da aplicação.
5. Respostas às perguntas de reflexão.

## 19. Referências

- React — Passing Data Deeply with Context: https://react.dev/learn/passing-data-deeply-with-context
- React — `createContext`: https://react.dev/reference/react/createContext
- React — `useContext`: https://react.dev/reference/react/useContext
- React — `useState`: https://react.dev/reference/react/useState
- Vite: https://vite.dev/

**Resultado esperado:** uma aplicação React organizada, capaz de compartilhar e atualizar informações entre componentes utilizando Context API, reduzindo o Prop Drilling e facilitando a manutenção do código.
