# Encarte de Oferta

## Manual completo · Versão PHP para cPanel

**Da instalação à primeira impressão — com referências para manutenção.**

> **Escopo:** este manual descreve a versão independente em `exports/cpanel/`. A aplicação original em TanStack Start continua separada.

## 01 · Visão geral

O sistema cria um cartaz por vez, com seis linhas possíveis, sobre uma folha A4 em retrato. Foi pensado para papel amarelo com o título “OFERTA” pré-impresso. A tela simula o papel; a impressão exclui a cor de fundo e o título do topo.

A digitação atualiza a prévia automaticamente. As letras crescem até os limites de largura e de altura; quando a largura limita o crescimento, o espaço restante é distribuído entre as linhas. Não há deformação vertical das letras para forçar o preenchimento.

### O que está incluído

- Edição de títulos, destaque, medida, preço e rodapé.
- Quatro seletores independentes, cada um com nove fontes. Tema escuro e crédito com link “Construído por VKTCloud” na página, fora da imagem.
- Espaço reservado fixo de 80 mm no topo.
- Download e compartilhamento de PNG; impressão de uma única imagem PNG em A4.
- Salvamento local de textos, fontes e reserva fixa de 80 mm no topo.
- Fontes locais com suas licenças.

### O que não está incluído

Login próprio, banco de dados, sincronização entre dispositivos, histórico, catálogo de produtos, importação de planilhas, montagem de múltiplas ofertas numa folha, envio automático para impressoras. A exportação PNG está disponível.

## 02 · Requisitos e preparação

| Item | Recomendação |
| --- | --- |
| Hospedagem | cPanel com PHP habilitado |
| PHP | 8.1 ou mais recente |
| Navegador | Chrome ou Edge atualizado |
| JavaScript | Habilitado |
| Fontes | Pasta local `assets/fonts/` incluída |
| Impressora | Papel A4; verificar margens físicas |
| Segurança | HTTPS e acesso restrito quando necessário |

Não precisa criar banco de dados, instalar Composer, executar Node.js na hospedagem ou configurar APIs. PHP serve a página; a lógica de edição roda no navegador.

Antes de publicar, meça a distância entre a borda superior do papel e o fim da área já impressa. O sistema reserva sempre 80 mm; utilize papel compatível. Não existe mais um controle para alterar essa altura.

## 03 · Instalação no cPanel

### Opção A — pacote ZIP

1. Entre no cPanel da hospedagem.
2. Abra **Gerenciador de Arquivos**.
3. Entre em `public_html` ou na raiz indicada para o seu domínio.
4. Crie uma pasta chamada `encarte`.
5. Envie o ZIP para essa pasta e clique em **Extrair**.
6. Confira que `index.php` ficou diretamente dentro de `encarte`, sem uma pasta extra intermediária.
7. Abra o endereço do seu domínio com `/encarte/` ao final.
8. Confirme que os campos e a folha amarela aparecem e que o botão IMPRIMIR fica disponível.

### Opção B — repositório completo

Envie somente o conteúdo de `exports/cpanel/` para a pasta de destino. Não envie `node_modules`, arquivos do repositório Git, configurações da aplicação original ou a pasta `src`.

### Estrutura esperada

```text
public_html/
└── encarte/
    ├── index.php
    ├── README.md
    ├── LEIA-ME.txt
    ├── assets/
    │   ├── app.js
    │   ├── formatar.js
    │   ├── style.css
    │   ├── fonts.css
    │   └── fonts/
    │       └── arquivos .ttf
    ├── licencas/
    │   └── licenças das fontes
    └── docs/
        ├── DOCUMENTACAO.md
        └── manual.html
```

### Instalação na raiz do domínio

É possível enviar os arquivos diretamente para `public_html`, desde que não sobrescrevam outro site. Faça backup antes. Se já houver `index.html`, a hospedagem pode exibi-lo antes do `index.php`; confirme a prioridade com a configuração da sua hospedagem.

### Permissões

Em hospedagens comuns, pastas usam `755` e arquivos `644`. Preserve as permissões recomendadas pelo provedor. Não use `777`: o sistema não precisa escrever arquivos no servidor.

## 04 · Preenchimento do encarte

| Campo | Finalidade | Exemplo ilustrativo |
| --- | --- | --- |
| Linha 1 | Nome ou categoria | BISCOITO |
| Linha 2 | Marca ou complemento | PIRAQUE |
| Destaque | Sabor, variante ou chamada | QUEIJINHO |
| Peso / medida | Quantidade do produto | 138g |
| Preço (sem R$) | Valor da oferta | 5,99 |
| Rodapé | Condição ou observação | ACIMA DE 2 UNIDADES OU + |

Os exemplos são os textos iniciais do sistema, não ofertas comerciais verificadas.

- Digite o preço sem o símbolo: a página acrescenta `R$` automaticamente.
- Use a vírgula decimal conforme necessário. O campo aceita texto livre e não corrige valores.
- Campos de texto vazios são omitidos do cartaz. O preço mantém o prefixo `R$`, mesmo vazio; portanto, preencha o valor antes de imprimir.
- Textos longos ficam menores para caber na largura. Divida o título nas duas linhas quando necessário.
- A medida fica alinhada à direita; as demais linhas ficam centralizadas.
- Clique em **Limpar** para remover os textos. O topo continua em 80 mm; as fontes voltam ao padrão.

## 05 · Fontes e legibilidade

Todos os seletores permitem as mesmas nove opções; você não fica limitado às fontes recomendadas para cada grupo.

| Nome no menu | Fonte utilizada | Peso/estilo |
| --- | --- | --- |
| Bebas Neue | Bebas Neue | 400 |
| Impact (Anton) | Anton | 400 |
| Montserrat Black | Montserrat | 900 |
| Arial Black (Archivo Black) | Archivo Black | 400 |
| Futura Bold (Jost) | Jost | 800 |
| Rubik Black Itálico | Rubik | 900, itálico |
| Roboto Bold | Roboto | 700 |
| Open Sans Bold | Open Sans | 700 |
| Calibri (Carlito) | Carlito | 700 |

### Seleção inicial

| Grupo | Padrão |
| --- | --- |
| Títulos | Montserrat Black |
| Destaque | Bebas Neue |
| Preço | Impact (Anton) |
| Medida/rodapé | Roboto Bold |

O menu informa as substituições por fontes livres. A versão PHP carrega arquivos locais, sem consultar o Google durante o uso. Preserve `assets/fonts.css`, `assets/fonts/` e `licencas/`.

O botão de impressão aguarda o redimensionamento. Se houver falha no carregamento das fontes, a página pode impedir a impressão pelo botão até ser recarregada.

## 06 · Maiúsculas e unidades de medida

O texto comum vai para maiúsculas. Uma unidade reconhecida imediatamente depois de um número, com ou sem espaços, fica em minúsculas.

| Entrada | Saída |
| --- | --- |
| biscoito leite | BISCOITO LEITE |
| 138G | 138g |
| 2 KG | 2 kg |
| 350ML | 350ml |
| 1,5L | 1,5l |
| 10 UN | 10 un |

**Unidades reconhecidas:** `kg`, `mg`, `g`, `ml`, `l`, `lt`, `cm`, `mm`, `m`, `un`, `und`, `pct`, `cx`, `dz`.

A regra se aplica aos campos de texto formatados, não ao preço, que é exibido como digitado. Uma unidade isolada, sem número anterior, não é convertida para minúscula. Unidades fora dessa lista não têm conversão específica.

> A regra foi implementada conforme a exigência operacional informada para o projeto. O sistema não verifica todos os requisitos legais de uma oferta e não oferece certificação de conformidade com o Procon.

## 07 · Papel e redimensionamento

### Medidas

| Parâmetro | Valor |
| --- | --- |
| Folha | 210 × 297 mm — A4 |
| Margens laterais internas | 12 mm de cada lado |
| Margem inferior interna | 12 mm |
| Largura útil | 186 mm |
| Reserva no topo | 80 mm fixos |
| Altura útil | 297 − topo − 12 mm |

Com topo de 80 mm, a altura útil é **205 mm**. O conteúdo nunca deve ser posicionado sobre a área pré-impressa. Use papel com reserva compatível de 80 mm.

### Como o ajuste funciona

1. Aguarda o carregamento das fontes escolhidas.
2. Mede a largura de cada texto no navegador.
3. Calcula o tamanho máximo permitido pela largura.
4. Distribui a altura usando pesos por tipo de linha.
5. Redistribui a altura quando uma linha atinge seu limite de largura.
6. Distribui o espaço livre entre as linhas.

O preço tem prioridade visual e o símbolo `R$` usa 45% do tamanho dos números. A medida usa no máximo 35% da largura útil; o rodapé, 85%. O limite é calculado por largura, não por número de caracteres.

## 08 · PNG, impressão e WhatsApp

### Configuração da janela de impressão

| Configuração | Seleção |
| --- | --- |
| Orientação | Retrato |
| Papel | A4 |
| Escala | 100% |
| Margens | Nenhuma |
| Cabeçalhos e rodapés | Desativados |

Não selecione “Ajustar à página”, pois isso altera a posição do conteúdo em relação ao topo pré-impresso.

A impressão usa uma imagem PNG separada da prévia: os controles, o amarelo e o texto “OFERTA (pré-impresso)” não são impressos. O conteúdo sai em preto.

### Primeiro teste

1. Confirme a medida do topo e o valor da oferta.
2. Imprima uma folha.
3. Compare o início do texto com o fim do topo pré-impresso.
4. Verifique cortes nas laterais e no rodapé.
5. Ajuste as configurações da impressora antes de imprimir em quantidade.

A aplicação define a geometria, mas não controla as margens físicas, o alimentador, a escala aplicada pelo driver ou o consumo exato de tinta.

### Baixar PNG

Clique em **Baixar PNG**. O arquivo `encarte.png` tem **2480 × 3508 pixels**, proporção A4 e fundo transparente, com texto preto. O amarelo, o título pré-impresso e a assinatura VKTCloud não entram na imagem. Ao abrir em um visualizador, use fundo claro para enxergar a tinta preta.

### Compartilhar no WhatsApp

Clique em **Compartilhar PNG**. Em dispositivos compatíveis, o seletor de compartilhamento do sistema abre: escolha WhatsApp e o destinatário. O navegador não escolhe o aplicativo automaticamente. Se o compartilhamento de arquivos não estiver disponível, o sistema baixa o PNG e informa que você deve anexá-lo manualmente no WhatsApp. Não há envio automático nem armazenamento da imagem no servidor. HTTPS é necessário para o compartilhamento nativo em navegadores compatíveis.

### Uma folha, página 1

A impressão usa somente uma imagem PNG e produz uma folha A4. A página original, os controles e o crédito ficam fora da impressão. Selecione **página 1** e uma cópia na janela de impressão. Por segurança, navegadores não permitem que a página imponha o intervalo ou a quantidade de cópias no driver. O sistema evita páginas extras no conteúdo gerado, mas não controla alterações manuais de papel ou escala. Uma impressora virtual pode oferecer PDF por conta própria; a aplicação não exporta PDF.

## 09 · Salvamento e privacidade

A versão PHP usa o armazenamento local do navegador, com a chave `encarte-cpanel`. Guarda os seis campos, as quatro escolhas de fonte e a reserva fixa de 80 mm no topo.

- Os textos não são enviados para o servidor pela aplicação.
- O mesmo navegador e endereço podem recuperar a edição anterior.
- Outro dispositivo, navegador ou origem não compartilha esses dados.
- Mudar entre HTTP e HTTPS ou mudar o domínio altera a origem do armazenamento.
- Limpar os dados do site elimina o conteúdo salvo.
- No modo privado, a persistência pode ser limitada à sessão.
- Com armazenamento bloqueado, editar e imprimir continuam disponíveis, mas os valores podem não ser conservados.

O servidor da hospedagem ainda pode registrar acessos normais à página e aos arquivos estáticos. O sistema não adiciona analytics nem recursos de rastreamento próprios.

## 10 · Segurança de publicação

A página não possui autenticação própria. Se o endereço deve ser usado somente pela equipe, habilite **Privacidade do Diretório** no cPanel para a pasta instalada e utilize HTTPS.

O sistema insere os textos no cartaz como texto, não como HTML. Digitar uma marcação não a transforma em código executável. Ainda assim, mantenha PHP e o navegador atualizados e não exponha senhas ou arquivos privados na pasta pública.

- Não configure permissões `777`.
- Não publique credenciais ou arquivos de ambiente.
- Faça backup antes de substituir arquivos.
- Remova o ZIP de instalação do diretório público depois da extração, caso não queira disponibilizar o código para download.
- Preserve as licenças das fontes nas distribuições.

## 11 · Solução de problemas

| Sintoma | O que conferir |
| --- | --- |
| Página não encontrada | Pasta do domínio e posição do `index.php` |
| Navegador baixa ou mostra código PHP | PHP não está sendo executado; revise a configuração da hospedagem |
| Outro site aparece | Arquivo de entrada anterior ou pasta errada |
| Página sem aparência ou sem fontes | `assets/` completo, caminhos relativos e permissões |
| IMPRIMIR fica desabilitado | Aguarde as fontes; verifique arquivos `.ttf`, recarregue e confira a mensagem na tela |
| Texto não muda | JavaScript habilitado; arquivos `app.js` e `formatar.js` carregando |
| Letras pequenas | Texto longo limitado pela largura; divida entre Linha 1 e Linha 2 |
| Texto sobre o topo | Use papel com topo de 80 mm; o valor não é ajustável |
| Cortes ou folha extra | A4, retrato, escala 100%, margens Nenhuma e cabeçalhos desligados |
| Dados não reaparecem | Mesmo navegador e origem; armazenamento permitido |
| Alterações de arquivos não aparecem | Atualização forçada no navegador ou limpeza de cache |
| `KG` permanece maiúsculo | A conversão exige número antes da unidade; não se aplica ao preço |
| `R$` aparece sem valor | Preencha o preço; Limpar também apaga o valor |

Para diagnosticar arquivos ausentes, abra as ferramentas do navegador e confira a aba de rede. Não compartilhe cookies, senhas ou informações privadas ao pedir ajuda.

## 12 · Manutenção e personalização

### Responsabilidade de cada arquivo

| Arquivo | Responsabilidade |
| --- | --- |
| `index.php` | Documento, metadados, campos e carregamento dos arquivos |
| `assets/app.js` | Estado, fontes, tamanhos, prévia, salvamento e impressão |
| `assets/output.js` | Configuração A4/PNG e desenho da imagem |
| `assets/formatar.js` | Maiúsculas e unidades reconhecidas |
| `assets/style.css` | Cores, disposição e regras de impressão |
| `assets/fonts.css` | Declarações das fontes locais |
| `assets/fonts/` | Arquivos tipográficos |
| `licencas/` | Licenças de distribuição das fontes |

### Alterar padrões

Os valores iniciais ficam em `PADRAO`, em `assets/app.js`. Ajuste também os valores iniciais do formulário em `index.php` para manter a primeira exibição coerente. Dados já salvos no navegador prevalecem sobre os novos padrões; use um navegador sem dados anteriores para conferir.

### Adicionar uma unidade

Edite a lista em `assets/formatar.js` e atualize o teste em `src/lib/cpanel-formatar.test.ts` no repositório completo. Confira entradas com e sem espaços, além de texto que não deve mudar.

### Adicionar ou trocar uma fonte

Inclua o arquivo e sua licença, declare a fonte em `assets/fonts.css` e atualize o catálogo `FONTES` em `assets/app.js`. Confira largura, altura, acentos e impressão antes de publicar.

### Atualizações

1. Faça backup dos arquivos atuais.
2. Teste a nova versão localmente ou em outra pasta.
3. Envie os arquivos atualizados mantendo a estrutura.
4. Recarregue a página e confira todas as fontes afetadas.
5. Baixe um PNG e faça uma impressão física de teste.

Não há atualização automática nem numeração de versão incorporada à página.

## 13 · Referência técnica

A versão PHP usa HTML, CSS e JavaScript sem etapa de compilação. Depende de APIs modernas do navegador, incluindo Canvas 2D, Font Loading API, ResizeObserver e localStorage.

A imagem é desenhada em Canvas e convertida em PNG; impressão, download e compartilhamento reutilizam o mesmo resultado. O arquivo de configuração fixa o topo e a proporção da folha.

O PHP envia o cabeçalho `Content-Type: text/html; charset=utf-8`. Não há sessão, endpoints de gravação, dependências PHP, banco de dados ou tarefas agendadas.

### Executar a versão PHP localmente

Na pasta com `index.php`:

```sh
php -S localhost:8000
```

Abra `http://localhost:8000`. Use esse servidor somente para desenvolvimento.

### Aplicação original

O repositório completo também contém a versão TanStack Start, React, TypeScript e Tailwind CSS. Para desenvolvimento dela:

```sh
bun install
bun run dev
```

Os dois caminhos são independentes. Não é necessário compilar a aplicação original para publicar o conteúdo de `exports/cpanel/`.

### Testes de regras

No repositório completo:

```sh
bunx vitest run src/lib/formatar.test.ts src/lib/cpanel-formatar.test.ts src/lib/cpanel-output.test.ts
```

Esses testes verificam a formatação; não substituem a inspeção da prévia, do PNG e da impressão física. A edição, as nove fontes, o salvamento local e o fluxo de impressão da versão PHP foram conferidos localmente; a instalação em uma hospedagem específica depende do ambiente do provedor.

## 14 · Checklist de entrega

- [ ] Página abre pelo endereço final.
- [ ] Todos os arquivos de fontes carregam.
- [ ] Campos atualizam a prévia.
- [ ] `2 KG` aparece como `2 kg` no cartaz.
- [ ] Preço preenchido e condição da oferta revisada.
- [ ] Altura do topo corresponde ao papel real.
- [ ] PNG A4 contém apenas texto preto e topo em branco.
- [ ] Uma impressão física foi aprovada.
- [ ] Acesso restrito quando necessário.
- [ ] Backup e licenças preservados.

## 15 · Licenciamento

As fontes possuem licenças próprias incluídas em `licencas/`. Os nomes das fontes comerciais no menu indicam referências de estilo e seus substitutos, não a distribuição dos arquivos comerciais originais.

O projeto ainda não define uma licença de uso e redistribuição para o código da aplicação. Escolha e inclua uma licença antes de conceder essas permissões publicamente; tornar o repositório visível não equivale, por si só, a atribuir uma licença livre.
