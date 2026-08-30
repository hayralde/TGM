# TGM — Indicadores

Aplicação web estática de indicadores para a disciplina **TGM** (Turbinas) durante a Parada Programada de Agosto/Setembro de 2026.

Estrutura de indicadores clonada do painel da Parada Elétrica (`hayralde/rrp`), mas com tarefas próprias do TGM — atualmente a revisão da turbina **BTE-40** (38 atividades).

## Funcionalidades

- Estatísticas gerais (tarefas, horas alocadas, concluídas, em andamento)
- Curva S do Projeto TGM (planejado x realizado)
- Distribuição por status — TGM (pendente / em andamento / concluída / atrasada)
- Tabela completa de tarefas, numerada, com filtros e marcação de status — o status é salvo no Supabase e fica visível para qualquer pessoa que acessar o site (atualiza sozinho a cada 5s, sem precisar recarregar a página)

## Backend (status compartilhado)

As tarefas em si (data, turno, TAG, atividade, duração) vivem direto no `index.html` — sem backend. Só o **status** de cada tarefa (Pendente/Em Andamento/Concluída) é compartilhado entre todos os visitantes, salvo na tabela `tgm_task_status` do projeto Supabase `hayralde's Project` (`rsqbbcsaqmxfriwwbamv`). A página consulta essa tabela a cada 5s para refletir o que outros visitantes marcaram.

Alterar o status pede uma senha (`rrp`, ver `EDIT_PASSWORD` no `index.html`) — pedida uma vez por sessão do navegador (guardada em `sessionStorage`, não persiste entre sessões). Isso é só uma barreira leve contra alterações acidentais/de curiosos: a senha fica visível no código-fonte da página, então não serve como controle de acesso real.

## Como adicionar tarefas novas

Para adicionar uma tarefa nova, edite o `index.html` e acrescente uma `<tr>` dentro de `<tbody id="pt-tbody">`, seguindo o padrão das demais linhas:

```html
<tr data-date="2026-09-08" data-setor="1º Turno" data-resp="BTE-40" data-os="BTE-40-39" data-hours="4" data-key="tgm-39">
  <td>08/09</td><td>07:00–11:00</td><td>1º Turno</td><td>BTE-40</td>
  <td class="pt-activity">DESCRIÇÃO DA ATIVIDADE</td><td class="num">4h</td>
  <td class="pt-status-cell"><select class="pt-status-select" onchange="setStatus(this)"><option value="">Pendente</option><option value="andamento">Em Andamento</option><option value="concluida">Concluída</option></select></td>
</tr>
```

- `data-setor` = turno (1º/2º/3º Turno) — alimenta o gráfico "Tarefas por turno e status"
- `data-resp` = TAG do equipamento
- `data-os` = identificador único da atividade (TAG + número)
- `data-key` = chave única da linha (usada como `task_key` na tabela `tgm_task_status` do Supabase)

A numeração (coluna `#`) é gerada automaticamente pelo JS a partir da ordem das linhas — não precisa adicionar manualmente.

Os painéis de indicadores (stat tiles, gráficos, cards, filtros) recalculam tudo automaticamente a partir das linhas da tabela — não é preciso editar mais nada em outro lugar do arquivo.

## Site

**https://hayralde.github.io/TGM/**
