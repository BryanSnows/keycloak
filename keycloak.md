# Configuração do Keycloak com Docker

## 1. Subir o ambiente com Docker

Para iniciar o Keycloak e o banco de dados PostgreSQL, crie um arquivo `docker-compose.yml` com o seguinte conteúdo:

```yaml
version: "3"

services:
  keycloak:
    image: quay.io/keycloak/keycloak:22.0.1
    container_name: keycloak
    environment:
      - KEYCLOAK_ADMIN=admin
      - KEYCLOAK_ADMIN_PASSWORD=admin
      - KC_DB=postgres
      - KC_DB_URL=jdbc:postgresql://db:5432/keycloak
      - KC_DB_USERNAME=keycloak
      - KC_DB_PASSWORD=keycloak
    ports:
      - 8080:8080
    command: start-dev
    depends_on:
      - db

  db:
    image: postgres:15
    container_name: keycloak-db
    environment:
      POSTGRES_DB: keycloak
      POSTGRES_USER: keycloak
      POSTGRES_PASSWORD: keycloak
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```

### Iniciar o ambiente

Para subir os containers, navegue até o diretório onde está o arquivo `docker-compose.yml` e execute o seguinte comando:

```bash
docker-compose up -d
```

## 2. Acessar o Keycloak

Após subir os containers, você poderá acessar o Keycloak no navegador. Para isso, abra seu navegador e vá até o endereço:

```bash
http://localhost:8080
```

Use as credenciais abaixo para fazer login:

- **Usuário**: `admin`
- **Senha**: `admin`

## 3. Criar um Realm

Após fazer login:

1. Clique no botão **Admin Console**.
2. No menu lateral esquerdo, clique em **Create Realm**.
3. Defina o nome do Realm como `RD2_I40` e clique em **Create**.

## 4. Importar os Clients

1. No painel de administração, vá até **Clients**.
2. Clique em **Create** para adicionar um novo client.
3. Defina o nome do client como `wile-c` e preencha os campos conforme necessário.
4. Repita o processo para os clients `wile-c-backend` e `wile-c-frontend`.

## 5. Criar um Usuário

1. No painel de administração, vá até **Users**.
2. Clique em **Add User**.
3. Preencha os dados do usuário e clique em **Save**.
4. Depois de criado, vá até a aba **Credentials** e defina uma senha para o usuário.

## 6. Atribuir Roles ao Usuário

1. No perfil do usuário, vá até a aba **Role Mappings**.
2. No campo **Client Roles**, selecione o client `wile-c`.
3. Atribua as roles necessárias ao usuário, clicando em **Add selected**.

## 7. Testar a Autenticação

Agora você pode testar a autenticação dos usuários criados utilizando os clients configurados e o Realm `RD2_I40`.

## 8. Explicação das Roles

Cada role representa um módulo específico do projeto. Abaixo estão os detalhes sobre o que cada role significa:

- **Beyonder**: Representa o módulo de **Automação de Testes**.
- **Elektro**: Representa o módulo de **Eficiência Energética**.
- **Esd**: Representa o módulo de **ESD (Descargas Eletrostáticas)**.
- **Organization**: Representa o módulo de **Organização**.
- **PrevMaintenance**: Representa o módulo de **Manutenção**.
- **Shell**: Representa o módulo de **Shell**.
- **Staff**: Representa o módulo de **Paletizadora**.
- **StyleGuide**: Representa o módulo de **Guideline**.
- **t800**: Representa o módulo de **Pessoal**.

Cada um desses módulos tem roles que permitem o acesso e controle específicos de cada parte do sistema. Ao atribuir essas roles aos usuários, você está permitindo que eles acessem as funcionalidades relacionadas aos respectivos módulos.
