# Template de Pipeline do GitHub Actions (reusable workflows + composite actions) :recycle: 

Esse repositório contém um reusable workflow para Node js e Python, que pode ser chamado a partir de outros workflows usando **workflow_call**.

Exemplo de uso para Node:

```
jobs:
  call-template:
    uses: ./.github/workflows/ci-template.yml
    with:
      language: node
      version: "20"
      run_tests_cmd: "cd src/node-app && npm test --silent"
    secrets: inherit
```
Exemplo de uso para Python:

```
jobs:
  call-template:
    uses: ./.github/workflows/ci-template.yml
    with:
      language: python
      version: "3.12"
      run_tests_cmd: "cd src/py-app && pytest -q"
    secrets: inherit
```

### Tabela de Inputs:
| Input          | Obrigatório | Default | Exemplo                          |
|----------------|-------------|---------|----------------------------------|
| Language       | Sim         | -       | Node ou python                   |
| Version        | Não         | lts     | 20, 3.12                         |
| Run_tests_cmd  | Não         | echo no-tests | cd src/node-app && npm test |
| Extra_token    | Não         | -       | ${{ secrets.EXTRA_TOKEN }}       |


### Troubleshooting

Repositório não encontra a composite action local
➜ cheque o caminho relativo (uses: ./.github/actions/setup-env).

Secrets não chegam no reusable
➜ use secrets: inherit ou mapeie explicitamente com o mesmo nome dos inputs.

Falha de permissão para comentar no PR
➜ adicione em permissions:
```
permissions:
  contents: read
  pull-requests: write
```
Cache não bate ➜ confira se o arquivo de lock (package-lock.json/requirements.txt) está no hashFiles e versionado.

### Evidências 

**Runs**:
- Node.js:
   - Push/PR: https://github.com/lu-rocha/gha-template-playground/actions/runs/17023147680
   - Segunda execução - alteração script package-lock(com cache restaurado): https://github.com/lu-rocha/gha-template-playground/actions/runs/17024779857

- Python:
    - Tag v1.0.0: https://github.com/lu-rocha/gha-template-playground/actions/runs/17023325071
    - Segunda execução - tag v2.0.0(com cache restaurado): https://github.com/lu-rocha/gha-template-playground/actions/runs/17024905380
 
### Demo:

:white_check_mark: Workflow de Node.js rodando:
<img width="1898" height="870" alt="python-3" src="https://github.com/user-attachments/assets/3bcbc5ed-abc2-43b3-8d19-49f41cf6bf7d" />

:white_check_mark: Workflow de python rodando:
<img width="1900" height="870" alt="node-3" src="https://github.com/user-attachments/assets/35933b46-9d37-49a3-ba97-3de7a3544806" />

:white_check_mark: Cache restored do workflow de Node.js:
<img width="1869" height="867" alt="cache-tag-python" src="https://github.com/user-attachments/assets/1263f75c-e314-47a9-ba2e-15218ffab26e" />

:white_check_mark: Cache restored do workflow de Python: 
<img width="1880" height="858" alt="cache-node" src="https://github.com/user-attachments/assets/e860ade6-9b11-409d-924c-5d551724392c" />
