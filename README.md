# Simulador de estrutura curricular do curso de Direito de Santa Rita

Ferramenta de trabalho do Núcleo Docente Estruturante do curso de Bacharelado em Direito de Santa Rita (DCJ/CCJ/UFPB) para montar e testar a matriz curricular do novo Projeto Pedagógico do Curso.

Os dados carregados são propostas de trabalho. Nenhuma delas representa deliberação do NDE ou do Colegiado do Curso.

## O que o simulador faz

- Mostra a grade por período e destaca a linha de pré-requisitos de cada componente: o que ele exige e o que depende dele.
- Permite ligar e desligar pré-requisitos clicando nos componentes ("Modo ligação").
- Permite arrastar os componentes de um período para outro. O simulador recusa o arrasto que poria um componente no mesmo período de seu pré-requisito, ou antes dele, e avisa quando uma alteração cria problema novo, como período acima do teto de horas em sala.
- Registra, para cada componente, a carga horária teórica, prática e de extensão, com os créditos correspondentes (1 crédito = 15 h).
- Calcula os totais por categoria do histórico escolar: componentes obrigatórios, optativos, atividades complementares e extensão por aproveitamento.
- Mostra as optativas fora das colunas de períodos, no banco, com o período a que cada uma pertence: o seguinte ao de seu pré-requisito mais adiantado, obrigatório ou optativo, ou o período 0 se não houver pré-requisito. O banco lista as optativas do período 0 ao 10º.
- Verifica a conformidade da proposta, separando exigências normativas de diretrizes do NDE.
- Compara a proposta de trabalho com as cópias alteradas a partir dela.

## Verificações

**Exigências normativas**

| Verificação | Fonte |
|---|---|
| Carga horária mínima de 3.700 h | DCN (Res. CNE/CES 5/2018), art. 12; Res. CNE/CES 2/2007, anexo |
| Integralização mínima de cinco anos | Res. CNE/CES 2/2007, art. 2º, III, "d" |
| Extensão entre 10% e 15% da carga total, em qualquer trajetória | Res. CONSEPE 02/2022, art. 6º |
| Conteúdos básicos profissionais, no mínimo 50% | RGG (Res. CONSEPE 29/2020), art. 16, § 2º, I |
| Atividades complementares e prática jurídica, no máximo 20% | DCN, art. 13; Res. CNE/CES 2/2007, art. 1º, parágrafo único |
| Pré-requisito em nível anterior da estrutura | RGG, art. 33, § 1º |
| Extensão creditada apenas pelas modalidades do art. 7º | Res. CONSEPE 02/2022, art. 7º |

**Diretrizes do NDE**: teto de horas em sala por período, disciplinas obrigatórias com 60 h, componentes numerados em períodos seguidos, Penal acompanhando Civil, cadeia de disciplinas até o último período, pré-requisitos sem redundância, limites de matrícula semestral, oferta para o bloco de UCE optativas, vedação de dupla contagem entre extensão e prática jurídica.

## Propostas incluídas

- **Grade atualizada (16/10) (padrão)**: base de 19.09.2026 com as decisões de 15 e 16.10.2026. A extensão integra a carga total (piso de 10%, teto de 15% no histórico); acaba o mínimo de UCE optativas; atividades complementares e extensão em programas e projetos formam um bloco flexível de 240 h, do qual até X h contam como extensão; Medicina Legal passa a optativa e entra Criminologia, Política Criminal e Leis Penais Especiais no 10º período; entram Libras e duas UCE optativas a criar.
- **Variante: duas UCE obrigatórias (16/10)**: sai a UCE-OB III e as optativas sobem para 300 h.
- **Versão de 19/09** e **Versão de 18/09 (estudo preliminar)**: mantidas para comparação. Passam pelas mesmas verificações.

Componentes com a etiqueta **a decidir** dependem do NDE ou do Colegiado; a etiqueta se marca ou desmarca no painel do componente.

As alterações são recalculadas na hora. Para testar alternativas sem perder a base, use **Duplicar** e altere a cópia; a aba Totais compara as propostas lado a lado. **Restaurar originais** volta às quatro propostas de partida.

## Como usar

Abra `index.html` no navegador ou acesse a página publicada. Nada é salvo automaticamente: use **Salvar ponto de controle** para guardar a situação atual no navegador e **Pontos de controle** para voltar a uma situação salva. Para levar uma proposta a outro computador ou compartilhá-la, use **Exportar** e depois **Importar** no destino.

O simulador também importa o JSON exportado pelo [simulador original do NDE](https://hugobelmorais-oss.github.io/simulador-ppc-dcj/). Nesse caso as categorias são convertidas automaticamente e os pré-requisitos precisam ser preenchidos.

## Publicação no GitHub Pages

1. Crie um repositório público e envie os arquivos `index.html` e `README.md` para a raiz.
2. Em **Settings → Pages**, escolha **Deploy from a branch**, branch `main`, pasta `/ (root)`.
3. Em alguns minutos a página fica disponível em `https://<usuario>.github.io/<repositorio>/`.

O arquivo é autocontido: usa apenas as fontes IBM Plex do Google Fonts e não depende de servidor.
