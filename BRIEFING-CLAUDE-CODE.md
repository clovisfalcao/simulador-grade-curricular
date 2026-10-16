# Simulador do PPC de Direito de Santa Rita: briefing para o Claude Code

Este documento descreve o aplicativo contido em `index.html`, o raciocínio que o originou e o que precisa ser preservado em qualquer alteração. Leia-o inteiro antes de mexer no código.

## 1. Contexto

O curso de Bacharelado em Direito de Santa Rita (Departamento de Ciências Jurídicas, Centro de Ciências Jurídicas, UFPB) está elaborando um novo Projeto Pedagógico do Curso (PPC), com curricularização da extensão. O trabalho é conduzido pelo Núcleo Docente Estruturante (NDE) e será submetido ao Colegiado do Curso e, no que couber, ao colegiado departamental.

Existe um simulador anterior, feito por um professor do NDE: <https://hugobelmorais-oss.github.io/simulador-ppc-dcj/> (repositório `hugobelmorais-oss/simulador-ppc-dcj`). Ele soma cargas horárias por período e por natureza e verifica apenas a faixa de extensão. Este aplicativo foi criado para cobrir o que aquele não cobre: pré-requisitos, duração mínima, decomposição da carga horária e as demais exigências normativas.

O aplicativo traz duas propostas de partida. A **Grade de trabalho (16/10)** (id `v1610`) é a única versão de trabalho: o estado inicial de 16.10.2026, com 180 h de extensão fixa, sobre o qual se montam todos os outros estados (por exemplo, a variante com duas UCE obrigatórias: arrastar a UCE-OB III para o banco e elevar as optativas a 300 h). A **Versão de 18/09 (memória)** (id `anterior`) fica como registro do estudo preliminar e referência de comparação. A variante de 16/10 e a versão de 19/09 saíram dos dados; quem as tinha na área de trabalho perde essas duas uma única vez (marcador `retirados-1` em `S.modelos`), sem perder edições na grade de trabalho. Estados intermediários se guardam em Pontos de controle. Nenhuma proposta foi deliberada pelo NDE ou pelo Colegiado. Isso precisa continuar claro em qualquer texto do aplicativo ou do repositório.

## 2. Finalidade

Permitir que os membros do NDE montem e alterem a matriz curricular e vejam, a cada alteração, se a proposta cumpre as normas e as diretrizes adotadas pelo curso. O usuário principal é o coordenador do curso, professor de Direito, sem formação em programação. O público secundário são os demais professores do NDE.

O aplicativo é ferramenta de simulação e conferência. Ele não substitui a nota técnica do PPC nem a deliberação dos colegiados.

## 3. Arquivos

| Arquivo | Conteúdo |
|---|---|
| `index.html` | Aplicativo completo, autocontido: HTML, CSS e JavaScript inline, com os dados de partida embutidos. Única dependência externa: fontes IBM Plex do Google Fonts, com fallback. |
| `README.md` | Apresentação pública do repositório. |
| `BRIEFING-CLAUDE-CODE.md` | Este documento. Não precisa ser publicado no repositório, a critério do usuário. |

Existe também uma versão publicada como artefato no claude.ai, com o mesmo código. A única diferença é a exportação: no claude.ai ela usa a capacidade `downloads` do visualizador (`window.claude.use('downloads')`); fora dele, usa `Blob` com link de download. O código já trata os dois casos.

## 4. Tarefa imediata

1. Criar um repositório no GitHub com `index.html` e `README.md` na raiz.
2. Ativar o GitHub Pages a partir da branch `main`, pasta raiz.
3. Confirmar que a página abre, que os dados de partida aparecem e que as quatro abas funcionam.

Antes de criar o repositório, confirme com o usuário se ele deve ser público. A página publicada no Pages fica pública e contém propostas não deliberadas. Alternativas: repositório privado sem Pages, ou contribuição ao repositório do simulador original.

Não altere cálculos, textos normativos ou dados de partida nesta tarefa.

## 5. Modelo de dados

O estado é `{props: [proposta, ...], ativo: id}`. Não há salvamento automático, por decisão do coordenador: a página funciona como área de trabalho, e o usuário salva pontos de controle quando quer. Salvar um ponto grava a lista de pontos em `simulador-ppc-dcj-sr-v1-pontos` (cada ponto com nome, data, resumo e cópia do estado) e grava o estado em `simulador-ppc-dcj-sr-v1`, que é a situação aberta na próxima visita. Voltar a um ponto também atualiza essa chave. Ao sair com alterações não salvas, o navegador pede confirmação. A aba ativa fica em `simulador-ppc-dcj-sr-v1-tab`.

**Proposta**

| Campo | Significado |
|---|---|
| `id`, `nome`, `sub` | Identificação e descrição |
| `cfg.optExig` | Carga de optativas exigida do estudante (240 h) |
| `cfg.blocoUCE` | Mínimo dessas optativas a cumprir em unidades curriculares de extensão (120 h) |
| `cfg.tetoSala` | Teto de horas em sala por período (300 h) |
| `cfg.durMin` | Duração mínima em períodos (10) |
| `cfg.cargaMin` | Carga mínima do curso (3.700 h) |
| `cfg.perMax` | Prazo máximo de integralização em períodos (15) |
| `cfg.matMax` | Matrícula máxima por semestre; `null` significa automática |
| `comps` | Lista de componentes |

**Componente**

| Campo | Valores e significado |
|---|---|
| `id`, `n` | Identificador e nome |
| `tipo` | `obr` obrigatório; `opt` optativo (banco, não somado); `ac` atividades complementares; `apr` extensão por aproveitamento |
| `nat` | `disc` disciplina; `mod` módulo; `aoc` atividade de orientação coletiva; `aoi` atividade de orientação individual; `apr` aproveitamento |
| `eixo` | `geral`, `tec` (técnico-jurídica), `ext`, `prat` (prática jurídica), `pesq` (pesquisa e Trabalho de Curso), `ac` |
| `p` | Período, de 1 a 10. Nos obrigatórios, `0` significa sem período, situação que gera aviso. Nas optativas, o campo não é usado: o período é sempre deduzido do pré-requisito (ver seção 6) |
| `teo`, `prat`, `ext` | Carga horária teórica, prática e de extensão. O total é a soma |
| `ch` | Só para `tipo: ac`, que não se decompõe |
| `pre` | Ids dos pré-requisitos |
| `ded` | Ids dos pré-requisitos deduzidos das ementas, sem indicação expressa em documento |
| `bas` | Conta como conteúdo básico profissional |
| `a13` | Conta no teto de atividades complementares e prática jurídica |
| `obs` | Observação livre, exibida no painel |
| `status` | `"possibilidade"` marca componente que depende do NDE ou do Colegiado; o cartão mostra a etiqueta "a decidir", que se marca ou desmarca no painel |
| `extTeto`, `extTetoOpcoes` | Só no bloco flexível: até quanto conta como extensão e as opções em discussão |
| `ref` | Referência interna aos documentos de trabalho; não é exibida |

Convenções: disciplinas têm carga integralmente teórica; atividades de orientação têm carga integralmente prática; unidades curriculares de extensão têm carga integralmente extensionista. Um crédito equivale a 15 h. Só disciplina e módulo ocupam dia no horário (RGG, art. 38, § 1º).

## 6. Regras de cálculo

Princípio adotado pelo coordenador: há duas classificações independentes. A categoria do componente (obrigatório, optativo, atividades complementares, extensão por aproveitamento) é uma; a composição da carga (teórica, prática, extensão) é outra. Elas não se cruzam nem se somam. Uma optativa de extensão entra uma vez na carga total, como optativa, e suas horas de extensão entram no percentual de extensão. Não existe bloco de extensão separado do bloco de optativas. Qualquer cálculo novo (percentuais, tetos, mínimos) deve respeitar essa separação.

Ficam no bloco "núcleo de cálculo", nas funções `calc(P)` e `verificacoes(P, R)`, que não acessam o DOM. Preserve essa separação.

- **Carga total** = obrigatórios + atividades complementares + extensão por aproveitamento + `optExig`. Optativas do banco não entram; a carga de optativas entra pelo parâmetro.
- **Extensão** (decisões de 15 e 16.10.2026): a extensão integra a carga total. As mesmas horas contam na categoria do componente (obrigatório, optativo, flexível) e como extensão; os 10% não elevam a carga total. Extensão pendente impede a conclusão.
- **Parte fixa** (`R.extMin`) = extensão dos obrigatórios + extensão por aproveitamento + `blocoUCE` (este só nas versões de 19/09 e 18/09; nas novas, `blocoUCE = 0`, sem mínimo de UCE optativas).
- **Piso por estudante** (`R.piso`) = 10% da carga total, arredondado para cima (372 h com 3.720 h). **Teto no histórico** (`R.tetoExt`) = 15%, arredondado para baixo (558 h). Não são parâmetros: seguem a carga total.
- **Parte livre** (`R.livre`) = piso − parte fixa. **Caminho das optativas** (`R.optExtLivre`) = menor valor entre a CH de optativas exigida e a oferta de extensão no banco. **Caminho do bloco flexível** (`R.extFlex`) = soma, nos componentes de tipo `ac`, do menor valor entre a carga e `extTeto`. Cada caminho fecha o piso se cobrir sozinho a parte livre.
- **Bloco flexível**: componente de tipo `ac` (240 h) com `extTeto` (até quanto conta como extensão) e `extTetoOpcoes` (120, 135, 150, 180, 195; padrão 195), escolhido na aba Conformidade.
- **Extensão máxima** (`R.extMax`) = parte fixa + caminho das optativas + caminho do bloco flexível.
- **Básicos profissionais** = soma dos obrigatórios com `bas`.
- **Atividades complementares e prática jurídica** = soma dos itens com `a13`, exceto optativas.
- **Horas em sala por período** = obrigatórios de natureza disciplina ou módulo no período.
- **Matrícula mínima** = (carga total − atividades complementares) ÷ `perMax`, arredondada para cima em múltiplo de 15 h.
- **Matrícula máxima** = `matMax`, ou a carga do período mais pesado + 60 h.
- **Duração mínima**: profundidade da maior cadeia de pré-requisitos entre obrigatórios, com detecção de ciclo. Confirmada por simulação do estudante mais adiantado: a cada período ele cursa tudo o que os pré-requisitos permitem, até a matrícula máxima, priorizando os componentes com cadeia mais longa à frente.
- **Cadeia só de disciplinas**: maior cadeia formada apenas por componentes de natureza disciplina ou módulo.
- **Elos sensíveis**: pré-requisitos cuja retirada isolada reduz a duração mínima abaixo de `durMin`.
- **Período das optativas** (`R.optPer`), conforme a prática da UFPB informada pelo coordenador: o período seguinte ao do pré-requisito mais adiantado, seja ele obrigatório ou optativo (limitado ao 10º); sem pré-requisito, período 0. Uma optativa que exige outra de período 0 fica no 1º. O período não é escolhido à mão. As optativas ficam fora das colunas de períodos, porque o estudante cumpre a carga de optativas em qualquer período, e não entram em nenhuma soma por período: a carga de optativas vem de `optExig`.

## 7. Verificações exibidas

**Exigências normativas** (cada uma traz a fonte na tela)

| Verificação | Fonte |
|---|---|
| Carga total mínima | DCN (Res. CNE/CES 5/2018, alterada pela Res. CNE/CES 2/2021), art. 12; Res. CNE/CES 2/2007, anexo |
| Duração mínima de cinco anos | Res. CNE/CES 2/2007, art. 2º, III, "d" |
| Extensão: piso de 10% alcançável por todo estudante; parte fixa até o teto de 15% | Res. CONSEPE 02/2022, art. 6º |
| Básicos profissionais no mínimo 50% | RGG (Res. CONSEPE 29/2020), art. 16, § 2º, I |
| Atividades complementares e prática jurídica no máximo 20% | DCN, art. 13; Res. CNE/CES 2/2007, art. 1º, parágrafo único |
| Pré-requisito em nível anterior | RGG, art. 33, § 1º |
| Extensão apenas por disciplina, módulo ou UCE | Res. CONSEPE 02/2022, art. 7º |

Cada verificação guarda, no campo `ref`, o código interno dos documentos de trabalho do NDE (H1, I3, D1 etc.). Esse campo nunca é exibido: a tela mostra só a fonte legível (norma ou "Diretriz do NDE"), porque quem opera o aplicativo não tem acesso a esses documentos. O mesmo vale para o campo `ref` dos componentes.

**Diretrizes do NDE**: caminhos para o piso de extensão (aviso se só um fecha; atenção se nenhum fecha sozinho); teto em sala por período; obrigatórios com período; disciplinas obrigatórias com 60 h; componentes numerados (I, II, III) em períodos seguidos, com as UCE dispensadas; Penal I a III nos mesmos períodos de Civil I a III; cadeia de disciplinas até o último período; elos sem redundância; limites de matrícula; oferta suficiente para o bloco de UCE optativas; vedação de carga de extensão em componente de prática jurídica; créditos inteiros.

A distinção entre norma e diretriz interna é deliberada e deve ser mantida.

## 8. Interface

- **Cabeçalho**: título "Simulador de estrutura curricular"; seleção da proposta; duplicar, renomear, excluir, exportar, importar e restaurar a proposta original (substitui a área de trabalho pelos dados de partida do arquivo, sem apagar os pontos de controle). Abaixo, "Salvar ponto de controle", "Pontos de controle" (lista com voltar e excluir, ambos com confirmação) e a situação do salvamento ("Alterações não salvas" ou o último ponto salvo). Ações destrutivas pedem confirmação na própria página, sem `confirm()`.
- **Faixa de indicadores**: carga total, extensão, básicos, art. 13, duração mínima e créditos, com cor de situação.
- **Grade e pré-requisitos**: faixa horizontal "Como usar" no alto da aba, com uma nota discreta sobre a pendência do cálculo da extensão nas optativas, que pode ser recolhida (o estado fica em `simulador-ppc-dcj-sr-v1-ajuda`). Colunas do 1º ao 10º período, mais "obrigatórios sem período" (que deve ficar vazia), "aproveitamento" e "banco de optativas", sobre fundo mais escuro que os cartões dos componentes. Clicar num componente destaca em laranja seus pré-requisitos e em roxo o que depende dele; o vermelho fica reservado a erros. Chaves: mostrar todas as ligações; destacar a cadeia mais longa; modo ligação (clicar no pré-requisito e depois no componente que o exige; repetir o par desfaz). Ligações que violam o art. 33, § 1º, aparecem em vermelho tracejado. As optativas ficam fora da grade, no banco, agrupadas pelo período a que pertencem, em ordem crescente do período 0 ao 10º, e com o período num rótulo em cada cartão. As colunas de períodos mostram só obrigatórios. No painel, o período da optativa aparece como informação, com o pré-requisito que o determina. O painel lateral de edição só aparece com um componente selecionado; sem seleção, a grade ocupa a largura toda.
- **Arrastar componentes**: obrigatórios e optativos podem ser arrastados para um período (vira obrigatório naquele período), para "obrigatórios sem período" ou para o banco de optativas (vira optativo). No celular, toca-se e segura antes de arrastar. O arrasto que colocaria o componente no mesmo período ou antes de um pré-requisito, ou no mesmo período ou depois de um componente que dele depende, é recusado com aviso (RGG, art. 33, § 1º). Atividades complementares e extensão por aproveitamento não se arrastam.
- **Extensão como categoria sobreposta**: a totalização segue o histórico escolar da UFPB (`catHist`): básicos profissionais (obrigatórios com `bas`), complementares obrigatórios, optativos e flexíveis (mais extensão por aproveitamento, só na versão de 18/09). A extensão não é categoria primária nem coluna dessas tabelas: aparece à parte, no quadro "Creditação de extensão" (exigida, fixa, pendente, teto) e na lista dos componentes que a incorporam. Na grade, cada cartão com extensão traz a etiqueta "extensão 60 h" (no bloco flexível, "até X h de extensão"), e o cabeçalho do período diz quanto de extensão ele inclui. Nos gráficos, a extensão é uma faixa sobreposta às categorias (`extSobreposta`), nunca um segmento somado a elas.
- **Como se monta a oferta de extensão** (aba Totais, quadro "Creditação de extensão"): tabela em ordem (obrigatórios, que todos cursam; optativos com extensão, à escolha; flexíveis, até `extTeto`), com o acumulado da oferta, uma minibarra com piso e teto e a marcação da linha em que o acumulado alcança o piso e passa do teto. Grupo sem componente com extensão aparece com linha em branco. O rodapé distingue oferta total, piso exigido de cada estudante e máximo que um estudante integraliza (limitado ao teto). O excedente sobre o teto é oferta: pode ser cursado, mas não integraliza como extensão.
- **Gráficos** (aba própria, `renderGraficos`): composição da carga total nas categorias do histórico, com a extensão em faixa sobreposta e o mínimo; extensão em quatro faixas (parte fixa, fixa + optativas, fixa + flexível, máximo) com piso e teto; percentuais de básicos, AC e prática jurídica e extensão fixa contra os limites; carga por período com o teto em sala; componentes que incorporam extensão; comparação entre as propostas da área de trabalho. Barras em HTML, sem biblioteca, com dica ao passar o mouse. Cores das séries em `--s1` a `--s4`, validadas para daltonismo nos dois temas.
- **O que mudou**: a cada alteração, `indicadores(R)` compara antes e depois (carga total, categorias, extensão fixa, piso, parte livre, caminhos, percentuais, duração, maior período em sala) e a faixa de avisos lista as diferenças, junto com os problemas novos.
- **Avisos**: a cada alteração, a função `problemas(R)` compara a situação anterior com a nova e mostra, numa faixa abaixo das abas, apenas os problemas novos: pré-requisito no mesmo período ou depois, ciclo, cadeia abaixo da duração mínima, período acima do teto em sala (diretriz do NDE), dependência de componente sem período ou não obrigatório, obrigatório sem período. Avisos resolvidos somem na alteração seguinte. Trocar, duplicar, excluir, restaurar ou importar proposta não gera aviso. Os avisos não substituem a aba Conformidade.
- **Tabela de componentes**: edição em linha, com colunas de CH teórica, CH prática, caixa "Ext.", CH de extensão, total, créditos, básico e art. 13.
- **Conformidade**: parâmetros da proposta e lista de verificações.
- **Totais**: categorias do histórico escolar da UFPB (obrigatórios, optativos, atividades complementares, extensão por aproveitamento) com decomposição e créditos; obrigatórios por eixo; carga por período; composição da extensão; composição do art. 13; comparação entre propostas, exibida quando houver mais de uma (com uma só, a aba orienta a duplicar).

Importação aceita o formato próprio (`{app: "simulador-ppc-dcj-santa-rita", versao: 1, propostas: [...]}`) e o JSON do simulador original (`{proposals: [{nome, componentes: [{nome, natureza, periodo, ch}]}]}`), que é convertido; nesse caso os pré-requisitos ficam vazios. Importar acrescenta propostas, nunca substitui as existentes.

A página abre sempre no visual claro e sóbrio (fundos quase brancos, cores dessaturadas), mesmo com o sistema em modo escuro. O visual escuro, de cores mais vivas, fica como alternativa no botão "Visual escuro" do cabeçalho; a escolha fica em `simulador-ppc-dcj-sr-v1-tema`. A página funciona em tela de celular sem rolagem horizontal da página (o quadro da grade tem rolagem própria) e está toda em português.

## 9. Valores de referência para conferência

Qualquer alteração no núcleo de cálculo deve reproduzir estes resultados com os dados de partida:

| Indicador | Grade de trabalho (16/10) | Versão 18/09 (memória) |
|---|---:|---:|
| Obrigatórios + optativas + flexíveis/AC + aproveitamento | 3.240 + 240 + 240 + 0 | 3.150 + 240 + 120 + 288 |
| Carga total | 3.720 h | 3.798 h |
| Extensão fixa | 180 h | 468 h |
| Piso / teto de extensão | 372 / 558 h | 380 / 569 h |
| Parte livre | 192 h | 0 |
| Fecha só por optativas / só pelo flexível (X = 195) | sim / sim | dispensa |
| Com X = 120 | sim / não (faltam 72 h) | — |
| Básicos profissionais | 56,5% | 53,7% |
| AC e prática jurídica | 15,3% | 19,4% |
| Cadeia mais longa / simulação | 10 / 10 | 10 / 10 |
| Elos sensíveis | 3 | 5 |
| Matrícula mínima / máxima | 240 / 420 h | 255 / 405 h |

Estado montado sobre a grade de trabalho, para conferência: com a UCE-OB III arrastada para o banco, a carga cai a 3.660 h, a parte fixa a 120 h e nenhum caminho fecha sozinho o piso (aviso). Elevando as optativas a 300 h, a carga volta a 3.720 h, a parte livre fica em 252 h e o caminho das optativas fecha, porque a UCE-OB III passa a integrar a oferta de optativas com extensão.

A versão de 18/09 segue o documento: Sociologia Jurídica e Direito Previdenciário com esses nomes; Serviço de Assessoria I no 6º período, com pré-requisito em Teoria Geral do Processo, e Serviço II no 7º; UCE-OB III dependente de Processual Civil I e Processual Penal I; Trabalho de Curso como atividade de orientação coletiva, sem período, no bloco de creditação; nenhuma optativa cadastrada, porque o documento não as lista. Os pré-requisitos de UCE-OB I e II e os demais que o documento não detalha vêm da versão original dos dados.

Na proposta de trabalho, as duas cadeias de dez períodos são: Introdução à Teoria do Direito I e II, Direito Civil I e II, Teoria Geral do Processo, Mediação e Arbitragem, Laboratórios I a IV; e Introdução I e II, Direito Civil I a VI, Direito da Criança e do Adolescente, Direito da Seguridade Social. Os três elos sensíveis são os pré-requisitos que ligam Introdução I a Civil II, comuns às duas cadeias.

## 10. Limitações conhecidas

- Os pontos de controle ficam no navegador de cada usuário; limpar os dados do navegador os apaga. Para guardar fora do navegador, use Exportar.
- Os dados ficam no navegador de cada usuário. Não há sincronização nem edição compartilhada; o compartilhamento se faz por exportação e importação de arquivo.
- O aplicativo não trata equivalências nem transição do currículo de 2019. Esse levantamento está na nota técnica.
- Os pré-requisitos dos dados de partida são sugestões, a validar com os professores de cada área. Os marcados como deduzidos não têm indicação expressa em documento.
- A extensão máxima depende das UCE optativas cadastradas no banco. Com o banco atual, a trajetória máxima coincide com a mínima.
- A verificação de componentes numerados depende do nome terminar em algarismo romano.
- A simulação do estudante mais adiantado usa um critério guloso. Ela confirma a duração mínima, mas não prova que nenhuma outra ordem de matrícula seria mais rápida; a prova está na profundidade da cadeia.
- O Trabalho de Curso está no 10º período. Como atividade de orientação individual, com pré-requisito apenas em Pesquisa Aplicada ao Direito (8º), o estudante pode antecipá-lo; a grade mostra o lugar previsto.
- Medicina Legal aparece como obrigatória no 10º período porque não houve deliberação, mas é provável que passe a optativa. As duas substitutas estudadas, Criminologia e Política Criminal e Leis Penais Especiais, estão no banco de optativas; para simular a troca, basta mudar o tipo e o período.

## 11. Pendências normativas que afetam o aplicativo

- A contagem da extensão dentro da categoria do componente está confirmada pela Res. CONSEPE 02/2022, pelo Manual da Creditação PROEX/PRG, pela Res. CONSEPE 26/2023 e pelos históricos do SIGAA. A antiga pendência sobre o registro no histórico foi retirada da tela.
- Ficam a decidir (etiqueta "a decidir"): Medicina Legal como optativa, o obrigatório Criminologia, Política Criminal e Leis Penais Especiais (nome provisório), Libras, as duas UCE optativas a criar e o bloco flexível com o valor de X. A proposta usa 240 h de optativas; o currículo atual, 180 h.

- A vedação de dupla contagem entre extensão e prática jurídica é citada na nota como IN PRG-PROEX 02/2024, art. 4º, IV. O texto dessa instrução normativa não foi conferido. Por isso o aplicativo apoia a regra apenas no Ementário 2026 e a classifica como diretriz. Não altere essa classificação sem o texto da norma.
- A tipologia dos componentes curriculares está no RGG, art. 16, § 1º (disciplina, módulo, bloco, atividade de orientação individual e coletiva); o regime das atividades de orientação, nos arts. 38, § 3º, 49 e 50. O aplicativo não oferece a natureza "bloco".

## 12. Orientações para alterações futuras

- Trate o usuário como especialista em Direito e não em programação. Explique o que mudou na tela, não no código.
- Nunca altere referência normativa sem conferir o texto da norma. Na dúvida, pergunte ao usuário, que tem o acervo normativo.
- Mantenha a distinção entre exigência de norma e diretriz do NDE.
- Mantenha o arquivo único e autocontido, sem etapa de build.
- Ao mudar o núcleo de cálculo, confira os valores da seção 9.
- Toda alteração de pré-requisito nos dados de partida deve ser verificada quanto à duração mínima antes de publicada.
