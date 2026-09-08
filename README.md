# Help Vagões

Sistema que orienta os usuários a se locomoverem entre as linhas de metrô e trem do Brasil

O usuário coloca as estações de origem e destino, o sistema apresentará as melhores opções de deslocamento(o caminho que precisa pecorrer menos estações e o caminho que precisa realizar menos baldeações, se um mesmo caminho aparecer nos dois filtros é um forte indicativo que é o caminho ideal)

Inicialmente iremos possuir somente as linhas do estado de São Paulo e com o tempo ir expandindo para o restante do Brasil.



## Stacks

Banco de dados: PostgreSQL com hospedagem no Supabase
API: ASP.NET Core
Front: HTML, CSS e JS

### Schema 

inicialmente nosso schema será o seguinte:

```


CREATE TABLE linhas (
    id_linha BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    numero SMALLINT NOT NULL,
    nome VARCHAR(100) NOT NULL,
    cor VARCHAR(20) NOT NULL,
    ativo BOOLEAN NOT NULL DEFAULT TRUE,

    CONSTRAINT uq_linhas_numero UNIQUE (numero),
    CONSTRAINT uq_linhas_nome UNIQUE (nome)
);



CREATE TABLE estacoes (
    id_estacao BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    nome VARCHAR(150) NOT NULL,
    latitude NUMERIC(9,6),
    longitude NUMERIC(9,6),
    ativo BOOLEAN NOT NULL DEFAULT TRUE,

    CONSTRAINT uq_estacoes_nome UNIQUE (nome)
);




CREATE TABLE linha_estacao (
    id_linha_estacao BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,

    id_linha BIGINT NOT NULL,
    id_estacao BIGINT NOT NULL,

    ordem INTEGER NOT NULL,

    CONSTRAINT fk_linha_estacao_linha
        FOREIGN KEY (id_linha)
        REFERENCES linhas (id_linha)
        ON DELETE CASCADE,

    CONSTRAINT fk_linha_estacao_estacao
        FOREIGN KEY (id_estacao)
        REFERENCES estacoes (id_estacao)
        ON DELETE CASCADE,

    CONSTRAINT uq_linha_estacao
        UNIQUE (id_linha, id_estacao),

    CONSTRAINT uq_linha_ordem
        UNIQUE (id_linha, ordem),

    CONSTRAINT ck_linha_estacao_ordem
        CHECK (ordem > 0)
);

```


Esse arquivo sofrerá mudanças no futuro, no momento é apenas organização de idéias.

