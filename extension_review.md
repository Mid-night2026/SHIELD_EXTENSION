# Guia privado para iniciante: como estudar, planejar e criar uma extensão

## 1. Objetivo geral

Este documento tem como finalidade orientar uma pessoa iniciante no processo de criação de uma extensão, com foco em organização, estudo e execução em etapas. A ideia é transformar o processo em passos simples e objetivos, para que o desenvolvedor saiba o que aprender, quais ferramentas usar e o que fazer em cada momento.

## 2. O que você deve estudar primeiro

Antes de começar a escrever código, é importante entender os conceitos básicos que envolvem extensões.

### Fundamentos essenciais

- HTML
  - estrutura básica da interface
  - elementos visuais e containers

- CSS
  - estilos da extensão
  - layouts e responsividade
  - temas e aparência visual

- JavaScript
  - lógica da extensão
  - manipulação de elementos
  - eventos e interações do usuário

### Conceitos específicos de extensões

- Manifest JSON
  - estrutura principal da extensão
  - permissões
  - ícones
  - scripts e ações

- Background scripts
  - execução em segundo plano
  - monitoramento e eventos persistentes

- Content scripts
  - injetados em páginas web
  - leitura e modificação de conteúdo da página

- Popup
  - interface visual da extensão
  - botões, filtros, configurações

- Storage e APIs do navegador
  - salvar preferências do usuário
  - armazenar dados locais

- Permissions
  - permissões exigidas pela extensão
  - impacto na privacidade e segurança

### Segurança e boas práticas

- nunca coletar dados sem necessidade
- evitar scripts maliciosos
- validar entradas
- limitar permissões ao mínimo necessário
- revisar o código antes de publicar

## 3. O que você deve aprender antes de criar a extensão

Você deve estudar os seguintes tópicos em ordem:

### Etapa 1: fundamentos web

Estude:

- HTML básico
- CSS básico
- JavaScript básico
- eventos do navegador

### Etapa 2: extensão de navegador

Estude:

- como a extensão é estruturada
- manifest.json
- popup.html
- popup.js
- background.js
- content.js
- browser APIs

### Etapa 3: arquitetura da extensão

Entenda:

- quando usar content script
- quando usar background script
- como passar mensagens entre partes da extensão
- como salvar configurações do usuário

### Etapa 4: segurança e privacidade

Estude:

- permissões mínimas
- impacto de coleta de dados
- uso responsável de APIs
- como evitar comportamento suspeito

## 4. Ferramentas que você pode usar

### Para desenvolvimento

- VS Code
  - editor principal
- Google Chrome ou Microsoft Edge
  - testes da extensão
- Git e GitHub
  - versionamento e organização do projeto

### Para testes e depuração

- Developer Tools do navegador
- console do navegador
- painel de extensões
- ferramentas de inspeção de rede

### Para documentação

- Markdown
- README.md
- arquivos de notas e documentação técnica

## 5. Como criar a extensão em etapas

### Etapa 1: definir a ideia

Antes de escrever qualquer código, responda:

- qual problema a extensão resolve?
- qual funcionalidade ela terá?
- qual página ou tarefa ela vai interagir?
- o usuário vai clicar em botão, automatizar algo ou analisar conteúdo?

Exemplo:

- bloquear anúncios
- extrair informações de uma página
- resumir texto
- validar links
- alterar comportamento da página

### Etapa 2: planejar a estrutura

Crie a base do projeto com uma estrutura simples, por exemplo:

```text
minha-extensao/
├── manifest.json
├── popup.html
├── popup.css
├── popup.js
├── background.js
├── content.js
├── icons/
│   ├── icon16.png
│   ├── icon48.png
│   └── icon128.png
└── README.md
```

### Etapa 3: criar o manifesto

O arquivo manifest.json define a extensão. Nele você informa:

- nome da extensão
- versão
- descrição
- ícones
- permissões
- ações
- scripts que serão usados

Exemplo de elementos básicos:

- manifest_version
- name
- version
- description
- action
- permissions

### Etapa 4: criar a interface do popup

O popup é a parte visual que o usuário vê ao clicar na extensão.

Você deve:

- criar HTML da interface
- estilizar com CSS
- adicionar botões e campos
- conectar os elementos com JavaScript

### Etapa 5: criar a lógica da extensão

Aqui entra a programação real:

- capturar eventos de clique
- alterar o conteúdo da página
- salvar preferências
- enviar mensagens entre popup e background
- consumir APIs ou detectar elementos da página

### Etapa 6: criar scripts de conteúdo

Se a extensão precisa interagir diretamente com uma página web, o content script é usado.

Você pode:

- ler elementos da página
- modificar texto
- inserir botões
- bloquear ou alterar conteúdo

### Etapa 7: criar script em segundo plano

O background script é usado para tarefas mais permanentes, como:

- ouvir eventos do navegador
- monitorar navegação
- executar ações sem depender da página ativa
- manter lógica central da extensão

### Etapa 8: testar no navegador

Depois de criar a extensão:

- carregue no navegador em modo de desenvolvedor
- teste a funcionalidade
- abra o console para verificar erros
- verifique se as permissões são necessárias
- ajuste a lógica conforme o comportamento real

### Etapa 9: revisar segurança e privacidade

Antes de considerar a extensão pronta, verifique:

- a extensão pede permissões desnecessárias?
- ela coleta dados que não serão usados?
- há comunicação com servidores externos?
- há código suspeito, confuso ou sem necessidade?
- a extensão funciona com mínimo de privilégios?

### Etapa 10: documentar e publicar

Para finalizar:

- escreva o README explicando o funcionamento
- registre como instalar e testar
- documente as funcionalidades
- se necessário, prepare a extensão para publicação

## 6. Checklist de começo

Use esta lista ao iniciar:

- [ ] entendi o que é uma extensão de navegador
- [ ] estudei HTML, CSS e JavaScript
- [ ] conheço o manifest.json
- [ ] defini a funcionalidade da extensão
- [ ] planejei a estrutura dos arquivos
- [ ] criei o popup
- [ ] implementei a lógica
- [ ] testei no navegador
- [ ] revisei permissões e segurança
- [ ] documentei o projeto

## 7. O que evitar no início

Evite:

- criar uma extensão muito complexa logo no começo
- pedir permissões sem necessidade
- misturar lógica de interface, conteúdo e backend sem organização
- pular a etapa de testes
- publicar sem revisar segurança

## 8. Dicas práticas para quem está começando

- comece por uma extensão simples
- use um projeto pequeno e bem definido
- teste peça por peça
- consulte documentação oficial do navegador
- mantenha o código organizado e bem nomeado
- documente tudo o que você aprender

## 9. Objetivo final

O objetivo principal de quem está começando é aprender a criar uma extensão funcional, organizada, segura e útil. A prática correta é dividir o processo em etapas claras, estudar os conceitos fundamentais e validar cada parte antes de avançar.

Com disciplina, cada etapa se torna mais simples e a extensão deixa de ser um projeto intimidante para virar um projeto concreto e executável.
