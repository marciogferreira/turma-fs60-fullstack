SISTEMA DE LOJAS:

O SISTEMA DEVE POSSIBILITAR O USUARIO SE CADASTRAR, CADASTRAR UMA LOJA OU MAIS PODENDO ADMINISTRAR TODAS ELAS SELECIONANDO A LOJA DESEJADA.



TABELAS EM MENTE:

- USUARIOS_PROPRIETARIOS (id, nome, email, senha, tipo_usuario, status)
- TIPO_SEGUIMENTO - (id, nome, proprietario_id)
- LOJAS - QUALQUER SEGUIMENTO - (id, nome, endereco, telefone, tipo_seguimento)

- USUARIOS_COLABORADORES - (id, nome, email, senha, loja_id)
- CATEGORIAS - POR TIPO DE SEGUIMENTO - (id, nome, tipo_seguimento)
- PRODUTOS - (id, nome, preco)
- CLIENTES - POR LOJA - (id, tipo_seguimento)
- PEDIDOS - POR LOJA - (id, tipo_seguimento)
- ITENS_PEDIDOS - (id, pedido_id)
- TIPOS_PAGAMENTOS - (id, nome)
- PAGAMENTOS - (id, tipo_pagamento, pedido_id, status)