# AWS EC2 - Elastic Compute Cloud

Virtual Machines hosted on AWS

## AMI (Amazon Machine Image)

Required template that contains the software configuration (OS, application server and applications). Also includes launch permissions and Volume information.

- Virtualization types:
    - **HVM (Hardware-Assisted Virtual Machine)**: Best performance.
    - **Paravirtualization**: 
- Root Device Type:
    - **EBS (Elastic Block Storage)**: 
    - **Instance Store**: 
- By purpose:
    - **General purpose**: Code (t-).
    - **Compute optimized**: Code (c-).
    - **GPU Graphics**: Code (p-).
    - **GPU Compute**: Code (p-).
    - **Memory optimized**: Code (x-).
    - **Storage optimized**: Code (h-).
- Purchasing options:
    - **On-demand**:
        - Suitable when you are not sure about the capacity needed (sudden spike of traffic, short projects, R&D, etc).
        - Pay-as-you-go: You pay instances when needed, with current listed price per usage.
        - Most expensive.
    - **Reserved**:
        - Suitable when you are aware about the capacity needed.
        - Proactive type: You pay the VMs beforehand.
        - Earlier and longer the period reserved, lower cost.
        - Instances can be reserved from 1 to 3 years.
    - **Scheduled**:
        - Purchase instances that are always available on the specified schedule for a one-year term.
    - **Spot**:
        - Give-me-if-available-for-this-price.
        - Saves costs significantly.
        - Little risky.
        - You bid for the unused EC2 instance capacity of AWS: If the actual price of that EC2 instance becomes equal or lower than the bid price, the instance gets assigned to you at **your bid price**, but if the actual price increases, the instance may get terminated with a half an hour's noticed (this half hour will not be charged).
        - Some concepts:
            - **Spot Instance Pool**: a set of unused instances with the same instance type.
            - **Spot Price**: Currently hourly price of the instance.
            - **Spot Instance Request**: Your bid or the price you are willing to pay for an instance.
            - **Spot Fleet**: You specify the criteria of the instance and then the Spot Fleet selects the Spot Instance Pool that meets your needs and launches Spot Instances.
            - **Spot Instance Interruption**: When Amazon terminates your instance.
    - **Dedicated Host**:
        - Instance it's not shaded with other customer's VMs, but may be in a different machine.
    - **Dedicated Instance**:
        - Instance it's not shader with other customer's VMs, and always will be running in the same machine.
        - Useful when using hardware licensing.
