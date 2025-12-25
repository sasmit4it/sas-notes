1. To launch ASG, you use launch configuration or launch template. launch config is the older/deprecated way , so better to stick with launch templates.
    
2. While launching ASG, so refer one of the launch template.
    
3. Next you provide min, max and desired capacity of ASG.
    
    1. min capacity: minimum number is instances in ASG after a scale down operation.
        
    2. desired capacity : these much instances will be generally running in this ASG
        
    3. max capacity : max number of instances after scale up operation.
        
4. e.g. min is 1 , desired is 2 and max is 4. Normally ASG will have 2 instances. Let’s say there is scale out(scale up) event triggered. The number of instances will go up to 4, but never beyond 4. Similar is the case for min.
    
5. scale in /scale out events(auto scaling policies)
    
    1. maintain constant instance levels : just set min=max=desired. This will cause ASG to maintain constant number of instances
        
    2. scale manually as needed
        
    3. scale based on schedule : configurable in ASG->automatic scaling-> scheduled actions
        
    4. dynamic scaling
        
        1. target tracking scaling : monitor instance CPU or network usage or request count. e.g. if cpu utilization is over 70% scale out.
            
        2. simple scaling : take scaling action based on alarm
            
        3. step scaling : take multiple steps based on alarm.
            
6. lifecycle hooks: asg->instance management → lifecycle hooks
    

these are used to provide a hook when asg automatically creates or terminates instance. Hook will basically trigger a notification, which can be consumed and additional action performed.

1. default termination policy : asg-> details → advanced config → Termination policies
    

Policy defines how instances are terminated when scale in event happens.

1. default cool down period : asg → details → advanced config
    

This setting prevents frequent scale in /scale out operations. After each scale in/out operation, asg waits for cool down period before triggering new operation