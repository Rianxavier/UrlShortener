# URL Shortener - AWS Serverless Project

Este projeto consiste em uma implementação de um encurtador de URLs utilizando tecnologias serverless da AWS. A solução está dividida em dois módulos principais:

1. **CreateUrlLambda**: Responsável por criar URLs encurtadas.
2. **RedirectUrlShortener**: Responsável por redirecionar os usuários para as URLs originais com base nos códigos curtos gerados.

O projeto foi desenvolvido como parte de um curso em parceria com a Rocketseat e Fernanda Kipper, utilizando uma arquitetura moderna e escalável.

---

## 🎯 Objetivo

- Criar um sistema simples e eficiente de encurtamento de URLs.
- Garantir que as URLs tenham um tempo de expiração configurável.
- Proporcionar uma experiência escalável e econômica utilizando AWS Lambda e S3.

---

## 🚀 Tecnologias Utilizadas

- **Java**: Linguagem principal para o desenvolvimento das funções Lambda.
- **AWS Lambda**: Para execução das funções serverless.
- **Amazon S3**: Para armazenamento das URLs encurtadas e seus metadados.
- **API Gateway**: Para gerenciar as requisições HTTP.
- **Maven**: Para gerenciar as dependências e build.
- **Lombok**: Para reduz código repetitivo gerando métodos automaticamente via anotações.
- **UUID**: Para geração de identificadores únicos para as URLs.

---

## 🛠️ Estrutura dos Módulos

### CreateUrlLambda

Este módulo lida com a criação de URLs encurtadas. Ele recebe a URL original e o tempo de expiração como entrada, gera um código curto único e salva essas informações no Amazon S3.

- **Input**: JSON contendo:
  - `originalUrl`: URL a ser encurtada.
  - `expirationTime`: Tempo de expiração em segundos.
- **Output**: JSON contendo:
  - `code`: Código curto gerado para a URL.

#### Fluxo:

1. O usuário envia uma URL e o tempo de expiração.
2. A aplicação gera um código único (UUID encurtado).
3. Salva os dados no S3 em formato JSON.

# RedirectUrlShortener - AWS Serverless Project

Este módulo gerencia o redirecionamento no sistema de encurtamento de URLs. Ele utiliza tecnologias serverless da AWS para verificar a validade de URLs encurtadas e redirecionar os usuários para as URLs originais ou retornar um erro caso a URL tenha expirado.

---

## 🎯 Como Funciona

1. O código curto é extraído da URL enviada na requisição.
2. Os dados correspondentes são recuperados de um bucket no Amazon S3.
3. A aplicação verifica a validade da URL:
   - **Redireciona (302)**: Caso a URL ainda seja válida.
   - **Erro (410)**: Caso a URL tenha expirado.

---

## 🛠️ Configuração e Deploy

### Pré-requisitos

- **Conta AWS** configurada.
- **Java 17**
- **Maven** instalado.
- **AWS CLI** configurada.

---

### Etapas

1. **Criar Bucket S3**:
   - Crie um bucket S3 na AWS.
   - Atualize o nome do bucket no código do módulo.
     
2. **Criar as Funções Lambda:**:
   - Acesse o console da AWS Lambda e crie duas funções.
     - **CreateUrlLambda** Para o módulo de criação de URLs.
     - **RedirectUrlShortener** Para o módulo de redirecionamento.

3. **Deploy da Função Lambda**:
   - Compile o código utilizando o comando:
     ```bash
     mvn package
     ```
   - Faça o upload do arquivo JAR gerado para a AWS Lambda.
   - Configure o **API Gateway** para gerenciar as requisições:
     - **POST** para o módulo **CreateUrlLambda**.
     - **GET** para o módulo **RedirectUrlShortener**.

4. **Testar**:
   - Utilize ferramentas como Postman para enviar requisições e validar o funcionamento.

---

## 🧪 Testes

### Testando **CreateUrlLambda**
1. Envie uma requisição **POST** para o endpoint configurado com os seguintes dados:
   ```json
   {
     "originalUrl": "https://www.example.com",
     "expirationTime": "3600"
   }
   
2. Acesse o endpoint com o código curto gerado, no formato:
   - https://{API_GATEWAY_URL}/{shortCode}
     
3. Verifique o comportamento:
   - Redirecionamento (302) para a URL original.
   - Erro (410) caso a URL tenha expirado.


