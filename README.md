# AIDR + Apigee

A shared flow bundle that can be used to guard LLM inputs and outputs using AIDR.

## Setup

Download the [latest version](https://github.com/pangeacyber/aidr-apigee/releases) of the AIDR shared flow.

Import the shared flow into Apigee by going to [Shared flows](https://console.cloud.google.com/apigee/sharedflows) and selecting
**Upload bundle**. Name the new shared flow "aidr-guard". Deploy the shared flow
to the same environment as your API proxy.

In your API proxy, create the following policies:

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<AssignMessage async="false" continueOnError="false" enabled="true" name="AM-SetAIGuardConfig">
  <DisplayName>AM-SetAIGuardConfig</DisplayName>
  <Properties/>
  <AssignVariable>
    <Name>aiguard_token</Name>
    <Value>pts_tokentokentoken</Value>
  </AssignVariable>
  <AssignVariable>
    <Name>aiguard_base_url</Name>
    <Value>ai-guard.aws.us-west-2.pangea.cloud</Value>
  </AssignVariable>
  <IgnoreUnresolvedVariables>true</IgnoreUnresolvedVariables>
</AssignMessage>
```

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<FlowCallout async="false" continueOnError="false" enabled="true" name="FC-LLMGuardInput">
  <DisplayName>FC-LLMGuardInput</DisplayName>
  <Properties/>
  <SharedFlowBundle>aidr-guard</SharedFlowBundle>
</FlowCallout>
```

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<FlowCallout async="false" continueOnError="false" enabled="true" name="FC-LLMGuardOutput">
  <DisplayName>FC-LLMGuardOutput</DisplayName>
  <Properties/>
  <SharedFlowBundle>aidr-guard</SharedFlowBundle>
</FlowCallout>
```
