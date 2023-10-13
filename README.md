# Casa dos Anjos - Backend
Aqui está o backend do projeto Casa dos Anjos, desenvolvido para ajudar na arrecadação de dinheiro para a ONG Casa dos Anjos, que cuida de animais abandonados e maltratados.

## Requisitos
- [Node.js](https://nodejs.org/en/)
- [PNPM](https://pnpm.js.org/) (Opcional)
- [Conta no OpenAI](https://beta.openai.com/)

## Instalação
1. Clone o repositório
2. Instale as dependências com `pnpm install` ou `npm install`
3. Crie um arquivo `.env` na raiz do projeto e preencha com as informações do arquivo `.env.example`
4. Rode o projeto com `pnpm dev` ou `npm run dev`

## Rotas
### POST /curiosities
> Acesse no [hoppscotch](https://hopp.sh/r/JChVSHyCI3Q4)

| Parâmetro | Tipo | Descrição |
| --- | --- | --- |
| `theme` | `string` | Tema da curiosidade |

Gera 5 curiosidades aleatórias sobre o assunto passado no corpo da requisição.

<details>
<summary>Exemplo de requisição</summary>

```json
{
  "theme": "dogs"
}
```
</details>
<details>
<summary>Exemplo de resposta</summary>

```json
{
  "status": "success",
  "curiosities": [
    {
      "title": "Comunicação olfativa",
      "curiosity": "Os cães possuem um olfato extremamente desenvolvido e são capazes de detectar odores em uma concentração de até 100.000 vezes menor do que os seres humanos."
    },
    {
      "title": "Variedade de raças",
      "curiosity": "Existem mais de 340 raças de cães reconhecidas oficialmente ao redor do mundo, desde pequenos como o Chihuahua até grandes como o São Bernardo."
    },
    {
      "title": "Melhor amigo do homem",
      "curiosity": "Os cães são conhecidos como o melhor amigo do homem devido à sua lealdade e capacidade de estabelecer laços afetivos com seus donos."
    },
    {
      "title": "Sentido térmico",
      "curiosity": "Os cães possuem sensores térmicos localizados nas pontas do focinho, o que lhes permite detectar variações de temperatura mesmo em ambientes escuros."
    },
    {
      "title": "Habilidades sociais",
      "curiosity": "Os cães têm a capacidade de interpretar as expressões faciais dos seres humanos e são capazes de reconhecer emoções como felicidade, tristeza e medo."
    }
  ]
}
```
</details>

### POST /chat
> Acesse no [hoppscotch](https://hopp.sh/r/JxfvUKtDfovW)

| Parâmetro | Tipo | Descrição |
| --- | --- | --- |
| `message` | `string` | Mensagem enviada pelo usuário |
| `parentMessageId` | `string` | ID das mensagens anteriores |
| `name` | `string` | Nome do usuário |
| `points` | `number` | Pontos do usuário |
| `checkins` | `number` | Check-ins do usuário |

Envia uma mensagem para o chatbot que está usando a API do chatGPT-3.5-Turbo para gerar respostas.

<details>
<summary>Exemplo de requisição</summary>

```json
{
  "message": "Olá, tudo bem?",
  "parentMessageId": "1",
  "name": "João",
  "points": 0,
  "checkins": 0
}
```
</details>
<details>
<summary>Exemplo de resposta</summary>

```json
{
  "status": "success",
  "message": "Olá! Tudo bem sim, e você? Como posso ajudar?",
  "parentMessageId": "chatcmpl-7kJPFCCsZ8Xx3A3kvpWkFWNhpaYxN"
}
```
</details>


## Produção
### VPS (Ubuntu)
> Se você é novo no Ubunto, veja como configurar um usuário com permissões de superusuário [aqui](https://gist.github.com/misterioso013/81a9f1ab97ebccb213c34b3d1260b1ac).

1. Instale a versão mais recente do Node.js [aqui](https://github.com/nvm-sh/nvm)
2. Instale o [pm2](https://pm2.keymetrics.io/) globalmente com `npm install -g pm2`
3. Clone o repositório
> Por se tratar de um repositório privado, é necessário configurar o SSH para clonar o repositório. Veja como fazer isso [aqui](https://gist.github.com/misterioso013/77fdf70ae956c5b08e19700d264b95ae).

4. Instale as dependências com `npm install`
5. Crie um arquivo `.env` na raiz do projeto e preencha com as informações do arquivo `.env.example`
```bash
cp .env.example .env && nano .env
```

6. Rode o projeto com `npm run prod`

#### NGINX e SSL
> Se você não tem um domínio, pode pular essa parte e acessar o site pelo IP da sua VPS.

1. Instale o NGINX com `sudo apt install nginx`
2. Instale o [Certbot](https://certbot.eff.org/). Siga esses [passos](https://certbot.eff.org/instructions?ws=nginx&os=ubuntufocal) para configurar o NGINX e o SSL no Ubuntu 20.04.
3. Crie um arquivo de configuração para o site com `sudo nano /etc/nginx/sites-available/default` e preencha com o seguinte conteúdo:
```nginx
server {
  listen 80;
  listen [::]:80;

  server_name <seu-dominio>;

  location / {
    proxy_pass http://localhost:3333;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection 'upgrade';
    proxy_set_header Host $host;
    proxy_cache_bypass $http_upgrade;        
  }
}
```
> O certbot já vai ter criado um arquivo de configuração para o site, então você deve procurar por essa parte no arquivo e substituir pelo código acima.

4. Reinicie o NGINX com `sudo systemctl restart nginx`
5. Rode o projeto com `npm run prod`

### Heroku
(Em breve)
