# Ampliando o Conhecimento em React

## Iniciando um Projeto com React CLI e Navegação

**Create React App** é um ambiente confortável para aprender React, e é a melhor maneira de começar um single-page application em React.

Além de configurar seu ambiente de desenvolvimento para utilizar as funcionalidades mais recentes do JavaScript, ele fornece uma experiência de desenvolvimento agradável e otimizada.

### React Router Dom

O **React Router Dom** é uma biblioteca popular e essencial para a construção de aplicativos React que possuem várias páginas ou rotas. Ele permite a criação de um sistema de navegação eficiente, onde diferentes componentes são renderizados com base na URL atual do navegador, sem a necessidade de recarregar a página.

O React Router Dom utiliza o conceito de "roteamento" para mapear diferentes URLs para componentes específicos, garantindo assim que apenas os componentes relevantes sejam renderizados conforme o usuário navega pelo aplicativo. Essa abordagem ajuda a criar uma experiência de usuário mais suave e responsiva.

- **BrowserRouter**: Usado para envolver toda a aplicação e fornecer o contexto de roteamento para os outros componentes do React Router.
- **Routes**: Contém todas as rotas da aplicação.
- **Route**: Define uma rota específica. Ele renderiza um componente quando a URL corresponde ao caminho especificado em sua propriedade
- **Link**: usado para criar links entre as rotas da aplicação. Ele é semelhante a um elemento \<a> HTML, mas em vez de recarregar a página inteira, ele atualiza apenas o conteúdo que mudou. Ele também mantém a URL sincronizada com o que está sendo exibido na página.

## Estilização com Styled-Components

**Styled Components** É uma biblioteca que possibilita escrever códigos CSS dentro do JavaScript.

`yarn add styled-components`

## Hooks

Hooks são funções que permitem que você se conecte aos recursos de estado e ciclo de vida do React a partir de componentes funcionais. Eles permitem que você use o state e outras features do React como os métodos do ciclo de vida, sem precisar escrever uma classe.

### useState(): Estado

Este hook permite que você adicione estado a um componente funcional.

```jsx
const [count, setCount] = useState(0);

const increment = () => {
  setCount(count + 1);
};
```

### useEffect(): Ciclo de Vida

Este hook permite que você realize efeitos colaterais em componentes funcionais. Efeitos colaterais são ações que acontecem fora do escopo da renderização normal do componente.

```jsx
const [data, setData] = useState([]);

useEffect(() => {
  const fetchData = async () => {
    try {
      const response = await fetch("https://api.example.com/data");
      const data = await response.json();
      setData(data);
    } catch (error) {
      console.error("Erro ao buscar os dados:", error);
    }
  };

  fetchData();
}, []);
```

### useMemo(): Renderizações de Variáveis

```jsx
const memoizedValue = useMemo(() => {
  // Cálculos ou lógica que você deseja memorizar
}, [dependencyList]);
```

Permite otimizar o desempenho de componentes funcionais, evitando cálculos desnecessários. Ele memoriza o resultado de uma função e retorna a mesma referência caso os argumentos não tenham sido alterados desde a última renderização.

```jsx
import React, { useState, useMemo } from "react";

const HeavyComputationComponent = () => {
  const [count, setCount] = useState(0);

  // Usando useMemo para memorizar o resultado do cálculo
  const expensiveResult = useMemo(() => {
    console.log("Realizando cálculos pesados...");
    let result = 0;
    for (let i = 0; i < 1000000000; i++) {
      result += i;
    }
    return result;
  }, [count]); // A função de cálculo será reexecutada apenas se 'count' mudar

  const increment = () => {
    setCount(count + 1);
  };

  return (
    <div>
      <p>Contagem: {count}</p>
      <p>Resultado calculado: {expensiveResult}</p>
      <button onClick={increment}>Incrementar</button>
    </div>
  );
};
```

Sem o useMemo(), o cálculo seria reexecutado em cada renderização, mesmo que o estado count não tenha mudado, o que pode levar a uma degradação de desempenho. Com o useMemo(), o cálculo é executado apenas quando o estado count muda, o que melhora a eficiência do componente.

### useCallback(): Renderizações de Funções

```jsx
const memoizedCallback = useCallback(
  () => {
    // Código da função que você deseja memorizar
  },
  [dependencyList] // Lista de dependências (opcional)
);
```

Usado para memorizar uma função e evitar que ela seja recriada a cada renderização do componente. Ele é útil quando você precisa passar funções como propriedades para componentes filhos e deseja otimizar o desempenho, evitando recriações desnecessárias de funções.

```jsx
import React, { useState, useCallback } from "react";

const ChildComponent = ({ onClick }) => {
  // Componente filho recebe a função de callback através das props
  return <button onClick={onClick}>Clique aqui</button>;
};

const ParentComponent = () => {
  const [count, setCount] = useState(0);

  // Usando useCallback para memorizar a função de callback
  const handleClick = useCallback(() => {
    setCount(count + 1);
  }, [count]); // A função será recriada apenas se 'count' mudar

  return (
    <div>
      <p>Contagem: {count}</p>
      <ChildComponent onClick={handleClick} />
    </div>
  );
};
```

Sem o uso do useCallback(), sempre que o componente é re-renderizado, uma nova instância da função é criada, o que pode levar a perda de desempenho e pode afetar a performance de componentes filhos que dependem dessas funções.

### Comparações

- Muitas vezes você verá `useMemo` ao lado de `useCallback`. Ambos são úteis quando você está tentando otimizar um componente filho. Eles permitem que você memoize (ou, em outras palavras, armazene em cache) algo que está transmitindo.
  - `useMemo` armazena em cache o resultado da chamada de sua função. Quando necessário, o React chamará a função que você passou durante a renderização para calcular o resultado.
  - `useCallback` armazena em cache a própria função. Ao contrário de useMemo, ele não chama a função que você fornece.

## Formulários

### [React-Hook-Form](https://react-hook-form.com/)

React Hook Form é uma biblioteca de formulários para React que permite criar formulários de maneira performática, flexível e extensível. Ela minimiza o número de re-renderizações, minimiza a validação e montagem mais rápida.

```jsx
import { useForm, Controller } from "react-hook-form";
```

```jsx
const {
  control,
  handleSubmit,
  formState: { errors, isValid },
} = useForm();
```

```jsx
<Controller
  name="checkbox"
  control={control}
  rules={{ required: true }}
  render={({ field }) => <div {...field} />}
/>
```

Para validações, utilizar `@hookform/resolvers yup`.

## Projetos

- [Desenvolvendo a Tela de Cadastro da Plataforma Dio com React](https://github.com/Err0rGCeni/DIOProject_DIOCloneReact)
