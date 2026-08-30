# TGM — Indicadores

Aplicação web estática de indicadores para a disciplina **TGM** (Turbinas) durante a Parada Programada de Agosto/Setembro de 2026.

Estrutura de indicadores clonada do painel da Parada Elétrica (`hayralde/rrp`), mas com tarefas próprias do TGM — atualmente a revisão da turbina **BTE-40** (38 atividades).

## Funcionalidades

- Estatísticas gerais (tarefas, horas alocadas, concluídas, em andamento)
- Curva S do projeto (planejado x realizado)
- Distribuição por status (pendente / em andamento / concluída / atrasada)
- Tarefas por turno e status
- Atividades por TAG de equipamento
- Lista de atividades com % concluído
- Tabela completa de tarefas, com filtros e marcação de status (persistida no navegador via `localStorage`)

## Como adicionar tarefas novas

Não há backend — os dados vivem direto no `index.html`. Para adicionar uma tarefa nova, edite o arquivo e acrescente uma `<tr>` dentro de `<tbody id="pt-tbody">`, seguindo o padrão das demais linhas:

```html
<tr data-date="2026-09-08" data-setor="1º Turno" data-resp="BTE-40" data-os="BTE-40-39" data-hours="4" data-key="tgm-39">
  <td>08/09</td><td>07:00–11:00</td><td>1º Turno</td><td>BTE-40</td>
  <td class="pt-activity">DESCRIÇÃO DA ATIVIDADE</td><td class="num">4h</td>
  <td class="pt-status-cell"><select class="pt-status-select" onchange="setStatus(this)"><option value="">Pendente</option><option value="andamento">Em Andamento</option><option value="concluida">Concluída</option></select></td>
</tr>
```

- `data-setor` = turno (1º/2º/3º Turno) — alimenta o gráfico "Tarefas por turno e status"
- `data-resp` = TAG do equipamento — alimenta o painel "Atividades por TAG"
- `data-os` = identificador único da atividade (TAG + número) — alimenta a "Lista de atividades"
- `data-key` = chave única da linha (usada para guardar o status marcado no navegador)

Os painéis de indicadores (stat tiles, gráficos, cards, filtros) recalculam tudo automaticamente a partir das linhas da tabela — não é preciso editar mais nada em outro lugar do arquivo.

## Site

**https://hayralde.github.io/rrp1/**
