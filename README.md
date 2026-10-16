# Simulador de estrutura curricular do curso de Direito de Santa Rita

Ferramenta de trabalho do Núcleo Docente Estruturante do curso de Bacharelado em Direito de Santa Rita (DCJ/CCJ/UFPB) para montar e testar, em reunião, a estrutura curricular do novo Projeto Pedagógico do Curso.

Os dados carregados são propostas de trabalho. Nenhuma delas representa deliberação do NDE ou do Colegiado do Curso.

Página publicada: https://clovisfalcao.github.io/simulador-grade-curricular/

## Propostas

- **Grade de trabalho (16/10) (padrão)**: proposta atual, com os ajustes da reunião de 16.10.2026. São 3.270 h de obrigatórias, 240 h de optativas e 240 h de flexíveis, num total de 3.750 h (em discussão).
- **Versão de 18/09 (memória)**: estudo preliminar discutido pelo NDE em 18.09.2026, mantido para comparação.

Componentes com a etiqueta **a decidir** dependem do NDE ou do Colegiado. A etiqueta se marca ou desmarca no painel do componente.

## O que o simulador faz

- **Grade**: mostra os componentes por período e a linha de pré-requisitos. Permite arrastar componentes entre períodos e ligar ou desligar pré-requisitos. Recusa o arrasto que poria um componente no mesmo período de seu pré-requisito, ou antes dele. As optativas ficam fora das colunas, no período seguinte ao de seu pré-requisito.
- **Tabela de componentes**: edição em linha da carga teórica, prática e de extensão de cada componente.
- **Conformidade**: separa as exigências normativas das diretrizes do NDE.
- **Totais**: totalização no formato do histórico escolar (básicos profissionais, complementares obrigatórios, optativos e flexíveis). A creditação de extensão aparece num quadro à parte, porque a extensão se sobrepõe a essas categorias.
- **Gráficos**: composição da carga, caminhos da extensão até o piso, carga por período e comparação entre propostas.
- **Equivalências**: tabela do currículo de 2019 para a proposta, pelo estudo de 16.10.2026.

## Como usar

Abra `index.html` no navegador ou acesse a página publicada. Nada é salvo automaticamente. Use **Salvar ponto de controle** para guardar a situação atual no navegador e **Pontos de controle** para voltar a uma situação salva.

Em **Exportar**, há duas saídas:

- o arquivo `.json`, que se abre em outro computador com **Importar**;
- a planilha da estrutura curricular (`.csv`), com período, categoria no histórico, natureza, carga decomposta, créditos e pré-requisitos.

O arquivo é autocontido: usa apenas as fontes IBM Plex do Google Fonts e não depende de servidor. O GitHub Pages publica a branch `claude/nice-babbage-pqpsnj`.
