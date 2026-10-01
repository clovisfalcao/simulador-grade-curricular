# Simulador do PPC de Direito de Santa Rita

Ferramenta de trabalho do Núcleo Docente Estruturante do curso de Bacharelado em Direito de Santa Rita (DCJ/CCJ/UFPB) para montar e testar a matriz curricular do novo Projeto Pedagógico do Curso.

Os dados carregados são propostas de trabalho. Nenhuma delas representa deliberação do NDE ou do Colegiado do Curso.

## O que o simulador faz

- Mostra a grade por período e destaca a linha de pré-requisitos de cada componente: o que ele exige e o que depende dele.
- Permite ligar e desligar pré-requisitos clicando nos componentes ("Modo ligação").
- Permite arrastar os componentes de um período para outro. O simulador recusa o arrasto que poria um componente no mesmo período de seu pré-requisito, ou antes dele, e avisa quando uma alteração cria problema novo, como período acima do teto de horas em sala.
- Registra, para cada componente, a carga horária teórica, prática e de extensão, com os créditos correspondentes (1 crédito = 15 h).
- Calcula os totais por categoria do histórico escolar: componentes obrigatórios, optativos, atividades complementares e extensão por aproveitamento.
- Mostra as optativas fora das colunas de períodos, no banco, com o período a que cada uma pertence: o seguinte ao de seu pré-requisito mais adiantado, obrigatório ou optativo, ou o 1º se não houver pré-requisito.
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

## Proposta incluída

O simulador abre com uma única **Proposta de trabalho**: a grade revisada após a reunião do NDE, com os Laboratórios como atividade de orientação coletiva, Direito do Consumidor obrigatório e o novo modelo de extensão. As alterações feitas nela são recalculadas na hora. Para testar alternativas sem perder a base, use **Duplicar** e altere a cópia; a aba Totais compara as propostas quando houver mais de uma. **Restaurar original** volta à proposta de partida.

## Como usar

Abra `index.html` no navegador ou acesse a página publicada. As alterações ficam guardadas no navegador de quem usa. Para levar uma proposta a outro computador ou compartilhá-la, use **Exportar** e depois **Importar** no destino.

O simulador também importa o JSON exportado pelo [simulador original do NDE](https://hugobelmorais-oss.github.io/simulador-ppc-dcj/). Nesse caso as categorias são convertidas automaticamente e os pré-requisitos precisam ser preenchidos.

## Publicação no GitHub Pages

1. Crie um repositório público e envie os arquivos `index.html` e `README.md` para a raiz.
2. Em **Settings → Pages**, escolha **Deploy from a branch**, branch `main`, pasta `/ (root)`.
3. Em alguns minutos a página fica disponível em `https://<usuario>.github.io/<repositorio>/`.

O arquivo é autocontido: usa apenas as fontes IBM Plex do Google Fonts e não depende de servidor.
