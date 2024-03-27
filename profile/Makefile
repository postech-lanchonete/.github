CLIENTES_URL := http://127.0.0.1:49702
PRODUTOS_URL := http://127.0.0.1:49732
PEDIDOS_URL := http://localhost:49713
PAGAMENTOS_URL := http://localhost:49719
PRODUCAO_URL := http://localhost:49725

all: criar_pedido

curl_clientes:
	@echo "Criando clientes"
	curl -X POST '$(CLIENTES_URL)/v1/clientes' -H 'accept: */*' -H 'Content-Type: application/json' -d '{ "nome": "Daniel", "sobrenome": "Silva", "email": "daniel.silva@postech.com", "cpf": "123", "senha": "123" }'
	curl -X POST '$(CLIENTES_URL)/v1/clientes' -H 'accept: */*' -H 'Content-Type: application/json' -d '{ "nome": "Roberto", "sobrenome": "Pacheco", "email": "Roberto.Pacheco@postech.com", "cpf": "456", "senha": "123" }'
	curl -X POST '$(CLIENTES_URL)/v1/clientes' -H 'accept: */*' -H 'Content-Type: application/json' -d '{ "nome": "Jose", "sobrenome": "Alberto", "email": "Jose.Alberto@postech.com", "cpf": "789", "senha": "123" }'

curl_produtos_comidas:
	@echo "Criando produtos"
	curl -X 'POST' '$(PRODUTOS_URL)/v1/produtos' -H 'accept: */*' -H 'Content-Type: application/json' -d '{"nome": "Hambúrguer", "categoria": "LANCHE", "preco": 10.99, "descricao": "Hambúrguer de carne bovina com queijo, alface, tomate e molho especial", "imagem": "https://example.com/hamburguer.jpg"}'
	curl -X 'POST' '$(PRODUTOS_URL)/v1/produtos' -H 'accept: */*' -H 'Content-Type: application/json' -d '{"nome": "Sanduíche de Frango", "categoria": "LANCHE", "preco": 8.99, "descricao": "Sanduíche de filé de frango grelhado com alface, tomate e maionese", "imagem": "https://example.com/sanduiche_frango.jpg"}'
	curl -X 'POST' '$(PRODUTOS_URL)/v1/produtos' -H 'accept: */*' -H 'Content-Type: application/json' -d '{"nome": "Batata Frita", "categoria": "ACOMPANHAMENTO", "preco": 5.99, "descricao": "Porção de batatas fritas crocantes e saborosas", "imagem": "https://example.com/batata_frita.jpg"}'
	curl -X 'POST' '$(PRODUTOS_URL)/v1/produtos' -H 'accept: */*' -H 'Content-Type: application/json' -d '{"nome": "Onion Rings", "categoria": "ACOMPANHAMENTO", "preco": 6.99, "descricao": "Anéis de cebola empanados e fritos, servidos com molho barbecue", "imagem": "https://example.com/onion_rings.jpg"}'
	curl -X 'POST' '$(PRODUTOS_URL)/v1/produtos' -H 'accept: */*' -H 'Content-Type: application/json' -d '{"nome": "Refrigerante", "categoria": "BEBIDA", "preco": 3.50, "descricao": "Lata de refrigerante de cola gelada", "imagem": "https://example.com/refrigerante.jpg"}'
	curl -X 'POST' '$(PRODUTOS_URL)/v1/produtos' -H 'accept: */*' -H 'Content-Type: application/json' -d '{"nome": "Suco Natural de Laranja", "categoria": "BEBIDA", "preco": 4.99, "descricao": "Suco fresco e natural de laranja", "imagem": "https://example.com/suco_laranja.jpg"}'
	curl -X 'POST' '$(PRODUTOS_URL)/v1/produtos' -H 'accept: */*' -H 'Content-Type: application/json' -d '{"nome": "Sorvete de Chocolate", "categoria": "SOBREMESA", "preco": 7.99, "descricao": "Sorvete cremoso de chocolate com calda quente", "imagem": "https://example.com/sorvete_chocolate.jpg"}'

criar_pedido:
	@echo "Criando pedido"
	PEDIDO_ID=$$(curl -s -X POST '$(PEDIDOS_URL)/v1/pedidos' -H 'accept: */*' -H 'Content-Type: application/json' -d '{ "idCliente": 1, "idsProdutos": [ 1 ] }' | jq -r '.id'); \
    echo "ID do pedido: $$PEDIDO_ID"
	@echo "Consultando pagamentos"
	export PAGAMENTO_ID := $(shell curl -s -X GET '$(PAGAMENTOS_URL)/v1/pagamentos/status/PENDENTE' -H 'accept: application/json' | jq -r '.[0].id')
	echo "Aprovando pagamento"
	curl -X PATCH 'http://localhost:49719/v1/pagamentos/$(PAGAMENTO_ID)/status/APROVADO' -H 'accept: application/json'
	echo "Atualizando status e consultando pedido"
	curl -X PATCH '$(PRODUCAO_URL)/v1/pedidos/$$PEDIDO_ID/status?novoStatus=PRONTO' -H 'accept: */*'
	curl -X GET '$(PEDIDOS_URL)/v1/pedidos/$$PEDIDO_ID' -H 'accept: */*'
