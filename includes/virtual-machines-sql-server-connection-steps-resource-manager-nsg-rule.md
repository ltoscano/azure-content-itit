### Configurare una regola in ingresso del gruppo di sicurezza di rete per la macchina virtuale

Se si intende connettersi a SQL Server tramite Internet, è necessario configurare una regola in ingresso nel gruppo di sicurezza di rete per la porta su cui è in ascolto l'istanza di SQL Server che, per impostazione predefinita, è la porta TCP 1433.

1. Nel portale selezionare **Macchine virtuali** e quindi la propria macchina virtuale di SQL Server.

3. Selezionare le **interfacce di rete**.

	![interfaccia di rete](./media/virtual-machines-sql-server-connection-steps/rm-network-interface.png)

4. Selezionare quindi l'interfaccia di rete relativa alla propria macchina virtuale.

4. Fare clic sul collegamento **Gruppo di sicurezza di rete**.

	![interfaccia di rete](./media/virtual-machines-sql-server-connection-steps/rm-network-security-group.png)

6. Nelle proprietà del gruppo di sicurezza di rete espandere **Regole di sicurezza in ingresso**.

5. Fare clic su **Add**.

6. Nel campo **Nome** specificare "SQLServerPublicTraffic".

7. Impostare l'opzione **Protocollo** su **TCP**.

8. Nel campo **Intervallo di porte di destinazione** specificare 1433 o la porta su cui è in ascolto l'istanza di SQL Server.

9. Verificare che l'opzione **Azione** sia impostata su **Consenti**. La finestra di dialogo della regola di sicurezza deve avere un aspetto simile alla schermata seguente.

	![regola di sicurezza di rete](./media/virtual-machines-sql-server-connection-steps/rm-network-security-rule.png)

9. Fare clic su **OK** per salvare la regola creata per la macchina virtuale.

>[AZURE.NOTE] È possibile disporre di un secondo gruppo di sicurezza di rete associato con la subnet, che è separato dal gruppo di sicurezza di rete nella VM. Ciò non avviene automaticamente per impostazione predefinita. Tuttavia, se si crea un gruppo di sicurezza di rete sulla subnet, è necessario aprire la porta 1433 sul gruppo di sicurezza di rete della subnet e della VM.

<!---HONumber=AcomDC_0921_2016-->