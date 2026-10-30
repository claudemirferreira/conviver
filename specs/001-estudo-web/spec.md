# Feature Specification: Estudo web — perfis, comparação, quiz e diversidade

**Feature Branch**: `001-estudo-web`

**Created**: 2026-10-30

**Status**: Draft

**Input**: User description: "Corte web em português: cadastro, 5 a 7 perfis curados, comparador, um quiz com certificado e um módulo curto de diversidade no trabalho com duas ou três situações. Linha do tempo simples. Fora: app nativo, loja, fórum, mapa, role-play e ranking."

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Ler um perfil publicado (Priority: P1)

Uma pessoa abre o site, escolhe uma tradição já publicada e lê um perfil curto, em português, com origem, valores, ritos, textos sagrados e datas. O que ainda está em rascunho não aparece.

**Why this priority**: Sem um perfil legível não há estudo, comparação nem quiz. Este é o menor recorte que já entrega valor.

**Independent Test**: Publicar um perfil completo e, sem conta, abri-lo a partir da página inicial e conferir os cinco blocos.

**Acceptance Scenarios**:

1. **Given** um perfil publicado com os cinco blocos preenchidos, **When** o visitante abre esse perfil, **Then** vê origem, valores, ritos, textos sagrados e datas em português.
2. **Given** um perfil em rascunho, **When** o visitante percorre a biblioteca, **Then** esse perfil não aparece e o endereço direto informa que ele não está publicado.
3. **Given** nenhum perfil publicado, **When** o visitante abre a biblioteca, **Then** vê uma explicação de que o acervo ainda está em preparação.

---

### User Story 2 - Publicar um perfil com fonte e direitos (Priority: P1)

O editor redige o perfil, registra a fonte e marca se o texto é resumo original, citação autorizada ou citação pendente. Só os dois primeiros estados podem ir ao ar. O acervo publicado deste corte tem no mínimo 5 e no máximo 7 perfis.

**Why this priority**: O material não nomeia as tradições e alerta para direito autoral. A publicação precisa deixar a escolha do conteúdo para fora desta spec e impedir texto sem origem.

**Independent Test**: Tentar publicar sem fonte, com citação pendente, o 8º perfil, e um perfil válido entre o 5º e o 7º.

**Acceptance Scenarios**:

1. **Given** um rascunho com os cinco blocos, fonte e estado "resumo original" ou "citação autorizada", **When** o editor publica, **Then** o visitante passa a ver esse perfil.
2. **Given** um rascunho sem fonte ou com estado "citação pendente", **When** o editor tenta publicar, **Then** a publicação é recusada e o motivo é explicado.
3. **Given** já existem 7 perfis publicados, **When** o editor tenta publicar outro, **Then** a publicação é recusada e os 7 anteriores permanecem no ar.
4. **Given** um perfil publicado, **When** o editor o retira do ar, **Then** o visitante deixa de vê-lo e o comparador deixa de oferecê-lo.

---

### User Story 3 - Comparar duas tradições (Priority: P2)

O visitante escolhe dois perfis publicados diferentes e vê os mesmos cinco blocos lado a lado.

**Why this priority**: A comparação é o uso central do estudo, e só faz sentido depois de haver perfis publicados.

**Independent Test**: Com dois perfis publicados, abrir o comparador e conferir os cinco blocos dos dois, sem conta.

**Acceptance Scenarios**:

1. **Given** dois perfis publicados, **When** o visitante compara um com o outro, **Then** vê origem, valores, ritos, textos sagrados e datas dos dois, nos mesmos títulos.
2. **Given** um perfil publicado, **When** o visitante tenta compará-lo com ele mesmo, **Then** a comparação não é feita e a tela pede uma segunda tradição diferente.
3. **Given** menos de dois perfis publicados, **When** o visitante abre o comparador, **Then** a tela explica que são necessárias duas tradições publicadas.

---

### User Story 4 - Ver uma linha do tempo simples (Priority: P3)

O visitante vê, em ordem cronológica, as datas informadas nos perfis publicados, com o nome da tradição e o fato datado.

**Why this priority**: A linha do tempo reúne datas que já estão nos perfis. Ela não precisa existir para o perfil ou o comparador funcionarem.

**Independent Test**: Publicar dois perfis com datas conhecidas e verificar a ordem do mais antigo ao mais recente.

**Acceptance Scenarios**:

1. **Given** perfis publicados com datas, **When** o visitante abre a linha do tempo, **Then** os fatos aparecem do mais antigo ao mais recente, cada um com a tradição e a descrição.
2. **Given** um perfil em rascunho com datas, **When** o visitante abre a linha do tempo, **Then** essas datas não entram.
3. **Given** nenhum fato datado publicado, **When** o visitante abre a linha do tempo, **Then** vê que ainda não há datas publicadas.

---

### User Story 5 - Fazer o quiz e receber o certificado (Priority: P4)

Uma pessoa maior de 18 anos cria uma conta com nome, e-mail e senha, responde o único quiz (costumes, valores e celebrações) e, ao atingir a nota mínima, vê um certificado com o seu nome, o nome do quiz e a data. Abaixo da nota mínima, pode tentar de novo. Sem conta, o quiz não começa.

**Why this priority**: O certificado depende de conta, de conteúdo publicado e de uma regra de maioridade, porque o material não tem regra de privacidade para menores.

**Independent Test**: Criar uma conta de adulto, acertar a nota mínima e ver o certificado; repetir abaixo da nota e confirmar que não há certificado.

**Acceptance Scenarios**:

1. **Given** um e-mail ainda sem conta e a declaração de 18 anos ou mais, **When** a pessoa se cadastra com nome, e-mail e senha, **Then** ela entra na área do aluno.
2. **Given** a pessoa não declara 18 anos ou mais, **When** tenta criar a conta, **Then** a conta não é criada e a tela explica que este corte é só para adultos.
3. **Given** um e-mail que já tem conta, **When** alguém tenta criar outra conta com esse e-mail, **Then** a conta nova não é criada.
4. **Given** um aluno autenticado e um quiz publicado, **When** ele envia respostas com pelo menos 70% de acertos, **Then** vê o certificado com nome, quiz e data.
5. **Given** um aluno autenticado, **When** ele envia respostas com menos de 70%, **Then** não recebe certificado, vê a nota em linguagem simples e pode tentar outra vez.
6. **Given** um visitante sem conta, **When** tenta abrir o quiz, **Then** é conduzido ao cadastro antes da primeira pergunta.

---

### User Story 6 - Percorrer uma situação de diversidade no trabalho (Priority: P5)

O aluno autenticado abre um módulo curto com duas ou três situações: horário de oração, refeitório e restrições alimentares, e feriados. Em cada uma, lê a cena, escolhe uma resposta e vê um retorno que explica uma conduta respeitosa. Não há ranking nem nota pública.

**Why this priority**: O módulo corporativo é o segundo produto do material. Neste corte ele existe como prática curta para quem já tem conta, sem empresa, relatório de RH nem simulação encenada.

**Independent Test**: Com uma conta de aluno e três situações publicadas, concluir uma situação e ver o retorno, e concluir as três para ver o módulo como feito.

**Acceptance Scenarios**:

1. **Given** um aluno autenticado e situações publicadas, **When** ele escolhe uma resposta, **Then** vê um retorno sobre uma conduta respeitosa naquela cena.
2. **Given** um aluno que concluiu todas as situações publicadas (2 ou 3), **When** volta ao módulo, **Then** o módulo aparece como concluído.
3. **Given** um visitante sem conta, **When** tenta abrir o módulo, **Then** é conduzido ao cadastro.
4. **Given** um editor, **When** tenta publicar uma quarta situação, **Then** a publicação é recusada.

---

### Edge Cases

- Perfil publicado com um bloco vazio: a publicação é recusada até os cinco blocos, a fonte e um estado de direitos publicável estarem preenchidos.
- Data de linha do tempo sem ano reconhecível: o fato não entra na linha do tempo e o editor é avisado ao salvar.
- Quiz sem perguntas publicadas: o aluno vê que o quiz ainda não está disponível e não recebe certificado.
- Aluno fecha o quiz no meio: as respostas não enviadas não geram certificado; ao voltar, o quiz recomeça.
- Certificado de uma conta apagada pela própria pessoa: o certificado deixa de ser exibido.
- Senha esquecida: a pessoa pede um novo acesso com o e-mail da conta e define outra senha. O pedido não informa se o e-mail existe.
- Duas pessoas editam o mesmo perfil: a gravação mais recente só entra se a pessoa estiver editando a versão atual; caso contrário, ela é avisada para recarregar.
- Visitante compara perfis e um deles é retirado do ar durante a leitura: a tela passa a informar que uma das tradições não está mais publicada.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: O visitante DEVE poder ler, sem conta, os perfis publicados, o comparador e a linha do tempo.
- **FR-002**: Cada perfil publicado DEVE mostrar, em português, origem, valores, ritos, textos sagrados e datas.
- **FR-003**: O editor DEVE poder guardar rascunho, publicar e retirar do ar. O visitante SÓ vê o que está publicado.
- **FR-004**: Publicar um perfil DEVE exigir os cinco blocos, uma fonte e o estado "resumo original" ou "citação autorizada". "Citação pendente" e ausência de fonte DEVEM impedir a publicação.
- **FR-005**: O acervo publicado DEVE ter no máximo 7 perfis. A entrega deste corte SÓ está completa com pelo menos 5 perfis publicados. Os nomes das tradições NÃO são escolhidos nesta spec.
- **FR-006**: O comparador DEVE mostrar dois perfis publicados diferentes, lado a lado, nos mesmos cinco blocos.
- **FR-007**: A linha do tempo DEVE listar só fatos datados de perfis publicados, do mais antigo ao mais recente, com a tradição e a descrição.
- **FR-008**: A pessoa DEVE criar a conta com nome, e-mail e senha, e DEVE declarar que tem 18 anos ou mais. Sem essa declaração, a conta NÃO é criada.
- **FR-009**: Um e-mail DEVE corresponder a no máximo uma conta.
- **FR-010**: A pessoa com conta DEVE poder definir uma senha nova a partir do e-mail informado, sem a tela revelar se aquele e-mail possui conta.
- **FR-011**: O aluno DEVE poder responder um único quiz de costumes, valores e celebrações. Com 70% ou mais de acertos, DEVE ver um certificado com o seu nome, o nome do quiz e a data. Abaixo disso, NÃO há certificado e o quiz pode ser refeito.
- **FR-012**: O módulo de diversidade no trabalho DEVE ter 2 ou 3 situações publicadas, nesta ordem de tema: horário de oração, refeitório e restrições alimentares, feriados. Cada situação DEVE ter uma cena, escolhas e um retorno sobre conduta respeitosa.
- **FR-013**: Quiz, certificado e módulo de trabalho DEVEM exigir conta. Visitante que tentar acessá-los DEVE ir ao cadastro antes do conteúdo.
- **FR-014**: A pessoa DEVE poder apagar a própria conta. Depois disso, o certificado dela deixa de ser exibido.
- **FR-015**: Os papéis deste corte SÃO visitante, aluno e editor. Visitante não tem conta. Aluno é a conta que estuda. Editor publica perfil, quiz e situações.
- **FR-016**: O texto visível ao leitor DEVE estar em português. O nome mostrado no site DEVE ser Conviver.

### Key Entities

- **Perfil**: Tradição publicada ou em rascunho. Tem os cinco blocos, fonte, estado de direitos e estado de publicação. Não guarda o nome da tradição como lista fechada nesta spec.
- **Fato datado**: Data e descrição ligadas a um perfil, usadas na linha do tempo.
- **Conta**: Nome, e-mail, senha e a declaração de maioridade. Uma conta é aluno ou editor.
- **Quiz**: Um conjunto de perguntas de costumes, valores e celebrações, com gabarito e nota mínima de 70%.
- **Certificado**: Registro de que um aluno passou no quiz, com nome, quiz e data.
- **Situação de trabalho**: Cena, opções de resposta e retorno de conduta respeitosa. Há no máximo três publicadas.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: Um visitante abre um perfil publicado a partir da página inicial em até 2 minutos.
- **SC-002**: Um visitante conclui a comparação de duas tradições publicadas em até 3 minutos.
- **SC-003**: Uma pessoa nova cria a conta, faz o quiz com a nota mínima e vê o certificado em até 20 minutos.
- **SC-004**: Um aluno conclui uma situação de trabalho, da cena ao retorno, em até 10 minutos.
- **SC-005**: A entrega só é aceita quando existem de 5 a 7 perfis publicados e de 2 a 3 situações de trabalho publicadas.
- **SC-006**: Em uma rodada com 5 adultos que não participaram da redação, pelo menos 4 encontram o comparador e abrem um perfil sem ajuda de quem construiu o site.

## Assumptions

- Este corte segue a leitura de MVP web do material de produto (cadastro, 5 a 7 perfis, comparador, um quiz com certificado, módulo curto de trabalho e linha do tempo simples). A versão Angola ou mundo do material não muda estes fluxos. Instituições angolanas citadas no material não são tratadas como parceiras nem como autoras do conteúdo.
- Os nomes das 5 a 7 tradições ficam para uma decisão de conteúdo, antes da publicação. Esta spec não os escolhe.
- Perfis, comparador e linha do tempo são de leitura pública. Conta só é exigida para quiz, certificado e módulo de trabalho.
- Não há administrador de empresa, turma, relatório de RH nem separação do que cada empresa vê. O módulo de trabalho é do aluno, não de uma organização.
- Não há escola nem conta de menor. A maioridade é uma declaração da própria pessoa, sem conferência de documento.
- Não há conselho editorial dentro do produto. O editor registra fonte e estado de direitos. Revisão por comunidades, se existir, acontece fora desta entrega.
- A nota mínima de 70% é o padrão deste corte.
- O nome na interface é Conviver, até alguém escolher Diálogo.
- Ficam fora: aplicativo nativo, loja, preço, fórum, mapa, número de fiéis, role-play, ranking, distintivo além do certificado, e variantes de língua que não sejam o português deste texto.
- Preço, receita e prazo comercial do material não fazem parte desta spec.
