# PROVA-TEMPLATE-REPO

Template remoto per Backstage che:

1. genera repository Spring Boot Hello World (`fetch:template`)
2. risolve utente GitLab dal token (`gitlab:user:info`)
3. crea la repo GitLab (`publish:gitlab`)
4. crea/aggiorna la pipeline Jenkins (`jenkins:create-pipeline`)
5. avvia automaticamente una build Jenkins della pipeline appena creata/aggiornata
6. registra `catalog-info.yaml` nel Catalog (`catalog:register`)

## File principali

- `template.yaml`
- `skeleton/README.md`
- `skeleton/catalog-info.yaml`
- `skeleton/main.jenkinsfile`
- `skeleton/build.jenkinsfile`
- `skeleton/pom.xml`
- `skeleton/src/main/java/com/example/hello/HelloApplication.java`
- `skeleton/src/main/java/com/example/hello/HelloController.java`
- `skeleton/src/main/resources/application.properties`
- `skeleton/src/test/java/com/example/hello/HelloApplicationTests.java`

## Come usarlo in Backstage

1. Pubblica questa cartella in un repository GitLab/GitHub.
2. In Backstage vai su `Register existing component`.
3. Inserisci la URL del `template.yaml` del repo, ad esempio:
   - GitLab: `https://gitlab.com/<group>/<repo>/-/blob/main/template.yaml`
   - GitHub: `https://github.com/<org>/<repo>/blob/main/template.yaml`
4. Dopo la registrazione, in `Create` vedrai `Repository GitLab (Remote)`.

## Token e credenziali

Questo template non contiene token hardcoded.

Le action (`gitlab:user:info`, `publish:gitlab`, `jenkins:create-pipeline`) usano i token lato backend Backstage da:

- `integrations.gitlab[].token` in config
- tipicamente valorizzato con `${GITLAB_TOKEN}` da env del pod
- env del pod caricata da `backstage-secret` in Kubernetes

Per Jenkins:

- legge `jenkins.instances[].baseUrl`, `username`, `apiKey` da `app-config`
- `apiKey` puo' essere `${JENKINS_API_TOKEN}` da `backstage-secret`
- il nome job Jenkins e' automatico: `<nome-repository>-jenkinsJob`
- configurazione Jenkins nel template e' preimpostata (non visibile nel form utente):
  - `jenkinsGitCredentialsId: accesso-prova-gitlab`
  - `gitBranch: main`
  - `jenkinsfilePath: main.jenkinsfile`
  - `jenkinsInstance: default`

Il template imposta automaticamente in `catalog-info.yaml`:

- `jenkins.io/job-full-name`

cosi' la tab `CI/CD` di Backstage mostra i dati Jenkins del componente.

Quindi il comportamento e' lo stesso del template locale in immagine, ma aggiornabile da repo senza rebuild dell'immagine.

## Nota tecnica: template vs action

Per popolare la nuova repo con codice applicativo (es. Spring Boot Hello World) basta modificare il `skeleton` del template.

Le action custom backend non vanno modificate per questo caso, perche':

- `fetch:template` copia i file nello workspace scaffolder
- `publish:gitlab` fa commit/push sulla nuova repo
- `jenkins:create-pipeline` crea o aggiorna solo il job Jenkins
  - con `triggerBuild: true` avvia anche una build subito dopo il provisioning
