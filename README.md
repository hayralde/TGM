# TGM — Indicadores

Aplicação web estática de indicadores para a disciplina **TGM** (Turbinas) durante a Parada Programada de Agosto/Setembro de 2026.

Estrutura de indicadores clonada do painel da Parada Elétrica (`hayralde/rrp`), mas com tarefas próprias do TGM — atualmente a revisão da turbina **BTE-40** (38 atividades).

## Funcionalidades

- Estatísticas gerais (tarefas, horas alocadas, concluídas, em andamento)
- Curva S do Projeto TGM (planejado x realizado)
- Distribuição por status — TGM (pendente / em andamento / concluída / atrasada)
- Tabela completa de tarefas, numerada, com filtros e marcação de status — o status é salvo no Supabase e fica visível para qualquer pessoa que acessar o site (atualiza sozinho a cada 5s, sem precisar recarregar a página)

## Backend (status compartilhado)

As tarefas em si (data, turno, TAG, atividade, duração) vivem direto no `index.html` — sem backend. Só o **status** de cada tarefa (Pendente/Em Andamento/Concluída) é compartilhado entre todos os visitantes, salvo na tabela `tgm_task_status` do projeto Supabase `hayralde's Project` (`rsqbbcsaqmxfriwwbamv`). A página consulta essa tabela a cada 5s para refletir o que outros visitantes marcaram. Qualquer visitante pode alterar o status — não há senha nem login.

## Como adicionar tarefas novas

Para adicionar uma tarefa nova, edite o `index.html` e acrescente uma `<tr>` dentro de `<tbody id="pt-tbody">`, seguindo o padrão das demais linhas:

```html
<tr data-date="2026-09-08" data-setor="1º Turno" data-resp="BTE-40" data-os="BTE-40-39" data-hours="4" data-key="tgm-39">
  <td>08/09</td><td>07:00–11:00</td><td>1º Turno</td><td>BTE-40</td>
  <td class="pt-activity">DESCRIÇÃO DA ATIVIDADE</td><td class="num">4h</td>
  <td class="pt-status-cell"><select class="pt-status-select" onchange="setStatus(this)"><option value="">Pendente</option><option value="andamento">Em Andamento</option><option value="concluida">Concluída</option></select></td>
</tr>
```

- `data-setor` = turno (1º/2º/3º Turno)
- `data-resp` = TAG do equipamento
- `data-os` = identificador único da atividade (TAG + número)
- `data-key` = chave única da linha (usada como `task_key` na tabela `tgm_task_status` do Supabase)

A numeração (coluna `#`) é gerada automaticamente pelo JS a partir da ordem das linhas — não precisa adicionar manualmente.

Os painéis de indicadores (stat tiles, gráficos, cards, filtros) recalculam tudo automaticamente a partir das linhas da tabela — não é preciso editar mais nada em outro lugar do arquivo.

## Backup e restauração do status

`backup.html` (link no rodapé do painel) baixa um `.json` com o status atual de todas as tarefas (tabela `tgm_task_status` do Supabase) para o computador, e restaura a partir de um arquivo baixado anteriormente. Só o status é coberto — o restante do app (código, tarefas cadastradas) já vive no GitHub e não precisa de backup à parte. Restaurar sobrescreve o status atual para todos os visitantes, por isso pede confirmação antes de gravar.

## Instalar como aplicativo (PWA)

O site é um PWA instalável: `manifest.json` + `sw.js` (service worker, cacheia o essencial para abrir mesmo offline) + `icon-192.png`/`icon-512.png` (gerados a partir do `favicon.svg`). No Chrome/Android, um botão "Instalar aplicativo" aparece no topo da página quando o navegador considera o site instalável (evento `beforeinstallprompt`); clicar nele abre o prompt nativo do Android para adicionar à tela inicial. No iOS/Safari não existe esse prompt automático — a pessoa precisa usar Compartilhar → Adicionar à Tela de Início manualmente (os metadados `apple-touch-icon`/`apple-mobile-web-app-*` no `<head>` cuidam do ícone e do modo tela cheia nesse caso).

## Site

**https://hayralde.github.io/TGM/**
