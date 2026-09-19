# Guia de organização do SHIELD_EXTENSION

## 1. Finalidade

O SHIELD_EXTENSION é um repositório para examinar extensões de navegador e identificar sinais de malware, abuso de permissões, coleta indevida de dados e comunicação externa sem justificativa clara.

O trabalho deve ser técnico, reproduzível e autorizado. O objetivo não é provar que uma extensão é maliciosa apenas por conter uma API sensível, mas relacionar intenção declarada, permissões, código, comportamento observado e evidências.

## 2. Escopo de cada análise

Antes de coletar qualquer dado, registre em `extension_review.md`:

- nome, versão, origem e hash do material analisado;
- navegador, sistema operacional e perfil de teste utilizados;
- objetivo declarado e funcionalidades esperadas;
- permissões solicitadas e riscos que serão priorizados;
- data, responsável e limitações da análise.

Use um perfil ou máquina de teste sem dados pessoais. Não instale a extensão em uma conta de uso diário e não envie informações reais para servidores de terceiros.

## 3. Estrutura do repositório

```text
SHIELD_EXTENSION/
├── README.md
├── GUIA_CRIACAO_REPOSITORIO.md
├── INICIANTE_GUIA_EXTENSAO.md
├── extension_review.md
├── docs/
│   ├── analise-estatica.md
│   ├── analise-dinamica.md
│   └── checklist-seguranca.md
├── evidencias/
│   ├── capturas/
│   ├── logs/
│   └── relatorios/
├── src/
│   └── codigo-analisado/
└── reports/
    └── relatorio-final.md
```

Mantenha o código analisado separado das notas. Cada evidência deve indicar origem, data, ambiente e relação com o achado. Nunca inclua tokens, cookies, senhas ou dados pessoais nos arquivos versionados.

## 4. Método de análise

### 4.1 Preparação

1. Defina o escopo e registre a versão exata do artefato.
2. Faça uma cópia somente leitura do material original.
3. Calcule um hash e registre ferramentas e versões utilizadas.
4. Leia a documentação da extensão para estabelecer o comportamento esperado.

### 4.2 Análise estática

Inspecione `manifest.json`, scripts, bibliotecas, arquivos de configuração e recursos. Compare cada permissão com uma funcionalidade necessária.

Procure especialmente por:

- `permissions` e `host_permissions` amplas;
- acesso a cookies, histórico, abas, armazenamento e formulários;
- `fetch`, `XMLHttpRequest`, WebSocket e destinos externos;
- injeção de scripts, `eval`, `new Function` e código ofuscado;
- carregamento remoto, atualizações ou comandos recebidos do servidor;
- coleta, transformação e envio de identificadores ou conteúdo de páginas;
- dependências incluídas sem origem ou justificativa.

Para cada achado, registre arquivo, linha ou função, comportamento esperado, comportamento observado e evidência correspondente. Código minificado, isoladamente, não é prova de ameaça: avalie o que ele executa.

### 4.3 Análise dinâmica

Execute a extensão em ambiente isolado e com dados sintéticos. Observe:

- requisições de rede, domínio, método, momento e conteúdo;
- criação ou alteração de arquivos, processos e armazenamento;
- eventos de navegação, abas, downloads e formulários;
- mensagens entre popup, content scripts e service worker;
- ações que ocorrem sem interação do usuário ou após reinicialização.

Repita os testes com a extensão inativa, ativa e submetida a fluxos relevantes. Salve logs e capturas com nomes descritivos, por exemplo `2026-09-19-requisicao-dominio-x.txt`.

### 4.4 Correlação e triagem

Não classifique um risco por um único indicador. Correlacione:

1. a capacidade concedida pela permissão;
2. o caminho de código que usa essa capacidade;
3. o dado acessado ou enviado;
4. a finalidade declarada;
5. a evidência reproduzível.

Classifique a severidade assim:

- **Crítica:** exfiltração comprovada de credenciais ou dados sensíveis, execução remota arbitrária ou controle persistente do navegador;
- **Alta:** coleta ou envio não autorizado de dados relevantes, abuso de permissões amplas ou comportamento oculto com impacto significativo;
- **Média:** comportamento inesperado, exposição limitada ou permissão desnecessária sem exfiltração comprovada;
- **Baixa:** inconsistência documental, telemetria pouco transparente ou risco de impacto reduzido.

Indique também a confiança da conclusão: baixa, média ou alta. Ausência de evidência não equivale a evidência de ausência.

## 5. Registro de achados

Use este formato em `extension_review.md` ou em um relatório intermediário:

```text
ID: SHIELD-001
Título: [comportamento observado]
Severidade: [crítica|alta|média|baixa]
Confiança: [baixa|média|alta]
Arquivo e localização: [caminho, linha ou função]
Evidência: [log, captura, hash ou reprodução]
Descrição: [o que acontece e em quais condições]
Impacto: [dado, usuário ou recurso afetado]
Justificativa: [por que é esperado ou suspeito]
Recomendação: [ação de correção ou mitigação]
```

Separe fatos observados, interpretação e hipótese. Isso torna a revisão auditável e evita conclusões maiores que as evidências.

## 6. Relatório final

O arquivo `reports/relatorio-final.md` deve conter:

1. resumo executivo e conclusão;
2. escopo, versão analisada e limitações;
3. ambiente e ferramentas;
4. permissões e comportamento esperado;
5. achados ordenados por severidade;
6. evidências e passos de reprodução;
7. recomendações e decisão de risco;
8. itens que permanecem inconclusivos.

Use conclusões precisas, como “não foram observadas evidências de exfiltração durante os cenários testados”, em vez de afirmar que a extensão é completamente segura.

## 7. Checklist de encerramento

- [ ] o escopo, a versão e o hash foram registrados;
- [ ] o ambiente de teste não contém dados reais;
- [ ] permissões e domínios foram revisados;
- [ ] os principais fluxos foram analisados estaticamente e dinamicamente;
- [ ] cada achado possui localização e evidência;
- [ ] segredos e dados pessoais foram removidos dos artefatos;
- [ ] severidade e confiança foram justificadas;
- [ ] limitações e cenários não testados foram documentados;
- [ ] o relatório final responde se há comportamento suspeito e por quê.

## 8. Princípio central

Uma análise de qualidade é organizada, autorizada e verificável. O SHIELD_EXTENSION deve documentar o que foi observado, como foi observado e quais medidas reduzem o risco, sem transformar suposições em acusações.
