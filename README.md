# Liberty-Profile-For-Mq

file, version 8.5.5.3.

my server.xml looks the following:

<server description="new server">

    <!-- Enable features -->
    <featureManager>
        <feature>jsf-2.0</feature>
        <feature>jpa-2.0</feature>
        <feature>jndi-1.0</feature>
        <feature>localConnector-1.0</feature>
        <feature>beanValidation-1.0</feature>
        <feature>wasJmsClient-1.1</feature>
        <feature>jaxws-2.2</feature>
        <feature>jmsMdb-3.1</feature>
    </featureManager>

    <!-- To access this server from a remote client add a host attribute to the following element, e.g. host="*" -->
    <httpEndpoint id="defaultHttpEndpoint" httpPort="9080" httpsPort="9443" />

    <application id="myapp" name="myApp" type="ear" location="d:\somehwere">
        <classloader delegation="parentLast" commonLibraryRef="global" />
    </application>

    <variable name="wmqJmsClient.rar.location" value="D:\opt\was_liberty_profile\was_8_5_5_3\wlp\usr\shared\wmq\wmq.jmsra.rar"/>

    <library id="global">
        <file name="d:/dev/mavenrepo/com/h2database/h2/1.4.181/h2-1.4.181.jar" />
    </library>

    <jmsQueue id="MYAPP_QUEUE" jndiName="jms/test/testq">
        <properties.wmqJms 
            baseQueueName="MYAPP.TEST.A" 
            persistence="PERS" 
            targetClient="MQ"/>
    </jmsQueue>

    <jmsQueueConnectionFactory id="TEST.SVRCONN.001" jndiName="jms/test/testcf"><!-- connectionManagerRef="ConMgr2">-->
     <properties.wmqJms 
        channel="WM026D.SVRCONN.001" 
        hostName="i19328.myhost.ch" 
        port="1439" 
        queueManager="WM026D" 
        targetClientMatching="false"/>
    </jmsQueueConnectionFactory>

    <jmsActivationSpec id="fvtapp/fvtmdb/FVTMessageDrivenBean">
  <properties.wmqJms destinationRef="MYAPP_QUEUE"
                     destinationType="javax.jms.Queue"
                     queueManager="WM026D"/>
</jmsActivationSpec>
</server>


<?xml version="1.0" encoding="UTF-8"?>
<server description="new server">

  <!-- Enable features -->
  <featureManager>
    <feature>wmqJmsClient-1.1</feature>
    <feature>jndi-1.0</feature>
    <feature>jsp-2.2</feature>
    <feature>localConnector-1.0</feature>
  </featureManager>

  <variable name="wmqJmsClient.rar.location" value="D:\wlp\wmq\wmq.jmsra.rar"/>

  <jmsQueueConnectionFactory jndiName="jms/wmqCF" connectionManagerRef="ConMgr6">
    <properties.wmqJms
            transportType="CLIENT"
            hostName="hostname"
            port="1514"
            channel="SYSTEM.DEF.SVRCONN"
            queueManager="QM"           
            />
  </jmsQueueConnectionFactory>

  <connectionManager id="ConMgr6" maxPoolSize="2"/>

  <applicationMonitor updateTrigger="mbean"/>
  <application id="App"
               location="...\app.war"
               name="App" type="war"/>
  <!-- To access this server from a remote client add a host attribute to the following element, e.g. host="*" -->
  <httpEndpoint id="defaultHttpEndpoint"
                httpPort="9080"
                httpsPort="9443"/>
</server>
