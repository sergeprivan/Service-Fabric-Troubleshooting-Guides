```mermaid
graph TD;
    A[Start Here] --> Decision1[VMSS running on deprecated image] 
    Decision1 --> HostingContainerDecision{Hosting Containerized Applications}    
    HostingContainerDecision -->|Yes| PickReplacementOption{Choose a replacement image}
    HostingContainerDecision -->|No| NodeTypeDecision2{Which NodeType is affected}
    PickReplacementOption --> CustomOSImage[Custom OS Image]    
    PickReplacementOption --> ManualInstallImage[Install MCR manually]
    PickReplacementOption --> MirantisGalleryImage[MCR Image from Azure Gallery]
    CustomOSImage --> PrepNewImageStep1[Get started: Prep Windows for containers] --> NewImageIsPrepped[Automatic OS image upgrade for custom images]
    ManualInstallImage --> ManualInstallStep1[Install Mirantis on Azure Service Fabric via Custom Script VM Extension] --> NewImageIsPrepped[Sequence extension provisioning in virtual machine scale sets]
    MirantisGalleryImage --> NewImageIsPrepped[Find and use Azure Marketplace VM images with Azure PowerShell]
    NewImageIsPrepped --> NodeTypeDecision1{Which NodeType is affected}
    NodeTypeDecision1 --> YesHostingContainersPrimary[Primary Node Type]
    NodeTypeDecision1 --> YesHostingContainersSecondary[Secondary Node Type]
    NodeTypeDecision2 --> NoHostingContainersPrimary[Primary Node Type]
    NodeTypeDecision2 --> NoHostingContainersSecondary[Secondary Node Type]
    NoHostingContainersPrimary --> IsProductionDecision{Production System}
    IsProductionDecision --> K[Scenario 1/Option 2]
    IsProductionDecision --> L[Scenario 1/Option 3]
    NoHostingContainersSecondary --> AddNodeType1[Add new Node Type]
    AddNodeType1 --> MigrateWorkloads1[Migrate Workloads]
    YesHostingContainersPrimary --> HostingPrepImagePrimary[Scenario 2/Option 2]
    HostingPrepImagePrimary --> HostingPrepImagePrimaryStep1[Create new VMSS based on prepped Image] --> HostingPrepImagePrimaryStep2[Install MCR] --> HostingPrepImagePrimaryStep3[OS SKU upgrade]
    YesHostingContainersSecondary --> HostingPrepImageSecondaryStep1[Create new VMSS based on prepped Image] --> AddNodeType2[Add New Node Type]
    AddNodeType2 --> MigrateWorkloads2[Migrate workloads]
```