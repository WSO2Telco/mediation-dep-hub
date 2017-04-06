## WSO2Telco HUB 3.0.0 SNAPSHOT

Build Status : [![Build Status](http://ci.wso2telco.com/job/mediation-dep-hub/badge/icon)](http://ci.wso2telco.com/job/mediation-dep-hub)

## Setting up the Deployment

The deployment consists of configuring 2 products:

   1 wso2esb
   
   2 wso2telcohub

## Configuring WSO2 ESB

Download a fresh WSO2 ESB 5.0.0 pack from website: http://wso2.com/products/enterprise-service-bus/

Add following .jar files to ESB as described (WSO2.Telco related files are bundled with wso2telco_esb_hub.zip):

* To *ESB_HOME/repository/components/dropins*

```
 dbutils.jar (repository: WSO2Telco/core-util)
 mnc-resolver.jar (repository: WSO2Telco/core-util)
 msisdn-validator.jar (repository: WSO2Telco/core-util)
 operator-service.jar (repository: WSO2Telco/component-dep)
 subscription-validator.jar (repository: WSO2Telco/component-dep)
 javax.persistence_1.0.0.jar (external: http://www.java2s.com/Code/Jar/j/Downloadjavaxpersistence100jar.htm)
```

* To  *ESB_HOME/repository/components/lib*

```
 com.wso2telco.dep.spend.limit.mediator.jar (repository: WSO2Telco/mediation-dep)
 mysql-connector-java-5.1.36-bin.jar (external: http://central.maven.org/maven2/mysql/mysql-connector-java/5.1.36/mysql-connector-java-5.1.36.jar)
 com.wso2telco.dep.common.mediation.jar(repository: WSO2Telco/mediation-dep-common) 
```

Add following configuration files:
* *mediator-conf.properties* to *ESB_HOME/repository/conf*
(repository: https://github.com/WSO2Telco/component-dep/blob/master/features/com.wso2telco.dep.hub.core.feature/src/main/resources/config/mediator-conf.properties)

* *MobileCountryConfig.xml* to *ESB_HOME/repository/conf*
(repository: https://github.com/WSO2Telco/component-dep/blob/master/features/com.wso2telco.dep.hub.core.feature/src/main/resources/config/MobileCountryConfig.xml)

* *spendLimit.xml* to *ESB_HOME/repository/conf*
(repository:https://github.com/WSO2Telco/component-dep/blob/master/features/com.wso2telco.dep.hub.core.feature/src/main/resources/config/spendLimit.xml)

Add following property to *ESB_HOME/repository/conf/synapse.properties*
`synapse.commons.json.output.autoPrimitive=false`

## Configuring datasources

Add following database references: proddepdb and prodUMdb (with suitable user credentials) at *ESB_HOME/repository/conf/datasources/masterdatasources.xml*

proddepdb : *http://docs.wso2telco.com/display/HG/Setup+DEP+database*

prodUMdb : *http://docs.wso2telco.com/display/HG/Setup+++User+Manager+database*

Important: Same databases are referred while setting up wso2telcohub

There will be 10 CApp files (.car files)

* com.wso2telco.dep.common.capp_3.0.0-SNAPSHOT.car
* com.wso2telco.dep.hub.creditapi.capp_3.0.0-SNAPSHOT.car
* com.wso2telco.dep.hub.customerinfoapi.capp_3.0.0-SNAPSHOT.car
* com.wso2telco.dep.hub.locationapi.capp_3.0.0-SNAPSHOT.car
* com.wso2telco.dep.hub.paymentapi.capp_3.0.0-SNAPSHOT.car
* com.wso2telco.dep.hub.provisionapi.capp_3.0.0-SNAPSHOT.car
* com.wso2telco.dep.hub.ussdapi.capp_3.0.0-SNAPSHOT.car
* com.wso2telco.dep.hub.walletapi.capp_3.0.0-SNAPSHOT.car

Start WSO2 ESB and upload CApp files (Refer: *https://docs.wso2.com/display/ESB481/Creating+and+Deploying+a+Carbon+Application*)


## Configuring WSO2 TELCO HUB

1. Download WSO2 TELCO HUB (for Gateway) from website: http://wso2telco.com/hub
2. Configure databases and workflow. (If database has already created, you only need to add relevant configurations).

Database configurations: *http://docs.wso2telco.com/pages/viewpage.action?pageId=1507746*

Workflow configurations: *http://docs.wso2telco.com/display/HG/Install+workflows*

3. Start WSO2 TELCO HUB and goto Publisher app

4. Create APIs for necessary use-cases and configure endpoint to ESB APIs. API __context__  and __name__ should be as follows:

* payment
* ussd
* location
* smsmessaging
* credit
* wallet
* provision
* customerinfo

If WSO2 Telco Hub is port offset, change the port numbers at the following files accordingly
    (Ref: http://docs.wso2telco.com/display/HG/Offsetting+Product)

* TELCO_HUB_HOME/repository/deployment/server/jaggeryapps/manage/site/conf/site.json
* TELCO_HUB_HOME/repository/conf/workflow.properties
* TELCO_HUB_HOME/repository/resources/workflow-extensions.xml

If MSISDN blacklist feature needs to be enabled for a particular api, then add following property to the insequence of the corresponding api synapse file in API manager.

```
<property name="api.check.blacklist" value="true" scope="transport"/>
```

If MSISDN whitelist feature needs to be enabled for a particular api, then add following property to the insequence of the corresponding api synapse file in API manager.
```
<property name="api.check.whitelist" value="true" scope="transport"/>
```

Note:
API compatible with the WSO2.Telco_HubAPI_V1.2 spec 








