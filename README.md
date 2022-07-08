# Desafio Backend

![PHP Version](https://img.shields.io/badge/7.3-100000?style=for-the-badge&logo=PHP&logoColor=white&labelColor=8780E4&color=FFFFFF)
![Laravel Version](https://img.shields.io/badge/8.75-100000?style=for-the-badge&logo=Laravel&logoColor=white&labelColor=CA4641&color=FFFFFF)

O desafio consiste em criar um programa que consulte a api do [reddit](https://www.reddit.com/dev/api/) uma vez por dia (deve ser uma tarefa agendada para rodar em um horário específico que você definir).

A sua tarefa diária deve salvar num banco de dados SQL as postagens que estejam HOT do subredit [artificial](https://api.reddit.com/r/artificial/hot). Você deve salvar título da postagem, nome do autor, timestamp da criação, número de "ups" e número de comentários, e criar dois endpoints para consulta desses dados (endpoints REST ou usando graphql).

O primeiro endpoint deve receber como parâmetro uma data inicial, uma data final e uma ordem (as ordens possíveis são número de "ups" e número de comentários) e deve retornar as postagens criadas dentro desse range seguindo a ordem estipulada (em ordem decrescente).

O segundo endpoint deve receber como parâmetro uma ordem (as ordens possíveis são número de "ups" e número de comentários) e deve retornar uma lista de autores
seguindo a ordem estipulada (em ordem decrescente)

Você pode utilizar qualquer linguagem para resolver o desafio, mas preferimos que seja em PHP (é a linguagem que mais utilizamos aqui). Além disso, não esqueça de incluir instruções sobre como executar o seu projeto.

O que vamos avaliar:
- Se atende ao que foi pedido;
- Arquitetura bem definida;
- Legibilidade e organização;
- Falhas de segurança;
- Tratamento de erros;
- Quantidade de bugs.

Pontos extras:
- Testes unitários;
- Uso de container;
- Documentação;
- Utilizar framework CakePHP

-----------------------------------

## Instruções para desenvolvimento
- Instalação do [Docker](https://www.docker.com/get-started/)
- Instalação do [GIT](https://git-scm.com/download/)
- Instalação do [Composer](https://getcomposer.org/download/) para utilização do laravel
- Instalação do [HeidiSql](https://www.heidisql.com/download.php) Editor de BD (Opcional)


### Inicilização do servidor via terminal (Windows)
- Iniciar o terminal
- Clonar o projeto `git clone https://github.com/celostad/desafio-tecnico-it-health.git`
- Entrar na pasta do projeto `cd desafio-tecnico-it-health`
- instalar o projeto `docker-compose up -d --build`
- Entrar na pasta do Laravel `cd www`
- Instalar o composer `composer install`
- Criar o .env `cp .env.example .env`
- Gerar a chave do servidor artisan `php artisan key:generate`
- Inicializar o servidor `php artisan serve`
- Inicializar a fila que executará a consulta da API `php artisan schedule:work`

### Banco de dados
- Iniciar o terminal
- Entrar na pasta do projeto `cd desafio-tecnico-it-health`
- Criar as tabelas necessárias (já populadas) `php artisan migrate --seed`

## Principais arquivos do projeto
- [Rotas e validação de parâmetros GET](routes/web.php)
- [Model das postagens que estejam HOT do subredit](app/Models/HotPost.php)
- [Repositório das postagens](app/Repositories/HotPostRepository.php)
- [Controller dos endpoins](app/Http/Controllers/HotPostController.php)
- [Definição do agendamento da tarefa de consulta a API](app/Console/Kernel.php)
- [Seeder que consulta a API e alimenta o banco](database/seeders/HotPostsTableSeeder.php)

## Endpoints

### Created posts
`http://127.0.0.1:8000/created-posts/{initial_date}/{final_date}/{order}`
> Retorna as postagens criadas dentro desse range seguindo a ordem estipulada (em ordem decrescente)

Parâmetros de entrada:
- initial_date: datetime string `yyyy-mm-dd`
- final_date: datetime string `yyyy-mm-dd`
- order: `num_comments` ou `ups` 

Exemplo de saída:
// http://127.0.0.1:8000/created-posts/2022-07-01/2022-07-07/ups

```
 

[
  {
    "id": 19,
    "title": "Tom Cruise without the power of Scientology.",
    "author": "cganimater",
    "ups": 112,
    "num_comments": 10,
    "post_created_at": "2022-07-05 09:26:06",
    "created_at": "2022-07-07T23:31:36.000000Z",
    "updated_at": "2022-07-07T23:31:36.000000Z"
  },
  {
    "id": 7,
    "title": "Meta's latest open source AI can translate 200 languages",
    "author": "much_successes",
    "ups": 84,
    "num_comments": 8,
    "post_created_at": "2022-07-06 04:00:07",
    "created_at": "2022-07-07T23:31:36.000000Z",
    "updated_at": "2022-07-07T23:31:36.000000Z"
  },
]
```

cURL:
```
    curl http://127.0.0.1:8000/created-posts/2022-07-01/2022-07-30/ups
    curl http://127.0.0.1:8000/created-posts/2022-07-01/2022-07-30/num_comments
    curl http://127.0.0.1:8000/created-posts/2022-07-01/2022-07-24/num_comments
    curl http://127.0.0.1:8000/created-posts/2022-07-07/2022-07-07/ups
```

### Authors
`http://127.0.0.1:8000/authors/{order}`
> Retorna uma lista de autores seguindo a ordem estipulada (em ordem decrescente)

Parâmetros de entrada:
- order: `num_comments` ou `ups` 

Exemplo de saída:
```
[
    {"author":"snoggel"},
    {"author":"CyberByte"},
    {"author":"bigshinna"},
    {"author":"No_Coffee_4638"}
]
```

cURL:
```
    curl http://127.0.0.1:8000/authors/ups
    curl http://127.0.0.1:8000/authors/num_comments
```
