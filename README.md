# My Profile App (teste)

Aplicação React + TypeScript mínima criada com Vite para testes.

Como usar:

1. Instale dependências:

   npm install

2. Rode em modo desenvolvimento (será servido na porta 9999):

   npm run dev

Abra: http://localhost:9999

Deploy automático via Jenkins
--------------------------------

Uma opção é configurar um job Jenkins no seu servidor para checar a release mais recente do GitHub e fazer deploy automaticamente quando houver uma nova release. Fluxo sugerido:

- No Jenkins crie uma credencial do tipo "Secret text" com um token GitHub (somente leitura é suficiente) e anote o ID, por exemplo `github-token`.
- Configure um pipeline job que use o `Jenkinsfile` em `.jenkins/Jenkinsfile` neste repositório. Ajuste `GITHUB_REPO` para `owner/repo` ou configure como parâmetro do job.
- O pipeline faz:
   - consulta a API de releases do GitHub (usando o token)
   - baixa o primeiro asset da release (espera um tar.gz)
   - executa `scripts/deploy_release.sh <arquivo> /var/www/my-profile` para extrair e substituir a versão atual

Observações de segurança
- Use um token com permissões mínimas (apenas leitura pública) e crie-o como credencial no Jenkins.
- Mantenha o repositório privado se desejar; o pipeline usa o token para acessar as releases.
- O script `deploy_release.sh` usa `sudo` para mover arquivos para zonas como `/var/www`; adapte permissões conforme seu servidor.

