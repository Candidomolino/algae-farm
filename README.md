# Algae Farm — campanha 1.0.1

Jogo de exploração livre para uma aula de Engenharia Bioquímica. Após uma breve apresentação, os alunos cultivam microalgas, escolhem rotas de biodiesel ou bioplástico e comparam decisões, composição e uso de recursos. O padrão não tem cronômetro. A fazenda mantém a apresentação diurna, as animações de nuvens e culturas e os controles para celular.

## Jogar e escolher a versão

- Campanha atual: https://candidomolino.github.io/algae-farm/
- Revisão de continuidade 1.0.1: https://candidomolino.github.io/algae-farm/versions/v1.0.1/
- Marco final 1.0.0 preservado: https://candidomolino.github.io/algae-farm/versions/v1.0.0/
- Integração inicial 1.0.0-rc.1: https://candidomolino.github.io/algae-farm/versions/v1.0.0-rc.1/
- Revisão de uso 1.0.0-rc.2: https://candidomolino.github.io/algae-farm/versions/v1.0.0-rc.2/
- Referência clássica 0.8.1: https://candidomolino.github.io/algae-farm/versions/v0.8.1/
- Referência clássica fixa para os cartões anteriores: https://candidomolino.github.io/algae-farm/versions/v0.8.1/?cenario=fixo

A campanha 1.0.0 usa o modelo 0.5.0 e o registro da fazenda v2. Os PDFs, as tabelas originais e os guias anteriores de progressão/espécies descrevem a versão clássica; não representam a nova economia ou seus 18 turnos. O registro de publicação e os limites da verificação ficam em `classroom/release.json` e `docs/verification.md`.

## Uma fazenda que continua entre lotes

Uma nova campanha recebe 120 moedas uma única vez, 55 de energia e 60 de água. Operações e equipamentos usam o mesmo saldo. Moedas e estoques restantes passam ao lote seguinte. Cada entrega renova até 25 de energia e 12 de água nova, respeitando as capacidades de 55 de energia e 60 de água total. Água recuperada entra no estoque real e é consumida antes da água nova. Cada lote dispõe de 18 turnos de trabalho; reposições e análises também gastam trabalho.

A receita é `piso(80 × mínimo(produto/meta, 1,5))`, até 120 moedas. Produto zero e ausência de entrega não pagam. O relatório separa custo, receita e lucro. Abandonar conserva os gastos e não concede renovação. Equipamentos custam 20, 40 e 70 por nível/categoria; compras entre lotes preservam 60 moedas de operação e ficam bloqueadas se houver dívida de apoio da cooperativa.

Comece com *Chlorella vulgaris*. *Nannochloropsis gaditana* exige 2 lotes vendidos; *Tetraselmis subcordiformis*, 3 lotes e fazenda nível 3; *Chlamydomonas pacifica*, 4 lotes e nível 4. O manejo alcalino de C. pacifica protege contra os eventos comuns de contaminação do modelo, com custo adicional. As probabilidades e os rendimentos são fictícios; isso não implica imunidade universal na natureza.

Respostas corretas deixam benefícios específicos de uso único para escolhas futuras. Analisar o rio custa 5 moedas e 1 turno, com uma oportunidade por ciclo concluído. A cepa Chlorella Rio acrescenta 12% à biomassa de lotes futuros da espécie. O guia completo está em `classroom/campaign-guide-v1.md`.

A moldura de recursos permanece disponível durante as decisões e mostra saldos em relação às capacidades. Os custos dos cartões aparecem em relação ao disponível. Amido é apresentado como componente intracelular que pode ser separado e usado como insumo para placas, em vez de um estoque externo de recurso operacional.

## Continuidade e cenários

A campanha salva o registro v02 separado do v01 clássico. A migração lê o histórico validado, preserva moedas, compras e desbloqueios e acrescenta o capital operacional uma única vez. O v01 original fica intacto. Um lote antigo em andamento pode terminar sob suas regras originais, sem duplicar o pagamento. Novos lotes seguem a campanha.

Clima, condição de operação e contaminação são definidos pelo cenário. O código numérico permite repetir os eventos, sem novo sorteio ao recarregar. Compare também espécie, pH, cepa, estoques, equipamentos e benefícios de conhecimento: um código igual não torna todos esses fatores iguais. O cenário fixo da campanha remove contaminação e conserva o clima de referência, mas não reproduz os 14 turnos ou o orçamento renovado da versão clássica.

O botão Professor prepara links. `?modo=direto` abre decisões sem caminhar; `?cenario=fixo` usa o cenário fixo da versão acessada. Rodadas cronometradas continuam opcionais com `?minutos=6` ou `?minutos=10` explícitos. O recomendado para a aula é manter a exploração livre.

## Materiais

- `classroom/campaign-guide-v1.md`: regras atuais, exemplos de saldo/estoques, pH, riscos, benefícios, exploração e migração.
- `docs/contratos-e-modelo.md`: resumo do contrato atual e referência clássica claramente arquivada.
- `classroom/Algae_Farm_Abertura.pdf` e `Algae_Farm_Kit_Piloto.pdf`: materiais anteriores; as rotas numéricas do kit pertencem ao arquivo 0.8.1.
- `classroom/progression-guide.md`, `species-guide.md`, `pilot-controls.md` e exemplos antigos de sessão: documentação histórica das versões indicadas em cada arquivo.
- `classroom/release.json` e `docs/verification.md`: publicação, verificações e limites.
- `docs/original-plan.md`: plano original recebido por email, preservado como histórico.

O QR principal mantém o endereço da campanha. Para comparar com os cartões antigos, use o link do arquivo clássico acima e informe essa diferença aos alunos.

## Código, arte e manutenção

O cálculo atual fica em `src/model.mjs`; a economia e seu histórico em `src/progression.mjs`; os perfis em `src/species.mjs`; aprendizagem e relatórios em `src/learning.mjs` e `src/session.mjs`. `src/legacy-model.mjs` e `src/legacy-progression.mjs` preservam a reconstrução das versões anteriores. O mapa e os controles ficam em `src/world.mjs`; o desenho e a animação em `src/scene.mjs`; a interface em `src/app.mjs`, `src/index.html`, `src/style.css` e `src/icons.mjs`.

Com Node instalado, execute `node tools/test.mjs` e `node tools/build.mjs`. `node tools/balance.mjs` enumera a referência clássica; os testes de campanha verificam os novos estoques, transações e riscos. A construção gera `dist/index.html`, com código e imagens incorporados e sem bibliotecas, fontes ou imagens externas. Para testar localmente, use `python -m http.server 8766 --directory dist` e abra `http://localhost:8766`. Os dados ficam neste navegador; o salvamento depende do armazenamento local. Quando indisponível, o jogo continua na memória da aba.

As fontes editáveis estão em `art/custom` (Piskel) e `art/maps` (Tiled). O ciclo do pesquisador foi conferido no Piskel; os mapas foram exportados e renderizados com Tiled 1.12.2. Consulte `art/README.md`. O pacote público `dist/Algae_Farm_Art_Source.zip` inclui fontes visuais e licenças.

## Publicação e créditos

O jogo é publicado no GitHub Pages pelo repositório https://github.com/Candidomolino/algae-farm, na raiz da branch `main`, com HTTPS. O arquivo clássico fica em `versions/v0.8.1/`. Notas docentes, email e planejamento locais não integram automaticamente o repositório público. O código incorporado ao HTML é visível no navegador.

Terreno, árvores, cercas e pequenos objetos: Kenney Tiny Town e Tiny Farm, CC0, com licenças originais preservadas. Pesquisador, aeradores, equipamentos, edifícios e interface são arte original do projeto. A marca EEL/USP vem da identidade visual oficial da biblioteca, tem sua própria referência em `assets/institution/REFERENCE.txt` e não integra os recursos CC0. Não foram usados recursos pagos.

Todos os perfis, probabilidades, perdas, moedas e rendimentos são parâmetros de ensino. O jogo não prevê produtividade industrial, emissões evitadas, qualidade certificada ou fim de vida do material. As referências científicas e os limites da adaptação estão no guia atual. O piloto com alunos, aparelhos e rede da sala continua parte da revisão docente.

A revisão 1.0.1 esclarece a retomada de lotes clássicos: a água recuperada antiga continua como indicador histórico, sem entrada no estoque novo, e a revisão antiga não promete benefícios que só passam a valer nas entregas da campanha. O modelo 0.5.0 e os cálculos da campanha permanecem iguais.
