# GUIA DE INSTALAÇÃO QUARKUS-SVELTE
Referências no fim do documento.
## 0.1. PRÉ-REQUISITOS
* JDK (https://www.oracle.com/br/java/technologies/downloads/#jdk21-windows)
* Maven (https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-maven)
* Node.js (https://nodejs.org/en/download/current)

## 0.2. CASO ESTEJA CRIANDO UM PROJETO QUARKUS DO ZERO
### Instale e configure o seu projeto Quarkus utilizando este comando no terminal (powershell) do seu IDE:
    ./mvnw io.quarkus.platform:quarkus-maven-plugin:3.5.3:create -DprojectGroupId=my-meuGrupo -DprojectArtifactId=nome-do-meu-projeto


## 1. PREPARANDO O QUARKUS
### Instale o Quarkus Quinoa nas dependências do seu projeto para poder rodar Frameworks FrontEnd:
    ./mvnw quarkus:add-extension -Dextensions="io.quarkiverse.quinoa:quarkus-quinoa"

### Vá em application.properties e cole estas referências (Defina onde será o diretório do Svelte):
    quarkus.http.cors=true
    quarkus.http.cors.origins=http://127.0.0.1:8080,http://localhost:8080
    quarkus.http.cors.headers=accept, authorization, content-type, x-requested-with
    quarkus.http.cors.methods=GET, POST, OPTIONS, DELETE, PUT

    # ATENÇÃO, Aqui você colocará a pasta destino do seu FrontEnd:
    quarkus.quinoa.ui-dir=../meu-diretorio-svelte

    quarkus.quinoa.build-dir=build
    quarkus.quinoa.enable-spa-routing=true
    %dev.quarkus.quinoa.dev-server.index-page=/

## 2. PREPARANDO O SVELTE

### Instale o Svelte no diretório que desejar utilizando o seguinte comando (requer Node.js):
    npm create svelte@latest meu-diretorio-svelte

### Vá no caminho `"meu-diretorio-svelte/src/routes/"` e crie o aquivo:
    +layout.js

### Cole o seguinte código no `"+layout.js"` para desativar a renderização do servidor (Funcionará como SPA):
    export const ssr = false;


### Utilize o seguinte comando no diretório onde está instalado o Svelte:
    npm i -D @sveltejs/adapter-static

### Vá no diretório em que está instalado o Svelte, selecione e substitua o código em `"svelte.config.js"` por:

    import adapter from '@sveltejs/adapter-static';
    import { vitePreprocess } from '@sveltejs/kit/vite';

    /** @type {import('@sveltejs/kit').Config} */
    const config = {
        preprocess: vitePreprocess(),
        kit: {
            adapter: adapter({
                fallback: 'index.html'
            })
        }
    };

    export default config;

## 3. REFERÊNCIAS

Instalação do Quarkus Quinoa:
https://docs.quarkiverse.io/quarkus-quinoa/dev/index.html

Configuração do Svelte no Quarkus:
https://docs.quarkiverse.io/quarkus-quinoa/dev/web-frameworks.html#svelte-kit-config

Referência de configuação para diretório FrontEnd:
https://docs.quarkiverse.io/quarkus-quinoa/dev/config-reference.html#quarkus-quinoa_quarkus.quinoa.ui-dir

Renderização do servidor Svelte:
https://kit.svelte.dev/docs/page-options#ssr



