# CloudFormation

## Physical and Logical Resources

- CloudFormation begins with a template defined in YAML or JSON file
- The template contains logical resources (what we want to create)
- Templates can be used to create CloudFormation Stacks (one ore many stacks)
- The initial job of a stack is to create physical resources based on the logical resources defined in the template
- If a stack's template are changed, physical resources are changed as well
- If a stack is deleted, normally the physical resources are deleted

## Template Parameters and Pseudo Parameters

- Template parameters allow input via the console, CLI or API when the stack is created or updated
- Parameters are defined within the resources and they can be referenced from within the logical resources
- Parameters can have default values, allowed values, min/max length, allowed patterns, no echo (useful for passwords, the value is not displayed when typed) and types
- Pseudo Parameters: 
    - AWS makes available parameters which can be referenced by the CF template
    - Example:
        - `AWS::Region`
        - `AWS::StackId`
        - `AWS::StackName`
        - `AWS::AccountId`
    - Pseudo parameters are parameters which can not be populated by us, they are populated by AWS and provided for us to reference them
- Parameters provide portability for the template
- Best practice:
    - Minimize number of parameters and provide defaults where applicable
    - Use pseudo parameters where possible

## Intrinsic Functions

- Intrinsic functions can be used in templates to assign values to properties that are not available until runtime
- Examples of functions:
    - `Ref` and `Fn::GetAtt`: reference a value from one logical resource
    - `Fn::Join` and `Fn::Split`: join/split strings to create new ones
    - `Fn::GetAZs` and `Fn::Select`: get availability zones in a regions and select one
    - Conditions: `Fn::IF`, `And`, `Equals`, `Not`, `Or`
    - `Fn::Base64` and `Fn::Sub`: encode strings to base64, substitute replacement on variables in the text
    - `Fn:Cidr`: build CIDR blocks
- `Fn::GetAZs` - returns the available AZs in region. If the region has a default VPC configured, it return the AZs which are available in the default VPC

## Mappings

- Templates can contain a `Mappings` objects which can contain keys to values objects
- Mappings can have one level or tep and second level keys
- Mappings use another intrinsic function `Fn::FindInMap`
- Mappings are used to improve template portability
- Example:
    ```
    Mappings:
        RegionMap:
            us-east-1:
                HVM64: 'ami-xxx'
                HVMG2: 'ami-yyy'
            us-east-2:
                HVM64: 'ami-zzz'
                HVMG2: 'ami-vvv'
    ```

## Outputs

- The `Outputs` section in a template is optional
- We can declare values in this section which will be visible as output in the CLI/Console
- Output will be accessible from a parent stack when using nesting
- Outputs can be exported allowing cross-stack references
- Example:
    ```
    Outputs:
        WordPressUrl:
            Description: 'description text'
            Value: !Join['', 'https://', !GetAtt Instance.DNSName]
    ```

## Conditions

- Allows stack to react to certain conditions and change infrastructure based on those
- They declared in an optional section named `Conditions`
- We can declare many conditions, each of them being evaluated to `TRUE` or `FALSE`
- Conditions are evaluated before resources are created
- Conditions use other intrinsic functions: `AND`, `EQUALS`, `IF`, `NOT`, `OR`
- Any resource can have associated a condition which will define if the resource will be created or not
- Examples: we can have conditions which evaluate based on the environment (dev, test, prod) in which the template is executed
- Condition example:
    ```
    Conditions:
        IsProd: !Equals
            - !Ref EnvType
            - `prod`
    ```
- Conditions can be nested

## DependsOne

- Allows us to establish dependencies between resources
- CF tried to be efficient by creating/updating/deleting resources in parallel
- Also, it tries to determine a dependency order (example: VPC => SUBNET => EC2) by using references or functions
- Dependency can be defined using the `DependsOn` property specify the resource on which we depend on
- `DependsOn` can accept a single resource or a list of resources

## Creation Policies, Wait Conditions and cfn-signal

- Creation Policies, Wait Conditions and cfn-signals provide a few ways to notify CF with details signals on completion or not of creation of resources
- We can configure CF to wait for a certain number of success signals
- We also configure a timeout within which the signals are received (max 12H)
- The the number of success signals are received within the timeout, CF stacks moves to `CREATE_COMPLETE`
- `cfn-signal` is an utility running on the EC2 instances sending success/failure signals to CF
- If the timeout is reached and the number of success signals are not met, the stack will fail creation
- For provisioning EC2 and ASG, we should us a `CreationPolicy`
- For other requirements we might chose to use a `WaitCondition`
- A `WaitCondition` is defined as a logical resource, meaning it can have `DependsOn` property. It can be used as a general progress gait in the template
- A `WaitCondition` relies on a `WaitHandle`, which is another logical resource. Its job is to generate a presigned url which can be used to send signals to `WaitCondition`
- With `WaitHandle` we can pass back data to the template. This data can be retrieved using the `!GetAtt WaitCondition.Data` function

## Nested Stacks

- Most simple projects will generally utilise a CF stack
- Stacks can have limits:
    - Resource limit: 500 resources per stack
    - We can't easily reuse resources, example reference a VPC
- There are 2 ways to architect multi-stack projects:
    - Nested Stacks
    - Cross-Stack References
- Nested Stacks:
    - Root Stack: the stack which is created first, created manually or using some automation
    - A Parent Stack is the parent of any stack which it immediately creates
    - A root stack can create nested stacks having several parent stacks
    - A root stack can have parameters and outputs (just like a normals stack)
- A stack can have another CF stack as a resource using `AWS::CloudFormation::Stack` type which needs an url to the template
- We can provide input values to the nested stacks. We need to supply values to any parameters from a nested stack if the parameter does not have a default value defined
- Any outputs of a nested stack are returned to the root stack which can be referenced as `NESTEDStack.Outputs.XXX`
- Benefits of a nested stack is to reuse the same template, not the actual stack created
- We should use nested stacks when we want to link the lifecycles of different stacks
