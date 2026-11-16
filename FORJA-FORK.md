# Fork da Forja — `forja`

Este é um fork de `tangency-labs/pcbcraft` mantido pela plataforma **Forja**
(`dionarley/forja`). O `pcbcraft` é a bancada de **eletrônica** da plataforma:
esquema, placa, DRC e a sessão de MCP em que a pessoa e o agente editam o mesmo
projeto, com os mesmos comandos e o mesmo histórico de desfazer.

A branch `forja` carrega as mudanças locais. **Não abra PR na origem**: o que
servir para todo mundo volta ao upstream por quem cuida dele, não por aqui.

## Como a Forja usa

Submodule em `apps/pcbcraft`. É **app, não biblioteca**: tem workspace próprio
(`edition 2024`, rust 1.95) e fica fora do workspace do `forja-pdm` e do build
do `tools/build.sh`. Compile dentro do submodule, sob demanda:

```sh
cd apps/pcbcraft
cargo build --locked -p pcbcraft -p pcbcraft-cli
```

O agente fala com ele por MCP: `pcbcraft-cli mcp` sobe uma sessão sem janela;
`pcbcraft-cli mcp --connect <porta> --token-file <arquivo>` se prende ao editor
aberto, para a pessoa acompanhar a edição acontecendo.

## O que não fazer

- Não troque as dependências `solidcraft-ui-*` do `Cargo.toml` para o fork
  `dionarley/solidcraft`: elas são pinadas por revisão de git e só entram aqui se
  a Forja precisar de uma correção que ainda não existe na origem.
- Não reformate nem renomeie código de terceiro. O patch tem que ser mínimo e
  explicado aqui.

## Atualizar a partir da origem

```sh
git remote add upstream https://github.com/tangency-labs/pcbcraft.git   # uma vez
git fetch upstream
git checkout forja
git rebase upstream/main
git push --force-with-lease origin forja
```
