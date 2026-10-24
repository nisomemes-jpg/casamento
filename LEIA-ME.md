# Lista de casamento — Sofia & Ykaro

Site responsivo em **um único `index.html`**, com 30 sugestões de presentes, preços médios estimados, links de pesquisa online e busca de lojas físicas em Natal (RN). Visual nas cores vinho, creme e amarelo-manteiga.

## Importante: demonstração x reservas para todos

- **Ao abrir o `index.html` sem configurar nada**, o site funciona em **MODO DEMONSTRAÇÃO**. Os presentes escolhidos ficam guardados somente no navegador (via `localStorage`). **Isso NÃO sincroniza entre convidados diferentes.** O aviso aparece na página.
- **Para compartilhar com convidados de verdade**, configure um banco gratuito **Cloud Firestore do Firebase**. Assim, a disponibilidade aparece para todos em tempo real; a regra de segurança impede que duas pessoas reservem o mesmo presente.

## Como ativar a lista compartilhada

1. Acesse https://console.firebase.google.com/ e crie um projeto Firebase. Você não precisa usar o Firebase Hosting: pode publicar o site no GitHub Pages.
2. No projeto, escolha **Build / Firestore Database / Criar banco de dados** e use a edição Standard no modo de produção. Escolha a região desejada. **Não use regras de teste abertas.**
3. Dentro de **Firestore Database > Regras**, cole **todo o conteúdo de `firestore.rules`** e clique em **Publicar**. (Se você já tinha publicado as regras antigas, **publique de novo**: as novas regras são necessárias para guardar o nome de quem escolhe.)
4. Na engrenagem **Configurações do projeto > Geral > Seus aplicativos**, adicione um aplicativo **Web** (`</>`). Copie os campos `apiKey`, `authDomain`, `projectId` e `appId` do bloco `firebaseConfig`.
5. Abra `index.html` em um editor de texto. Perto do começo, encontre:

   ```javascript
   const FIREBASE_CONFIG = {
     apiKey: "",
     authDomain: "",
     projectId: "",
     appId: ""
   };
   ```

   Preencha esses quatro campos exatamente com os valores fornecidos pelo Firebase e salve o arquivo. Os valores de configuração do Firebase são informações públicas do aplicativo; **não cole senhas privadas, chaves de contas de serviço ou segredos administrativos**.
6. Publique o `index.html` em um repositório do GitHub (veja a seção abaixo). Ao acessar o endereço público com internet, a faixa amarela de demonstração deve virar uma **faixa verde: "Lista sincronizada!"**. Só depois disso divulgue aos convidados.

**Atenção:** o banco permite criação pública de reservas para facilitar o acesso sem login. Pessoas com o link podem reservar itens; convém compartilhar o link apenas com os convidados. Não há autenticação, limitação por convidado nem mecanismo automático de impedir escolhas maliciosas. Cada convidado informa o **nome** ao escolher um presente; o nome fica numa coleção que o site não consegue ler, só você vê no Firebase Console.

## Como publicar no GitHub Pages

1. Entre no GitHub e crie um repositório público, por exemplo `lista-casamento`.
2. Selecione **Add file > Upload files** e envie o **arquivo `index.html`** (não envie um arquivo ZIP sem extrair e não renomeie como `.txt`). O arquivo deve estar na raiz do repositório.
3. Confirme o upload com **Commit changes**.
4. Vá em **Settings > Pages**, escolha **Deploy from a branch**, depois **main / (root)** e **Save**.
5. O endereço será parecido com `https://SEU-USUARIO.github.io/lista-casamento/`. O GitHub pode levar alguns minutos para publicar o site. Atualize a página de Settings > Pages para ver o endereço.

## Como ver quem escolheu cada presente

No **Firebase Console > Firestore Database > Dados**, abra a coleção **`nomes`**. Cada documento é um presente (`p13` etc.) com os campos `nome` (quem escolheu), `presente` (nome do item) e `criadoEm` (data e hora). A lista atualiza sozinha quando alguém escolhe.

## Como corrigir ou liberar um presente

No **Firebase Console > Firestore Database > Dados**, abra a coleção `reservas` e apague o documento do presente que deseja liberar, por exemplo `p13` para a air fryer, e apague também o `p13` da coleção `nomes`. Ele ficará disponível novamente para todos. A regra impede o público de apagar uma reserva no site; a pessoa administradora pode fazer isso no console do Firebase.

## Personalização

- Os nomes, a data, o local e o texto do site estão no `index.html`.
- A lista está dentro da constante `GIFTS` no final do arquivo. Cada item contém `name` (nome), `category` (categoria), `price` (preço em reais), `icon` (emoji), `desc` (descrição), `local` (sugestão de loja física) e `color` (cor de fundo).
- Se adicionar mais presentes, será necessário também adaptar os identificadores permitidos nas regras do Firestore.
- Os valores no site **são estimativas ilustrativas, não cotações atuais de preços**. Links online abrem buscas na Amazon e links de lojas físicas abrem pesquisas no Google Maps. Verifique preço, voltagem e estoque antes da compra.

## Arquivos

- `index.html`: site completo — basta este arquivo no GitHub Pages.
- `firestore.rules`: regras para colar no Firebase.
- `LEIA-ME.md`: instruções.

Sem anúncios, links afiliados ou checkout. O site não realiza compras nem processa pagamentos.
