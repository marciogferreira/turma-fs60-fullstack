# Projeto Prático: CRUD com React JS e DummyJSON

## 1. Objetivo

Desenvolver uma aplicação web utilizando React JS para implementar as operações de CRUD (Create, Read, Update e Delete), consumindo a API DummyJSON.

O projeto deverá permitir visualizar, cadastrar, editar e excluir produtos, trabalhando com requisições HTTP e gerenciamento de estado no React.

## 2. Tecnologias utilizadas

- React JS
- JavaScript
- HTML5 e CSS3
- Bootstrap
- Fetch API
- API DummyJSON
- React Hooks (`useState` e `useEffect`)

## 3. O que é CRUD?

CRUD é um acrônimo que representa quatro operações fundamentais utilizadas no desenvolvimento de sistemas que manipulam dados.

| Operação | Significado | Método HTTP |
|---|---|---|
| Create | Criar um registro | POST |
| Read | Consultar registros | GET |
| Update | Atualizar um registro | PUT ou PATCH |
| Delete | Excluir um registro | DELETE |

### Exemplo prático

Em um sistema de gerenciamento de produtos:

- **Create:** cadastrar um novo produto.
- **Read:** listar os produtos cadastrados.
- **Update:** alterar o nome, o preço ou a descrição de um produto.
- **Delete:** excluir um produto da listagem.

## 4. Conhecendo a API DummyJSON

A DummyJSON é uma API de demonstração que disponibiliza dados fictícios para praticar o consumo de serviços REST.

Documentação oficial:

https://dummyjson.com/docs/products

Neste projeto, utilizaremos o recurso de produtos.

### Endpoint principal

```http
https://dummyjson.com/products
```

A API permite consultar, adicionar, atualizar e excluir produtos utilizando os métodos HTTP correspondentes.

**Importante:** as operações de criação, atualização e exclusão da DummyJSON são simuladas. Os dados retornados demonstram o resultado da operação, mas as alterações não ficam persistidas permanentemente no servidor.

## 5. Preparando o ambiente

Crie um projeto React utilizando Vite:

```bash
npm create vite@latest crud-react -- --template react
```

Acesse a pasta do projeto:

```bash
cd crud-react
```

Instale as dependências:

```bash
npm install
npm install bootstrap
```

Execute o projeto:

```bash
npm run dev
```

No arquivo `src/main.jsx`, importe o Bootstrap:

```javascript
import "bootstrap/dist/css/bootstrap.min.css";
```

## 6. Estrutura sugerida do projeto

Organize os arquivos da seguinte maneira:

```text
src/
├── components/
│   ├── ProductForm.jsx
│   ├── ProductList.jsx
│   └── ProductItem.jsx
├── services/
│   └── productService.js
├── App.jsx
├── main.jsx
└── index.css
```

Responsabilidades:

- `ProductForm.jsx`: formulário para cadastrar e editar produtos.
- `ProductList.jsx`: apresentação da lista de produtos.
- `ProductItem.jsx`: exibição de um produto e suas ações.
- `productService.js`: funções responsáveis pelas requisições HTTP.
- `App.jsx`: gerenciamento do estado e integração dos componentes.

## 7. READ — Consultar produtos

A operação Read permite consultar os produtos disponíveis na API.

### Endpoint

```http
GET https://dummyjson.com/products
```

### Exemplo com Fetch API

```javascript
async function getProducts() {
  const response = await fetch(
    "https://dummyjson.com/products"
  );

  if (!response.ok) {
    throw new Error("Erro ao consultar produtos.");
  }

  const data = await response.json();

  return data.products;
}
```

### Requisitos

- Consultar os produtos ao carregar a aplicação.
- Utilizar o hook `useEffect` para iniciar a consulta.
- Armazenar os produtos no estado com `useState`.
- Exibir os produtos em cards ou em uma tabela Bootstrap.
- Apresentar título, preço, categoria e imagem do produto.
- Exibir uma mensagem caso não existam produtos para apresentar.
- Tratar erros de comunicação com a API.

## 8. CREATE — Cadastrar produtos

A operação Create permite enviar os dados de um novo produto para a API.

### Endpoint

```http
POST https://dummyjson.com/products/add
```

### Exemplo de corpo da requisição

```json
{
  "title": "Notebook Gamer",
  "description": "Notebook para desenvolvimento",
  "price": 4500,
  "category": "laptops"
}
```

### Exemplo com Fetch API

```javascript
async function createProduct(product) {
  const response = await fetch(
    "https://dummyjson.com/products/add",
    {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify(product),
    }
  );

  if (!response.ok) {
    throw new Error("Erro ao cadastrar produto.");
  }

  return response.json();
}
```

### Requisitos

Crie um formulário com os seguintes campos:

- Nome do produto.
- Descrição.
- Preço.
- Categoria.

Ao clicar no botão **Cadastrar**, a aplicação deverá:

1. Validar os campos obrigatórios.
2. Enviar os dados para a API.
3. Exibir uma mensagem de sucesso ou erro.
4. Atualizar a interface para apresentar o novo produto.
5. Limpar o formulário após o cadastro bem-sucedido.

Como a API simula a criação, o aluno deverá inserir o produto retornado no estado local para visualizá-lo na tela atual.

## 9. UPDATE — Editar produtos

A operação Update permite modificar informações de um produto existente.

A DummyJSON oferece dois métodos para atualização:

- `PUT`: utilizado para enviar os dados que se deseja atualizar como representação do recurso.
- `PATCH`: utilizado para enviar alterações parciais.

### Endpoint

```http
PUT https://dummyjson.com/products/1
```

Ou:

```http
PATCH https://dummyjson.com/products/1
```

O número `1` representa o identificador do produto que será atualizado.

### Exemplo com Fetch API

```javascript
async function updateProduct(id, product) {
  const response = await fetch(
    `https://dummyjson.com/products/${id}`,
    {
      method: "PATCH",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify(product),
    }
  );

  if (!response.ok) {
    throw new Error("Erro ao atualizar produto.");
  }

  return response.json();
}
```

### Requisitos

- Adicionar um botão **Editar** para cada produto.
- Ao clicar em Editar, preencher o formulário com os dados atuais.
- Permitir alterar o nome, a descrição, o preço e a categoria.
- Exibir um botão **Salvar alterações**.
- Enviar a atualização para a API.
- Atualizar o produto correspondente no estado local.
- Exibir mensagens de sucesso ou erro.
- Permitir cancelar a edição.

## 10. DELETE — Excluir produtos

A operação Delete permite solicitar a exclusão de um produto.

### Endpoint

```http
DELETE https://dummyjson.com/products/1
```

### Exemplo com Fetch API

```javascript
async function deleteProduct(id) {
  const response = await fetch(
    `https://dummyjson.com/products/${id}`,
    {
      method: "DELETE",
    }
  );

  if (!response.ok) {
    throw new Error("Erro ao excluir produto.");
  }

  return response.json();
}
```

### Requisitos

- Adicionar um botão **Excluir** para cada produto.
- Solicitar confirmação antes de excluir.
- Enviar a requisição DELETE para a API.
- Remover o produto da listagem local após o sucesso.
- Exibir uma mensagem de confirmação.
- Tratar possíveis erros.

Lembre-se de que a exclusão é simulada pela API. Para esta atividade, a remoção visual deve ser realizada atualizando o estado React.

## 11. Gerenciamento de estado no React

Utilize `useState` para armazenar os produtos:

```javascript
const [products, setProducts] = useState([]);
```

Utilize também estados para controlar:

- Os valores do formulário.
- O produto selecionado para edição.
- O carregamento das requisições.
- As mensagens de erro.
- As mensagens de sucesso.

Para adicionar um produto ao estado local:

```javascript
setProducts((previousProducts) => [
  newProduct,
  ...previousProducts,
]);
```

Para atualizar um produto:

```javascript
setProducts((previousProducts) =>
  previousProducts.map((product) =>
    product.id === updatedProduct.id
      ? { ...product, ...updatedProduct }
      : product
  )
);
```

Para remover um produto:

```javascript
setProducts((previousProducts) =>
  previousProducts.filter(
    (product) => product.id !== productId
  )
);
```

Esses exemplos representam atualizações imutáveis do estado, uma prática recomendada no React.

## 12. Interface da aplicação

A interface deverá ser construída utilizando Bootstrap e apresentar:

### Cabeçalho

- Nome da aplicação: Gerenciamento de Produtos.
- Botão para cadastrar um novo produto.

### Formulário

- Campo para nome.
- Campo para descrição.
- Campo para preço.
- Campo para categoria.
- Botões para salvar e cancelar.

### Listagem

Cada produto deverá apresentar:

- Imagem.
- Nome.
- Descrição.
- Preço.
- Categoria.
- Botão Editar.
- Botão Excluir.

### Estados da interface

A aplicação deverá exibir:

- Indicador de carregamento.
- Mensagem de sucesso.
- Mensagem de erro.
- Mensagem de lista vazia.
- Confirmação antes da exclusão.

## 13. Desafio prático

Desenvolva a aplicação completa atendendo aos seguintes requisitos:

- [ ] Criar o projeto React com Vite.
- [ ] Instalar e configurar o Bootstrap.
- [ ] Consultar produtos utilizando GET.
- [ ] Cadastrar produtos utilizando POST.
- [ ] Editar produtos utilizando PATCH ou PUT.
- [ ] Excluir produtos utilizando DELETE.
- [ ] Criar formulário reutilizável para cadastro e edição.
- [ ] Utilizar `useState` e `useEffect`.
- [ ] Atualizar a interface sem recarregar a página.
- [ ] Validar os campos do formulário.
- [ ] Implementar mensagens de sucesso e erro.
- [ ] Implementar indicador de carregamento.
- [ ] Organizar o código em componentes.
- [ ] Separar as chamadas HTTP em um serviço.
- [ ] Publicar o projeto no GitHub.

## 14. Desafio extra

Para aprofundar os conhecimentos, implemente as funcionalidades adicionais:

1. **Busca:** pesquisar produtos pelo nome.
2. **Filtro:** filtrar produtos por categoria.
3. **Ordenação:** ordenar produtos pelo preço.
4. **Paginação:** apresentar os produtos em páginas.
5. **Detalhes:** criar uma visualização individual do produto.
6. **Responsividade:** adaptar a interface para celulares, tablets e computadores.
7. **Confirmação personalizada:** utilizar um modal Bootstrap para confirmar a exclusão.

A DummyJSON permite consultar produtos com parâmetros como:

```http
GET https://dummyjson.com/products/search?q=phone
```

Para consultar categorias, utilize a documentação oficial de produtos e seus endpoints de categorias.

## 15. Critérios de avaliação

| Critério | Pontuação |
|---|---:|
| Consulta e apresentação dos produtos | 2,0 |
| Cadastro de produtos | 2,0 |
| Edição de produtos | 2,0 |
| Exclusão de produtos | 2,0 |
| Organização, validação e interface | 2,0 |
| **Total** | **10,0** |

## 16. Perguntas para reflexão

Responda às perguntas no README do seu repositório:

1. O que significa CRUD?
2. Qual é a diferença entre os métodos GET, POST, PUT, PATCH e DELETE?
3. Qual é a função do `useState`?
4. Para que serve o `useEffect`?
5. Por que devemos atualizar o estado sem modificar diretamente o array original?
6. Qual é a diferença entre uma resposta HTTP de sucesso e uma resposta de erro?
7. Por que devemos separar os componentes da aplicação?
8. Qual é a vantagem de centralizar as chamadas HTTP em um serviço?
9. Por que os dados cadastrados na DummyJSON podem desaparecer após recarregar a página?
10. O que seria necessário para desenvolver um CRUD com persistência real em um banco de dados?

## 17. Entrega da atividade

O aluno deverá entregar:

1. Link do repositório GitHub.
2. Código-fonte completo.
3. README com instruções de instalação e execução.
4. Capturas de tela das operações CRUD.
5. Respostas às perguntas de reflexão.

## 18. Referências

- React: https://react.dev/
- React Hooks: https://react.dev/reference/react
- Bootstrap: https://getbootstrap.com/
- Fetch API: https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API
- DummyJSON — Products: https://dummyjson.com/docs/products

**Resultado esperado:** uma aplicação React capaz de consultar, cadastrar, editar e excluir produtos por meio de requisições HTTP à API DummyJSON, utilizando componentes reutilizáveis, gerenciamento de estado e uma interface responsiva.