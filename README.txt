To test USSD Gateway with mobicents ss7-simulator make sure you follow the below configuration changes and the execute SS7 Command's

1) Copy folder ussd-mobicents-jainslee-x.y.z.FINAL-jboss-5.1.0.GA/extra/mobicents-ss7/ss7/mobicents-ss7-service and paste to ussd-mobicents-jainslee-2.6.0.FINAL-jboss-5.1.0.GA/jboss-5.1.0.GA/server/default/deploy
 
2) Copy below Resource Adaptors from ussd-mobicents-jainslee-2.6.0.FINAL-jboss-5.1.0.GA/resources/ and paste to ussd-mobicents-jainslee-2.6.0.FINAL-jboss-5.1.0.GA/jboss-5.1.0.GA/server/default/deploy

	a) map/mobicents-slee-ra-map-du-1.0.0.CR3.jar
	b) http-client/http-client-ra-DU-2.5.0.FINAL.jar
	c) jdbc/mobicents-slee-ra-jdbc-DU-1.0.0.FINAL.jar

3) If you are deploying USSD Gateway from source code, set JBOSS_HOME variable to point to "ussd-mobicents-jainslee-2.6.0.FINAL-jboss-5.1.0.GA/jboss-5.1.0.GA" and execute "mvn clean install" command, it should create "mobicents-ussd-gateway" and ussdhttpdemo.war in /deploy folder of JBoss. 

5) Execute below commands in SS7 CLI to setup SS7 

	5.1) sctp association create Assoc1 CLIENT 127.0.0.1 8012 127.0.0.1 8011 

	5.2) m3ua as create AS1 IPSP mode SE ipspType CLIENT rc 101 traffic-mode loadshare

	5.3) m3ua asp create ASP1 Assoc1

	5.4) m3ua as add AS1 ASP1

	5.5) m3ua route add AS1 2 -1 -1

	5.6) m3ua asp start ASP1

	5.7) sccp sap create 1 1 1 2

	5.8) sccp dest create 1 1 2 2 0 255 255

	5.9) sccp rsp create 1 2 0 0

	5.10) sccp rss create 1 2 8 0

6) Assuming mobicents ss7-simulator is started already you should see on jboss console "10:21:28,046 WARN  [SccpStackImpl-SccpStack] Rx : MTP-RESUME: AffectedDpc=2" indicating that USSD Gateway M3UA layer is now connected with ss7-simulator

7) Dial *519# on ss7-simulator and you should see USSD getting exchanged between simulator and server
