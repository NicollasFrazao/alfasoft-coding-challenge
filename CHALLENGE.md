Gerenciamento de Contatos
Aplicativo Web

Desenvolver um aplicativo web CRUD em Laravel para gerenciar contatos

Recursos do Aplicativo

O aplicativo web deve ter 5 recursos:

1 - Uma página de índice com uma lista de contatos existentes.

2 - Uma página com um formulário para adicionar novos contatos.

3 - Uma página para exibir os detalhes do contato (deve ser uma página, não um pop-up).

4 - Permitir a edição de um registro existente

5 - Permitir a exclusão de um registro existente

Considerações

Um contato é uma entidade com 4 campos: ID, Nome, Contato e endereço de e-mail. O nome deve ser uma string de qualquer tamanho maior que 5, o contato deve ter 9 dígitos e o e-mail deve ser um e-mail válido.

Cada linha na página de índice deve ter um link para editar o contato e um botão para excluí-lo. A exclusão deve realizar uma exclusão reversível do registro, usando os recursos do Laravel.

A página de detalhes do contato deve mostrar todos os campos do contato, além do link de edição e do botão de exclusão. Uma página independente deve ser implementada para exibir os detalhes de um contato, e não um pop-up exibindo as informações.

O contato e o endereço de e-mail devem ser únicos; dois contatos não podem ter o mesmo endereço.

Quaisquer informações ou dados necessários sobre a estrutura do banco de dados devem ser adicionados usando migrações e/ou seedings.

Sempre utilize os recursos nativos do Laravel sempre que possível, incluindo rotas, controladores, regras de validação de formulários, exclusões reversíveis e outros recursos.

Requisitos Adicionais
Os seguintes requisitos devem ser implementados durante a execução do teste:

- Permitir a visualização da lista de contatos por qualquer pessoa, mas os outros recursos devem ser acessados apenas por um usuário autenticado. Este pode ser um usuário estático criado anteriormente.
- Implementar testes para verificar erros de validação de formulário ao adicionar ou editar contatos.

Etapas de desenvolvimento

1 - Acessar o CloudCMD (gerenciador de arquivos e terminal)

2 - Criar e inicializar um repositório e enviar a versão base do site Laravel para ele

3 - Fazer vários envios com alterações de código de acordo com as melhores práticas
Observações
O ambiente de desenvolvimento já vem com o Laravel 10 pré-instalado. Em caso de erro, é possível instalar uma nova versão do Laravel pela linha de comando ou pelo gerenciador de arquivos CloudCMD.

Ao instalar uma nova versão do Laravel, não exclua os arquivos .env e .htaccess, ou o ambiente deixará de funcionar e o exercício não será considerado.

O ambiente do exercício utiliza o PHP versão 8.1; você precisará criar um código compatível com essa versão.

Os arquivos env e .htaccess são necessários para o funcionamento adequado do ambiente e já estão presentes nele. Não exclua ou altere esses arquivos, a menos que seja estritamente necessário.

Você não precisa e não deve executar o npm serve. O ambiente do exercício está pronto para apresentar a aplicação, desde que esteja configurado corretamente. Se precisar instalar dependências do npm, execute "npn run dev" ou equivalente, e o mesmo para o composer, conforme detalhado abaixo.
Composer
O Composer é necessário para o Laravel e está disponível no contêiner usado para o exercício. Para usar o composer, execute os comandos abaixo (ou outros comandos necessários):

composer install
composer update
composer dump-autoload
GIT
Para gerenciar o repositório, use um repositório externo (gitlab, bitbucket ou github), desde que o repositório seja público.