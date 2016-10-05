1. Nel portale passare alla rete virtuale a cui si vuole connettere un gateway.

2. Nella sezione **Impostazioni** del pannello della rete virtuale fare clic su **Subnet** per espandere il pannello Subnet.

3. Nel pannello **Subnet** fare clic su **+Subnet del gateway** nella parte superiore. Verrà aperto il pannello **Aggiungi subnet**. Il **Nome** per la subnet verrà compilato automaticamente con il valore 'GatewaySubnet'. Questo valore è obbligatorio per consentire ad Azure di riconoscere la subnet come subnet del gateway.

	![Aggiungere la subnet del gateway](./media/vpn-gateway-add-gwsubnet-rm-portal-include/addgwsubnet250.png)  

4. È possibile modificare il blocco CIDR dell'intervallo di indirizzi, se necessario. Controllare i requisiti specifici per la configurazione, in modo da confermare il blocco CIDR consigliato.

5. Fare clic su **OK** nella parte inferiore del pannello per creare la subnet.

<!-----HONumber=AcomDC_0810_2016-->