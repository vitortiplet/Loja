# Fliperama Games

Loja virtual de games (consoles, jogos, acessórios e colecionáveis) feita com **PHP + PDO + MySQL/MariaDB + Bootstrap 5**, para rodar em Apache/XAMPP.

## Funcionalidades
- Vitrine dinâmica com busca e filtro por categoria
- Página do produto (preço, descrição, imagem, disponibilidade de estoque)
- Carrinho em `$_SESSION` (adicionar, alterar quantidade respeitando o estoque, remover, total)
- Cadastro e login (`password_hash` / `password_verify`)
- Checkout só para logados, pagamento simulado (PIX, Boleto ou Cartão)
- Pedido gravado em transação: pedido → itens (preço congelado) → baixa de estoque → limpa carrinho
- Painel `/admin` (apenas perfil admin): CRUD de produtos com upload de imagem e listagem de pedidos

## Segurança aplicada
- Credenciais no `.env` (fora do Git); modelo em `.env.example`
- Prepared statements em todas as consultas (`ATTR_EMULATE_PREPARES = false`)
- Senhas com `password_hash()` / `password_verify()`
- Rotas `/admin` validadas no servidor (`exigir_admin()`)
- Token CSRF em todos os formulários POST, `session_regenerate_id()` no login
- Saída escapada com `htmlspecialchars` (XSS) e upload de imagem validado pelo conteúdo real do arquivo

## Como rodar
1. Instale o XAMPP e inicie **Apache** e **MySQL**.
2. Clone este repositório em `C:\xampp\htdocs\loja-virtual`.
3. Copie `.env.example` para `.env` e ajuste se necessário (no XAMPP padrão, `DB_USER=root` e `DB_PASS` vazio).
4. No phpMyAdmin (`http://localhost/phpmyadmin`), aba **Importar**, envie `database/schema.sql`
   (cria o banco `loja_games`, as tabelas e os dados de teste).
5. Acesse `http://localhost/loja-virtual/`.

## Contas de teste
| Perfil  | E-mail                  | Senha        |
|---------|-------------------------|--------------|
| Admin   | admin@fliperama.com     | Admin@123    |
| Cliente | cliente@fliperama.com   | Cliente@123  |

## Estrutura
```
config/      env.php (leitor do .env) e database.php (PDO)
includes/    bootstrap.php (sessão, CSRF, auth, carrinho), header.php, footer.php
admin/       painel administrativo (todas as páginas incluem _inicio.php)
database/    schema.sql
assets/      css, js e imagens
```

## Modelo de dados
`usuarios` · `categorias` · `produtos` (N:1 categorias) · `pedidos` (N:1 usuarios) · `itens_pedido` (resolve N:N pedidos × produtos e guarda `preco_unitario`).
