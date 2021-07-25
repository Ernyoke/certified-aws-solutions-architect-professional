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