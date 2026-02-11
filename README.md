[JAVA__BADGE]:https://img.shields.io/badge/java-%23ED8B00.svg?style=for-the-badge&logo=openjdk&logoColor=white
[Amazon S3__BADGE]:https://img.shields.io/badge/Amazon%20S3-FF9900?style=for-the-badge&logo=amazons3&logoColor=white
[AWS__BADGE]:https://img.shields.io/badge/AWS-%23FF9900.svg?style=for-the-badge&logo=amazon-aws&logoColor=white

# Encurtador de URL 🔗

![Java][JAVA__BADGE]
![AWS][AWS__BADGE]
![Amazon S3][Amazon S3__BADGE]

Este sistema permite encurtar URLs longas, gerando uma versão reduzida.  
Ao acessar o link encurtado, o usuário vê uma página intermediária com contagem regressiva antes de ser redirecionado para a URL original.

## 💻 Tecnologias

- **Java (AWS Lambda)** 
- **AWS S3 (armazenamento de redirecionamentos)**
- **JavaScript (frontend)**
- **HTML/CSS**

## 🚀 Como Começar

### Lambda

Crie uma função **AWS Lambda**, baixe o arquivo `.jar` presente na pasta `release` e faça o upload para a função.

### API Gateway

Configure um **API Gateway** na **AWS** para sua função **Lambda**, do tipo `POST` com caminho (endpoint) `/url` 

### S3

Crie um bucket **Amazon S3** e configure-o como um **Static Website Hosting** para que os arquivos e links possam ser acessados.  
Depois, envie os **três arquivos** presentes na pasta `paginas` para o **S3**.

## ⚙️ Configuração

### 📝 configAWS_exemplo.js

O arquivo de configuração `configAWS_exemplo.js` (na pasta `paginas`) deve ser preenchido com as informações da sua **URL pública do bucket S3** e da **URL pública do API Gateway**.  

Exemplo de conteúdo para o arquivo:

```javascript
// Configuração para o redireciona.html
const S3_WEBSITE_URL = "https://SEU-BUCKET-S3-WEBSITE.amazonaws.com/";

// Configuração para o encurtador_url.html
const API_ENCURTADOR_URL = "https://SUA-LAMBDA-ENDPOINT.amazonaws.com/prod";
```

### 📝 Handler.java

O arquivo `Handler.java` se encontra em `src/main/java/com/encurtador/`. Configure as constantes abaixo para apontar ao **bucket S3** e ao site estático

```java
private static final String BUCKET_NAME = "nome-do-seu-bucket"; 
//Nome do Bucket S3 criado

private static final Region REGION = Region.US_EAST_1; 
// Ajuste para a região correta da AWS, por exemplo US_EAST_1, EU_WEST_1, etc.

private static final String S3_WEBSITE_ORIGIN = "https://nome-do-seu-bucket.s3-website-us-east-1.amazonaws.com/"; 
// URL pública do website configurado no S3 (region e nome do bucket devem coincidir)

private static final String REDIRECT_BASE_URL = S3_WEBSITE_ORIGIN + "redireciona.html"; 
// Página intermediária que fará o redirecionamento
```

## 🔎 Como Usar

1. **Acesse** a URL pública do `encurtador_url.html` dentro do **bucket S3**

    Exemplo: `https://<seu-bucket>.s3-website-us-east-1.amazonaws.com/encurtador_url.html`

2. **Digite** a URL que deseja encurtar no campo indicado.
3. **Aperte** o botão para enviar o formulário
4. **Copie** ou clique no link encurtado que será gerado automaticamente.

## 🔧 Como Funciona

O encurtador de URL funciona da seguinte forma: ao enviar uma URL pelo formulário HTML, o sistema faz uma chamada para o API Gateway, que aciona a função AWS Lambda, onde está o código em Java.

Dentro dessa função, existe uma lógica para gerar uma URL curta única, garantindo que não existam duas URLs curtas iguais no S3. Além disso, as URLs curtas têm um prazo de validade de 24 horas, definido nativamente pela configuração do próprio S3.

Após todas as validações, a Lambda retorna uma URL curta que leva o usuário a uma página intermediária, onde ocorre uma contagem regressiva, antes do redirecionamento final para a URL original.

## 🤝 Colaboradores

| [João Victor Farah](https://github.com/joaovfarah) |
| :--------------------------------------------: |
