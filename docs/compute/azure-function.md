# compute
## serverless
#### azure function
- service is made of two components : runtime (provides binding) and scaling component (manages invocation and number of instances)
- hosting : on all plans, azure function service needs azure storage account that provides blob, queue, file and table.
    - consumption plan
    - premium plan
    - app service plan
    - kubernetes using KEDA
    - app service environment (isolated app service plan)
- scaling : 
    - all functions within a function app share resources. (Consumption plan : 1.5 GB RAM + 1 CPU core)
    - scaling increases function host instance count : scales all the functions in the function app
    - max scale out : controlled by `functionAppScaleLimit`
        - consumption  plan : 200 instances
        - premium plan : 100 instances
        - app service plan : manual / auto scale number of instance VMs

:::mermaid
    classDiagram
    direction LR
    class FunctionService
    class HostService
    class Scale
    class Identity
    class Function {
        run()
    }
    
    FunctionService--*HostService : runs-on
    FunctionService-->Scale
    FunctionService--*Identity
    FunctionService "1"--> "*" Function
:::

#### durable function
there are four interactions that durable functions support
- function chaining
- fan out
- monitor
- human interaction
there are four types of durable functions
- orchestration function : needs the behavior to be deterministic
- activity function : does the actual work
- entity function : controls entity persistance
- xx ?