# Boas práticas no React

## Estados derivados

Um estado derivado no React é um estado que é calculado a partir de outros estados ou propriedades do componente.

Essa prática provoca `renderizações desnecessárias` que afetam a performace da sua aplicação. Exemplo:

```ts
import React, { useState } from 'react';

function App() {
  const [items, setItems] = useState([...]);
  const [filteredItems, setFilteredItems] = useState([])
  const [search, setSearch] = useState('')

  useEffect(() => {
    if(search.length > 0) {
      setFilteredItems(items.filter(item => item.nome.includes(search)))
    }
  , [search]})

  ...
}
```

O código acima executaria duas renderizações todas as vezes que o valor do state `search` fosse alterado. A primeira renderização seria pela mudança no valor do próprio state search e a segunda pela mudança no valor do state `filteredItems` que tem seu valor recalculado todas as vezes que o valor do state search é alterado.

Para corrigir este caso de estado derivado, bastaria tornar o state `filteredItems` uma simples variável. Exemplo:

```ts
import React, { useState } from 'react';

function App() {
  const [items, setItems] = useState([...]);
  const [search, setSearch] = useState('')

  const filteredItems = search.length > 0 ? items.filter(item => item.nome.includes(search)) : []
  ...
}
```

Neste caso ocorreria apenas uma renderização todas as vezes que o valor do state `search` fosse alterado.

Referência: https://www.youtube.com/watch?v=kCpca2z2cls