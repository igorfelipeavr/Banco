# Banco
TrabsonBanco
//*1. Listar id, primeiro nome, e 
	//quantidade de pedidos efetuados pelos clientes que tem o nome iniciando pela letra "A" em ordem alfabética.
	@Test
	public void testAgregacaoidnome() throws Exception{
		ResultSet result = connection.createStatement().executeQuery("SELECT id,first_name,quantity FROM customers "
				+ "INNER JOIN items ON items.id = items.product_id "
				+ "WHERE first_name LIKE '%A%'"
				+ " ORDER BY first_name " );
		while (result.next()){
			System.out.println(result.getString(1) + " " + result.getString(2) + " " + result.getInt(3)) ;	
		}
}


	//*2. Listar id cliente, nome cliente, id pedido, total do pedido para os 3 pedidos com os maiores valores totais efetuados 
	//para a cidade chamada "Seattle".*	
	@Test
	public void testAgregacaoidnomeSeattle() throws Exception{
		ResultSet result = connection.createStatement().executeQuery("SELECT customer_id,first_name,total,city FROM customers "
				+ "INNER JOIN invoices ON invoices.id = invoices.customer_id "
				+ "WHERE city LIKE '%Seattle'"
				+ " ORDER BY total " );
		while (result.next()){
			System.out.println(result.getInt(1) + " " + result.getString(2) + " " + result.getInt(3)+ " " + result.getString(4)) ;	
		}
}
	
///

	/*3. Listar o id do produto, nome do produto, e valor total(item.quantidade X produto.preço) em todos os pedidos em que ele foi mencionado.*/
	@Test
	public void testAgregacaoid3() throws Exception{
		ResultSet resultado = connection.createStatement().executeQuery("SELECT products.id, products.name,(items.quantity * products.price) as valor_total FROM products"
	    + "JOIN items on (products.id = items.product_id) ");
		while (resultado.next()){
			System.out.println(resultado.getInt(1) + " " + resultado.getString(2) + " " + resultado.getDouble(3)) ;	
		}
}		
	

	/*4. Listar nome do cliente, quantidade dos pedidos, e média do valor dos pedidos por cliente.*/
	@Test
	public void testAgregacao4() throws Exception{
		ResultSet resultado = connection.createStatement().executeQuery ("SELECT customers.first_name ' '  customers.last_name, count(invoices.id), AVG (items.quantity * items.cost) as mediacliente FROM customers"
				+ "JOIN invoices on (invoices.customer_id = customers.id) "
				+ "JOIN items on (invoices.id = items.invoice_id) "
				+ "GROUP BY customers.first_name, customers.last_name ");
		while (resultado.next()){
			System.out.println(resultado.getString(1) + " " + resultado.getString(2) + " " + resultado.getDouble(3)) ;	
		}
}	

	/*5. Listar nome do produto e a média da quantidade vendida por pedido.*/
	@Test
	public void testAgregacao5() throws Exception{
		ResultSet resultado = connection.createStatement().executeQuery ("SELECT products.name, AVG(items.quantity) FROM products"
				+ "JOIN items on (products.id = items.product_id)"
				+ "GROUP BY products.name");
		while (resultado.next()){
			System.out.println(resultado.getString(1) + " " + resultado.getString(2) + " " + resultado.getDouble(3)) ;	
		}
}	

	/*6. Listar produtos com quantidade total vendida maior que 10, para a rua que contenha "Seventh Av." no nome.*/
	@Test
	public void testAgregacao6() throws Exception{
		ResultSet resultado = connection.createStatement().executeQuery ("SELECT products.name,sum(items.quantity) as total FROM products "
	  + "JOIN items on (products.id = items.product_id)"
	  + "JOIN invoices on (items.invoice_id = invoices.id)"
	  + "JOIN customers on (invoices.customer_id = customers.id)"
	  +  "WHERE total > 10 and customers.street like '%Seventh Av.%'"
	  + "GROUP BY products.name"); 
		while (resultado.next()){
			System.out.println(resultado.getString(1) + " " + resultado.getDouble(2));
		}
}	
		

	/*7. Listar os somente os produtos que ainda não estão relacionados a nenhum pedidos.*/
	@Test
	public void testAgregacao7() throws Exception{
		ResultSet resultado = connection.createStatement().executeQuery ("SELECT DISTINCT products.name FROM products"
				+ "WHERE products.id NOT IN (SELECT items.product_id FROM items");
	
	while (resultado.next()){
		System.out.println(resultado.getString(1));	
	}
}	
	/*8. Listar somente os clientes que já estão relacionados a algum pedido.*/
	@Test
	public void testAgregaca7() throws Exception{
		ResultSet resultado = connection.createStatement().executeQuery ("SELECT customers.first_name ' ' customers.last_name FROM customers"
				+ " WHERE customers.id IN (SELECT invoices.customer_id FROM invoices");
		while (resultado.next()){
			System.out.println(resultado.getString(1) + " " + resultado.getString(2));	
	}
}

	//*9. Listar nome do cliente e rua quando a cidade for "New York".*
	@Test
	public void testAgregacaNewYork() throws Exception{
		ResultSet result = connection.createStatement().executeQuery("SELECT first_name,street,city FROM customers "
				+ "INNER JOIN customers ON customers.id = customers.id "
				+ "WHERE city LIKE '%New York'"
				+ " ORDER BY city " );
		while (result.next()){
			System.out.println(result.getString(1) + " " + result.getString(2) + " " + result.getString(3));	
		}
}
	
	

	//*10. Listar distintamente o nome dos produtos comprados por "Laura".*/
	@Test
	public void testAgregacaLaura() throws Exception{
		ResultSet result = connection.createStatement().executeQuery("SELECT DISTINCT first_name, name FROM products "
				+ "INNER JOIN customers ON customers.id = products.id "
				+ "WHERE first_name LIKE '%Laura'"
				+ " ORDER BY first_name " );
		while (result.next()){
			System.out.println(result.getString(1) + " " + result.getString(2));	
		}
}
	
	
}
