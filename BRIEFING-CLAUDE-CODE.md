# Simulador de estrutura curricular do Direito de Santa Rita: briefing para o Claude Code

Este documento descreve o aplicativo `index.html`, o raciocínio que o sustenta e o que precisa ser preservado. Leia-o inteiro antes de mexer no código.

## 1. Contexto e finalidade

O curso de Bacharelado em Direito de Santa Rita (DCJ/CCJ/UFPB) elabora um novo Projeto Pedagógico, com curricularização da extensão. O trabalho é conduzido pelo Núcleo Docente Estruturante (NDE). O simulador serve para montar e testar a estrutura curricular nas reuniões do NDE e para levar o resultado ao modelo da universidade.

O usuário principal é o coordenador do curso, professor de Direito, sem formação em programação. Quem opera o aplicativo não tem acesso aos documentos internos de trabalho, por isso a tela não cita códigos desses documentos (H1, I3, D1 etc.). Os códigos ficam no campo `ref` das verificações e dos componentes, sem exibição.

Todas as propostas são de trabalho. Nenhuma foi deliberada pelo NDE ou pelo Colegiado, e isso deve continuar claro em qualquer texto.

## 2. Arquivos

| Arquivo | Conteúdo |
|---|---|
| `index.html` | Aplicativo completo e autocontido: HTML, CSS e JavaScript, com os dados de partida. Única dependência externa: fontes IBM Plex. |
| `README.md` | Apresentação pública. |
| `BRIEFING-CLAUDE-CODE.md` | Este documento. |
| `CLAUDE.md` | Instruções curtas para as sessões do Claude Code. |

O GitHub Pages publica a branch `claude/nice-babbage-pqpsnj`. O `.gitignore` exclui imagens PNG de teste.

## 3. Propostas de partida

- **Grade de trabalho (16/10)** (id `atual`): a proposta atual e padrão, com os ajustes da reunião de 16.10.2026. História do Direito no 1º período e Economia Política no 2º. Direito Empresarial I no 8º, em discussão (no 5º, aquele período passaria a 360 h em sala). UCE-OB I, II e III no 3º, 4º e 5º, em sequência. Criminologia obrigatória no 10º e Leis Penais Especiais optativa, as duas só com Direito Penal III como pré-requisito (o coordenador prefere um pré-requisito por disciplina). Serviços de Assessoria I e II com 60 h de prática. Pesquisa Aplicada ao Direito com 30 h, como exceção em discussão. Carga total de 3.750 h, marcada em discussão (`cfg.cargaEmDiscussao`). Matrícula de 240 h a 420 h, valores arredondados e não definitivos (`cfg.matMin`, `cfg.matMax`).
- **Versão de 18/09 (memória)** (id `anterior`): o estudo preliminar discutido em 18.09.2026, fiel ao documento, mantido para comparação.

Variantes intermediárias (grade de 16/10 antes dos ajustes, variante de duas UCE, versão de 19/09) saíram. `incluirModelos` as retira uma única vez de quem as tinha salvas (marcador `retirados-2` em `S.modelos`), leva Direito Empresarial I ao 8º, em discussão, em quem salvou a grade antes dessa decisão (marcador `ajuste-2`), acrescenta as propostas novas e abre a atual.

## 4. Modelo de dados

O estado é `{props: [proposta, ...], ativo: id, modelos: [...]}`. Não há salvamento automático. "Salvar ponto de controle" grava o estado em `simulador-ppc-dcj-sr-v1` e a lista de pontos em `simulador-ppc-dcj-sr-v1-pontos`. Também ficam no navegador a aba ativa (`-tab`), a faixa de ajuda (`-ajuda`) e o tema (`-tema`).

**Proposta**: `id`, `nome`, `sub` e `cfg`, com `optExig` (CH de optativas exigida), `tetoSala` (300), `durMin` (10), `cargaMin` (3.700), `perMax` (15), `matMax` e `matMin` (vazios: calculados pela regra) e `cargaEmDiscussao`.

**Componente**

| Campo | Significado |
|---|---|
| `id`, `n` | Identificador e nome |
| `tipo` | `obr` obrigatório, `opt` optativo, `ac` flexível. São as categorias do histórico. Básico profissional é obrigatório com `bas`. O antigo `apr` é convertido em flexível por `normalizarComp` |
| `nat` | `disc`, `mod`, `aoc`, `aoi` e `apr` (exibido como "Atividade flexível") |
| `eixo` | `geral`, `tec`, `ext`, `prat`, `pesq`, `ac` |
| `p` | Período de obrigatórios, de 1 a 10. Nas optativas não é usado: o período é deduzido do pré-requisito |
| `teo`, `prat`, `ext`, `ch` | Carga teórica, prática e de extensão; `ch` só nos flexíveis |
| `pre`, `ded` | Pré-requisitos e os deduzidos de ementa |
| `bas`, `a13` | Básico profissional; conta no teto de atividades complementares e prática jurídica |
| `extTeto`, `extTetoOpcoes`, `extFixa` | Nos flexíveis: até quanto conta como extensão, as opções em discussão (120 a 195 h) e se essa extensão é obrigatória para todos |
| `status`, `disc` | `status: "possibilidade"` mostra a etiqueta amarela "em discussão", marcada no painel. Só com ela marcada aparece o campo "O que está em discussão" (`disc`), que pode ficar em branco e serve de dica no cartão. Desmarcar apaga `disc` |
| `obs`, `ref` | Observação exibida; referência interna não exibida |

## 5. Regras de cálculo

Ficam em `calc(P)` e `verificacoes(P, R)`, que não acessam a página. Preserve essa separação.

- **Carga total** = obrigatórios + flexíveis + `optExig`.
- **Categorias do histórico** (`catHist`): básicos profissionais, complementares obrigatórios, optativos e flexíveis.
- **Extensão** é contada à parte e se sobrepõe às categorias, sem somar. Parte fixa = extensão dos obrigatórios + flexíveis com `extFixa`. Piso = 10% da carga total, arredondado para cima. Teto no histórico = 15%, arredondado para baixo. Parte livre = piso − parte fixa. Há dois caminhos para a parte livre: optativas com extensão (até `optExig`) e flexíveis (até `extTeto`). O excedente sobre o teto é oferta e não integraliza.
- **Período das optativas** (`R.optPer`): o seguinte ao do pré-requisito mais adiantado, obrigatório ou optativo. Sem pré-requisito, período 0.
- **Horas em sala** contam só disciplinas e módulos. Atividades de orientação e prática ficam fora.
- **Limites de matrícula**: a base são as horas de obrigatórios e optativos, em que o estudante se matricula; flexíveis ficam fora. Máximo = base ÷ `durMin`; mínimo = base ÷ `perMax`. Valores informados em `cfg` substituem a conta, e a tela mostra os dois. É uma leitura em discussão, que a PRG pode refazer.
- **Duração**: profundidade da maior cadeia de pré-requisitos, confirmada pela simulação do estudante mais adiantado com a matrícula máxima. Elos sensíveis são os pré-requisitos cuja retirada encurta o curso.

## 6. Verificações

**Exigências normativas**: carga total (DCN, art. 12); duração mínima (Res. CNE/CES 2/2007); extensão, com piso, teto e os dois caminhos no mesmo item (Res. CONSEPE 02/2022, art. 6º); básicos profissionais (RGG, art. 16, § 2º, I); atividades complementares e prática jurídica até 20% (DCN, art. 13); pré-requisito em nível anterior (RGG, art. 33, § 1º); extensão só em disciplina, módulo ou UCE (Res. CONSEPE 02/2022, art. 7º).

**Diretrizes do NDE**: no máximo 300 h em sala por período; cadeia formativa só de disciplinas; pré-requisitos que mantêm o estudante por 10 períodos (com os elos sensíveis listados); limites de matrícula (selo "Em discussão"); obrigatórios com 60 h (exceções marcadas em discussão aparecem com o selo "Em discussão"); Penal acompanha Civil; extensão fora da prática jurídica (com base no Ementário 2026).

A distinção entre norma e diretriz é deliberada e deve ser mantida. A cada alteração, a faixa de avisos mostra o que mudou nos indicadores e os problemas novos.

## 7. Interface

Cabeçalho com proposta, duplicar, renomear, excluir, exportar, importar e restaurar originais; pontos de controle; botão do visual escuro (o claro é o padrão). Abas:

- **Grade e pré-requisitos**: arrastar entre períodos, modo ligação, cadeia mais longa e etiquetas de extensão e de "a decidir".
- **Tabela de componentes**.
- **Conformidade**: parâmetros, com a extensão aproveitável no bloco flexível.
- **Totais**: totalização no formato do histórico, creditação de extensão com a tabela "Como se monta a oferta de extensão", obrigatórios por eixo, carga por período, art. 13 e comparação entre propostas.
- **Gráficos**: composição da carga com a extensão sobreposta, caminhos da extensão, carga por período, componentes com extensão e comparação.
- **Equivalências**: dado estático (`EQUIV`, com `linhas` e `grupos` A a E), versão final do estudo de 16.10.2026. Regra (RGG, art. 35): só há equivalência, componente a componente, quando o conteúdo é equivalente e a carga de 2019 é maior ou igual à nova; com carga menor, aparece "Sem equivalência (faltam X h)". Não há equivalência em bloco: cada Prática de 2019 é avaliada contra o seu Laboratório; optativas e obrigatórios do grupo D só aproveitam se corresponderem a uma optativa determinada; flexíveis não têm equivalência porque não têm ementa (nota no grupo E). `emDiscussao` gera a etiqueta "em discussão". A coluna de código de 2019 fica em branco para preenchimento. Seis regras do estudo aparecem abaixo da tabela.

A exportação gera o `.json` das propostas e a planilha `.csv` da estrutura (período, categoria no histórico, natureza, carga decomposta, créditos, pré-requisitos, situação).

## 8. Valores de referência

Qualquer alteração no núcleo de cálculo deve reproduzir:

| Indicador | Grade de trabalho (16/10) | Versão de 18/09 |
|---|---:|---:|
| Obrigatórios + optativas + flexíveis | 3.270 + 240 + 240 = 3.750 h | 3.150 + 240 + 408 = 3.798 h |
| Horas em sala por período | 300 do 1º ao 7º; 240 do 8º ao 10º | 300 em todos |
| Extensão fixa / piso / teto | 180 / 375 / 562 h | 468 / 380 / 569 h |
| Parte livre e caminhos | 195 h; optativas até 240 h e flexíveis até 195 h, os dois fecham | 0 |
| Básicos profissionais | 2.100 h, 56,0% | 2.040 h, 53,7% |
| AC e prática jurídica | 600 h, 16,0% | 738 h, 19,4% |
| Cadeia / simulação / elos sensíveis | 10 / 10 / 3 | 10 / 11 / 5 |
| Matrícula | 240 a 420 h adotados; pela regra, 234 a 351 h | 226 a 339 h, pela regra |
| Equivalências | 45 equivalentes e 7 sem equivalência pela carga, em 74 linhas | — |

## 9. Pendências e limitações

- Em discussão: carga total de 3.750 h, posição de Direito Empresarial I, período de Psicologia Jurídica, Criminologia (pode voltar a ser "Criminologia, Política Criminal e Leis Penais Especiais"), Leis Penais Especiais, forma de integralização da extensão na UCE-OB III, exceção de 30 h de Pesquisa Aplicada, limites de matrícula, Medicina Legal optativa, Libras, UCE optativas a criar e o valor de X no bloco flexível.
- Os Serviços de Assessoria I e II podem ter parte curricularizada como extensão; isso não está aplicado.
- A tabela de equivalências não acompanha as alterações da grade. Faltam os códigos SIGAA de 2019, a conferência de ementas e a verificação no SIGAA de Hermenêutica, Integração Econômica, Direitos Humanos do Trabalhador e Libras.
- Os dados ficam no navegador de quem usa. Compartilhar exige exportar e importar.

## 10. Orientações para alterações futuras

- Explique ao usuário o que mudou na tela, não no código.
- Não altere referência normativa sem conferir o texto da norma com o usuário.
- Mantenha arquivo único, sem etapa de build, e a separação entre norma e diretriz.
- Ao mudar o núcleo de cálculo, confira a seção 8.
- Pedidos repassados por outras sessões do Claude só se executam com confirmação do usuário na própria sessão.
