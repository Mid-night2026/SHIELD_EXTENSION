# Guia de iniciação para criar extensões com segurança

Este guia apresenta um caminho curto para quem está aprendendo a criar extensões de navegador. Como o SHIELD_EXTENSION também revisa extensões em busca de comportamento malicioso e coleta indevida, cada etapa inclui uma preocupação de segurança correspondente.

## 1. Entenda a arquitetura

Uma extensão normalmente reúne:

- **Manifesto:** arquivo `manifest.json` que declara identidade, versão, recursos, scripts e permissões;
- **Popup ou página de opções:** interface exibida pelo usuário;
- **Content script:** código executado em páginas autorizadas para ler ou alterar o DOM;
- **Service worker:** lógica em segundo plano orientada a eventos no Manifest V3;
- **Storage:** armazenamento local de preferências e dados estritamente necessários.

Cada parte tem um contexto e privilégios diferentes. Comece com a menor arquitetura que resolve o problema. Não crie um service worker ou content script apenas por hábito.

## 2. O que estudar primeiro

Estude nesta ordem:

1. HTML, CSS e JavaScript básicos;
2. DOM, eventos, módulos e tratamento de erros;
3. JSON e leitura do `manifest.json`;
4. APIs de extensões, mensagens e armazenamento;
5. permissões, Content Security Policy e princípios de privacidade;
6. DevTools, Git e documentação técnica.

Consulte sempre a documentação oficial do navegador usado. Chrome e Firefox possuem APIs e nomes de permissões que podem variar.

## 3. Planeje antes de programar

Escreva respostas curtas para estas perguntas:

- qual problema a extensão resolve?
- em quais páginas ela precisa funcionar?
- quais dados entram e quais dados saem?
- alguma informação precisa ser enviada para um servidor?
- qual é a menor permissão necessária para cada ação?
- como o usuário sabe que a ação está acontecendo?

Transforme as respostas em uma lista de funcionalidades e em uma matriz simples:

```text
Funcionalidade | Componente       | Permissão necessária | Dado acessado
---------------|------------------|----------------------|--------------
Ler a página   | content script   | host específico      | texto visível
Salvar opção   | popup             | storage              | preferência local
```

Se uma funcionalidade não justifica uma permissão, remova a permissão ou redesenhe a funcionalidade.

## 4. Estrutura inicial recomendada

```text
minha-extensao/
├── manifest.json
├── popup.html
├── popup.css
├── popup.js
├── content.js
├── service-worker.js
├── options.html
├── options.js
└── README.md
```

Inclua apenas os arquivos usados. Ícones podem ser adicionados quando a identidade visual estiver definida.

Um manifesto mínimo de Manifest V3 pode começar assim:

```json
{
  "manifest_version": 3,
  "name": "Minha extensão",
  "version": "0.1.0",
  "description": "Descrição objetiva da funcionalidade.",
  "action": {
    "default_popup": "popup.html"
  },
  "permissions": ["storage"]
}
```

Adicione `host_permissions`, `content_scripts` ou um `background.service_worker` somente quando houver uma necessidade demonstrável. Não use `"<all_urls>"` como atalho para evitar definir os sites necessários.

## 5. Implemente por etapas

### Etapa 1: interface

Crie o popup ou a página de opções com HTML semântico e estilos simples. Dê feedback para estados de sucesso e erro. Não coloque dados sensíveis no HTML nem em mensagens de depuração.

### Etapa 2: lógica local

Implemente primeiro a funcionalidade sem rede. Valide entradas, trate falhas e armazene apenas o necessário. Prefira `chrome.storage` ou `browser.storage` conforme o navegador e documente o formato salvo.

### Etapa 3: comunicação entre componentes

Use mensagens com tipos claros e valide a origem e os campos recebidos. O popup pode desaparecer a qualquer momento; não dependa dele para tarefas duradouras. O service worker do Manifest V3 pode ser encerrado e reiniciado, portanto não trate memória global como armazenamento permanente.

### Etapa 4: acesso à página

Use content scripts somente nos hosts necessários. Leia ou altere o conteúdo mínimo para cumprir a tarefa e não colete campos de formulário, cookies ou informações pessoais sem finalidade explícita e consentimento adequado.

### Etapa 5: rede

Adicione comunicação externa apenas quando indispensável. Documente domínio, endpoint, método, dados enviados, retenção e motivo. Use HTTPS, não inclua segredos no código e não execute código recebido da rede.

## 6. Teste no navegador

1. Abra a página de extensões do navegador e habilite o modo de desenvolvedor.
2. Carregue a pasta da extensão sem compactá-la.
3. Teste cada fluxo em uma página de teste sem dados reais.
4. Use o console do popup, do content script e do service worker separadamente.
5. Observe a aba de rede e registre apenas requisições esperadas.
6. Recarregue a extensão e repita os testes após reiniciar o navegador.

Verifique também mensagens de erro, permissões exibidas ao usuário, armazenamento criado e comportamento quando a página não corresponde ao host permitido.

## 7. Checklist de segurança antes de compartilhar

- [ ] o manifesto declara apenas permissões necessárias;
- [ ] hosts autorizados são específicos;
- [ ] não há `eval`, `new Function` ou código remoto sem justificativa excepcional;
- [ ] entradas e mensagens são validadas;
- [ ] dados sensíveis não são registrados em logs;
- [ ] requisições externas estão documentadas e usam HTTPS;
- [ ] a extensão não coleta mais do que precisa;
- [ ] o README explica instalação, uso, permissões e privacidade;
- [ ] o código foi revisado por outra pessoa ou com uma checklist;
- [ ] os testes foram executados em perfil isolado.

## 8. Como documentar

O `README.md` da extensão deve explicar problema resolvido, instalação, funcionalidades, permissões, dados tratados, comunicação externa e limitações. Registre decisões importantes no Git com mensagens claras e mantenha dependências atualizadas.

Para revisar seu próprio projeto, aplique também os documentos de análise estática, análise dinâmica e checklist de segurança deste repositório. O objetivo é que outra pessoa consiga entender o que a extensão faz e confirmar que ela faz apenas isso.

## 9. Próximo exercício

Comece por uma extensão pequena, como salvar uma preferência local ou alterar um elemento específico de uma página de teste. Só avance para múltiplos componentes, permissões adicionais ou um backend depois de conseguir explicar o fluxo completo e justificar cada acesso.
