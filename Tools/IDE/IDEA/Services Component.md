# Services Component

Make this available

<img src="https://i.imghippo.com/files/DFkYP1718332958.jpg" alt="" border="0" height="200px" width="1000px">

### Step 1: Find your `workspace.xml`

<img src="https://i.imghippo.com/files/YrFlF1718333077.jpg" alt="" border="0">

### Step 2: Paste in Code

```xml
<component name="RunDashboard">
    <option name="ruleStates">
        <list>
            <RuleState>
                <option name="name" value="ConfigurationTypeDashboardGroupingRule"/>
            </RuleState>
            <RuleState>
                <option name="name" value="StatusDashboardGroupingRule"/>
            </RuleState>
        </list>
    </option>
    <option name="configurationTypes">
        <set>
            <option value="SpringBootApplicationConfigurationType"/>
        </set>
    </option>
</component> 
```