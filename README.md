# Backend

## Sobre a API
- A API deve ouvir para http na porta 6000 e para https na porta 6001.
- Deve ser documentada pelo swagger (openAPI).
- Ao iniciar a aplicação o swagger (openAPI) deve ser iniciado.
- A aplicação deve ser separada nas camadas domínio, aplicação, infra e webapi.

## Banco de dados
A API deve utilizar o entity framework para fazer acesso a dados utilizando o banco de dados em memória. Ao iniciar a aplicação o banco de dados deve conter 10 entidades criadas, para isso deve ser feito o ```seed``` do banco de dados.

## Modelo de dados

O blog deve conter um lista de posts. Um post pode ter nenhum, uma ou muitas tags e as tags podem se repetir em vários posts.

A entidade Blog de ter as propriedades: título e resumo, ambas obrigatórias. O título deve ter no máximo 90 caracteres e o resumo 300 caracteres.

```csharp
public class Blog
{
    public Guid Id { get; set; }
    public string Titulo { get; set; }
    public string Resumo { get; set; }
}
```

O título deve ser obrigatório e conter, no máximo, 90 caracteres. A descrição é obrigatória e deve ser limitada em 2000 caracteres. A thumbnail deve ser opcional e limitada a 1024 caracteres. A data de publicação é opcional e como valor padrão a data em que foi inserido o registro. A propriedade rascunho também é opcional e deve ter como valor padrão true.

```csharp
public class Post
{
    public Guid Id { get; set; }
    public string Titulo { get; set; }
    public string Descricao { get; set; }
    public string Thumbnail { get; set; }
    public DateTime? DataPublicacao { get; set; }
    public bool? Rascunho { get; set; }
}
```

A entidade tag deve ter a propriedade nome como obrigatória e limitada em 50 caracteres.

```csharp
public class Tag
{
    public Guid Id { get; set; }
    public string Nome { get; set; }
}
```

## Endpoints

### POST - /blog

Este endpoint é responsável por criar um blog. Ele deve receber um objeto com o seguinte formato:

```json
{
    "titulo": "Lorem Ipsum",
    "resumo": "Lorem Ipsum is simply dummy text of the printing and typesetting industry"
}
```

Ao receber as informações deve ser validado se todos os campos estão preenchidos e se nenhum campo tem quantidade de caracteres superior limite do banco de dados. Caso alguma propriedade esteja inválida deve retornar uma mensagem de erro dizendo o motivo do erro e qual propriedade está incorreta. Como retorno deve ter a seguinte resposta

```json
{
    "sucesso": "true",
    "mensagem": "Blog criado com sucesso",
    "dados": {
        "blogId": "2bbc5284-08dd-410b-aaec-9cb6e3f58d46",
        "titulo": "Lorem Ipsum",
        "resumo": "Lorem Ipsum is simply dummy text of the printing and typesetting industry"
    }
}
```

### GET - /blog

Este endpoint deve listar todos os blogs disponíveis e apresentar a seguinte resposta:

```json
[
    {
        "id": "2bbc5284-08dd-410b-aaec-9cb6e3f58d46",
        "titulo": "Lorem Ipsum",
        "resumo": "Lorem Ipsum is simply dummy text of the printing and typesetting industry"
    }
]
```

### GET - /blog/{blogId}

Este endpoint deve listar todos os detalhes de um blog. Essas informações devem ficar em cache na memória por 1 dia.

```json
{
    "id": "2bbc5284-08dd-410b-aaec-9cb6e3f58d46",
    "titulo": "Lorem Ipsum",
    "resumo": "Lorem Ipsum is simply dummy text of the printing and typesetting industry"
}
```

### PUT - /blog/{blogId}

Este endpoint deve editar todos os detalhes de um blog. Ao atualizar as informações com sucesso deve ser removido do cache da aplicação. O request tem a seguinte estrutura

```json
{
    "blogId": "2bbc5284-08dd-410b-aaec-9cb6e3f58d46",
    "titulo": "Lorem Ipsum",
    "resumo": "Lorem Ipsum is simply dummy text of the printing and typesetting industry"
}
```

e o response segue esse padrão:

```json
{
    "sucesso": "true",
    "mensagem": "O blog ##TITULO## foi atualizado com sucesso",
    "dados": {
        "blogId": "2bbc5284-08dd-410b-aaec-9cb6e3f58d46",
        "titulo": "Lorem Ipsum",
        "resumo": "Lorem Ipsum is simply dummy text of the printing and typesetting industry"
    }
}
```

### POST - /post

Esse endpoint é responsável por criar um post em um determinado blog. A sua estrutura de request deve ser:

```json
    {
        "blogId": "2bbc5284-08dd-410b-aaec-9cb6e3f58d46",
        "titulo": "Lorem Ipsum",
        "descricao": "Lorem Ipsum is simply dummy text of the printing and typesetting industry",
        "thumbnail": "https://www.enderecodathumb.com/imagem.jpg"
    }
```

E a response segue essa estrutura:

```json
{
    "sucesso": "true",
    "mensagem": "Post adicionado com sucesso",
    "dados": {
        "id": "2bbc5284-08dd-410b-aaec-9cb6e3f58d46",
        "blogId": "2bbc5284-08dd-410b-aaec-9cb6e3f58d46",
        "titulo": "Lorem Ipsum",
        "descricao": "Lorem Ipsum is simply dummy text of the printing and typesetting industry",
        "thumbnail": "https://www.enderecodathumb.com/imagem.jpg"
    }
}
```

### PUT - /post/{postId}

Este endpoint é responsável por atualizar um post e deve seguir a estrutura:

```json
    {
        "postId": "2bbc5284-08dd-410b-aaec-9cb6e3f58d46",
        "titulo": "Lorem Ipsum",
        "descricao": "Lorem Ipsum is simply dummy text of the printing and typesetting industry",
        "thumbnail": "https://www.enderecodathumb.com/imagem.jpg",
        "dataPublicacao": "YYYYMMDDHHMMSS",
        "rascunho": 1
    }
```

e como response deve retornar o seguinte objeto.

```json
{
    "sucesso": "true",
    "mensagem": "Post editado com sucesso",
    "dados": {
        "postId": "2bbc5284-08dd-410b-aaec-9cb6e3f58d46",
        "titulo": "Lorem Ipsum",
        "descricao": "Lorem Ipsum is simply dummy text of the printing and typesetting industry",
        "thumbnail": "https://www.enderecodathumb.com/imagem.jpg",
        "dataPublicacao": "YYYYMMDDHHMMSS",
        "rascunho": 1
    }
}
```
