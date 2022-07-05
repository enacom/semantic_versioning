# Ambientes e branchs

## Gateway

No Gateway os ambientes estão relacionados às *branchs* da seguinte maneira:

- Ambiente de devenvolvimento: *branch develop*
- Ambiente de homologação: *branch staging*
- Ambiente de produção: *branch master*

> *branch master* deveria se chamar *main*. Para uma mudança futura:
>
> [Branch Management - Changing the master branch name](https://git-scm.com/book/en/v2/Git-Branching-Branch-Management#_changing_the_master_branch_name)

## Como atualizar os ambientes

### Ambiente de desenvolvimento
1. Acesse o repositório do gateway
2. Mude para a *branch develop*: `git checkout develop`
3. Atualize a *branch develop*: `git pull`
4. Mude as versões das dependências no arquivo `requirements.txt`
5. Aceite as modificações: `git add requirements.txt`
6. Confirme as modificações: `git commit`
7. Escreva uma mensagem explicando as modificações
8. Publique as modificações: `git push`

A partir da publicação o *pipeline* do ambiente de desenvolvimento será iniciado. Quando o *pipeline* for completado estará disponível um ambiente atualizado.

### Ambiente de homologação
1. Acesse o repositório do gateway
2. Mude para a *branch staging*: `git checkout staging`
3. Atualize a *branch staging*: `git pull`
4. Verifique qual a última versão gerada: `git tag`
> Atenção à ordem das versões, são ordenadas como texto.
>
> Exemplo, a última versão é a **3.11.5** e não a *3.9.0*:
>
```sh
3.1.0
3.10.0
3.11.0
3.11.1
3.11.2
3.11.3
3.11.4
3.11.5
3.2.0
3.3.0
3.4.0
3.5.0
3.6.0
3.7.0
3.8.0
3.9.0
```
5. Verifique a última *release* que foi incorporada à *staging*: `git log`
> Merge branch 'release/3.11.6'
6. Finalize essa *release* se ela ainda estiver aberta para que os códigos que estão em homologação possam ir para a produção: `git flow finish release 3.11.6`
> Uma nova etiqueta (com o número da versão) será gerada e é muito importante publicá-la:
>
```sh
git push origin --tags
```
`3.11.5 -> 3.11.5`
7. Inicie uma nova *release* para que os códigos que estão em desenvolvimento possam ir para homologação: `git flow start release 3.12.0`, considerando uma nova funcionalidade mantendo compatibilidade.
8. Incorpore a nova *release* à *staging*: `git merge release/3.12.0`
9. Publique as *branchs* atualizadas:
```sh
git push --all
```
```sh
develop -> develop
staging -> staging
master -> master
```
A partir da publicação o *pipeline* dos ambientes de desenvolvimento, homologação e produção serão iniciados. Quando os *pipelines* forem completados estarão disponíveis ambientes atualizados.

#### ATENÇÃO AO VERSIONAMENTO SEMÂNTICO!

Referência: [Semantic Versioning 2.0.0](https://semver.org/)

Dado um número de versão **MAJOR**.*MINOR*.PATCH, incremente a:

    versão Maior(MAJOR): quando fizer mudanças incompatíveis,
    versão Menor(MINOR): quando adicionar funcionalidades mantendo compatibilidade,
    versão de Correção(PATCH): quando corrigir falhas mantendo compatibilidade.

Sob este esquema, os números de versão e a forma como eles mudam transmitem o significado do código subjacente e o que foi modificado de uma versão para a próxima.

---


### Ambiente de produção
1. Acesse o repositório do gateway
2. Mude para a *branch master*: `git checkout master`
3. Atualize a *branch master*: `git pull`
4. Finalize a *release* aberta para que os códigos que estão em homologação possam ir para a produção: `git flow finish release 3.12.0`
> Uma nova etiqueta (com o número da versão) será gerada e é muito importante publicá-la:
>
> ```sh
> git push origin --tags
> ```
> `3.12.0 -> 3.12.0`

5. Publique as *branchs* atualizadas:
```sh
git push --all
```
`master -> master`

A partir da publicação o *pipeline* do ambiente de produção será iniciado. Quando o *pipeline* for completado estará disponível um ambiente atualizado.

## Como gerar novas versões

Referência: [git-flow cheatsheet](https://danielkummer.github.io/git-flow-cheatsheet/)

---

### Simulação customizada, Otimização simplificada ou Estruturas de dados comuns
1. Acesse o repositório da simulação customizada, da otimização simplificada ou das estruturas de dados comuns
2. Mude para a *branch develop*: `git checkout develop`
3. Atualize a *branch develop*: `git pull`
4. Verifique qual a última versão gerada: `git tag`
> Atenção à ordem das versões, são ordenadas como texto.
5. Inicie uma nova *release*: `git flow start release 4.5.0`
6. Revise os testes e verifique se ainda são necessárias algumas alterações de *último minuto*
7. Finalize essa *release* se ela ainda estiver aberta para que os códigos que estão em homologação possam ir para a produção: `git flow finish release 4.5.0`
> Uma nova etiqueta (com o número da versão) será gerada e é muito importante publicá-la:
> ```sh
> git push origin --tags
> ```
> `4.5.0 -> 4.5.0`
8. Ao finalizar um *release* todas as modificações serão enviadas para a *branch main* e para a *branch develop*
9. Publique as *branchs* atualizadas:
```sh
git push --all
```
```sh
develop -> develop
master -> master
```

#### ATENÇÃO AO VERSIONAMENTO SEMÂNTICO!

Referência: [Semantic Versioning 2.0.0](https://semver.org/)

Dado um número de versão **MAJOR**.*MINOR*.PATCH, incremente a:

    versão Maior(MAJOR): quando fizer mudanças incompatíveis,
    versão Menor(MINOR): quando adicionar funcionalidades mantendo compatibilidade,
    versão de Correção(PATCH): quando corrigir falhas mantendo compatibilidade.

Sob este esquema, os números de versão e a forma como eles mudam transmitem o significado do código subjacente e o que foi modificado de uma versão para a próxima.

---
