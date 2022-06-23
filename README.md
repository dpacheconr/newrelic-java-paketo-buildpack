
# newrelic-java-paketo-buildpack
## The Paketo New Relic Agent Buildpack is a Cloud Native Buildpack that contributes and configures the New Relic Java Agent.

**Behavior**
  
This buildpack will participate if all the following conditions are met

<br/>
The `$BP_NEW_RELIC_ENABLED` is set to true (defaults to false)

The buildpack will do the following for Java applications:
<br/>
Contributes the New Relic Java Agent to the newrelic_java layer configures `$JAVA_TOOL_OPTIONS` to use it
<br/>
Copies New Relic Java Agent configuration file in /resources/newrelic.yml to the newrelic_java layer
<br/><br/>

**Variables**
<br/>

| Key | Description |
|--|--|
| `BP_NEW_RELIC_ENABLED` | Defaults to false - will not participate - as set in buildpack.toml   |
| `NEW_RELIC_APP_NAME` | Defaults to app_name variable set in newrelic.yml or newrelic.js respectively   |
| `NEW_RELIC_LICENSE_KEY`  | Required at build time or runtime     |
| `NEW_RELIC_AGENT_ENABLED`  | Defaults to agent_enabled variable set in newrelic.yml or newrelic.js respectively |

<br/>
You can override any setting from a system property in the newrelic.yml by setting an environment variable.

The environment variable corresponding to a given setting in the config file is the setting name prefixed by NEW_RELIC with all dots (.) and dashes (-) replaced by underscores (_). 

For this to work as part of the **build stage**, you will need to precede the variables with BPE, i.e. `BPE_NEW_RELIC_LOG_LEVEL`

For this to work during **runtime**, simply prefix the setting name, i.e. `NEW_RELIC_LOG_LEVEL`

Please refer New Relic Java agent documentation for more information

https://docs.newrelic.com/docs/apm/agents/java-agent/configuration/java-agent-configuration-config-file/#Environment_Variables


<br/>

**Example**
  
pack build ... \

--env `BP_NEW_RELIC_ENABLED`=true \ -----------> Required condition to build the buildpack as default is false

--env `BPE_NEW_RELIC_APP_NAME`=xxxxxxxxxx \ -----------> Optional can be set on runtime

--env `BPE_NEW_RELIC_LICENSE_KEY`=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx -----------> Optional can be set on runtime

 <br/>

**Configuration settings precedence**
 

![Settings precedence](https://docs.newrelic.com/static/60ca967eab99ca225186310913ae2de6/8c557/java-config-cascade.png)

With the Java agent, server-side configuration overrides all other settings.
Environment variables override Java system properties.
Java properties override user configuration settings in your newrelic.yml file.
User settings override the newrelic.yml default settings.
<br/>
Please refer to New Relic Java agent documentation for more information

[https://docs.newrelic.com/docs/apm/agents/java-agent/configuration/java-agent-configuration-config-file/#config-options-precedence](https://docs.newrelic.com/static/60ca967eab99ca225186310913ae2de6/8c557/java-config-cascade.png)

<br/>
By default the Java agent configuration file will be located at /layers/newrelic_java/nr-agent-java/newrelic.yml, this can be overwritten at runtime, with configmap for kubernetes deployments.

Please refer to Kubernetes documentation for more information about configmaps

[https://kubernetes.io/docs/concepts/configuration/configmap/](https://kubernetes.io/docs/concepts/configuration/configmap/)
