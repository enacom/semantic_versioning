# ENACOM - Versionamento Semântico na prática

Referência: [Semantic Versioning 2.0.0](https://semver.org/)

---

## 1. Os números de versão
Dado um número de versão **MAJOR**.*MINOR*.PATCH, incremente a:

    versão Maior(MAJOR): quando fizer mudanças incompatíveis na API,
    versão Menor(MINOR): quando adicionar funcionalidades mantendo compatibilidade,
    versão de Correção(PATCH): quando corrigir falhas mantendo compatibilidade.

Sob este esquema, os números de versão e a forma como eles mudam transmitem o significado do código subjacente e o que foi modificado de uma versão para a próxima.

## 2. Clonar repositórios de exemplo

### Usar chave SSH
```sh
git clone git@github.com:enacom/simplified_example.git
git clone git@github.com:enacom/customized_example.git
git clone git@github.com:enacom/gateway_example.git
```

### Usar URL
```sh
git clone https://github.com/enacom/simplified_example.git
git clone https://github.com/enacom/customized_example.git
git clone https://github.com/enacom/gateway_example.git
```

## 3. Inicializar GitFlow

Referência: https://danielkummer.github.io/git-flow-cheatsheet/

---


Acessar um diretório dos repositórios de exemplo e iniciar o **git flow** com *branchs* com nomenclatura padrão (usar opção *--defaults*)
```sh
cd gateway_example/
git flow init --defaults
```

### GitFlow já inicializado
```sh
git flow init --defaults

Already initialized for gitflow.
To force reinitialization, use: git flow init -f
```

Não é necessário reinicializar o GitFlow, a menos que queria redefinir os nomes das *branchs*.

### GitFlow sem inicializar
```sh
git flow init --defaults

Using default branch names.

Which branch should be used for bringing forth production releases?
   - main
Branch name for production releases: [main]
Branch name for "next release" development: [develop]

How to name your supporting branch prefixes?
Feature branches? [feature/]
Bugfix branches? [bugfix/]
Release branches? [release/]
Hotfix branches? [hotfix/]
Support branches? [support/]
Version tag prefix? []
Hooks and filters directory? [.../.git/hooks]
```

Em *hooks* e filtros `...` corresponde ao caminho absoluto do repositório.

## 4. Desenvolvimento de uma nova funcionalidade
Criar nova *branch* em `feature` nova funcionalidade, de documentação, por exemplo.

```sh
git flow feature start documentation
```

### Ambiente virtual e mkdocs
Vídeo de referência: [Documentado projetos com MkDocs - Live de Python #189](https://www.youtube.com/watch?v=GW6nAJ1NHUQ)

Ativar o ambiente virtual e instalar as dependências
```sh
poetry shell
poetry install
```

Criar novo projeto de documentação, a partir do diretório corrente
```sh
mkdocs new .
```
Será criado o diretório `docs` e o arquivo de configuração `mkdocs.yml`

Caso já exista o arquivo de configuração, será informado que o projeto já exite
```sh
mkdocs new .
INFO     -  Project already exists.
```

### Desenvolvimento e controle de versão

Desenvolver a funcionalidade, visualizar e adicionar as mudanças
```sh
git add docs mkdocs.yml
```

Confirmar as mudanças para uma nova versão
```sh
git commit
```
Um editor de texto irá abrir para colocar uma mensagem clara sobre as mudanças

Publicar as mudanças
```sh
git flow feature publish documentation
```

## 5. Criar uma nova versão

Gerar uma nova *release* de acordo com a versionamento semântico **MAJOR**.*minor*.patch.
```sh
git flow release start v0.1.0
```

Publicar a nova *release*
```sh
git flow release publish v0.1.0
```

Homologar a nova funcionalidade e finalizar a versão
```sh
git flow release finish v0.1.0
```
