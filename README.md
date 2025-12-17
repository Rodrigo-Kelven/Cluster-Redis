
# Cluster Redis
[![Databases](https://skillicons.dev/icons?i=redis)](https://skillicons.dev)

### Descrição do Projeto: Redis Cluster com RedisInsight para Gerenciamento

Este projeto configura um cluster Redis de 6 nós utilizando o Docker Compose. O cluster é formado por 3 nós mestres e 3 réplicas, e o RedisInsight é utilizado para monitoramento e gerenciamento visual do cluster via interface web.

## Como Rodar o Projeto

- #### 1. Clonar o repositório ou baixar os arquivos do projeto.
- #### 2. Certifique-se de ter o Docker e o Docker Compose instalados.

#### O arquivo `docker-compose.yml` já vem configurado para criar 6 containers Redis com as configurações necessárias para um cluster. Para rodá-lo, basta executar:


    docker compose -f docker-compose-cluster-redis.yml up 


# Cria o cluster
### Após os containers estarem em execução, crie o cluster manualmente executando:

    docker exec -it redis-node-1 redis-cli --cluster create \
    redis-node-1:6379 redis-node-2:6379 redis-node-3:6379 \
    redis-node-4:6379 redis-node-5:6379 redis-node-6:6379 \
    --cluster-replicas 1

#### Quando solicitado, digite `yes` para confirmar a criação do cluster.


# Limpar dados antigos (se necessário)

### Caso você:
- Derrube o cluster
- Tente subi-lo novamente
- Encontre erros de configuração
- Precise recriar o cluster do zero
### Remova os dados persistentes com o comando abaixo:

    rm -rf ./data/redis-node-*


## Como Acessar e Configurar a Interface RedisInsight
#### Acesse o RedisInsight pelo navegador
#### O RedisInsight está configurado para rodar na porta 5540. Abra seu navegador e acesse:
    http://localhost:5540


## Adicionar o Cluster Redis no RedisInsight
### Após acessar a interface:

- #### 1 Clique em Add Redis Database.
- #### 2 Selecione Redis Cluster e preencha as informações:

    - #### Host: redis-node-1
    - #### Port: 6379
    - #### Authentication: Deixe como None (a menos que tenha configurado senha).
    - #### Auto-discover topology: Habilite.

## Visualizar o Cluster
#### O RedisInsight irá detectar automaticamente os nós e mostrar a topologia do cluster, com todos os nós mestres e réplicas, além da distribuição dos slots.


## Como Inserir e Ler Dados no Redis Cluster
### Conectar ao Cluster via redis-cli
#### Abra um terminal e conecte-se ao nó redis-node-1 para interagir com o Redis Cluster:
    docker exec -it redis-node-1 redis-cli -c


## Verificar o status do cluster

#### Após se conectar ao Redis, você pode verificar o estado do cluster com o seguinte comando:

    CLUSTER INFO

## Verificar os nós do cluster

#### Para visualizar todos os nós no cluster:

    CLUSTER NODES

## Inserir dados

#### Insira uma chave e valor no Redis Cluster:

    SET user:1 "kd6"

## Ler dados

#### Para recuperar o valor da chave inserida:

    GET user:1


#### O resultado esperado será:

    "kd6"
