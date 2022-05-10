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

### Ambiente virtual e MkDocs
Vídeos e documentações de referência:

- Poetry:
    - [Gerenciando pacotes e ambientes com Poetry - Live de Python #179](https://www.youtube.com/watch?v=ZOSWdktsKf0)
    - [Documentação do Poetry](https://python-poetry.org/docs/)
- MkDocs e tema Material
    - [Documentado projetos com MkDocs - Live de Python #189](https://www.youtube.com/watch?v=GW6nAJ1NHUQ)
    - [Criação do site com o tema Material](https://squidfunk.github.io/mkdocs-material/creating-your-site/)
    - [Configuração do MkDocs](https://www.mkdocs.org/user-guide/configuration/)

---

Inicializar o ambiente virtual com poetry
```sh
poetry init --no-interaction
```
Será criado o arquivo de configuração `pyproject.toml`

> Edite o arquivo **pyproject.toml** e atualize os metadados sempre que mudar a versão.

Ativar o ambiente virtual
```sh
poetry shell
```

Adicionar as dependências
```sh
poetry add MkDocs MkDocs-material
```

Instalar as dependências
```sh
poetry install
```

Criar novo projeto de documentação, a partir do diretório corrente
```sh
MkDocs new .
```
Será criado o diretório `docs` e o arquivo de configuração `mkdocs.yml`.
> Editar o **mkdocs.yml** e incluir ou atualizar as configurações das páginas de documentação.

Caso já exista o arquivo de configuração, será informado que o projeto já exite
```sh
MkDocs new .
INFO     -  Project already exists.
```

Editar o arquivo `docs/index.md` com os dados principais da documentação. Criar outros arquivos no diretório `docs` para organizar melhor os assuntos.

### Desenvolvimento e controle de versão

Desenvolver a funcionalidade, visualizar e adicionar as mudanças
```sh
git add docs mkdocs.yml
```

Confirmar as mudanças para uma nova versão
```sh
git commit
```
Um editor de texto irá abrir para colocar uma mensagem clara sobre as mudanças.

> Acesse [alterar o editor padrão do git](https://git-scm.com/book/en/v2/Customizing-Git-Git-Configuration/#_core_editor), caso queira mudar o editor para incluir as mensagem de *commit*.

Publicar as mudanças
```sh
git flow feature publish documentation
```

Testar e finalizar a feature
```sh
git flow feature finish documentation

Merge branch 'feature/documentation' into develop
```

Resolver possíveis conflitos com a *branch* develop

Publicar as mudanças na develop
```sh
git push
```

Se a *branch* develop não existir remotamente será necessário definir a referência no repositório remoto
```sh
git push --set-upstream origin develop
```

Publicar a develop
```sh
git push
```

## 5. Criar uma nova versão

Gerar uma nova *release* de acordo com a versionamento semântico **MAJOR**.*minor*.patch. Como estamos adicionando uma nova funcionalidade mantendo compatibilidade, alteramos o *minor*.
```sh
git flow release start v0.1.0
```

Publicar a nova *release*
```sh
git flow release publish v0.1.0
```

> Caso sejam necessárias correções, de acordo com o versionamento semântico temos que alterar o `patch` da versão. Exemplo: `v0.1.1`.

Homologar a nova funcionalidade e finalizar a versão
```sh
git flow release finish v0.1.0
```

> Escrever uma mensagem simples e explicativa das mudanças da release!

Publicar as *tags*
```sh
git push origin --tags
```
