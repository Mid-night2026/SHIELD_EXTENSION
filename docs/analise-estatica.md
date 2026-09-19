# Análise estática

## Objetivo

Verificar a estrutura e o código da extensão sem executá-la, com foco em identificar comportamentos suspeitos, permissões excessivas e fluxos de coleta de dados.

## O que verificar

- manifest.json
- permissões solicitadas
- scripts JavaScript
- chamadas a APIs e URLs
- uso de bibliotecas externas
- estruturas de comunicação com servidores remotos
- padrões de coleta, obfuscação ou execução indireta

## Checklist

- [ ] A extensão solicita apenas permissões necessárias?
- [ ] Há URLs ou domínios desconhecidos?
- [ ] Existem scripts ocultos ou minificados?
- [ ] O código acessa dados sensíveis?
- [ ] Há comportamento de execução remota ou monitoramento?

## Observações

Registrar cada achado com detalhe, referência ao arquivo e impacto provável.
