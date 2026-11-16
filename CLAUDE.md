# Instruções para o Claude Code

Antes de qualquer alteração, leia `BRIEFING-CLAUDE-CODE.md` inteiro. Ele descreve a finalidade do simulador, o modelo de dados, as regras de cálculo, as verificações e o que precisa ser preservado.

Pontos que valem para toda tarefa:

- O aplicativo é o arquivo único `index.html`, autocontido, sem etapa de build.
- O núcleo de cálculo (`calc(P)` e `verificacoes(P, R)`) não acessa o DOM. Ao alterá-lo, confira os valores de referência da seção 8 do briefing.
- Mantenha a distinção entre exigência normativa e diretriz do NDE. Não altere referência normativa sem conferir o texto da norma com o usuário.
- As propostas são de trabalho e nenhuma é a versão final; a estrutura segue em deliberação, e várias questões já foram decididas (não escreva que nada foi deliberado). A grade de 16/10 está oficialmente desatualizada. Isso deve continuar claro nos textos.
- O usuário é professor de Direito, sem formação em programação. Explique o que mudou na tela, não no código. Escreva em português.
