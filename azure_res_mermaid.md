---
Azure ressources
---
classDiagram
  class NetworkSecurityGroup
    
  class SecurityRule

  class VirtualNetwork

  class SubNet

  class NetworkInterface

  class VirtualMachine

  NetworkSecurityGroup *-- "1..n" SecurityRule
  VirtualNetwork *.. "0..n" SubNet
  SubNet ..> NetworkSecurityGroup
  NetworkInterface ..> NetworkSecurityGroup
  VirtualMachine --> NetworkInterface
  VirtualMachine --> VirtualNetwork : isIn

